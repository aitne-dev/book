# Context

Context allows passing data through the component tree without threading it through every level as props. This is useful for shared concerns like theming, user authentication, or router state.

## Providing Context

In a parent component, make a value available to all descendants:

```mbt
struct Theme { background : String; color : String }

fn app() -> &View {
  let theme = Theme::new(background: "#333", color: "#fff")
  @reactive.provide_context(theme)
  // children can now access the theme
}
```

## Using Context

Any descendant can retrieve the nearest matching context:

```mbt
fn themed_button() -> &View {
  let theme = @reactive.use_context::<Theme>()
  match theme {
    Some(t) => <button style={"background: \{t.background}; color: \{t.color}"}>"Click"</button>
    None => <button>"Click"</button>
  }
}
```

`use_context` returns `Some(value)` if a context of that type exists in the owner chain, or `None` otherwise.

## Resolution Behavior

Context walks up the reactive owner tree. The nearest matching value wins. A child component can override a context from a parent by providing a new value of the same type.

## Context vs Props

- **Props** — explicit, passed directly to a component
- **Context** — implicit, propagated automatically through the tree

Use props for component-specific data. Use context for cross-cutting concerns that many components share.
