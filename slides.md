---
title: 'Class 5: Chapter 11: Dictionaries'
separator: '\-\-\-\-\-'
verticalSeparator: '\+\+\+\+\+'
theme: 'moon'
revealOptions:
    transition: 'fade'
---

### ITSE-1402 Intermediate Python

-----

##### Chapter 11: Dictionaries

+++++

##### Dictionaries

Dictionaries are the equivalent of today's object storage such as Amazon Web Services S3, but in python. You have a key, that key returns an object. 

+++++

If you think abstractly, you kind of view a dictionary a list that you can use (almost) anything as an index. List values can only be accessed by an integer index that you have no control over. Dictionaries are very fast and can be used as a great tool in your code. In many cases, they are much more efficient than lists. 

+++++

The index of a dictionary is called a key. The object behind the key is called a value. If you prefer mathematical terminalogy, you can call this relationship mapping. The key maps to a value.

+++++

Empty dictionaries can be created in two ways:
    - using the built-in function dict
    - using braces

```python
myDict = dict()   # Empty dictionary using built-in
myDict2 = {}      # Empty dictionary using curly brackets
```

+++++

Example of creating a blank english to spanish dictionary and adding one value

```python
eng2sp = {}
eng2sp['one'] = 'uno'
# eng2sp = {'one': 'uno'}    
```

+++++

Example of defining all the values at once. 

```python
eng2sp = {'one': 'uno', 'two': 'dos', 'three': 'tres'}
```

Note:
This won't display in the same order. 

+++++

Two Parts to a Dictionary:

- Keys
    - Anything immutable (numbers, tuples, strings)
    - Keys are hashed for fast lookups
- Values
    - Values can be anthing you would like. Mutable or immutable. (lists, other dictionaries, strings, etc)
        
Note:
We'll discuss how to implement a custom dictionary in Appendix B
    
+++++

<pre class="stretch"><code class="python" data-trim data-noescape>
#!/usr/bin/env python3

# Exercise 11.1
#
# 1. Write a function name "make_dict" that reads the words in words.txt,
# stores them as keys in a dictionary and returns the dictionary. It 
# doesn't matter what the values. For simplicity, you can set them to 0. 
#
# This would allow you to use the in operator as a fast way to check whether a 
# word is in the dictionary. If you are feeling ambitious you can compare this
# to the in operator with a list and the bisect search from Chapter 10.
#
</code></pre>

+++++


With our previous example of the english to spanish dictionary, this is how you pull a value:

```python
eng2sp['one']
# 'dos'
eng2sp['three']
# 'tres'
eng2sp['four']
# KeyError: 'four'
```

+++++

A couple more things we could do with our dictionary:

```python
len(eng2sp)
# 3
'one' in eng2sp
# True
'four' in eng2sp
# False
```

Note:
in checks keys only

+++++

If you need to check for a value, you can do something like this:

```python
vals = eng2sp.values()
'uno' in vals
# True
```

You shouldn't be doing this though.

Note:
Dictionaries should be referenced by keys always

+++++

Counting characters with a dictionary

```python
def histogram(s):
    d = dict()
    for c in s:
        if c not in d:
            d[c] = 1
        else:
            d[c] += 1
    return d

h = histogram('brontosaurus')
# {'n': 1, 'a': 1, 'r': 2, 'u': 2, 't': 1, 'b': 1, 's': 2, 'o': 2}
h.get('a',0)
# 1
h.get('z',0)
# 0
```

NOTE:
The method get() returns a value for the given key, however if the key is not available then returns default value "None" or n this case,  the second argument of the method.

+++++

Looping and dictionaries

<pre class="stretch"><code class="python" data-trim data-noescape>
print_hist(h):
    for c in h:
        print(c, h[c])

h = histogram("parrot")
print_hist(h)
# a 1
# r 2
# p 1
# t 1
# o 1

for key in sorted(h):
    print(key, h[key])
# a 1
# o 1
# p 1
# r 2
# t 1
</code></pre>

Note: 
When using a for statement, python will travese the keys of a dictionary

In order for it to be in alphabetical order, you will have to process the keys with sorted()

+++++

Dictionaries are meant for one-way lookup with the key. Doing the reverse is not necessarily simple or efficient. Here is one way to do it:

