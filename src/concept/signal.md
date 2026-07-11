# Signal

Signals are the foundation of reactivity in Aitne. Think of a signal as a piece of state that, when read inside a reactive context, automatically tracks as a dependency. When the signal's value changes, everything that depends on it updates automatically — no manual wiring needed.

## Creating a Signal

Signals are created in pairs. The most common way is `create_signal`, which returns a tuple of a read signal and a write signal:

```mbt
let (count, set_count) = @reactive.create_signal(0)
```

- `count` — read the current value
- `set_count` — set a new value

## Reading in MBX

In MBX, reading a signal inside a `{}` block creates a reactive binding. A closure `() => ...` makes it update automatically when the signal changes:

```mbx
<p>{ () => count.get().to_string() }</p>
```

A plain expression (non-closure) is evaluated once:

```mbx
<p>{count.get().to_string()}</p>
```

## Writing

Write to a signal using `set` or `update`:

```mbt
set_count.set(5)            // replace value
set_count.update(|v| v + 1) // transform value
```

## A Complete Example

```mbx
fn counter() -> &View {
  let (count, set_count) = @reactive.create_signal(0)
  <div>
    <p>{ () => count.get().to_string() }</p>
    <button on:click={(_) => set_count.set(count.get() + 1)}>+1</button>
    <button on:click={(_) => set_count.update(|v| v - 1)}>-1</button>
  </div>
}

fn main {
  let _ = @dom.mount_to_body(fn() { counter() })
}
```

When the user clicks +1, only the `<p>` text updates — not the whole page. This is fine-grained reactivity.

## How It Works Under the Hood

Aitne's signals form a dependency graph. When you read a signal inside a reactive scope (like `{}` with a closure, or an effect), the signal registers that scope as a subscriber. When the signal changes, it notifies all subscribers, which then re-execute.
