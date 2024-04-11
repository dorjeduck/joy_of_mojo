+++
title = 'Variant keyword arguments'
date = 2024-04-11T06:52:46+05:45
author = 'dorjeduck' 
draft = false


categories = ["stdlib.utils"]
tags = ["mojo 24.2.0","variant","kwargs"]
+++


### Context

- **Mojo Reference**: [Variant](https://docs.modular.com/mojo/stdlib/utils/variant)
- **Mojo Version**: 24.2.0

---

### Demo: Dynamic CSS Property Handler

This demo showcases a flexible CSS property handler, utilizing a `Variant` type to accept both string literals and integers as CSS property values. The `CSS` function dynamically processes various CSS properties provided as keyword arguments. It distinguishes between integers and string literals, allowing for a versatile way to handle CSS properties like color, font_size, background-color, and padding, demonstrating the language's capability for type-safe yet flexible argument handling.

---
  
```python
from utils.variant import Variant

alias CSS_T=Variant[StringLiteral,Int]

fn CSS(**kwargs: CSS_T) raises:
    for i in kwargs:
        var kw=i[]
        if kwargs[kw].isa[Int](): print(kw, kwargs[kw].take[Int]())
        if kwargs[kw].isa[StringLiteral](): print(kw, kwargs[kw].take[StringLiteral]())
def main():
    CSS(color="yellow",font_size=16,`background-color`="blue",padding=4)
```

---

Output:

> color yellow
>
> font_size 16
>
> background-color blue
>
> padding 4

---

### Remarks

Special thanks to @rd4com from the Modular Discord community for sharing this very useful demo on Discord.
