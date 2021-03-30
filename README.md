# lit-html-rs

A Rust library for using the HTML template library [lit-html](https://lit-html.polymer-project.org/).

**this library is still in very early stages**

```rust
use js::*;
use lit_html::*;

#[no_mangle]
pub fn main() {
    let data = TemplateData::new();
    data.set("name", "Ferris");
    render(html!(r#"<h1>Hello ${_.name}</h1>"#, &data), DOM_BODY);
}
```

See it working [here](https://richardanaya.github.io/lit-html-rs/examples/helloworld/).

# Example

```rust
use js::*;
use lit_html::*;

static mut COUNT: u32 = 0;

fn counter() -> Template {
    let data = TemplateData::new();
    data.set("count", unsafe { COUNT });
    data.set("increment", || {
        unsafe { COUNT += 1 };
        rerender();
    });
    html!(
        r#"The current count is ${_.count} <button @click="${_.increment}">+</button>"#,
        &data
    )
}

fn app() -> Template {
    let data = TemplateData::new();
    data.set("content", &counter());
    html!(
        r#"<div>This is a counter in Rust!</div><div>${_.content}</div>"#,
        &data
    )
}

fn rerender() {
    render(&app(), DOM_BODY);
}

#[no_mangle]
pub fn main() {
    rerender();
}
```

See it working [here](https://richardanaya.github.io/lit-html-rs/examples/counter/).