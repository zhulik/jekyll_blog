---
layout: post
title:  "Python's inconsistency Part 1: built-in functions"
categories: posts
tags: python inconsistency
---

This is the first post in the series about Python and about how bad it is. I'm not a Python programmer, but sometimes I
have to use it. And every time I do it, I feel pain. Usually, this pain is caused by total inconsistency of Python's
APIs even in the standard library.

Today I'm going to tell you about built-in functions.

[Here](https://docs.python.org/3/library/functions.html#func-list) you can find an official list of these functions.

The title of the document reads **Built-in Functions**, but the text below the title states:
*The Python interpreter has a number of functions and types built into it that are always available. They are listed here in alphabetical order.*

Really? Are we talking about functions or types? Or maybe Python's authors don't know the difference? In fact, there is no
syntactic difference between instantiating a class and calling a function. These actions are mixed by language itself, and this
one of the worst architectural decisions ever.

Anyway, let's see what we have.

```python
In [1]: type(abs)
Out[1]: builtin_function_or_method
In [2]: type(len)
Out[2]: builtin_function_or_method
```
Ok, this is fine.

```python
In [1]: type(list)
Out[1]: type
```
Hmm, ok, it seems like `list` is not a function, but a class name.

And what about the functions for collections?
```python
In [1]: type(sorted)
Out[1]: builtin_function_or_method
```
Ok, good.

```python
In [1]: type(reversed)
Out[1]: type
```
Wait, WAT?! `reversed` is not a function. Even more: `map` and `filter` are not functions too.
How did it happen that the common elements of functional programming are not functions in Python world?
Also, I forgot about `reduce` function.

```python
In [1]: type(reduce)
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-15-7165211847b3> in <module>()
----> 1 type(reduce)

NameError: name 'reduce' is not defined
```
Ummm. It seems like `reduce` is some kind of special function, and Python's developers did not include it in the
built-in list. Why is there a `map` and `filter` in the list, and `reduce` is not? I have no answer. For some reason, it's defined in the `functools` module. Let's import it and check.

```python
In [1]: from functools import reduce
In [2]: type(reduce)
Out[3]: builtin_function_or_method
```
God, why did I get into this... It's a built-in function, but it's defined in an external module. And it's not a `type` like
`map` and `filter`.

You ask, what's wrong here if calling a function and instantiating a class have the same syntax?

Because it's implicit: you think you work with functions, but you work with some obscure `type`s. They look like functions,
but they are not functions. Do you remember so-called *The Zen of Python*? I will remind you:
```
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```
Any сontradictions?

Yes: **Explicit is better than implicit**.

# Conclusions
Python, a language that wants to be explicit and simple turns out to be confusing and broken from the very beginning.

Is python an explicit language? No, it's not.

You might think that this guy is spewing nonsense. Who gives a shit if these `type`s act like a function and work as expected?
Ok, it's just a beginning, next time I will show something more interesting and confusing.
