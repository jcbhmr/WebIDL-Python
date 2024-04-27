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

<table><td>

```ts
interface Response {
    constructor(optional BodyInit? body = null, optional ResponseInit init = {});

    readonly attribute USVString url;
    readonly attribute boolean redirected;

    // ...
}
```

<td>

```py
class Response:
    def __init__(self, body = None, init = None):
        if init is None:
            init = {}
        # ...
    
    @property
    def url(self):
        return self.__url
    
    @property
    def redirected(self):
        return self.__redirected

    # ...
```

</table>
