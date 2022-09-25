---
title: Type predicates | CRONje.ME
label: Type predicates
order: 10
authors:
  - name: Charl Cronje
    email: charl@cronje.me
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
<script type="text/javascript">(function(w,s){var e=document.createElement("script");e.type="text/javascript";e.async=true;e.src="https://cdn.pagesense.io/js/webally/f2527eebee974243853bcd47b32631f4.js";var x=document.getElementsByTagName("script")[0];x.parentNode.insertBefore(e,x);})(window,"script");</script>

# 

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