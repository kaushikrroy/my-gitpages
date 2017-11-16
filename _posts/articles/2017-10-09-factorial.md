---
layout: article
title: "Notes on Factorial Again!"
modified:
categories: articles
excerpt: "The famous factorial program in Java and Scala. :P"
tags: []
image:
  feature:
  teaser:
  thumb:
date: 2017-10-09T22:08:14-05:00
---

We all know what factorial of a given number is. Let's look at the definition; before we go ahead:   
> **From wikipedia:** _In mathematics, the factorial of a non-negative integer n, denoted by n!, is the product of all positive integers less than or equal to n_  
> _For e.g, 6! = 6 x 5 x 4 x 3 x 2 x 1_ 
>   
> _The mathematical definition of factorial is as follows:_  
> ![definition](https://wikimedia.org/api/rest_v1/media/math/render/svg/26dd1fcd06593cc552a96cc44f360b94bb791534)

So basically, it means to multiply each term of the series of descending natural numbers beginning from the given number up to 1. Now, 0! is defined as 1, it may seem funny but it makes sense in terms of empty product:  

> _The empty product of numbers is the borderline case of product, where the number of factors is zero, that is, the set of the factors, is empty. In such a "borderline" case, the empty product of numbers is equal to the multiplicative identity, which is 1._  
>         _**0! = 1**_

With the above in mind, we could jot down a [simple program in Java](https://github.com/kaushikrroy/programmers-notes/blob/master/src/main/java/io/github/kaushikrroy/programmers/java/notes/factorial/FactorialIterative.java) using a for loop:

```java
  public int factorial(final int n) {
    int result = 1;

    for (int i = n; i >= 2; i--) {
      result *= i;
    }

    return result;
  }
```

We could do it in [Scala](https://github.com/kaushikrroy/programmers-notes/blob/master/src/main/scala/io/github/kaushikrroy/programmers/notes/factorial/Factorials.scala) as:

```scala
  def factorial(n: Int) = (1 to n foldLeft 1)(_ * _)
```

Factorial of a number 'n' can be calculated from previous number's factorial value, i.e, 'n - 1' factorial as shown:

| n | n!                    | n x (n-1)! | Result |
|:-:|:---------------------:|:----------:|:------:|
| 1 | 1                     | 1          |    1   |
| 2 | 2 x 1                 | 2 x 1!     |    2   |
| 3 | 3 x 2 x 1             | 3 x 2!     |    6   |
| 4 | 4 x 3 x 2 x 1         | 4 x 3!     |   24   |
| 5 | 5 x 4 x 3 x 2 x 1     | 5 x 4!     |   120  |
| 6 | 6 x 5 x 4 x 3 x 2 x 1 | 6 x 5!     |   720  |

Therefore, factorial of a given number n, f(n), can take the following definition:  
> **_f(n) = 1 if n == 0 (Base condition)_**  
> **_f(n) = n * f(n - 1) otherwise_**  

The above is a pattern for recursion. We could jot this down in [Java](https://github.com/kaushikrroy/programmers-notes/blob/master/src/main/java/io/github/kaushikrroy/programmers/notes/factorial/Recursive.java) as follows:  
```java
  public int factorial(final int n) {
    if (0 == n) {
      return 1;
    } else {
      return n * factorial(n - 1); // n * f(n -1)
    }
  }
```

We could do it in [Scala](https://github.com/kaushikrroy/programmers-notes/blob/master/src/main/scala/io/github/kaushikrroy/programmers/notes/factorial/Factorials.scala) as:
```scala
def factorial(n: BigInt): BigInt = if (0 == n) 1 else n * factorial(n - 1)
```

Using implicits of Scala we could make the code more close to mathematics, and actually make **"5!"** a valid expression.  
For this to happen we need to define an implicit class. The code is given below:  

```scala
// Using an implicit class
// Base factorial function
def factorial(n: BigInt): BigInt = if (0 == n) 1 else n * factorial(n - 1)

// You may have to import postfix options and implicit conversions.
import scala.language.postfixOps
import scala.language.implicitConversions

// Wrapper on top of Int which defines the ! function.
implicit class PimpedInteger(n: Int) {
  def ! = Recursive factorial (BigInt(n))
}

// If you play with the REPL and use the above code.
// Now, 5! will work, and yield 120. If you type 12! on REPL
scala> 12!
res3: BigInt = 479001600
``` 

We could use a implicit function as well:

```scala
// Using an implicit function
// Base factorial function
def factorial(n: BigInt): BigInt = if (0 == n) 1 else n * factorial(n - 1)

// You may have to import postfix options and implicit conversions.
import scala.language.postfixOps
import scala.language.implicitConversions

// Wrapper on top of Int which defines the ! function.
implicit class PimpedInteger(n: Int) {
  def ! = Recursive factorial (BigInt(n))
}

implicit def intToFactorial(n: Int) = new WrappedInteger(n)

// If you play with the REPL and use the above code.
// Now, 5! will work, and yield 120. If you type 12! on REPL
scala> 12!
res3: BigInt = 479001600
``` 

This illustrates how powerful syntactic sugars can be. The implicit feature of scala is a very handy feature for reducing code, but can make the code more confusing. This feature has to be wisely used. Happy reading!