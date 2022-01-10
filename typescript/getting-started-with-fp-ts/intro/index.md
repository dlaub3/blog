---
date: 2021-12-20T00:00:00-00:01
title: "Getting Started with fp-ts Part 1: Introduction"
description: An fp-ts tutorial for kids who can't fp-ts good and want to learn how to do other stuff good too.
draft: false
categories: ["functional programming"]
tags: ["fp-ts"]
toc: true
series: "Getting Started with fp-ts"
series_weight: 1
hero_img: 'so-hot-right-now'
hero_img_caption: 'fp-ts is so hot right now'

---

_fp-ts_ is a library (and growing ecosystem) for functional programming in
TypeScript.  It leverages a type system hack to provide [Higher Kinded 
Types](https://en.wikipedia.org/wiki/Kind_(type_theory)), a language feature 
that TypeScript doesn't naively support. The goal is to facilitate the creation 
of well structured type safe applications using the same constructs as those 
found in purely functional languages like Haskell and PureScript. However, 
assembly instructions are not included: 

> **Disclaimer.** Teaching functional programming is out of scope of this 
> project, so the documentation assumes you already know what FP is.

And thus, the motivation for this series. This first article with briefly cover 
some terms and concepts. However, understanding these concepts is not required 
to effectively use _fp-ts_.

## Algebraic Structures
https://en.wikipedia.org/wiki/Algebraic_structure
An algebraic structure is a structure with a set of operations that may be 
performed on it. The operations must satisfy specific lawful constraints. 
Wikipedia lists Magama and Semigroup as examples of algebraic structures. For 
now just consider the meaning of "algebraic structure" to be an abstract concept 
that is used to describe or categorize a thing based on it's behavior using 
math.

## Type Classes
https://en.wikipedia.org/wiki/Type_class
Functor, Semigroup, Monad, etc., are all examples of type classes. Type classes 
may inherit from other type classes as the chart below illustrates. Type classes 
must implement specific lawful operations and are in fact algebraic structures 
if you want to think about them from a mathematical perspective.

```bash
Setoid   Semigroupoid  Semigroup   Foldable        Functor      Contravariant  
Filterable
(equals)    (compose)    (concat)   (reduce)         (map)        (contramap)    (filter)
    |           |           |           \         / | | | | \
    |           |           |            \       /  | | | |  \
    |           |           |             \     /   | | | |   \
    |           |           |              \   /    | | | |    \
    |           |           |               \ /     | | | |     \
   Ord      Category     Monoid         Traversable | | | |      \
  (lte)       (id)       (empty)        (traverse)  / | | \       \
                            |                      /  | |  \       \
                            |                     /   / \   \       \
                            |             Profunctor /   \ Bifunctor \
                            |              (promap) /     \ (bimap)   \
                            |                      /       \           \
                          Group                   /         \           \
                         (invert)               Alt        Apply      Extend
                                               (alt)        (ap)     (extend)
                                                /           / \           \
                                               /           /   \           \
                                              /           /     \           \
                                             /           /       \           \
                                            /           /         \           \
                                          Plus    Applicative    Chain      Comonad
                                         (zero)       (of)      (chain)    (extract)
                                            \         / \         / \
                                             \       /   \       /   \
                                              \     /     \     /     \
                                               \   /       \   /       \
                                                \ /         \ /         \
                                            Alternative    Monad     ChainRec
                                                                    (chainRec)
```
https://github.com/sanctuary-js/sanctuary-type-classes#type-class-hierarchy

## ADT (Algebraic Data Type)
https://en.wikipedia.org/wiki/Algebraic_data_type
A simple example of an algebraic data type:
```typescript
type Foo = 'Bar' | 'Baz'
```
In TypeScript a union is a "sum" type. In this example the algebraic operation 
'sum' is used to define the type `Foo` which is either `Bar` or `Baz`. A more 
relevant example is `Option` which represents a value that may be `null` or 
`undefined`.
```typescript
type Option<T> = Some<T> | None
```
_fp-ts_ provides many useful ADTs like `Option`, `Either`, `Array`, `Record`, 
etc. These ADTs satisfy the laws of various type classes. For example `Option`, 
`Either`, `Array` and `Record` are all Functor instances. 

## Parametric Polymorphism
https://en.wikipedia.org/wiki/Parametric_polymorphism
> In programming languages and type theory, parametric polymorphism is a way to 
> make a language more expressive, while still maintaining full static 
> type-safety. Using parametric polymorphism, a function or a data type can be 
> written generically so that it can handle values identically without depending 
> on their type.[1] Such functions and data types are called generic functions 
> and generic datatypes respectively and form the basis of generic programming.
> [Parametric polymorphism](https://en.wikipedia.org/wiki/Parametric_polymorphism)

In TypeScript parametric polymophism is accomplished using generics.
```typescript
const identity<T>(x: T): T => x
```

## Ad Hoc Polymorphism
https://en.wikipedia.org/wiki/Ad_hoc_polymorphism
Ad hoc polymophism refers to a functions that operates differently on different 
types of parameters. In TypeScript ad hoc polymophism is implemented using 
function overloading.
```typescript
const cat = {
    type: 'cat' as const,
    name: 'Tom',
    getCatName() {
        console.log(this.name)
    }
}

type Cat = typeof cat

const mouse = {
    type: 'mouse' as const,
    name: 'Jerry',
    getMouseName() {
        console.log(this.name)
    }
}

type Mouse = typeof mouse

function logName(mouse: Mouse, alias: string): void
function logName(cat: Cat): void
function logName(x: Mouse | Cat, alias?: string) {
    if(x.type === 'mouse') {
        if(alias) {
            console.log(alias)
        } else {
            x.getMouseName()
        }
    } else {
        x.getCatName()
    }
}

logName(mouse, 'Cheese Fiend')
logName(cat)
```

## Higher Kinded Types
https://en.wikipedia.org/wiki/Kind_(type_theory)
If TypeScript supported higher kinded types it wold be possible to write generic 
types, that receive generic type parameters like this.
```typescript
interface Functor<H<T>> {
    map<U>(func: (value: T) => U): H<U>;
}
```
Programming With Types 11.1.3

However, this is not possible in TypeScript, so _fp-ts_ uses a bit of a hack to 
achieve similar results. This is why the type signatures can be very large and 
difficult to understand.
```typescript
/**
 * @category type classes
 * @since 2.0.0
 */
export interface Functor<F> {
  readonly URI: F
  readonly map: <A, B>(fa: HKT<F, A>, f: (a: A) => B) => HKT<F, B>
}
/**
 * @category type classes
 * @since 2.0.0
 */
export interface Functor1<F extends URIS> {
  readonly URI: F
  readonly map: <A, B>(fa: Kind<F, A>, f: (a: A) => B) => Kind<F, B>
}
// and more for Functor2, Functor3, ...
```
This technique is how type classes are implemented in fp-ts. See the 
documentation for more information. https://gcanti.github.io/fp-ts/guides/HKT.html

> In the area of mathematical logic and computer science known as type theory, 
> a kind is the type of a type constructor...
> <cite>[Kind (type theory)](https://en.wikipedia.org/wiki/Kind_(type_theory))</cite>

Think of a higher kinded type as a generic type that receives a generic type 
parameter.

## Higher Kinded Polymorphism
> Polymorphism abstracts types, just as functions abstract values. Higher-kinded
> polymorphism takes things a step further, abstracting both types and type 
> constructors
> https://www.cl.cam.ac.uk/~jdy22/papers/lightweight-higher-kinded-polymorphism.pdf

Suppose that you wanted to write a generic `lift` function for both Identity and 
Either. In _fp-ts_ you have to supply an instance to the constructor for proper 
type inference. In this example `lift` supports constructors with up to two 
kinds by the use of `Functor2` and `Kind2`. This is how higher-kinded 
polymorphism is defined using _fp-ts_. It's the reason why the type signatures 
are so complex.

```typescript
export function lift<F extends URIS2>(
  F: Functor2<F>
): <A, B>(f: (a: A) => B) => <E>(fa: Kind2<F, E, A>) => Kind2<F, E, B>
export function lift<F extends URIS>(F: Functor1<F>): <A, B>(f: (a: A) => B) => (fa: Kind<F, A>) => Kind<F, B>
export function lift<F>(F: Functor<F>): <A, B>(f: (a: A) => B) => (fa: HKT<F, A>) => HKT<F, B>
export function lift<F>(F: Functor<F>): <A, B>(f: (a: A) => B) => (fa: HKT<F, A>) => HKT<F, B> {
  return (f) => (fa) => F.map(fa, f)
}

// https://gcanti.github.io/fp-ts/guides/HKT.html
```

Here is an example of creating a specific `lift` instance by providing a constructor.
```typescript
const double = (n: number): number => n * 2

const liftIdentity = lift(identity)
liftIdentity(double)

const liftEither = lift(either)
liftEither(double)

// https://gcanti.github.io/fp-ts/guides/HKT.html
```

## References
- https://jrsinclair.com/articles/2019/what-i-wish-someone-had-explained-about-functional-programming
