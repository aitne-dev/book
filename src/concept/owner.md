# Owner

Every reactive node in Aitne belongs to an owner. The owner tree manages lifecycle — when an owner is cleaned up, all its children, effects, and resources are disposed automatically.

## Why Ownership Matters

When components are created and destroyed (e.g., navigating between pages), their resources must be cleaned up: event listeners removed, timers cleared, network requests aborted. The owner system handles this automatically.

## How It Works

The owner tree mirrors the component tree:

- When a component is created, it gets an owner
- Effects, signals, and memos created inside a component are scoped to that owner
- When a component is removed, its owner is disposed, which cleans up everything under it

## Manual Cleanup

Effects can register cleanup callbacks that run when their owner is disposed:

```mbt
@reactive.create_effect(fn(_) {
  let timer = @ffi.set_timeout(fn() { /* ... */ }, 1000)
  @reactive.on_cleanup(fn() {
    @ffi.clear_timeout(timer)
  })
})
```

This pattern is essential for timers, subscriptions, and any resource that needs explicit teardown.

## Automatic Cleanup in Views

When a view is mounted with `mount_to_body` and later destroyed, the owner system automatically disposes everything created during that view's lifetime. You only need to write `on_cleanup` for manual resources.
