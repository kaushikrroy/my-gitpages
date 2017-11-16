---
layout: article
title: "Towers of Hanoi"
date: 2017-10-10T16:19:16-04:00
modified:
categories: articles
excerpt: "About towers of hanoi, and then programs in Java and Scala."
tags: []
ads: false
image:
  feature:
  teaser:
  thumb:
---


Towers of Hanoi is a mathematical puzzle. This puzzle is also known as Brahma's Puzzle or Lucas's Puzzle. It consists of three rods or pegs and a number of disks with a hole in the center, helping them slide into any one of those rods or pegs. The puzzle starts with all the disks neatly placed on top on other on one rod such that, no bigger disk sits on a smaller one. This makes a conical shape.  

The following image would make it more clear:

<p align="center">
 <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/07/Tower_of_Hanoi.jpeg/300px-Tower_of_Hanoi.jpeg"/>
</p>

The objective is move the entire stack of disks or pegs from the original rod or tower to another rod, obeying the following simple rules:  
> 1. Only one disc shall be moved at a time.
> 2. Move should only consist of taking the upper disk one from one of the stacks and placing it on top of another stack.
> 3. No disk shall be placed on top of a smaller disk.

So basically, it means to multiply each term of the series of descending natural numbers beginning from the given number up to 1. Now, 0! is defined as 1, it may seem funny but it makes sense in terms of empty product:  

> _The empty product of numbers is the borderline case of product, where the number of factors is zero, that is, the set of the factors, is empty. In such a "borderline" case, the empty product of numbers is equal to the multiplicative identity, which is 1._  
>         _**0! = 1**_

With the above in mind, we could jot down a [simple program in Java](https://github.com/kaushikrroy/programmers-notes/blob/master/src/main/java/io/github/kaushikrroy/programmers/notes/factorial/Linear.java) using a for loop:

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
implicit class WrappedInteger(n: Int) {
    def ! = factorial(BigInt(n))
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
class WrappedInteger(n: Int) {
    def ! = factorial(BigInt(n))
}

implicit def intToFactorial(n: Int) = new WrappedInteger(n)

// If you play with the REPL and use the above code.
// Now, 5! will work, and yield 120. If you type 12! on REPL
scala> 12!
res3: BigInt = 479001600
``` 

This illustrates how powerful syntactic sugars can be. Happy reading!