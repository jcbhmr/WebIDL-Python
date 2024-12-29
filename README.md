<p align=center>
  <b>You're probably looking for <a href="https://jcbhmr.github.io/WebIDL-Python/">jcbhmr.github.io/WebIDL-Python</a></b>
</p>

# Python language binding for Web IDL

🐍 A good way to represent Web IDL types in Python

<table align=center><td>

```py
# Fetch API via https://pypi.org/project/fetch2/
from fetch2 import fetch
response = await fetch("https://example.org/", {"method": "POST"})

# Web Serial API via https://pypi.org/project/serial2/
from serial2 import navigator
port = await navigator.serial.request_port()
await port.open({"baud_rate": 9600})

# DOM API via https://pypi.org/project/dom2/
from dom2 import document
button = document.query_selector("button")
button.add_event_listener("click", lambda event: print("Clicked!"))
```

</table>

📚 This is a **specification**, not an implementation \
🤷‍♀️ You don't have to conform to this \
📄 Standardizes the mapping of Web IDL ↔ Python types \
✅ Ensures uniform patterns across all Web API surfaces

## Usage

There's nothing to install. Just make sure that your bindings to a Python-implemented Web IDL API mostly conform to the concepts and ideas layed out in the specification document. You are free to write a Python implementation of a Web API or try your hand at binding to JavaScript APIs via a Python WASM & JS bridge membrane.

## Development

![Markdown](https://img.shields.io/static/v1?style=for-the-badge&message=Markdown&color=000000&logo=Markdown&logoColor=FFFFFF&label=)

The document's structure is intended to mimic the [JavaScript binding section](https://webidl.spec.whatwg.org/#javascript-binding) of the Web IDL standard and take inspiration from [the Java binding for Web IDL](https://www.w3.org/TR/WebIDL-Java/).

Some other prior art resources from how other ecosystems have started adopting Web IDL (or Web API bindings) into their own language idioms are the [web-sys](https://crates.io/crates/web-sys) crate from the Rust ecosystem and [Pyodide](https://pyodide.org/en/stable/usage/type-conversions.html) from the Python ecosystem.
