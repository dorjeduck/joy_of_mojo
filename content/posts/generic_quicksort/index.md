+++
title = 'Generic Quicksort'
date = 2024-04-23T16:36:02+05:30
author = 'dorjeduck' 
draft = false

categories = ["stdlib.algorithm"]
tags = ["mojo 24.2.1","quicksort"]
+++

### Context

- **Mojo Reference**: [Sort](https://docs.modular.com/mojo/stdlib/algorithm/sort)
- **Mojo Version**: 24.2.1

---

### Demo: Sorting a Group of People by Age

This demo showcases how to sort a group of people based on their age using a versatile QuickSort algorithm. This generic QuickSort implementation can be used to sort arrays of any type, provided a custom comparison function is specified. The flexibility of this approach allows for tailored sorting criteria, making it applicable across various data structures and scenarios.

---

```python
fn _swap[T:AnyType](inout a:Pointer[T], inout b:Pointer[T]) -> None:
    var temp = a
    a = b
    b = temp

fn _partition[T:AnyType, cmp_fn:fn(Pointer[T],Pointer[T])->Int](inout arr:Pointer[Pointer[T]],  low:Int,  high:Int) -> Int:
    var pivot = arr[high]  #pivot
    var i = low-1  
    for j in range(low,high):
        if cmp_fn(arr[j], pivot) <= 0 : 
            i+=1
            _swap(arr[i], arr[j])
                           
    _swap(arr[i + 1], arr[high])
    return i+1

fn quicksort[T:AnyType, cmp_fn:fn(Pointer[T],Pointer[T])->Int](inout arr:Pointer[Pointer[T]], low:Int, high:Int):
    
    if low < high:
        var pi = _partition[T,cmp_fn](arr, low, high,)
        quicksort[T,cmp_fn](arr, low, pi - 1)
        quicksort[T,cmp_fn](arr, pi + 1, high)


alias PersonPtr = Pointer[Person]
alias GroupPtr = Pointer[PersonPtr]

struct Person:
    var name:String
    var age:Int

    fn __str__(self) -> String:
        return self.name + " (" + self.age + ")"
    
    @staticmethod
    fn compare(p1:PersonPtr, p2:PersonPtr) -> Int:
        if p1[].age > p2[].age:
            return 1
        elif p1[].age < p2[].age:
            return -1; 
        else:
            return 0

    @staticmethod
    fn print_group(g:GroupPtr,num:Int) -> None:
    
        if num > 0:
            for i in range(num-1):
                print(g[i][],end=", ")
            print( g[num-1][])
        else:
            print("Empty Group")

alias GROUP_SIZE = 7

fn main():
    var group = GroupPtr.alloc(GROUP_SIZE)
    
    var names = List[String]("Rose","Tom","Julia","Sam") 
    
    for i in range(GROUP_SIZE):
        group[i] = PersonPtr.alloc(1)
        group[i][].age = 20 + i%3
        group[i][].name = names[i%len(names)]
 
    print("before sort: ",end="")
    Person.print_group(group,GROUP_SIZE)

    quicksort[Person,Person.compare](group,0, GROUP_SIZE-1)
    
    print(" after sort: ",end="")
    Person.print_group(group,GROUP_SIZE)

```

---

Output:

> before sort: Rose (20), Tom (21), Julia (22), Sam (20), Rose (21), Tom (22), Julia (20)
>
>  after sort: Rose (20), Sam (20), Julia (20), Tom (21), Rose (21), Tom (22), Julia (22)

---

### Remarks

The Mojo standard library includes a [partition](https://docs.modular.com/mojo/stdlib/algorithm/sort#partition) method that appears to function similarly to the custom `_partition` method defined in this demonstration. However, I have not yet figured out how to utilize it within the context of this demo.
