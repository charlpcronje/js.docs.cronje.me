---
title: Advanced Types | CRONje.ME
label: Advanced Types
order: 40
authors:
  - name: Charl Cronje
    email: charl@cronje.me
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
<script type="text/javascript">(function(w,s){var e=document.createElement("script");e.type="text/javascript";e.async=true;e.src="https://cdn.pagesense.io/js/webally/f2527eebee974243853bcd47b32631f4.js";var x=document.getElementsByTagName("script")[0];x.parentNode.insertBefore(e,x);})(window,"script");</script>

## Type Alias

```ts
type User = {
    readonly id: number,
    name: string,
    retire: (date: Date) => void
}
```

Now there is a new type called `User` that I can use for all `User` types

```ts
let user: User = {
    id: 1
    name: 'Shaun',
    retire: (date: Date) => {
        console.log(date)
    }    
}
```

## Union Types

```ts
// I declare a function that can take either a number or a sting for the weight argument,
// We call this union types
function kgToPounds(weight: number | string):number {
    // The code completion will only give suggestions of methods that is common to string and numbers, but byh using narrowing, you can get the fill code completion for either a number or a sting

    if (typeof weight === 'number') {
        return weight * 2.2;
    } else {
        return parseInt(weight) * 2.2;
    }
}
```

## Intersection Type

Instead of using the pipe `|` between the various types, use the `&` character to define a an intersection type. Here is an example of an intersection type

```ts
type Draggable = {
    drag: {} => void
};

type Resizeable = {
    resize: {} => void
}; 

// UIWidget is Draggable and Resizeable at the same time, making it an intersection type
type UIWidget = Draggable & Resizeable;
```

Now create an Element that implements UIWidget, it needs to implement both the methods, the `drag()` method and the `resize()` method

```ts
let textBox: UIWidget = {
    drag() => {},
    resize() => {}
}
```

## Literal Type

```ts
// The variable below can either have a value if 10 or 20
let qty: 10 | 20 = 20;
```

Or we can create a new Type that does the same

```ts
// Creating a new type 
type Qty = 10 | 20;

// Using the new literal type
let qty: Qry = 10;
```

## Nullable Types

```ts
function greet(name: string) {
    console.log(name.toUpperCase());
}

// This will give an error because in the function it is going to try and do a `toUpperCase()` on the `null`. So the compiler will moan about this, and won't allow this to happen.
greet(null);
```

There is a `TypeScript` setting that you can set to `false` to, so that it won't give and error

```json
"strictNullChecks": false
```

- But it is not a good idea to ever turn off that check.
- The best thing to do here is to make use of a union type again...

```ts
function greet(name: string | null) {
    console.log(name.toUpperCase());
}

greet(null);
```

- And if you want to be able to pass `undefined` as well...

```ts
function greet(name: string | null | undefined) {
    console.log(name.toUpperCase());
}

greet(undefined);
```

## Optional Chaining

It will happen that you will often do null checks

```ts
function getCustomer(id: number): Customer | nul | undefined {
    return id === 0 ? null : { birthday: new Date()};
}

let customer = getCustomer(0);
// Then I must perform all the checks below, before I can check for the birthday, otherwise it might happen that I try to get the birthday of `null` or `undefined`
if (customer !== null && customer !== undefined) {
    console.log(customer.birthday);
}
```

### Optional Property Access Operator

There is an easier way to do all of those checks by using the `optional property access operator`

```ts
// the question mark `?` before the `.birthday` is called the  `optional property access operator`, the the code below will only be executed of customer is not null or undefined
console.log(customer?.birthday);
```

Now lets get the year of the Birthday

```ts
console.log(customer?.birthday?.getFullYear());
```

### Optional Element Access Operator

```ts
// So to make sure the customer is not null or undefined, you need to do the following checks
if (customers !== null &&  customers !== undefined) {
    customers[0];
}
```

- Or you can do the same checks by using the `Optional Element Access Operator`

```ts
    customers?.[0];
```

Above the `?.` is the `Optional Element Access Operator`

### Optional call Operator

```ts
let log: any = {message: string} => console.log(message);

// Or it could be declared like 

let log: any = null;

// So if I would not try and do the following
log('a');
// it will crash because log is null
```

So to get past this you can use the `Optional call Operator`

```ts
log?.('a')l
```

The `?.` is the `Optional call Operator`. And the log function will only be called if it is an actual function











