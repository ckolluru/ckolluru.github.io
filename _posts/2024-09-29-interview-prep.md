---
title: 'Interview prep (C++)'
date: 2024-09-29
permalink: /posts/2024/interview-cpp
tags:
  - interview
---

Some pointers (pun intended) for C++ interviews.

These are my course notes for the C++ course on Udacity.

The best reference guidelines are published here: [C++ guidelines by Bjarne Stroustrup](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

A tutorial on using CMake is [here](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)

### Basics and OOP
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

For denoting unsigned integers, use `std::size_t`

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

`std::vector` is an example class that uses generic programming, allowing one to create a vector of int, floats, chars etc.

### Memory management

Here are some cheatsheets for GDB. Seeing the underlying memory addresses and 0s and 1s helps remove the complexity underlying memory management.

![GDB cheatsheet 1](/images/gdb_cheatsheet_1.png)
![GDB cheatsheet 2](/images/gdb_cheatsheet_2.png)

The bit size of the CPU decides how many bytes of data it can access in RAM memory at the same time. A 16-bit CPU can access 2 bytes (with each byte consisting of 8 bit) while a 64-bit CPU can access 8 bytes at a time.


Size of stack is limited to a few MBs. Memory management on the stack is straightforward. Heap is used with the `new` keyword, you need a corresponding `delete` statement, else there will be trouble. Heap size can grow much larger. Smart pointers provides some benefits in this regard.

If you call by value, you create a copy of the variable in both the caller and called functions, so effectively you're using double the amount of data. Call by reference just keeps one copy. Alternatively, you can pass a pointer to a variable by value, you can then use the pointer to directly manipulate the data. References bind to the same object after initialization. Advantage of using pointers instead of references is that you can initialize to null values, which cannot be done with references.


Locks, files will all be cleaned up at the end `}`.

With virtual memory, a mapping is performed between the virtual address space a program sees and the physical addresses of various storage devices such as the RAM but also the hard disk.

A memory page allow the OS to divide the program's memory space into manaageable pieces. You can then do swapping and segmentation (separate code, data and stack). Page sizes are usually around 4 or 8 KB. Memory frames are physical counterparts of virtual memory pages. This mapping happens if the page is in RAM. The mapping is done using a `page table`. If one tries to access memory that memory that is not in RAM, a page fault will occur.

Now that each process gets its own series of virtual address locations, different sections can be segregated into say program code, global variables etc. This is referred to as a process memory model.

![Process memory model](/images/process_management_model.png)

Heap and stacks: Stack is a contiguous block of memory with a fixed maximum size. If a function ends up calling itself recursively forever, there will be a `stack overflow` error. It stores local variables and function parameters. Each thread will have its own stack memory. New memory on stack is allocated if execution enters a new scope and is freed once the scope is left. There is automatic management. Stack is much faster than heap since it is managed by the OS. Heap is shared by multiple threads, so concurrency needs to be handled. It is the programmer's responsibility to free the heap. If you forget, you create a `memory leak`. Memory leaks can be detected using special tool like Valgrind. Once run, the log file will have a `LEAK_SUMMARY` section that shows how much memory is lost.

```
valgrind --leak-check=full --show-leak-kinds=all --track-origins=yes --verbose --log-file=./bin/valgrind-out.txt ./bin/a.out
```

BSS and data stores: either global/static variables initialized to zero or to a certain value, respectively. They persist throughout the life of the program. 

If you want to store images/videos, you have to use the heap.

```
#include <iostream>

void AddThree(int *val)
{
    *val += 3;
}

int main()
{
    int val = 0;
    AddThree(&val);
    val += 2;

    std::cout << "val = " << val << std::endl;
 
    return 0;
}
```

In the above case, a copy to the memory address of val is created. `val` becomes 5. 

In the heap, you can use malloc, realloc and free keywords (new and delete are operators, not functions and are the object oriented counterparts). If you access a memory location after releasing it, you could cause a `segmentation fault`. If there is a pointer that holds the address to a memory location that has been freed, you have a `dangling pointer`. If you allocate with malloc, the data is on the heap, so is persistent when you return from the function, you can just send a pointer back to the calling function. With malloc, a class' constructor and deconstructor are not called. `new` and `delete` call the constructors and destructors.

You can overload `new` and `delete` with operator keyword and explicitly allocate resources as needed. If you use a malloc there, it will work and give you memory on the heap. You can have exception handling, add additional parameters, and add customized behavior. 

#### Copy and Move semantics, smart pointers
Overloading copy functions to implement your own memory management policy. With the default assignment operator (say with the copy constructor), you are creating a copy of the object. If your class has a pointer member variable, a copy to the pointer is created, but both pointers are eventually pointing to the same data in the heap.

```
// copy constructor - this does a shallow copy, pointer copied, not resource
Myclass m2 (m1); 
```
When you go out of scope, the destructor will be called, and there will be a double free operation run if there a pointer member variable accessing memory on the heap.

```
class NoCopyClass2
{
public:
    NoCopyClass2(){}
    NoCopyClass2(const NoCopyClass2 &) = delete;
    NoCopyClass2 &operator=(const NoCopyClass2 &) = delete;
};
```
The above code will ensure that copying class objects is forbidden. Few options exist as listed below. With exclusive ownership, we transfer the handle from source to destination, and then invalidate the source by setting it to a nullptr. In the destructor, we only free the block of memory if it is not nullptr. A better way to do this is to use move semantics. The other option is shared ownership, where you leave the default copy constructor behavior as is, but keep track of number of instances pointing to the resource. Only after the last object is about to be deleted, the memory resource can be deleted. This is the idea of `unique_ptr`.

```
Default copying
No copying
Exclusive ownership
Deep copying
Shared ownership
```

Rule of Three: If you need either an overloaded copy constructor, copy assignment operator, or a destructor to delete resources, then you must also implement the other two as well to ensure that memory is managed consistently.

#### lvalues and rvalues
An lvalue (locator value) represents an object that occupies some identifiable location in memory (i.e. has an address). Examples of lvalue expressions include variable names, including const variables, array elements, function calls that return an lvalue reference, bit-fields, unions, and class members.

An rvalue is an expression that DOES NOT represent an object occupying an identifiable memory location (i.e. has NO address). 

```
int nVal1 = 10; //nVal1 is an lvaue, 10 is an rvalue
callMe(nVal1+nVal2); // The expression (nVal1+nVal2) is an rvalue
```
The rvalue can be assigned to an lvalue but not vice versa. If no move constructor is implemented, then std::move just calls the copy constructor.

- If your class is resource intensive or if it is going to be stored in containers, introduce a move constructor (and move assignment operator).

* When you create an instance of your expensive resource class from an rvalue and if move constructor exists, that will be invoked.

* If move constructor does not exist, copy constructor will be invoked.

* If you want to invoke move constructor by passing an lvalue, you explicitly call it by using std::move. This is called move semantics. 

std::move unconditionally casts its argument to an rvalue. lvalues as named containers for rvalue. rvalue references are themselves lvalues. If a function argument is an rvalue reference, it indicates that the caller does not want anything to do with this object. An rvalue reference cannot bind to an lvalue.


```
Widget&& widget = std::move(someWidget); // Explicitly converts lvalue to rvalue.
```

The Rule of Five states that if you have to write one of the functions listed below then you should consider implementing all of them with a proper resource management policy in place. If you forget to implement one or more, the compiler will usually generate the missing ones (without a warning) but the default versions might not be suitable for the purpose you have in mind. The five functions are:

The destructor: Responsible for freeing the resource once the object it belongs to goes out of scope.

The assignment operator: The default assignment operation performs a member-wise shallow copy, which does not copy the content behind the resource handle. If a deep copy is needed, it has be implemented by the programmer.

The copy constructor: As with the assignment operator, the default copy constructor performs a shallow copy of the data members. If something else is needed, the programmer has to implement it accordingly.

The move constructor: Because copying objects can be an expensive operation which involves creating, copying and destroying temporary objects, rvalue references are used to bind to an rvalue. Using this mechanism, the move constructor transfers the ownership of a resource from a (temporary) rvalue object to a permanent lvalue object.

The move assignment operator: With this operator, ownership of a resource can be transferred from one object to another. The internal behavior is very similar to the move constructor.

#### RAII (resource allocation is initialization)
In C++, a common way of safely accessing resources is by wrapping a manager class around the handle, which is initialized when the resource is acquired (in the class constructor) and released when it is deleted (in the class destructor). This concept is often referred to as Resource Acquisition is Initialization (RAII)    