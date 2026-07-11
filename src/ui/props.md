# Props

Props pass data into components. In MBX, they look like HTML attributes but compile to function parameters.
## Passing Props

Props in MBX are passed as labeled arguments, so the parameter name with `~` or `?` in MoonBit maps directly to the attribute name in the template:

```mbt
fn greeting(text~ : String) -> &View {
  <h1>"Hello, \{text}!"</h1>
}
```

```mbx
<Greeting text="World" />
```

String values are written directly. For expressions, use `{}`:

```mbx
<Greeting text={user_name} />
```

## Reactive Props

Pass signals as prop values to create reactive data flow. The prop parameter type matches whatever you pass — signal, value, or closure:

```mbx
fn app() -> &View {
  let (name, set_name) = @reactive.create_signal("Alice")
  let (age, set_age) = @reactive.create_signal(30)
  <UserCard name={name} age={age} />
}
```

## Children

Content between opening and closing tags is passed as the `children` labeled argument:

```mbt
fn card(children~ : Array[&View]) -> &View {
  <div class="card">{children}</div>
}
```

```mbx
<Card>
  <h2>"Title"</h2>
  <p>"Content"</p>
</Card>
```

## Event Handler Props

Pass event handlers as props using the `on:` namespace. The handler is received as a labeled argument in the component:

```mbx
<Btn on:click={handler} />
```

```mbt
fn btn(on_click~ : (@ffi.Event) -> Unit) -> &View {
  <button on:click={on_click}>"Click"</button>
}
```

## Optional Props

Props with `?` defaults can be omitted when calling:

```mbt
fn card(title~ : String, subtitle? : String = "") -> &View {
  ...
}
```

```mbx
<Card title="Hello" />
<Card title="Hello" subtitle="World" />
```
