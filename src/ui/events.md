# Events

Handle user interactions with the `on:` namespace in MBX.

## Basic Handlers

```mbx
<button on:click={fn(ev) {
  println("Button clicked!")
}}>
  "Click me"
</button>
```

## Reading Input Values

Use event helpers to read form data:

```mbx
let (value, set_value) = @reactive.create_signal("")

<input
  value={value}
  on:input={fn(ev) {
    set_value.set(@ffi.event_target_value(ev))
  }}
/>
```

## Event Types

| MBX Event | Fires On |
|---|---|
| `on:click` | Element clicked |
| `on:input` | Input value changed |
| `on:change` | Selection changed |
| `on:submit` | Form submitted |
| `on:keydown` | Key pressed down |
| `on:keyup` | Key released |

## Window Events

Subscribe to window events with cleanup handled automatically:

```mbt
@reactive.create_effect(fn(_) {
  let cleanup = @dom.on_window_event("resize", fn(ev) {
    ...
  })
  @reactive.on_cleanup(cleanup)
})
```

## Preventing Default

Call `event_prevent_default` in your handler:

```mbx
<form on:submit={fn(ev) {
  @ffi.event_prevent_default(ev)
  ...
}}>
```
