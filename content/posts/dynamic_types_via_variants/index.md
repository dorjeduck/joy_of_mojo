+++
title = 'Dynamic types via Variants'
date = 2024-04-07T09:41:36+05:45
author = 'dorjeduck'
draft = false

categories = ["stdlib"]
tags = ["mojo 24.2.0","utils","variant"]
+++

---

### Context

- **Mojo Reference**: [Variant](https://docs.modular.com/mojo/stdlib/utils/variant)
- **Youtube**: [Overview of Dict, Variant and Optional in Mojo🔥](https://www.youtube.com/watch?v=ywbzfY5v2ZM)
- **Mojo Version**: 24.2.0

---

### Demo: Summing Integers and String Representations

This demo illustrates how to sum elements in an array that may be either integers or strings representing integers. It uses Mojo's [Variant](https://docs.modular.com/mojo/stdlib/utils/variant) to accommodate mixed data types within the same array. Notably, the data type of variable `v3`, whether `String` or `Int`, is determined only at runtime in this demo (dynamic type).

---
  
```python
from random import seed,random_ui64
from utils.variant import Variant
 
alias IntOrString = Variant[Int, String]

fn sum(*elements:IntOrString) -> Int:

    var res:Int = 0 

    for i in range(len(elements)):
        if elements[i].isa[String]():
            try:
                res += int(elements[i].get[String]()[])
            except e:
                print("Error:", e,elements[i].get[String]()[])
        else:
            res += elements[i].get[Int]()[]
    return res 
    

fn main():

    seed()
   
    var v1 = IntOrString(4)
    var v2 = IntOrString(String("12"))
    
    var v3:IntOrString

    if random_ui64(0, 1):
        print("v3 = String(21)")
        v3 = String("21")
    else:
        print("v3 = 21")
        v3 = 21

    print("result:",sum(v1,v2,v3))
```

---

Output:

> v3 = String(21)
>
> result: 37


