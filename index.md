---
title: Python language binding for Web IDL
---

- **This version:** https://jcbhmr.me/WebIDL-Python/
- **Issue tracking:** [GitHub](https://github.com/jcbhmr/WebIDL-Python/issues)
- **Editors:** [Jacob Hummer](https://jcbhmr.me)

[CC0-1.0 License](https://github.com/jcbhmr/WebIDL-Python/blob/main/LICENSE)

<small>Not affiliated with Python, W3C, or WHATWG.</small>

---

## Abstract

Describe a convention for how Web IDL APIs can be mapped to Python types.

## Introduction

_This section is non-normative._

Python is similar to JavaScript. Both are dynamically typed, both support overloading, both support optional parameters, both have OOP concepts, both have getters & setters, both have modules, and more.

## Python binding

This section describes how definitions written and defined in [Web IDL](https://webidl.spec.whatwg.org/) correspond to types & constructs in [Python](https://docs.python.org/3/reference/index.html).

Authors are encouraged to create new Python packages for each specification individually.

> [!TIP]
> For example, the [Fetch](https://fetch.spec.whatwg.org/) specification could be implemented in your new `awesome-fetch` pip package. This implementation might need types & algorithms from [Streams](https://streams.spec.whatwg.org/) which could be implemented in the `another-streams` pip package. The point is that segmenting packages based on specifications seems like a good idea. It aids in SEO to help searchers corrolate API names like "Fetch API" to a particular pip package.
>
> Package authors are also encouraged to make use of optional features. For example, a [Fetch](https://fetch.spec.whatwg.org/) implementation needs to provide a `.formData()` parser. This is a heavy dependency. Instead, you can use [optional dependencies](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#dependencies-optional-dependencies) to enable `.formData()` integration only if the user chooses to do so. This helps cut down the "Honey, I imported the entire web browser" phenomenon to a more manageable level.
>
> > If you wish to make an apple pie from scratch, you must first invent the universe.
>
> &mdash; Cosmos by Carl Sagan

### Python environment

In a Python implementation of a given plethora of Web IDL fragments there will be a bunch of Python classes, objects, constants, etc. that correspond to definitions in those Web IDL fragments. These objects are termed **initial objects** and comprise the following:

- interface objects
- legacy callback interface objects that have properties
- legacy factory functions
- named properties objects
- attribute getters
- attribute setters
- the function objects that correspond to operations
- special Python magic methods

Python does not operate in terms of "Realms" like JavaScript (the primary Web IDL binding target) does. This means we can skip all that and assume that "realm" just means "the current global Python environment" and that there is only one such "realm".

> [!NOTE]
> All interfaces define which realms they are exposed in. This allows, for example, realms for Web Workers to expose different sets of supported interfaces from those exposed in realms for Web pages. This is not reflected in Python. All `[Exposed=...]` interfaces are _always_ exposed.

### Names

Python's naming convention is different than JavaScript's. Python also has different reserved words than JavaScript. Some identifiers need to be escaped to avoid conflicts.

A name is **Python escaped** as follows:

1. If the name is a Python reserved word, then the Python escaped name is the name suffixed with a `U+005F LOW LINE` ("_") character.
2. Otherwise the name is not a Python reserved word. The Python escaped name is simply the name.

> [!NOTE]
> At the time of writing, the list of Python keywords is the following:
> ```
> False      await      else       import     pass
> None       break      except     in         raise
> True       class      finally    is         return
> and        continue   for        lambda     try
> as         def        from       nonlocal   while
> assert     del        global     not        with
> async      elif       if         or         yield
> ```
> There are additional "soft keywords" that should also be included in the reserved words list: match, case, type and _.

> [!TIP]
> Here's some concrete examples of how this naming conversion works:
>
> | Web IDL name | Python name |
> | --- | --- |
> | `innerHTML`  | `inner_html` |
> | `XMLHttpRequest` | `XMLHTTPRequest` |
> | `htmlFor` | `html_for` |
> | `HTMLHtmlElement` | `HTMLHTMLElement` |
> | `CDATA_SECTION_NODE` | `CDATA_SECTION_NODE` |
> | `SVGSVGElement` | `SVGSVGElement` |

### Python type mapping

This section describes how types in Web IDL map to types in Python.

Each sub-section describes how values of a given Web IDL type are represented in Python.

> [!NOTE]
> Note that conversions are unidirectional (Python value ➡ Web IDL Python value). All "Web IDL Python value" types are already valid normal Python types (just with extra validation).

#### any

Since the Web IDL `any` type is the union of all other Web IDL types, it can correspond to any Python value type.

A Python value *V* is converted to an `any` Web IDL Python value by running the following algorithm:

1. If *V* is `None` the return `None` as the Web IDL `undefined` value.
3. If *V* is a `bool` then return it as a Web IDL `boolean` value.
4. If *V* is an `int` then return it as a Web IDL `bigint` value.
5. If *V* is a `float` then return it as a Web IDL `unrestricted double` value.
6. If *V* is a `str` then return it as a Web IDL `DOMString` value.
7. If *V* is an object then return it as a Web IDL `object` value.

> [!NOTE]
> Yes, Python `int` types are really just `bigint` with a fast path for values within the 32-bit window. Since Python doesn't distinguish between `BigInt` and `Number` like JavaScript does we ignore any `ToNumber(bi)` errors and seemlessly coerce between the two types. Remember, these fancy Web IDL type names are still represented by normal Python types -- there's just a bit of upfront validation.

#### undefined

A Python value *V* is converted to an `undefined` Web IDL Python value by returning `None` as a valid Web IDL `undefined` sentinel value.

#### boolean

A Python value *V* is converted to a `boolean` Web IDL Python value by returning `bool(V)`.

#### Integer types

##### byte

A Python value *V* is converted to a `byte` Web IDL Python value by returning `(int(V) - 2**(8-1)) % 2**8 + 2**(8-1)`

##### octet

A Python value *V* is converted to an `octet` Web IDL Python value by returning `int(V) % 2**8`

##### short

A Python value *V* is converted to an `short` Web IDL Python value by returning `(int(V) - 2**(16-1)) % 2**16 + 2**(16-1)`

##### unsigned short

A Python value *V* is converted to an `unsigned short` Web IDL Python value by returning `int(V) % 2**16`

##### long

A Python value *V* is converted to an `long` Web IDL Python value by returning `(int(V) - 2**(32-1)) % 2**32 + 2**(32-1)`

##### unsigned long

A Python value *V* is converted to an `unsigned long` Web IDL Python value by returning `int(V) % 2**32`

##### long long

A Python value *V* is converted to an `long long` Web IDL Python value by returning `(int(V) - 2**(64-1)) % 2**64 + 2**(64-1)`

##### unsigned long long

A Python value *V* is converted to an `unsigned long long` Web IDL Python value by returning `int(V) % 2**64`

#### float

A Python value *V* is converted to an `float` Web IDL Python value by returning `...`

#### unrestricted float

#### double

#### unrestricted double

#### bigint

A Python value *V* is converted to a `bigint` Web IDL Python value by returning `int(V)`

#### DOMString

A Python value *V* is converted to a `DOMString` Web IDL Python value by returning `str(V)`

#### ByteString

A Python value *V* is converted to a `ByteString` Web IDL Python value by returning `bytes(V)`

#### USVString

A Python value *V* is converted to a `USVString` Web IDL Python value by returning `str(V).encode("utf16")`