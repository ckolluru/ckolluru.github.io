---
title: 'Interview prep (C++)'
date: 2024-09-29
permalink: /posts/2024/interview-cpp
tags:
  - interview
---

For the `&` symbol, if it appears on the left side of an equation (e.g. when declaring a variable), it means that the variable is declared as a reference. If the `&` appears on the right side of an equation, or before a previously defined variable, it is used to return a memory address.

`
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
`

`
home root$ g++ ./code/memory_example_2.cpp && ./a.out
The address of i is:          0x7ffc0fd88afc
The variable pointer_to_i is: 0x7ffc0fd88afc
`