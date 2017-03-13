---
layout: post
title: Getting started with Functional Programming in C# 
excerpt: "So what is functional programming?"
categories: [C#]
tags: [c#,functional programming]
comments: true
date: 2017-03-13
modified: 2017-03-13
---

### What is it?

So what is functional programming? Straight from [wiki](https://en.wikipedia.org/wiki/Functional_programming),

_Definition_:
_"functional programming is a programming paradigm—a style of building the structure and elements of computer programs—that treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data. It is a declarative programming paradigm, which means programming is done with expressions or declarations instead of statements"_

In constrast, imprerative programming focuses on procedure, and control flow of statements.

The functional programming paradigm is used extensively in many places.
In javascript, the $.each(list, someFunction) , is an example. 
For each item in the list, do something by calling someFunction. If you notice, the result of the function on an element doesn’t effect the result of another. The result of the function, only depends on what input it gets, and it will produce the same result every time its executed. No side effects!
Because of this, concurrency is safe as well.

Functional programming abstracts away how to do things, and focuses on what to do. [Declarative!](https://en.wikipedia.org/wiki/Declarative_programming).
Call someFunction for each item in my list, don’t worry about how, just do it!

### Let's go!

Since we are going to use functions a lot more, lets give them some respect, and learn how to represent them.

In C# you can represent a function as a Func,Action or Predicate
1. Func<T1,T2,…Tn> - a function that takes parameters T1 to Tn-1, and returns type Tn
2. Action<T1,T2,…Tn> - a function that takes parameters T1 to Tn, and returns void
3. Predicate<T1,T2,…Tn> - a function that takes parameters T1 to Tn, and returns a boolean

We can define functions in different ways as well
1. delegate - delegate(){ //do something }
2. lambda expression - () => { // do something }

If you are using the delegate way, you would have to specify the parameter types explicitly.
However, with lambda expressions, you can do without them
{% highlight csharp %}
delegate(int x){ /*do something*/ }
(x) => { /*do something*/ }
{% endhighlight %}


Lets understand the basics with some examples.
+ `Func`. Lets define a function that will double a number.
It will take an integer as parameter, and will return an integer.
We can either use the delegate to define what the function is, or use a lambda expression
{% highlight csharp %}
Func<int,int> DoubleItDelegate = delegate(int number){
    return number * 2;
};
Func<int,int> DoubleItLambda = (e) => { return e*2; }; // nothing but a function that takes e as parameter, and returns double.
{% endhighlight %}
Now we can call the above functions
{% highlight csharp %}
var doubleItDelegate = DoubleItDelegate(5);
var doubleItLambda = DoubleItLambda(5);
{% endhighlight %}

+ `Action`. We can also assign functions to other functions.
In the below example, MyConsoleWriteLine is our own function, which points to Console.WriteLine.
The Console.WriteLine is nothing but a function that takes a string parameter, and returns void (Action)
{% highlight csharp %}
Action<string> MyConsoleWriteLine = Console.WriteLine;
MyConsoleWriteLine("My Console WriteLine wrote this!");
{% endhighlight %}

+  `Predicate`. Lets define a function that will check if a number is greater than zeros
{% highlight csharp %}
Predicate<int> IsGreaterThanZero = (e) => { return e > 0; };
MyConsoleWriteLine(IsGreaterThanZero(number) ? "Greater than zero" : "Not greater than zero");
{% endhighlight %}

Since we can represent a function like any other object, we can also pass them around to other functions. So much power!

Lets take another example to understand this.
Consider a list, and lets assume there is a necessity to get the count of elements from the list, based on some condition.
The conditions may vary ofcourse.

While writing code, it may not seem immediately apparent, but imagine 20 different developers, writing their own functions, because their conditions are different from one another.

Every developer would have something like below
{% highlight csharp %}
public static int CountBasedOnYourCondition(List<int> list){
    int count=0;
    foreach(var item in list)
        if(SatisfiesMyConditon(item)) 
            count++;
    return count;
} 
{% endhighlight %}
Now imagine if we could take that `SatisfiesMyConditon` function, as a parameter of `CountBasedOnYourCondition`.
Bingo!
{% highlight csharp %}
// a function that accepts a list, and gives out a count based on some condition
var list = new List<int>{1,-1,4,90,23,0};
Predicate<int> IsDivisibleByFive = e => { return e > 5; };
Predicate<int> IsDivisibleByTwo = e => { return e > 2; };

public static int CountBasedOnYourCondition(List<int> list, Predicate<int> condition){
    int count=0;
    foreach(var item in list)
        if(condition(item)) // Function that is passed gets executed here, IsDivisibleByFive(item) , IsGreaterThanZero(item) etc.
            count++;
    return count;
} 
{% endhighlight %}

Beautiful isn't it. This way we reuse almost all of our code, and maintanence is cheap.



