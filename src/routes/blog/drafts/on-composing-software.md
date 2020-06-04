---
title: On Composing Software
date: 2020-03-17T12:51:00.000Z
---

Notes on reading (Composing Software by Eric Elliot)[https://leanpub.com/composingsoftware].

<!-- more -->

# Composing Software: Part 1 (Introduction)

All software development is about breaking down large problems into smaller ones, and then building components to solve the smaller problems, and compose them to solve the entirety of the issue.

We compose everyday, we can do it unconciously or conciously (bad or good)

## Composing Functions

Software composition is the process of applying a function to the output of another function.

Ex:

### Chaining promises is composing
```js
const g = n => n + 1
const f = n => n * 2

const wait = time => new Promise((resolve, reject) => {
  return setTimeout(resolve, time)
})

wait(300)
  .then(()=>20)
  .then(g)
  .then(f)
  .then(value => console.log(value)) //42
```

If you're chaining, you're composing, passing return values into other functions? composing!

## Composing Objects

Favour object composition over class inheritance - The Gang of Four

In computer science, a composite data type or compound data type is any data type which can be constructed in a program using the programming language's primitive data types and other composite types.
The act of constructing a composite type is known as composition.

The most common form of object composition in JavaScript is known as object concatenation.

ex:

### composites with class inheritance

```js
class Foo {
  constructor () {
    this.a = 'a'
  }
}

class Bar extends Foo {
  constructor (options) {
    super(options)
    this.b = 'b'
  }
}

const myBar = new Bar() // {a: 'a', b: 'b'}
```

### composites with mixin composition
```js
const a = {a: 'a'}
const b = {b: 'b'}
const c = {...a,...b} // {a: 'a', b: 'b'}
```


# Composing Software: Part 2 (DAO of immutability)

Functional programming is a foundational pillar of JavaScript, and immutability is a foundational
pillar of functional programming.

Sometimes, the elegant implementation is just a function.

Immutability: The true constant is change. Mutation hides change. Hidden change manifests chaos. Therefore, the wise embrace history.
Separation: Logic is thought. Effects are action. Therefore the wise think before acting, and act only when the thinking is done.

Composition All thins in nature work in harmony. Atree cannot grow without water. A bird cannot fly without the air. Therefore the wise mix ingredients together to make their soup taste better.

Flow: Still waters are unsafe to drink. Food left alone will rot. The wise prosper because they let things flow.

# Composing Software: Part 2 (The rise and fall and rise of functional programming)

## The Rise

Alonzo Church invented lambda calculus, a universal mode of computation based on function application.
Alan Turing is known for the Turing machine, a universal model of computation that defines a theoritical device that manipulates symbols on a strip of tape.

There are 3 important points that make lambda calculus special:
* Functions are always anonymous.
* Functions in lambda calculus are always unary, meaning they only accept a single parameter.
* Functions are first-class, meaning functions can be used as inputs to other functions, and functions can return functions.

## The Fall

Somewhere between 1970 and 1980, the way that software was composed drifted away from simple algebraic math, and became a list of linear instructions for the computer to follow.

In 1972, Alan Kay's smalltalk was formalized, and the idea of objects as the atomic unit of composition took hold.Smalltalk's great idea about component encapsulation and message passing got distorted in the 80s and 90s, into a horrible idea about inheritance hierarchies.

## The Rise

Around 2010, JavaScript exploded. By 2015, the idea of functional programming was popular again, and so came arrow functions.

# Why Learn Functional Programming in JavaScript?

JavaScript has the most important features needed for functional programming:
1. First class functions.
2. Anonymous functions and concise lambda syntax.
3. Closures.

## What is JavaScript missing

The disadvantage of a multi-paradigm language is that imperative and object-oriented programming ted to imply that almost everything needs to be mutable.

Features that JavaScript does not have:
1. Purity.
2. Immutability.
3. Recursion. Lacks proper tail calls, a language feature which allows recursive functions to reuse stack frames for recursive calls.

Without proper tail calls, a call stack can grow without bounds. (Updated in ES6)

## What JavaScript has that pure functional languages lack

Diversity of thought and users in the ecosystem.
Apps ate the world, the web ate apps, and JavaScript ate the web.

## Pure Functions

Functions may server the following purposes:
* Mapping
* Procedures
* I/O

A pure function is a function which:
1. Given the same input, will always return the same output.
2. Produces no side effects.

A pure function has referential transparency, meaning you could replace the function with it's resulting value without changing the meaning of the program.

# Functional Programming
It's a programming paradigm where applications are composed using pure functions, avoiding shared mutable state and side-effects. Is usually more declarative, meaning we express what to do rather than how to do it.

* Pure functions
* Function composition
* Avoid shared state
* Avoid mutating state
* Avoid side effects
* Declarative vs imperative

The problem with shared state is that in order to understand the effect of a function, you have to know the entire history of every shared variable that the function uses or affects.

If you change the order of the composition the output will change. Order of operations still matters.
Remove function call timing dependency, and you eliminate an entire class of potential bugs.

A functor data structure is a data structure that can be mapped over. I other words it's a container which has an interface which can be used to apply a function to the values inside it. Functor -> "Mappable".

Imperative code frequently utilizes statements. A statement is a piece of code which performs some actions. Examples of common statements are: `for, if, switch, throw` etc.

Declarative code relies more on expressions. An expression is a piece of code which evaluates some value. Expressions are usually some combination of function calls, values, and operators which are evaluated to produce the resulting value.

Functional programming favors:
* Pure functions over shared state and side effects
* Immutability over mutable data
* Function composition over imperative flow control
* Generic utilities that act on many data types over object methods that only operate on their colocated data.
* Declarative over imperative code
* Expressions over statements

# Currying and Function Composition

A curried function is a function that takes multiple arguments one at a time.

A closure is a function bundled with its lexical scope. Closures are created at runtime during function creation. Fixed means that the variables are assigned values in the closure's bundled scope.

All curried functions return partial application, but not all partial applications are the result of curried functions.


## Point-free style

Point-free style is a style of programming where function definitions do not make reference to the function's arguments.

```js
const capitalise = str =>
    str && str.charAt(0).toLocaleUpperCase() + str.substr(1);

const words = ["foo", "bar", "baz"];

// logs [ 'Foo', 'Bar', 'Baz' ]
console.log(words.map(w => capitalise(w)));
// logs [ 'Foo', 'Bar', 'Baz' ]
console.log(words.map(capitalise));
```

We can use currying to specialize functions.

Curry can also be used to simplify function composition, because it takes one parameter at a time an returns a function which takes the next argument.

An approach to currying called "data last" which means you should take the specializing parameters first, and take the data the functino will act on last.

# Abstraction and Composition

Abstraction is the process of considering something independently of its associations, attributes, or concrete accompaniments.

Software solutions should be decomposable into their component parts, and recomposable into new solutions, without changing the internal component implementation details.

## Abstraction is simplification

The process of abstraction has to main components:
* Generalization
* Specialization

## Abstraction in Software

Abstraction is the process of considering something independently of its associations, attributes, or concrete accompaniments.

Software solutions should be decomposable into their component parts, and recomposable into new solutions, without changing the internal component implementation details.

## Abtraction is simplification

The process of abstraction has two main components:

* Generalization: is the process of finding similarities in repeated patterns, and hiding the similarities behind an abstraction.
* Specialization: is the process of using the abstraction, supplying only what is different for each use case.

## Abstraction in Software

* Algorithms
* Data Structures
* Modules
* Classes
* Frameworks
* Functions

"Sometimes, the elegant implementation is just a function"

Be deliberate about creating abstractions; learn to recognize characteristics of good abtractions:

* Composable
* Reusable
* Independent
* Concise
* Simple

## Reduce

A.K.A. Fold, accumulate.
Utility commonly used in functional programming that lets you iterate over a list, applying a function to an accumulated value and the next item in the list, until the iteration is complete and the accumulated value gets returned.

### Reduce is versatile

It's easy to define map, filter, forEach and lots of other interesting things using reduce.

# Functors & Categories

A functor data type is something you can map over. It's a container which has a mpa operation which can be used to applu a function to the values inside it. When you see a functor datatype, you should think mappable.

The javascript .map() method is a good exmpale of a functor.

The point of a functor is to apply a given function to values contained withing the context of the structure.

## Why Functors?

* The details of underlying data structure implementation are abstracted away.
* Functors hide the types of data they contain, which allows you to act on the containers using generic functions, without caring about what you're storing inside them.
* Mapping over an empty functor is the same as mapping over a functor containing many items.
* Functors allow you to easily compose function over the data inside.

Example:

```js
// Could we .chain the following function
const nextCharForNumberString = str => {
  const trimmed = str.trim() // how do I chain trim?
  const number = parseInt(trimmed)
  const nextNumber = new Number(number + 1) // how do I chain + 1
  return String.fromCharCode(nextNumber)
}

// Things aren't as straight forward out of the box.

// Box is an identity Functor

const Box = x => ({
  map: f => Box(f(x)),
  inspect: `Box(${x})`,
  fold: f(x)
})


const boxedNextCharForNumberString = str => {
  Box(str)
  .map(x => x.trim())
  .map(trimmed => parseInt(trimmed, 10))
  .map(number => new Number(number + 1))
  .fold(String.fromCharCode)
}

console.log(boxedNextCharForNumberString('   64    ')) // 'A'

//
```

We could also write compose in terms of Box:

```js
const compose = (f,g) => x => Box(x).map(g).fold(f)
```


## Functors Laws

Functors must respect identity and composition.

## Category Theory

Concepts:
* A category is a collction of objects and arrows between objecst (where object can mean just about anything).
* Arros are known as morphisms. Morphisms map between two objects A and B, connecting them with an arrow f.
* All objects must have identity arrows, which are arrows pointing back to the same object.
* For any group of connected objects, `A -> B -> C`, there must be a composition which goes directly from `A -> C`.
* All morphisms are equivalent to compositions.

## Conclusion

Functor data types are data types you can map over. Think of them as containers which can apply functions to the contents inside, which will return a new container with the results.

A functor is a structure preserving map from category to category, meaning, relationships between objects and morphisms are preserved.
Functors are grea higher order abstractions that allow you to compose functions over the contents of a container without coupling those functions to the structure or implementation details of the functor data type.

# Monads

Box is a Monad and a Functor

A monad is a way of composing functions that require context in addition to the return value, such as computation, branching, or effects.

Example

```js
const Box = x => ({
  map: f => Box(f(x)),
  chain: f => f(x),
  fold: f => f(x),
  toString: () => `Box(${x})`

})

// We need a monad instead of a functor here.
const applyDiscountOriginal = (price, discount) => {
  const cents = moneytoFloat(price)
  const savings = percentToFloat(discount)
  return cents (cents * savings)
}

const applyDiscount = (price, discount) => {
  Box(moneytoFloat(price))
  .chain(cents =>
      Box(percentToFloat(discount))
      .map(savings => cents - (cents * savings))
    ).fold(x => x)
}

```

Monads make composition easier. The essence of monads:

* Functions can compose: `a=>b=>c` becomes `a=>c`
* Functors can compose function with context: given `F(a)` and two functions, `a=>b=>c`, return `F(c)`
* Monads can compose type lifting functions `a=>M(b)`, `b=>M(c)` becomes `a=>M(c)`

A monad is based on a simple simmetry:
* of: A type lift  `a=>M(a)`
* map: The application of a function `a=>M(b)` inside the monad context, which yields `M(M(b))`
* flatten: The unwrapping of one layer of monadic context: `M(M(b)) => M(b)`

Combine map with flatten, and you get flatMap

You may have heard that a promise is not strictly a monad. That’s because it will only unwrap the
outer promise if the value is a promise to begin with. Otherwise, .then() behaves like .map().

Functors are things you can map over, Monads are things you can flatMap over, so a Monad is a pointed Functor with a chain.

# The Forgotten History of OOP

## The Big Idea

To use encapsulated mini-computers in software which communicated via message passing rather than direct data sharing.

The big idea is message sharing.

## The Essence of OOP

* Encapsulating state
* Decoupling objects from each other
* Runtime adaptability

Objects could abstract away and hide data structrure implementation.

## What OOP Doesn't mean

Essentials:
* Encapsulation
* Message passing
* Dynamic binding

Non Essential:
* Classes
* Class inheritance
* Special treatment for objects/functions/data
* Polymorphism
* Static types
* Recognizing a class as a type

# Object Composition

A composite data type or compound data type is any data type which can be constructed in a program using the programming language's primitive data types and other composite types.

Class inheritance is a subset of object composition.

## Three different forms of object composition

* Aggregation: When an object is formed from an enumerable colleciton of subojects.
* Concatenation: When an object is formed by adding new properties to an existing object.
* Delegation: When an object forwards or delegates to another object.

These different forms of composition aare not mutually exclusive.

# Factory Functions

Is any function which is not a class or constructor that returns a new object.
In javascript any function can return an object. When it does so without the new keyword, it is a factory function.

# Functional Mixins

Are composable factory functions which connect together in a pipeline; each function adding some properties or bheaviours like workers on an assembly line. Functional mixins do not depend on or require a base factory or constructor.

Features:
* Data privacy/encapsulation
* Inheriting private state
* Inheriting from multiple sources
* No diamond problem
* No base class requirement

## Mixins

Mixins are a form of object composition, where component features get mixed into a coposite object so that properties of each mixin becomes properties of the composite object.

## Functional inheritance

Is the process of inheriting features by applying an augmenting function to an object instance.

## When to use functional mixins

* Application state management
* certain cros cutting concerns and services e.g. logging
* Composable functional data types.

## Caveats

Most problems can be elegantly solved using pure functions. The same is not true of functional mixins. Like class inheritance, functional mixins can cause problems of their own. In fact, it's possible to faithfully reproduce all of the features and problems of class inheritance using functional mixins.

* Favor pure functions > factories > functional mixins > classes
* Avoid the creation of is-a relationship between objects, mixins, or data types.
* Avoid implicit dependencies between mixins - wherever possible, functional mixins should be self-contained, and have no knowledge of other mixins.
* "Functional mixins" doesn't mean "functional programming.
* There may be side-effects when you access a property using Object.assign(). You'll also skip any non-enumerable properties. (Object.getOwnPropertyDescriptors works around this)

## Classes

Class inheritance is very rarely the best approach in Javascript.

## Conclusion

Be aware, functional mixins does not imply functional programming, it simply means mixins using functions.

* Unlike simple object mixins, funcitonal mixins support true encapsulation.
* Unlike single ancestor class inheritance, functional mixins also support the ability to inherit from many ancestors.
* Unlike multiple inheritance the diamond problem is rarely problematic in JS because there is a simple rule when collisions arise, the last mixin added wins.
* Unlike class decorators, traits, or multiple inheritance, no base class is required.

Favor the simplest implementation over more complex constructions.

# Why Composition is Harder with Classes


The .of() method is a factory that returns a new instance of the data type containing whatever you pass into .of()

## Classes are ok if you are careful

* Avoid instanceof, it lies because Javascript is dynamic and has multiple execution conctexts, and instanceof fails in both situations.
* Avoid extends, don't extend a single hierarchy more than once.
* Avoid exporting your class. Use class internally form performance gains, but export a factory that creates instances in order to discourage users from extending your class and avoid forcing callers to use new.
* Avoid new. Try to avoid using it directly whenever it makes sense, and don't force your callers to use it. (Export a factory)

It's ok to use class if:
* You never inherit from your own classes or components.
* You need to optimize performance.

# Composable Data Types

In Javascript, the easiest way to compose is function composition, and a function is just an object you can add methods to.

```js
const t = value => {
  const fn = () => value
  fn.toString = () => `t(${value})`
  return fn
}

const someValue = t(2)
console.log(someValue.toString()) // t(2)

```

# Lenses

A lens is a composable pair of pure getter and setter functions which focus on a particular field inside an objectm and obey a set of axioms known as the lens laws.

## Why Lenses

State shape dependencies are a common source of coupling in software. Many components may depend on the shape of some shared state, so if you need to later change the shape of that state, you have to change logic in multiple places.

Lenses allow you to abstract state shape behind getters and setters.
Instead of littering your codebase with code that dives deep into the shape of a particular object, import a lens. If you later need to change the state shape, you can do so in the lens, and none of the code that depends on the lens will need to change.

## Composing Lenses

Lenses are composable, when you compose lenses, the resulting lens will dive deep into the object, traversing the full object path.

Lenses in general are a way to manage data through functions without tigh coupling data to code.

# Transducers

A transducer is a composable high order reducer. It takes a reducer as input, and returns another reducer.

* Are composable using simple function composition
* Efficient for large collections or infinite streams: Only enumerates over the signal once, regardless of the number of operations in the pipeline.
* Able to transduce over any enumerable source
* Usable for either lazy or eager evaluation with no changes to the transducer pipeline.

Transducers compose top to bottom, left to right.

## Why transducers?

It's useful to break up the procesing into multiple independent, composable stages.

## Background

In hardware signal processing systems, a transducer is a device which converts one form of energy to another. Likewise a transducer in code converts from signal to another signal.

## Transducers rules

1. Initialization: Given no initial accumulator value, a transducer must call the step function to produce a valid initial value to act on. The value should represent the empty state.
2. Early termination: A process that uses transducers must check for and stop when it receives a reduced accumulator value. Additionally, a transducer step function that uses a nested reduce must check for and convey reduced values when they are encountered.
3. Completion (optional): Some transducing process never complete, but those that do should call the completion function to produce a final value and/or flush state, and stateful transducers should suypply a completion operation that cleans up any accumulated resources and potentially produces one final value.

## The Transducer Protocol

Transducers are not really a single function, they are made from 3 different functions.
Javascript transducers are a function that take a transducer and return a transducer. The transducer is an object with three methods.

1. init Return a valid initial value for the accumulator (usually, just call the next step())
2. step Apply the transform.
3. result If a transducer is called without a new value it should handle its completion step (unless the transducer is stateful)

Example of a transducer:
```js
const map = f => next => transducer({
  init: () => next.init(),
  result: a => next.result(a),
  step: (a,c) => next.step(a, f(c))
})
```

# Elements of Javascript Style

You can improve your code by applying similar standards to your code that those of "The elements of style" by William Strunk Jr, applied to the English language.

1. Make the function the unit of composition. One job for each function. (Pure functions)
2. Omit needless code. (point free & composition)
3. Use active voice. (createUser is better than user.create)
4. Avoid a succession of loose statements.
5. Keep related code together.
6. Put statements and expressions in positive form
7. Use parallel code for parallel concepts

> Code should be simple, not simplistic.

# Mocking is a code smell.

Dependency injection is not the best way to accomplish decoupling.

## TDD should lead to better design

The process of learning effective TDD is the process of learning how to build more modular applications.

TDD tends to have a simplifying effect on code, not a complicating effect. If you find that your code gets harder to read or maintain when you make it mroe testable, or you have to bloat your code with dependency injection boilerplate, you are doing TDD wrong.

* You can write decoupled code without dependency injection.
* Maximizing code coverage brings diminishing returns.

Different kinds of codes need different levels of mocks. Some code exists primarily to facilitate I/O, in which case, there is little to do other than test I/O, and reducing mocks might mean your unit test coverage would be close to 0.

Mocking is required when our supposed atomic units of composition are not really atomic.

## How do we remove coupling?

Where does it come from?

Tight Coupling:

* Class inheritance
* Global Variables
* Other mutable state
* Module imports with side effects
* Implicit dependencies from compositions
* Dependency injection containers
* Dependency injection parameters
* Control parameters
* Mutable parameters

Loose Coupling:
* Module imports without side-effects
* Message passing - pub/sub
* Immutable parameters

Why is dependency injection part of the tight coupling problem:
Can the unit be testet without mocking dependencies? If it can't, it's tightly couple to the mocked dependency.

The more dependencies your unit has, the more likely it is that there may be problematic coupling.

## Mocking is great for integration tests

Because integration tests test collaborative integrations between units, it's perfectly OK to fake servers, network protocols, network messages, and so on in order to reproduce all the various conditions you'll encounter during communication with other units, potentially distributed across clusters of CPUs or separate machines on a network.