```python
def reverse_lookup(d, v):
    for k in d:
        if d[k] == v:
            return k
    raise LookupError()
```

Note:
This uses something we haven't learned yet, a raise. This causes an exception and in this case we are using a built-in one named "LookupError".

+++++

Lists in dictionaries

<pre class="stretch"><code class="python" data-trim data-noescape>
normDict = {'n': 1, 'a': 1, 'r': 2, 'u': 2, 't': 1, 'b': 1, 's': 2, 'o': 2}

def inverse_dict(d):
    inverse = dict()
    for key in d:
        val = d[key]
        if val not in inverse:
            inverse[val] = [key]
        else
            inverse[val].append(key)
    return inverse

inverse_dict(normDict)
# { 1: ['n','a','t','b'], 2: ['r','y','s','o'] } 
</code></pre>

+++++

<pre class="stretch"><code class="python" data-trim data-noescape>
#!/usr/bin/env python3

# Exercise 11.2
#
# 1. Read the documentation of the dictionary method setdefault and use it to 
# write a more concise version of invert_dict.
#
# def invert_dict(d):
#     inverse = dict()
#     for key in d:
#         val = d[key]
#         if val not in inverse:
#             inverse[val] = [key]
#         else:
#             inverse[val].append(key)
#     return inverse
#
</code></pre>

+++++

#### Memos

The concept of a memo is essentially to store reusable values in a dictionary to be looked up. An example of where this could be useful is the fibonacci function:

```python
known = {0:0, 1:1}

def fibonacci(n):
    if n in known:
        return known[n]
    
    res = fibonacci(n-1) + fibonacci(n-2)
    known[n] = res
    return res
```

+++++

<pre class="stretch"><code class="python" data-trim data-noescape>
#!/usr/bin/env python3

# Exercise 11.3
#
# 1. Memoize the Ackermann function from Exercise 6.2 and see if memoization 
# makes it possible to evaluate the function with bigger arguments. Hint: no.
# 
# def ack(m, n):
#     '''Ackermann's function;
#     m, n - integers greater-than-equal 0
#     '''
#     if m == 0:
#         return n + 1
#     elif m > 0 and n == 0:
#         return ack(m - 1, 1)
#     elif m > 0 and n > 0:
#         return ack(m - 1, ack(m, n - 1))
#
# print(ack(3, 4))
</code></pre>

+++++

#### Global Variables

In python there is only a loose definition of a global variable and it can only be defined in __main__(). This is common with programming flags:

```python
verbose = True

def example1():
    if verbose:
        print('running example 1')
```

+++++

Global variables cannot be reassigned though without an extra step.

```python
been_called = False

def example2():
    been_called = True
```

Note:
This creates a NEW local variable and does not effect the global

+++++

The proper way to do this would be with global.

```python
been_called = False

def example2():
    global been_called
    been_called = True
```

Note:
This will modify the global variable. This concept applies to any immutable value.

+++++

This gets a little tricky because only mutable objects don't have to be called with global unless reassigned:

```python
known = {0:0, 1:1}

def example3():
    known[2] = 1    # OK
    
def example4():
    global known
    known = dict()  # This is reassigning. This would need the global.
```

+++++

<pre class="stretch"><code class="python" data-trim data-noescape>
#!/usr/bin/env python3

# Exercise 11.4
#
# 1. If you did Exercise 10.7, you already have a function named "has_duplicates"
# that takes a list as a parameter and returns True if there is any object that
# appears more than once in the list.
# 
# Use a dictionary to write a faster, simpler version of has_duplicates.
#
def has_duplicates(li):
    dictionary = dict()   
    for word in li:
        if word in dictionary:
            return True
        dictionary[word] = True
    return False
</code></pre>

+++++

Quick and Dirty Rundown:

- Unknown order of items
- len(myDict) returns number of key:value pairs
- Values can be accessed with myDict['key-value']
- If no value is found, you will get a KeyError
- in operator looks at keys only (you can turn a dict into an interable like this)
- You can pull a dictionaries key/values or both with .keys(), .values(), and .items()

Note:

KeyError work arounds:
if key in myDict
myDict.get(key,[opt return on None])

+++++

Exercise 11.5 and 11.6 are homework.
