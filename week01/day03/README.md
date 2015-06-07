# Day #3
So far you have seen how to use some existing functions in F#. Now it's time to learn how to define your own. Enter: function literals.

In Functional Programming languages functions are values like integers and strings. So their values can also be written "inline". Here's a simple example:

```
fun x y -> x + y
```

Yes, that's a function literal, it describes a value. This value can be passed around like a _float_ or a _bool_ value. Think of it as an array of bytes containing executable code.

The syntax for function literals (or lambda expression) is:

```
"fun" { formal_parameter } "->" function_body
```

A value of a primitive type is just inert data. But a function value can become alive. Just call the function, e.g.

```
(fun x y -> x + y) 2 3
```

Like with an operator put the function literal in parentheses and pass values for its formal parameters.

Here's another example, this time including result display on standard output. Do you recognize the operation?

```
printfn "%b" ((fun x y -> x && not y || not x && y) true false)
```

This function implement an XOR operator for bool values.

Let me repeat: function literals are just values described using text. So the following basically is all the same:

```
123
"Hello!"
true
fun x -> -x
``` 

It's just values: an _int_ value, a _string_ value, a _bool_ value. Each literal describes a value of a certain type. But what is the type of the function literal? It's _int -> int_. It's a type you define.

Since a function is a transformation of some input value(s) of some type into an output value of some type a function type is written like this. The transformation is signified by the ->: what's on the left side is transformed into what's on the right side.

This transformation is also visible in the function literal: The input is represented by a list of formal parameters, the output by one or more expressions. It's almost like _fun input -> output_.

Strangely, though, the type of

```
fun x y -> x + y
```

is

```
int -> int -> int
```

What does that mean? It's a single function, but its type describes it as two transformations in a row? First a transformation _int -> int_, then another transformation _int -> int_?

Welcome to the world of Functional Programming!

Remember: Functions are just values. That means, they can be combined to form new values.

Here's an analogy: How is this expression evaluated?

```
2 * (3 + 4)
```

First _3 + 4_ is calculated resulting in 7, then _2 * 7_ is calculated resulting in 14. Right?

```
1. 2 * (3 + 4)
2. 2 * 7
3. 14
```

The final result is calculated step by step. There is no difference between _(3 + 4)_ and 7.

Now, the same can be done with F# functions. It's called partial application.

Here's the addition function from above, but called in a slightly different way:

```
((fun x y -> x + y) 2) 3
```

If you apply the "parentheses first" rule, then what's the sequence of calculations? It looks like this:

```
1. ((fun x y -> x + y) 2) 3
2. (fun y -> 2 + y) 3
3. 5
```

A function is evaluated by replacing occurrences of formal parameters with actual parameter values. If all occurrences have been replaced, the expressions get executed to produce the result. As long as there are values missing, though, an intermediate result is returned - which is a function. That's what step 2. is showing. The 2 provided in step 1. is used, but the second parameter is missing. The initial function requiring two parameters is transformed into a function requiring just one.

Here's sequence of evaluation of the mathematical expression showing the types involved:

```
2 * (3 + 4):

int * (int + int)
int * int
int
```

And here's the partial application of the function:

```
((fun x y -> x + y) 2) 3:

((int -> int -> int) int) int
(int -> int) int
int
```

Partial application is the reason for a function type like _int -> int -> int_. Because you can read it like _(int -> int) -> int_. But since function calls are left associative the parentheses are not needed.

***

Yeah, function literals and partial application can be a bit mind bending. But then... this is very powerful stuff. You'll come to like it. And then you'll come to miss it, once you return to a single-paradigm procedural or object-oriented language.

Tomorrow will be easier. Promised.