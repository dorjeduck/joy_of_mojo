+++
title = 'Conditional SIMD Operations'
date = 2024-04-07T08:18:01+05:45
author = 'dorjeduck'
draft = false

categories = ["stdlib.builtin"]
tags = ["mojo 24.2.0","simd"]
+++

---

### Context

- **Mojo Reference**: [SIMD select method](https://docs.modular.com/mojo/stdlib/builtin/simd#select)
- **Mojo Version**: 24.2.0

---

### Demo: Vectorized Relu Function

This demo illustrates a practical application of conditional SIMD operations to implement the Relu activation function in a vectorized manner. The Relu function is defined as returning 0 for all input values less than 0 and the input value itself for those greater than 0.

---
  
```python
from algorithm import vectorize

alias LEN = 7
alias dtype = DType.float32
alias simd_width = simdwidthof[dtype]()

fn relu(input: DTypePointer[dtype], inout output: DTypePointer[dtype]):
  @parameter
  fn _cond_op[width: Int](i: Int):
    var mask = input.load[width=width](i) > 0
    output.store[width=width](i, mask.select( input.load[width=width](i),0))
  vectorize[_cond_op, simd_width, size=LEN]()
  
fn main():

  var input = DTypePointer[dtype].alloc(LEN)
  var output = DTypePointer[dtype].alloc(LEN)

  for i in range(LEN):
    input[i] = -LEN // 2 + i + 1
    
  relu(input,output)
 
  for i in range(LEN):
    print("relu(",input[i],") = ", output[i],sep="")
```

---

Output:

> relu(-3.0) = 0.0
>
> relu(-2.0) = 0.0
>
> relu(-1.0) = 0.0
>
> relu(0.0) = 0.0
>
> relu(1.0) = 1.0
>
> relu(2.0) = 2.0
>
> relu(3.0) = 3.0

---

### Remarks

Special thanks to @sora from the Modular Discord community for elucidating this technique.
