---
title: 'Interview prep (C++)'
date: 2024-09-29
permalink: /posts/2024/interview-cpp
tags:
  - interview
---

Some pointers (pun intended) for C++ interviews.

The best reference guidelines are published here: [C++ guidelines by Bjarne Stroustrup](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

For the `&` symbol, if it appears on the left side of an equation (e.g. when declaring a variable), it means that the variable is declared as a reference. If the `&` appears on the right side of an equation, or before a previously defined variable, it is used to return a memory address.

```
#include <iostream>
using std::cout;

int main() 
{
    int i = 5;
    // A pointer pointer_to_i is declared and initialized to the address of i.
    int* pointer_to_i = &i;
    
    // Print the memory addresses of i and j
    cout << "The address of i is:          " << &i << "\n";
    cout << "The variable pointer_to_i is: " << pointer_to_i << "\n";
}
```

```
home root$ g++ ./code/memory_example_2.cpp && ./a.out
The address of i is:          0x7ffc0fd88afc
The variable pointer_to_i is: 0x7ffc0fd88afc
```

A pointer can be declared by using the * operator in the declaration. 

```
    // A pointer pointer_to_i is declared and initialized to the address of i.
    int* pointer_to_i = &i;
```

Once you have a pointer, you may want to retrieve the object it is pointing to. In this case, the `*` symbol can be used again. This time, however, it will appear on the right hand side of an equation or in front of an already-defined variable, so the meaning is different. In this case, it is called the "dereferencing operator", and it returns the object being pointed to.

Use parantheses to dereference pointers, especially if it is a pointer to a vector. `(*pointer_to_vec)[2]`. If a function returns a pointer, make sure the memory that it points to does not go out of scope after the function returns. References should always point to variables in the program, they cannot be null. Pointers on the other hand can be initialized to be `nullptr`. Let's say an object has to hold a reference to another object that has not yet been created, you can have a pointer and initialize it to `nullptr`.

The arrow operator -> is used to simultaneously dereference a pointer to an object and access an attribute or method.

A map is a collection of key-value pairs, where each key is unique. C++ offers two types of maps.

- std::map: The default map implementation, which uses a balanced binary tree providing O(log n) complexity for insertions, deletions, and lookups.

- std::unordered_map: Implements a hash table, providing average O(1) complexity for insertions and lookups but does not maintain order. Keys must be hashable.

Maps allow custom comparison functions to determine the order of keys. This might be helpful if you want to have a data structure like a priority queue.

For denoting unsigned integers, use std::size_t

A good rule of thumb to decide between structs and classes; use classes when member variables are related by an "invariant". If there is some rule that you would apply on the data (say the date of the month should always be valid), it is best to keep that logic in a function and make the variable private rather that checking that logic in all places that it can be modified (if made public).

`static` variables in a class belong to the class. If they are constants, use `constexpr` when initializing them since they are evaluated at compile-time in that case. 

Composition is a different kind of inheritance - say you have a car class and a wheel class. Car contains wheels, it is not a type of wheel.

Use reference variables in a class if the class doesn't actually manage an object. For instance see this example.

```
class LineSegment {
public:
    // Assume some properties and methods
};

class Circle {
public:
    Circle(LineSegment& radius) : radius_(radius) { }

    double Area() {
        // Use radius_ to calculate area, if necessary
        return 3.14 * (/* some calculation using radius_ */);
    }

private:
    LineSegment& radius_; // Reference to an existing LineSegment
};

int main() {
    LineSegment myRadius;
    Circle myCircle(myRadius); // Pass existing LineSegment
    return 0;
}

```
Using a reference indicates that the Circle does not own the LineSegment. This is important for conveying the design intent. The Circle uses the LineSegment, but it does not manage its lifetime (i.e., it doesn't delete or create it). This can help prevent memory management issues.