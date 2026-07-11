# Effect

An effect is a function that automatically re-runs whenever its tracked signal dependencies change. Effects are useful for side effects — logging to the console, making API calls, or synchronizing state with non-reactive systems.

## Creating an Effect

```mbt
let (count, set_count) = @reactive.create_signal(0)

@reactive.create_effect(fn(_prev) {
  println("Count changed to: \{count.get()}")
})

set_count.set(1)  // triggers the effect
```

The effect runs immediately on creation, then again whenever `count` changes.

## What Gets Tracked

During an effect's execution, any signal you read via `.get()` is automatically registered as a dependency. If `a` changes, but not `b`, the effect still re-runs because it read both:

```mbt
@reactive.create_effect(fn(_) {
  println("Sum: \{a.get() + b.get()}")
})
```

## Cleanup

Effects can register cleanup to run before the next execution or when the owning scope is disposed. This prevents stale callbacks and memory leaks:

```mbt
@reactive.create_effect(fn(_) {
  let id = @ffi.set_timeout(fn() {
    println("Delayed log")
  }, 1000)

  @reactive.on_cleanup(fn() {
    @ffi.clear_timeout(id)
  })
})
```

## Batching

When updating multiple signals, wrap them in `batch` so effects run only once after all updates:

```mbt
@reactive.batch(fn() {
  set_a.set(100)
  set_b.set(200)
  set_c.set(300)
})
```
