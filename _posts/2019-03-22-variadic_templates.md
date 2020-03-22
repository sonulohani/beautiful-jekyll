---
layout: post
title: std::variant
---

Variadic templates are templates that take variable number of arguments. Variadic templates are supported since C++ 11. Prior to C++11, templates could only take a fixed number of arguments. C++11 allows template definitions to take an arbitrary number of arguments of any type. 

### Syntax

```cpp
template<typename... Args> class abc;
```

The above template class ```abc``` will take any number of typenames as its template parameters. We use ```typename... Args``` to declare Args as **template parameter pack**.

### Parameter Pack

A template parameter pack is a template parameter that accepts zero or more template arguments (non-types, types, or templates). A function parameter pack is a function parameter that accepts zero or more function arguments.

A template with at least one parameter pack is called a *variadic template*. 

### Ellipsis (...) operator

The ellipsis (...) operator has two roles. 
1. When it occurs to the left of the name of a parameter, it declares a parameter pack. Using the parameter pack, the user can bind zero or more arguments to the variadic template parameters.
2. When the ellipsis operator occurs to the right of a template or function call argument, it unpacks the parameter packs into separate arguments

### Example

We can understand it better by seeing the simple example. The example below is an add function which takes any number of arguments of any type and returns the sum of the arguments.

```cpp
template<typename T>
auto sum(T arg)
{
	return arg;
}

template<typename T, typename... Args>
auto sum(T first_arg, Args... args)
{
	return first_arg + sum(args...);
}
```

Now when you call the sum function with variable number of arguments, the function will return the sum of all the arguments as output.

```cpp
std::cout << sum(1, 2, 3, 4.5);
```

will print

10.5

### How does the compiler generates code for sum function 

1. The compiler generates the code for ```sum(1, 2, 3, 4.5)```. Here, we have two overloaded funtions for sum. One is ```auto sum(T first_arg, Args... args)``` and the another one is ```auto sum(T arg)```.
2. For the first argument value '1' in the calling function, the compiler will deduces the type as int and will be passed to first_arg and the other arguments will be sent as a parameter pack to the args variable. 
3. The args... expands the parameter pack and now the 2 will be passed to the first_arg and rest all will be passed to the args variable, so on and so forth for the other values. 
4. For the value 4.5 since we dont have parameters left, so the ```sum(4.5)``` will call the overloaded sum function. 

In simple words, 
- sum(1, 2, 3, 4.5)
- 1 + sum(2, 3, 4.5)
- 1 + (2 + sum(3, 4.5))
- 1 + 2 + (3 + sum(4.5))
- 1 + 2 + (3 + (4.5))


