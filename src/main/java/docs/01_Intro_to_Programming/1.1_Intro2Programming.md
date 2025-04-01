# Introduction To Programming
- Computers can only understand 0s and 1s.
- 0s and 1s are like off and on switches in the transistors whose collective functions can achieve several operations.
- Programming languages are human readable(writable) which converts the programs/source code into system level languages i.e. series of 0s and 1s to perform specific tasks.
 
 There are several paradigms of programming under which several languages fall and the most important are listed below.

## Imperative Programming

- Control flow in imperative programming is explicit.
- Commands show _how_ the computation takes place, step by step. 
- Each step affects the global state of the computation.


### Procedural Programming
- Procedural programming is a paradigm where the program is structured around procedures or functions that manipulate data.
- It focuses on step-by-step instructions and emphasizes code reusability through the use of functions. 
- C and Pascal are examples of languages that follow this paradigm.

```c
    result = []
    i = 0
start:
    numPeople = length(people)
    if i >= numPeople goto finished
    p = people[i]
    nameLength = length(p.name)
    if nameLength <= 5 goto nextOne
    upperName = toUpper(p.name)
    addToList(result, upperName)
nextOne:
    i = i + 1
    goto start
finished:
    return sort(result)
```


### Object Oriented Programming 

- Object-Oriented Programming revolves around the concept of objects, which are instances of classes. 
- It organizes code into objects that encapsulate data and behavior. 
- OOP promotes modularity, reusability, and allows for concepts such as inheritance, polymorphism, and encapsulation.
- Java, C++, and Python are popular languages that support OOP.

```java
result = []
for p in people {
    if p.name.length > 5 {
        result.add(p.name.toUpper);
    }
}
return result.sort;
```

## Declarative Programming

Control flow in declarative programming is implicit: the programmer states only _what_ the result should look like, not how to obtain it.

### Functional Programming
- Functional Programming treats computation as the evaluation of mathematical functions.
- In functional programming, control flow is expressed by combining function calls, rather than by assigning values to variables.
- It avoids changing state and mutable data.
- It emphasizes immutability, pure functions, and higher-order functions. 

-  Haskell, Lisp, and Erlang are examples of languages that follow this paradigm.

```javascript
let
    fun uppercasedLongNames [] = []
      | uppercasedLongNames (p :: ps) =
          if length(name p) > 5 then (to_upper(name p))::(uppercasedLongNames ps)
          else (uppercasedLongNames ps)
in
    sort(uppercasedLongNames(people))
```

 - Functional Programming supports First-class functions (Functions are treated like other variables i.e. functions can be assigned to any other variable or passed as an argument or can be returned by another function).

 ```python
 def square(x): return x * x
def cube(x): return x * x * x

def print_result(x, func):
    """recieve a function and execute it and return result"""
    return func(x)

do_square = square     # assigning square function to a variable
do_cube = cube         # assigning cube function to a variable

res = print_result(5, do_square)   # passing function to another function
print res
res = print_result(5, do_cube)
print res
 ```

# Types of languages in terms of Data types
 **Static / Staticly Typed Language :** 
 - Performs type checking at compile time.
 - Declare datatype before use.
 - Error occurs at compile time.
 - Eg : Java

 **Dynamic / Dynamicly Typed Language :**
- Performs type checking at run time.
 - Not need declare datatype before use.
 - Error occurs at run time.
 - Eg : Javascript