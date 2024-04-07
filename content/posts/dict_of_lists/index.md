+++
title = 'Dictionary of Lists'
date = 2024-04-07T18:37:26+05:45
author = 'dorjeduck' 
draft = false

categories = ["stdlib"]
tags = ["mojo 24.2.0","collections","dict"]
+++
 
---
  
### Context

- **Mojo Reference**: [Dict](https://docs.modular.com/mojo/stdlib/collections/dict)
- **Youtube**: [Overview of Dict, Variant and Optional in Mojo🔥](https://www.youtube.com/watch?v=ywbzfY5v2ZM)
- **Mojo Version**: 24.2.0

---

### Demo: Categorizing numbers by their sum of digits

This program shows how to use a dictionary to organize numbers based on the sum of their individual digits. Within a specified numerical range, the program calculates the digit sum for each number and employs this sum as a key to group the numbers within a dictionary.

---
  
```python
from collections import Dict
from math import floor,sqrt

fn digit_sum(number:Int) -> Int:
    var n = number
    var sum_of_digits = 0
    while n > 0:
        sum_of_digits += n % 10
        n //= 10
    return sum_of_digits

fn categorize_numbers(start:Int,end:Int) raises -> Dict[String,List[Int]] : 
    
    var digit_sum_dict = Dict[String,List[Int]]()
    
    for number in range(start, end + 1): 
      
        var sum_of_digits = str(digit_sum(number))
        
        if sum_of_digits not in digit_sum_dict:
            digit_sum_dict[sum_of_digits] = List[Int]()
        
        digit_sum_dict[sum_of_digits].append(number)

    return digit_sum_dict ^

fn main() raises:
    var start = 100
    var end = 130

    var digit_sum_dict = categorize_numbers(start,end)
    
    print("Numbers between",start,"and",end,"by their sum of digits:")
    
    for num_list in digit_sum_dict.items():
        print('Digit sum ' + num_list[].key + ': ',end='')
        
        for number in num_list[].value:
            print(number[],end=' ')
        
        print("")
```

---

Output:

> Numbers between 100 and 130 by their sum of digits:
>
> Digit sum 1: 100
>
> Digit sum 2: 101 110
>
> Digit sum 3: 102 111 120
>
> Digit sum 4: 103 112 121 130
>
> Digit sum 5: 104 113 122
>
> Digit sum 6: 105 114 123
>
> Digit sum 7: 106 115 124
>
> Digit sum 8: 107 116 125
>
> Digit sum 9: 108 117 126
>
> Digit sum 10: 109 118 127
>
> Digit sum 11: 119 128
>
> Digit sum 12: 129

---

