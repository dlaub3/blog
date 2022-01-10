---
date: 2021-12-20T00:00:00-00:02
title: "Getting Started with fp-ts Part 2: pipe, flow, Array, Record"
description: An `fp-ts` tutorial for kids who can't `fp-ts` good and want to learn how to do other stuff good too.
draft: true
categories: ["functional programming"]
tags: ["fp-ts"]
toc: true
series: "Getting Started with fp-ts"
series_weight: 2

---

## Basic Types
- pipe
- flow
- Array
- Record
- Map

## Basic Types
- Either
- Option

## Basic Types
- Ord,Setoid
- SemiGroup
- Monoid

## Intermediate Types
- Alt
- Applicative
- Apply
- Traverse
- Sequence
- Contramap

## Intermediate Types
- IO
- Task
- TaskEither
- Reader
- ReaderTaskEither

## Optics (monocle-ts)
- Lens
- Prism 
- Iso
- Traversal
- Index
- At

## NewTypes / Branding
## IO-TS (encoding/decoding)
## RemoteData
## Validation

## Bonus
- Do Notation

## References


## Branded Types and Newtypes

It's popular to first design an application or feature by defining the types and 
type signatures of it's functions.

TypeScript uses [Structural Typing](#TODO) rather than [Nominal Typing](#TODO). 
In most cases this is a good thing since it can simplify code. But in other 
cases the type safety provided by Nominal types may be desired.

Type branding is a popular method for implementing Nominal types in TypeScript. 

```typescript
type BrandedNumber = number & { readonly brand: unique symbol }
const brandedNumber = 10 as BrandedNumber
const OneHundered = brandedNumber * 10 // type is number
```

This creates a type that is unique, but may still be easily converted to the 
underlying type. 

Newtypes are useful when you need more strictly enforced types than Branded 
types.
```typescript
type NewtypeNumber = { readonly NewType: unique symbol, type: number }
const newtypeNumber = 10 as unknown as NewtypeNumber
const OneHundered = newtypeNumber * 10 // Error
```
Newtypes require an unsafe type cast to assign to them. They also prevent 
operating on the underlying types without the use of specialized helpers.

The rule of thumb is to use Newtypes whenever a type is not going to be 
interacted with as it's underlying type. For example it's not useful to use 
string methods to manipulate a  UUID . So it may be useful to create a Newtype 
for `userId`. 

Below are the types for the TODO List application. The `itemId` is a Newtype, 
while the `TodoItem` is a branded type. 

```typescript
// IOTS is io-ts
// IOTST is io-ts-types
// NT is newtype-ts

export interface ItemId
  extends newTypes.Newtype<
    { readonly ItemId: unique symbol },
    IOTST.NonEmptyString
  > {}

type TodoItemBrandSymbol = { readonly TodoItem: symbol };

type TodoItemT = IOTS.TypeOf<typeof itemC>;

export type TodoItem = IOTS.Branded<TodoItemT, TodoItemBrandSymbol>;

export interface ListState {
  todoList: readonly TodoItem[];
}

```
See [newtype-ts](https://gcanti.github.io/newtype-ts/)

## Codecs
Since TypeScript compiles to JavaScript it doesn't provide any runtime type 
checking. This is the problem that codecs solve. IO takes places at the edges of 
an application and this is were codecs are most useful. This helps prevent an 
entire class of runtime bugs. No more "Cannot read properties of undefined".

```typescript
export const itemIdC = IOTST.fromNewtype<ItemId>(IOTST.NonEmptyString);

export const itemC = IOTS.type({
  id: itemIdC,
  title: IOTS.string,
  description: IOTS.union([IOTS.string, IOTS.undefined]),
  done: IOTS.boolean,
});

export const todoItemC = IOTS.brand(
  itemC,
  (x: unknown): x is TodoItem => itemC.is(x),
  "TodoItem",
);
```

At this point the application types have been defined. The types have been 
enhanced where appropriate with branded types and newtypes. And there is runtime 
type validation at the application boundary to guarantee that the application 
will run properly or gracefully handle any invalid types. 

See [io-ts](https://gcanti.github.io/io-ts/) & [io-ts-types](https://gcanti.github.io/io-ts-types/)


# Article #2

## Optics
Optics provide an elegant way to get, set, or modify data. 

### Lens
Lenses are fairly straight forward to understand. If you have an object and you 
want to get, set, or modify it's properties. Then a lens will work quite well. 
```typescript
export const done = Lens.fromProp<TodoItem>()("done");
const isDone = done.get(item)
done.set(false)(item)
done.modify(isDone => !isDone)(item)
```

### Optional
An Optional is like a lens, but it's indented for use with values that may not 
exist. Below is an example of using an Optional to update an item in a list.
```typescript
type Item = { name: string; details: { description?: string } };
const meal = Optional.fromPath<Item>()(["details", "description"]);
const updatedItem = meal.modifyOption((s) => s.toLowerCase())({
  name: "foo",
  details: {},
}); // O.Option<Item>
```

### Traversal
A traversal allows multiple items to be updated in a traversable data structure. 
```typescript
export const todoItemTraversal = fromTraversable(RA.Traversable)<TodoItem>();
export const traversableTodoList = todoListL.composeTraversal(todoItemTraversal);
export const setDoneTraversable = (x: boolean) => pipe(traversableTodoList, traversal.composeLens(done), traversal.set(x));
```

- See [monocle-ts](https://gcanti.github.io/monocle-ts/)
- See [Monocle](https://www.optics.dev/Monocle/docs/optics) for an Optic composition table.
- See https://codesandbox.io/s/monocle-ts-talk-forked-occrj for more examples including `Index`, `At`, and `Prism`

# Article #3

## State
The state Monad

# Article #4
## ReaderTaskEither
### Either
### Task
### Task Either
### Reader
### Reader TaskEither

# Article #5
## RemoteData
Loading states
- pending clears data

## Validation
- AND/Semigruop
- OR/Alt
- Semiring


## References
- fp-ts cheat sheet
