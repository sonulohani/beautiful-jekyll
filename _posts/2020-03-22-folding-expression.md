---
layout: post
title: Folding Expression
---

Reduces(folds) a parameter pack over a binary operator. This feature got introduced in C++17. With C++11, we have got the variadic templates feature. By use of fold expression one can apply a binary operation to the elements of the parameter pack.
- An expression of the form ```(... op e)``` or ```(e op ...)```, where op is a fold-operator which is any of the following 32 binary operators: ```+ - * / % ^ & | = < > << >> += -= *= /= %= ^= &= |= <<= >>= == != <= >= && || , .* ->*``` and e is an unexpanded parameter pack, are called unary folds.
- An expression of the form ```(e1 op ... op e2)```, where op are fold-operators, is called a binary fold. Either e1 or e2 is an unexpanded parameter pack, but not both.

### Example

If you can go the blog post [Variadic Templates](https://sonulohani.github.io/moderncpp/2020-03-22-variadic_templates/), there we have seen the example of add function which uses variadic template. Here we will see how to rewrite the same add function by using fold expression.

```cpp
template<typename... Args>
auto sum(Args... args)
{
	return (... + args);
}
```
```cpp
std::cout << sum(1, 2, 3, 4.5);
```

will print

10.5

This above example is a unary left fold. It's equivalent to (((1+2)+3)+4.5). It reduces/folds the parameter pack of integers into a single integer by applying the binary operator successively. 

```cpp
template<typename... Args>
auto sum(Args... args)
{
	return (0 + ... + args);
}
```

This above version of sum is a binary left fold. The init argument is 0 and it's redundant (in this case). That's because this fold expression is equivalent to (((((0+1)+2)+3)+4)+5).