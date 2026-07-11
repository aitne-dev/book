# Lists

Use `for_node` to render reactive lists. When items are added, removed, or reordered, only the affected DOM nodes change.

## Basic Usage

```mbx
<ul>
  {for_node(
    () => items.get(),
    item => item,
    item => {
      <li>{ () => item }</li>
    }
  )}
</ul>
```

Three arguments:

1. **Data source** — a function returning the current array
2. **Key function** — maps each item to a unique identifier
3. **Render function** — renders each item to a view

## Todo Example

```mbx
fn todo_app() -> &View {
  let (input_val, set_input_val) = @reactive.create_signal("")
  let (items, set_items) = @reactive.create_signal(["Example."])

  let add_item = fn(_ev) {
    let new_item = input_val.get()
    if new_item != "" {
      set_items.update(fn(list) { list + [new_item] })
      set_input_val.set("")
    }
  }

  let remove_item = fn(item : String) {
    set_items.update(fn(list) { list.filter(fn(i) { i != item }) })
  }

  <div>
    <h1>"Todo List"</h1>
    <p>Count: { () => items.get().length().to_string() }</p>
    <input
      type="text"
      value={input_val}
      on:input={fn(ev) { set_input_val.set(@ffi.event_target_value(ev)) }}
    />
    <button on:click={add_item}>"Add"</button>
    <ul>
      {for_node(
        () => items.get(),
        item => item,
        item => {
          <li>
            {() => item}
            <button on:click={(_) => remove_item(item)}>"Delete"</button>
          </li>
        }
      )}
    </ul>
  </div>
}
```

## Keyed Updates

Items are tracked by their key, not their index. This means:

- Adding an item at the start doesn't re-render existing items
- Removing an item only removes its DOM node
- Reordering only moves DOM nodes without recreating them

## How It Works

When the data array changes, Aitne computes the difference between the old and new key sets. It then applies only the necessary DOM changes — insertions, removals, and moves — directly, without touching unchanged nodes.
