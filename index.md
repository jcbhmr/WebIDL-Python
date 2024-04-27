---
title: Python language binding for Web IDL
---

# Python language binding for Web IDL

- **This version:** https://jcbhmr.me/WebIDL-Python/
- **Issue tracking:** [GitHub](https://github.com/jcbhmr/WebIDL-Python/issues)
- **Editors:** [Jacob Hummer](https://jcbhmr.me)

[CC-BY-4.0 License](https://github.com/jcbhmr/WebIDL-Python/blob/main/LICENSE)

---

## Abstract

## Introduction

**ℹ Example:**

TODO: Add example here!

## Python binding

### Names

**ℹ Example:**

| Web IDL name | Python name |
| --- | --- |
| `innerHTML`  | `inner_html` |
| `XMLHttpRequest` | `XMLHTTPRequest` |
| `htmlFor` | `html_for` |
| `HTMLHtmlElement` | `HTMLHTMLElement` |
| `CDATA_SECTION_NODE` | `CDATA_SECTION_NODE` |
| `SVGSVGElement` | `SVGSVGElement` |

### Python type mapping

| Web IDL type | Python type |
| --- | --- |
| `any` | `Any` |
| `void` | Omit or `None` |
| `boolean` | `bool` |
| `byte` | `int` |
| `octet` | `int` |
| `short` | `int` |
| `unsigned short` | `int` |
| `long` | `int` |
| `unsigned long` | `int` |
| `float` | `float` |
| `unrestricted float` | `float` |
| `double` | `float` |
| `unrestricted double` | `float` |
| `object` | `Any` |

### Interfaces

There should be a corresponding **public Python class** for each defined Web IDL interface. Even if that interface isn't `[Exposed=...]`. Why? Because you may or may not need those interfaces for type annotations or explicit type instantiations for dictionaries or other intermediary types. In the JavaScript ecosystem these types are exposed via TypeScript non-runtime `interface`-es. A similar idea applies in Python except types _do_ manifest at runtime (to some degree) and thus these dictionary types or other non-`[Exposed=...]` interfaces must be exposed to client code.

**Constants** should be left as-is in SCREAMING_SNAKE_CASE. **Static** operations or methods should be static in Python as well. **Attributes** should be implemented **not as simple properties** but instead as `@property` getters and setters.
