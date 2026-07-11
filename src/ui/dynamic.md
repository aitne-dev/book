# Dynamic

Aitne updates the DOM precisely where reactive values change — no Virtual DOM diffing needed.

## Reactive Text

A closure inside `{}` creates a reactive text binding that updates when signals change:

```mbx
<p>{ () => count.get().to_string() }</p>
```

Every time `count` changes, only this text node updates — not the parent element or siblings.

## Reactive Attributes

Pass a closure to make an attribute reactive:

```mbx
<div class={() =>
  if selected.get() { "active" } else { "inactive" }
}>
```

## Reactive Properties

Bind DOM properties with `value` or `prop`:

```mbx
<input value={input_val} />
```

## Conditional Rendering

Use standard MoonBit conditionals — no special directives needed:

```mbx
fn user_greeting(user : User?) -> &View {
  match user {
    Some(u) => <h1>"Welcome, \{u.name}!"</h1>
    None => <h1>"Welcome, Guest!"</h1>
  }
}
```

## How It Works

Unlike Virtual DOM frameworks that diff entire trees, Aitne compiles templates into precise DOM operations:

1. Each reactive expression targets a specific DOM node or attribute
2. When a signal changes, only that exact node or attribute updates
3. No diffing, no reconciliation — just targeted updates

Updating one signal in a list of 1000 items causes exactly one text node to change.
