---
title: TypeScript Type Guards | CRONje.ME
label: TypeScript Type Guards
order: 20
authors:
  - name: Charl Cronje
    email: charl@cronje.me
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
<script type="text/javascript">(function(w,s){var e=document.createElement("script");e.type="text/javascript";e.async=true;e.src="https://cdn.pagesense.io/js/webally/f2527eebee974243853bcd47b32631f4.js";var x=document.getElementsByTagName("script")[0];x.parentNode.insertBefore(e,x);})(window,"script");</script>

This page lists some of the more advanced ways in which you can model types, it works in tandem with the `Utility Types` doc which includes types which are included in TypeScript and available globally.

## Type Guards and Differentiating Types

Union types are useful for modeling situations when values can overlap in the types they can take on. What happens when we need to know specifically whether we have a `Fish`? A common idiom in JavaScript to differentiate between two possible values is to check for the presence of a member. As we mentioned, you can only access members that are guaranteed to be in all the constituents of a union type.

```ts
let pet = getSmallPet();
 
// You can use the 'in' operator to check
if ("swim" in pet) {
  pet.swim();
}
// However, you cannot use property access
if (pet.fly) {
Property 'fly' does not exist on type 'Fish | Bird'.
  Property 'fly' does not exist on type 'Fish'.
  pet.fly();
Property 'fly' does not exist on type 'Fish | Bird'.
  Property 'fly' does not exist on type 'Fish'.
}
```

To get the same code working via property accessors, we’ll need to use a type assertion:

## Type predicates


```ts
let pet = getSmallPet();
let fishPet = pet as Fish;
let birdPet = pet as Bird;
 
if (fishPet.swim) {
  fishPet.swim();
} else if (birdPet.fly) {
  birdPet.fly();
}
```

## User-Defined Type Guards

This isn’t the sort of code you would want in your codebase however.

It would be much better if once we performed the check, we could know the type of pet within each branch.

It just so happens that TypeScript has something called a type guard. A type guard is some expression that performs a runtime check that guarantees the type in some scope.

## Using type predicates

```ts
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

```

`pet is Fish` is our type `predicate` in this example. A `predicate` takes the form `parameterName is Type`, where `parameterName` must be the name of a `parameter` from the current function signature.

Any time `isFish` is called with some variable, `TypeScript` will `narrow` that variable to that specific type if the original type is compatible.

```ts
// Both calls to 'swim' and 'fly' are now okay.
let pet = getSmallPet();
 
if (isFish(pet)) {
  pet.swim();
} else {
  pet.fly();
}
```

Notice that TypeScript not only knows that pet is a Fish in the `if` branch; it also knows that in the else branch, you don’t have a `Fish`, so you must have a `Bird`.

You may use the type guard `isFish` to filter an array of `Fish` | `Bird` and obtain an array of `Fish`:

```ts
const zoo: (Fish | Bird)[] = [getSmallPet(), getSmallPet(), getSmallPet()];
const underWater1: Fish[] = zoo.filter(isFish);
// or, equivalently
const underWater2: Fish[] = zoo.filter<Fish>(isFish);
const underWater3: Fish[] = zoo.filter<Fish>((pet) => isFish(pet));
Argument of type '(pet: Fish | Bird) => boolean' is not assignable to parameter of type '(value: Fish | Bird, index: number, array: (Fish | Bird)[]) => value is Fish'.
  Signature '(pet: Fish | Bird): boolean' must be a type predicate.
```

## Using the `in` operator
The in operator also acts as a narrowing expression for types.

For a n `in` `x` expression, where `n` is a string literal or string literal type and `x` is a union type, the “true” branch narrows to types which have an optional or required property `n`, and the “false” branch narrows to types which have an optional or missing property `n`.

```ts
function move(pet: Fish | Bird) {
  if ("swim" in pet) {
    return pet.swim();
  }
  return pet.fly();
}
```

## `typeof` type guards

Let’s go back and write the code for a version of padLeft which uses union types. We could write it with type predicates as follows:

```ts
function isNumber(x: any): x is number {
  return typeof x === "number";
}
 
function isString(x: any): x is string {
  return typeof x === "string";
}
 
function padLeft(value: string, padding: string | number) {
  if (isNumber(padding)) {
    return Array(padding + 1).join(" ") + value;
  }
  if (isString(padding)) {
    return padding + value;
  }
  throw new Error(`Expected string or number, got '${padding}'.`);
}
```

However, having to define a function to figure out if a type is a primitive is kind of a pain. Luckily, you don’t need to abstract typeof `x === "number"` into its own function because TypeScript will recognize it as a type guard on its own. That means we could just write these checks inline.

```ts
function padLeft(value: string, padding: string | number) {
  if (typeof padding === "number") {
    return Array(padding + 1).join(" ") + value;
  }
  if (typeof padding === "string") {
    return padding + value;
  }
  throw new Error(`Expected string or number, got '${padding}'.`);
}
```

These typeof type guards are recognized in two different forms: typeof `v === "typename"` and typeof `v !== "typename"`, where "typename" can be one of `typeof operator’s return values` (`"undefined"`, `"number"`, `"string"`, `"boolean"`, `"bigint"`, `"symbol"`, `"object"`, or `"function"`). While `TypeScript` won’t stop you from comparing to other `strings`, the language won’t recognize those expressions as `type guards`.

