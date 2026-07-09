# Components

## Writing markups in MBX

MBX is a markup syntax which like JSX but for MoonBit. MBX use `.mbx` file extension.
You can embed MoonBit code in '{}' block.
```mbx
fn app() -> &View {
  let content = "Let's try something."
  <div class="container">
    <h1> Welcome To Aitne </h1>
    <div> {content} </div>
  </div>
}
```

## Creating components

A component is a reusable UI unit. In Aitne, a component is simply a function that returns a `&View`.

Here is the simple example:
```moonbit
using @dom { trait View }

fn hello_world() -> &View {
  <h1>"Hello, World!"</h1>
}
```

The usage of components are similar to HTML elements. You can write the tag both in the primary name or in `PasalCase`.
```mbx
fn app() -> &View {
  <hello_world />
  <HelloWorld />
}
```

They can also be nested with each other.
```mbx
fn app() -> &View {
  <div> 
    <HelloWorld />
  </div>
}
```
