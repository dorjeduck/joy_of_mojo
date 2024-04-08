+++
title = 'Special methods for structs'
date = 2024-04-08T12:40:53+05:45
author = 'dorjeduck' 
draft = false

categories = ["manual.struct"]
tags = ["mojo 24.2.0"]
+++


### Context

- **Mojo Manual**: [Structs - Special Methods](https://docs.modular.com/mojo/manual/structs#special-methods)
- **Mojo Version**: 24.2.0

---

### Demo: Defining a Dual Number struct

This demo showcases the creation of a struct to represent [Dual Numbers](https://en.wikipedia.org/wiki/Dual_number), which are numbers of the form `a+bϵ`, where `a` and `b` are real numbers, and `ϵ` is a special element with the property `ϵ * ϵ = 0` yet ϵ in not 0.

Addition and multiplication for dual numbers is defined as:

* `(a+bϵ) + (c+dϵ) = a+b + (c+d)ϵ`
* `(a+bϵ) * (c+dϵ) = a*b + (a*c+b*d)ϵ`


To implement this arithmetic for our struct, we can use the special methods `__add__()` and `__mul__()`. Further arithmetic operations could be implemented in a similar way. (See the [Int](https://docs.modular.com/mojo/stdlib/builtin/int) description in the Mojo documentation to learn about the names of the respective special functions in Mojo). Here, we also implement the special method `__str__()` which returns a string representation of a dual number. By implementing this method, the struct conforms to the [Stringable](https://docs.modular.com/mojo/stdlib/builtin/str.html#stringable)  trait and can be used in print statements."

---
  
```python
struct DualNumber(Stringable):
    var real:Float64
    var dual:Float64 

    fn __init__(inout self,real:Float64=0,dual:   Float64=0):
        self.real = real
        self.dual = dual 

    fn __add__(self,other:Self) -> Self :
        return Self(self.real+other.real,self.dual+other.dual)

    fn __mul__(self,other:Self) -> Self :
        return Self(self.real*other.real,self.real*other.dual+self.dual*other.real)

    fn __str__(self) -> String:
        return "(" + str(self.real) + ' + ' + str(self.dual) + "ϵ)"

fn main():

    var a = DualNumber(2,3)
    var b = DualNumber(1,4)

    print(a,'+',b,'=',a+b)
    print(a,'*',b,'=',a*b)
```

---

Output:

> (2.0 + 3.0ϵ) + (1.0 + 4.0ϵ) = (3.0 + 7.0ϵ)
>
> (2.0 + 3.0ϵ) * (1.0 + 4.0ϵ) = (2.0 + 11.0ϵ)

---

### Remarks

Mojo supports a long list of special methods (also called dunder methods) which generally match all of Python's special methods: [https://docs.python.org/3/reference/datamodel.html#special-method-names](https://docs.python.org/3/reference/datamodel.html#special-method-names)