## `instanceof` type guards

If you’ve read about `typeof` type guards and are familiar with the `instanceof` operator in JavaScript, you probably have some idea of what this section is about.

instanceof `type guards` are a way of narrowing types using their constructor function. For instance, let’s borrow our industrial strength string-padder example from earlier:

```ts
interface Padder {
  getPaddingString(): string;
}
 
class SpaceRepeatingPadder implements Padder {
  constructor(private numSpaces: number) {}
  getPaddingString() {
    return Array(this.numSpaces + 1).join(" ");
  }
}
 
class StringPadder implements Padder {
  constructor(private value: string) {}
  getPaddingString() {
    return this.value;
  }
}
 
function getRandomPadder() {
  return Math.random() < 0.5
    ? new SpaceRepeatingPadder(4)
    : new StringPadder("  ");
}
 
let padder: Padder = getRandomPadder();
      
let padder: Padder
 
if (padder instanceof SpaceRepeatingPadder) {
  padder;
    
let padder: SpaceRepeatingPadder
}
if (padder instanceof StringPadder) {
  padder;
    
let padder: StringPadder
}
```

The right side of the `instanceof` needs to be a constructor function, and TypeScript will narrow down to:

the type of the function’s `prototype` property if its type is not `any`
the union of types returned by that type’s construct signatures
in that order.

## Nullable types

TypeScript has two special types, `null` and undefined, that have the values null and undefined respectively. I mentioned these briefly in the [Types section](./types.md).

By default, the type checker considers `null` and `undefined` assignable to anything. Effectively, `null` and `undefined` are valid values of every type. That means it’s not possible to stop them from being assigned to any type, even when you would like to prevent it. The inventor of null, Tony Hoare, calls this his “`billion dollar mistake`”.

The `[strictNullChecks](./AdvancedTypes.md)` flag fixes this: when you declare a variable, it doesn’t automatically include null or undefined. You can include them explicitly using a union type:

```ts
let exampleString = "foo";
exampleString = null;
Type 'null' is not assignable to type 'string'.
 
let stringOrNull: string | null = "bar";
stringOrNull = null;
 
stringOrNull = undefined;
Type 'undefined' is not assignable to type 'string | null'
```

Note that `TypeScript` treats `null` and `undefined` differently in order to match `JavaScript` semantics. `string` | `null` is a different type than `string` | `undefined` and `string` | `undefined` | `null`.

From `TypeScript 3.7` and onwards, you can use optional chaining to simplify working with `nullable` types.

### Optional parameters and properties
With `strictNullChecks`, an optional parameter automatically adds | `undefined`:

```ts
function f(x: number, y?: number) {
  return x + (y ?? 0);
}
 
f(1, 2);
f(1);
f(1, undefined);
f(1, null);

// Argument of type 'null' is not assignable to parameter of type 'number | undefined'.
```

The same is true for optional properties:

```ts
class C {
  a: number;
  b?: number;
}
 
let c = new C();
 
c.a = 12;
c.a = undefined;
Type 'undefined' is not assignable to type 'number'.
c.b = 13;
c.b = undefined;
c.b = null;
Type 'null' is not assignable to type 'number | undefined'.
```

### Type guards and type assertions

Since nullable types are implemented with a union, you need to use a type guard to get rid of the `null`. Fortunately, this is the same code you’d write in JavaScript:

```ts
function f(stringOrNull: string | null): string {
  if (stringOrNull === null) {
    return "default";
  } else {
    return stringOrNull;
  }
}
```

The `null` elimination is pretty obvious here, but you can use terser operators too:

```ts
function f(stringOrNull: string | null): string {
  return stringOrNull ?? "default";
}
```

In cases where the compiler can’t eliminate `null` or `undefined`, you can use the type assertion operator to manually remove them. The syntax is postfix !: `identifier`! removes `null` and undefined from the type of `identifier`:

```ts
interface UserAccount {
  id: number;
  email?: string;
}
 
const user = getUser("admin");
user.id;
Object is possibly 'undefined'.
 
if (user) {
  user.email.length;
Object is possibly 'undefined'.
}
 
// Instead if you are sure that these objects or fields exist, the
// postfix ! lets you short circuit the nullability
user!.email!.length;
```

## Type Aliases

Type aliases create a new name for a type. Type aliases are sometimes similar to interfaces, but can name primitives, unions, tuples, and any other types that you’d otherwise have to write by hand.

```ts
type Second = number;
 
let timeInSecond: number = 10;
let time: Second = 10;
```

Aliasing doesn’t actually create a new type - it creates a new `name` to refer to that type. Aliasing a `primitive` is not terribly useful, though it can be used as a form of documentation.

Just like `interfaces`, `type aliases` can also be `generic` - we can just add type parameters and use them on the right side of the alias declaration:

```ts
type Container<T> = { value: T };
```

We can also have a type alias refer to itself in a property:

```ts
type Tree<T> = {
  value: T;
  left?: Tree<T>;
  right?: Tree<T>;
};
```

Together with intersection types, we can make some pretty mind-bending types:

```ts
type LinkedList<Type> = Type & { next: LinkedList<Type> };
 
interface Person {
  name: string;
}
 
let people = getDriversLicenseQueue();
people.name;
people.next.name;
people.next.next.name;
people.next.next.next.name;
        
(property) next: LinkedList<Person>
```