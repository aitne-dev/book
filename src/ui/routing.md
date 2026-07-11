# Routing

Aitne includes a client-side router with nested routes and URL parameters.

## Setting Up

Wrap your app in a `Router` with `Routes` and `Route` children:

```mbx
fn app() -> &View {
  <Router base="/">
    <Routes fallback={() => <NotFound />}>
      <Route path="/" view={() => <Home />} />
      <Route path="/about" view={() => <About />} />
    </Routes>
  </Router>
}
```

## Nested Routes

Use `ParentRoute` for layouts with child routes rendered inside `<Outlet />`:

```mbx
<Router base="/">
  <Routes fallback={() => <NotFound />}>
    <Route path="/" view={() => <Home />} />
    <ParentRoute path="/users" view={() => <UsersLayout />}>
      <Route path="/" view={() => <UsersList />} />
      <Route path="/:id" view={() => <UserDetail />} />
    </ParentRoute>
  </Routes>
</Router>
```

The parent layout uses `<Outlet />` where child content appears:

```mbx
fn users_layout() -> &View {
  <div class="users-layout">
    <aside>"Sidebar"</aside>
    <main>
      <Outlet />
    </main>
  </div>
}
```

## Client-Side Links

Use `<Anchor>` for navigation without page reloads:

```mbx
<Anchor href="/about">"About Us"</Anchor>
<Anchor href="/users/123">"User 123"</Anchor>
```

## URL Parameters

Define dynamic segments with `:` prefix:

```mbx
<Route path="/users/:id" view={() => <UserDetail />} />
```

Access the router context to read parameters:

```mbt
fn user_detail() -> &View {
  let ctx = @dom.use_router_context()
  // ctx contains current_url, location, etc.
  <div>"User Detail"</div>
}
```
