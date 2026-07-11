# Memo

A memo is a derived value — it computes a result from other signals and caches it. Memos are lazy: they only recompute when their dependencies change and their value is actually requested. This makes them efficient for expensive computations.

## Creating a Memo

```mbt
let (a, set_a) = @reactive.create_signal(1)
let (b, set_b) = @reactive.create_signal(2)

let sum = @reactive.Memo::new(fn(_prev) {
  a.get() + b.get()
})
```

`sum` now tracks `a` and `b` as dependencies. Reading `sum.get()` returns the cached value or triggers recomputation if `a` or `b` changed.

## When to Use Memos

Use memos when:

- A value is derived from multiple signals and used in several places
- The computation is expensive (filtering, formatting, transforming data)
- You want to avoid redundant work

Without a memo, the computation runs every time a component re-renders. With a memo, it runs once and caches.

## Glitch-Free Propagation

Aitne's reactive system handles diamond dependencies correctly. If `A` and `B` depend on `Source`, and `C` depends on both `A` and `B`, then `C` only recomputes once when `Source` changes, always seeing consistent values for `A` and `B`.
