# TypeScript Types

## Built in types

### `JavaScript` has the following `types`:

- `number`
- `string`
- `boolean`
- `null`
- `undefined`
- `object`

### TypeScript adds the following built in types

- `any`
- `unknown`
- `never`
- `enum`
- `tuple`

## large numbers in TypeScript

A large number can of course be declared as normal

```ts
let sales: number = 123456789;
```

But it can also be declared as

```ts
// This is purely for readability
let sales: number = 123_456_789;
```

## String variable declaration

```ts
let course: string = 'TypeScript';
```

## Inferring Types

But `TypeScript` can figure this out by itself if you initialize a variable with a value. So it is not necessary to specify that the `course` is a `string`, you can simply declare it like this:

```ts
let course = 'TypeScript';
```

The same with a `number`:

```ts
let sales = 123_456_789;
```

And the same with a `boolean`

```ts
let isPublished: boolean = true;

// Inferring by adding a start value for the variable
let isPublished = true;
```

## The `any` type

If a variable is declared like this:

```ts
let lvl;
```

then the `any` type is inferred because the variable was not instantiated with a value.

### Implicit `any` types

When you declare a function like this

```ts
function render(doc) {
    console.log(doc);
}
```

The compiler will complain because the `doc` variable given the `any` type implicitly. To fix this error you can add the following to the the function

```ts
function render(doc: any) {
    console.log(doc);
}
```

But it may happen that a library has hundreds of these problems and you don't want to go and update each function. In this case you can update the `tsconfig.json` and update the `noImplicitAny` setting to `false`. That will stop the compiler from complaining, but that also defeats the purpose of `TypeScript` so changing this setting should be avoided if at all possible

## The `array` type

- In `JavaScript` you can have an `array` like this

```js
let things = [1, 'John', 2];
```

- In `JavaScript` this is fine because the `arrays` are dynamic, but in TypeScript `arrays` have a `type`. 
- So in `TypeScript` you can declare and `array` like this:

```ts
let numbers: number[] = [1,2,3];
```

- Just like when declaring any other type in `TypeScript` the `type` can be inferred when a `value` is given when declaring the `array`

```ts
let numbers = [1,2,3];
```

### `any` type `array`

- But if you declared it with an empty `array`

```ts
let numbers = [];
```

- Then this is an `any array`, that should also be avoided.

## `tuple` Type

Basic `tuple` declaration

So a `tuple` is to declare a specific `collection` of `element`. In this case it is an `element` that is of type `number` and `string`, and is declared like this:

```ts
let user: [number, string] = [1, 'Shaun'];
```

In JavaScript a `tuple` is declared as just a normal `array`.

```js
let user = [1,'Shaun`];
```

- So to better describe a `tuple` in `TypeScript `: It is a set length `array` where every value has a specific `type`.
- Referencing a `tuple` in `TypeScript` looks like this:

```ts
let user: [number, string] = [1, 'Shaun'];

console.log(user[1]) 
// Output: Shaun
```

### You can also declare an `array` of `tuples`

```ts
var users: [number, string][];
users = [[1, "Steve"], [2, "Bill"], [3, "Jeff"]];
```

### Accessing Tuple Elements

```ts
users[0]; // returns 1
users[1]; // returns "Steve"
```


### Add Elements into Tuple

```ts
var user: [number, string] = [1, "Steve"];
user.push(2, "Bill"); 
console.log(user); //Output: [1, 'Steve', 2, 'Bill']
```

### Array methods on `Tuples` like `pop()`, `concat()` etc.

```ts
let user: [number, string] = [1, "Steve"];

// retrieving value by index and performing an operation 
user[1] = user[1].concat(" Jobs"); 
console.log(user); //Output: [1, 'Steve Jobs']
```


---
created: 2022-09-24T09:51:08 (UTC +02:00)
tags: [enum,typescript,typescript tutorials,free typescript tutorials,online typescript tutorial,Free typescript Tutorials for beginners,learn typescript,learn nodejs,learn,typescript for beginners,typescript tutorials,typescript Basics,typescript step by step,learn typescript from scratch,what is typescript]
source: https://www.tutorialsteacher.com/typescript/typescript-enum
author: 
---

## `Enum` TypeScript `Type` 

`Enums` or `enumerations` are a new data type supported in `TypeScript`. Most object-oriented languages like `Java` and `C#` use `enums`. This is now available in `TypeScript` too.

In simple words, `enums` allow us to declare a set of named constants i.e. a collection of related `values` that can be numeric or string values.

There are three types of enums:

1.  `Numeric` enum
2.  `String` enum
3.  `Heterogeneous` enum

## Numeric Enum

`Numeric` `enums` are number-based `enums` i.e. they store string values as numbers.

`Enums` can be defined using the keyword `enum`. Let's say we want to store a set of print media types. The corresponding `enum` in `TypeScript` would be:

### Example: Numeric Enum

```ts
enum PrintMedia {
  Newspaper,
  Newsletter,
  Magazine,
  Book
}
```

In the above example, we have an enum named PrintMedia. The enum has four values: Newspaper, Newsletter, Magazine, and Book. Here, enum values start from zero and increment by 1 for each member. It would be represented as:

```ts
Newspaper = 0
Newsletter = 1
Magazine = 2
Book = 3
```

Enums are always assigned numeric values when they are stored. The first value always takes the numeric value of 0, while the other values in the enum are incremented by 1.

We also have the option to initialize the first numeric value ourselves. For example, we can write the same enum as:

```ts
enum PrintMedia {
  Newspaper = 1,
  Newsletter,
  Magazine,
  Book
}
```

The first member, Newspaper, is initialized with the numeric value 1. The remaining members will be incremented by 1 from the numeric value of the first value. Thus, in the above example, Newsletter would be 2, Magazine would be 3 and Book would be 4.

It is not necessary to assign sequential values to Enum members. They can have any values.

```ts
enum PrintMedia {
    Newspaper = 1,
    Newsletter = 5,
    Magazine = 5,
    Book = 10
}
```

The enum can be used as a function parameter or return type, as shown below:

### Example: Enum as Return Type

```ts
enum PrintMedia {
    Newspaper = 1,
    Newsletter,
    Magazine,
    Book
}

function getMedia(mediaName: string): PrintMedia {
    if (  mediaName === 'Forbes' || mediaName === 'Outlook') {
        return PrintMedia.Magazine;
    }
 }

let mediaType: PrintMedia = getMedia('Forbes'); // returns Magazine
```

In the above example, we declared an enum `PrintMedia`. Next, we declare a function `getMedia()` that takes in an input parameter `mediaName` of the type string. This function returns an enum `PrintMedia`. In the function, we check for the type of media. If the media name matches 'Forbes' or 'Outlook', we return enum member `PrintMedia.Magazine`.

### Computed Enums:

Numeric enums can include members with computed numeric value. The value of an enum member can be either a constant or computed. The following enum includes members with computed values.

### Example: Computed Enum

```ts
enum PrintMedia {
    Newspaper = 1,
    Newsletter = getPrintMediaCode('newsletter'),
    Magazine = Newsletter * 3,
    Book = 10
}

function getPrintMediaCode(mediaName: string): number {
    if (mediaName === 'newsletter') {
        return 5;
    }
}

PrintMedia.Newsetter; // returns 5
PrintMedia.Magazine; // returns 15
```

When the enum includes computed and constant members, then uninitiated enum members either must come first or must come after other initialized members with numeric constants. The following will give an error.

```ts
enum PrintMedia {
    Newsletter = getPrintMediaCode('newsletter'),
    Newspaper, // Error: Enum member must have initializer
    Book,
    Magazine = Newsletter * 3,
}
```

The above enum can be declared as below.

```ts
enum PrintMedia {
    Newspaper,
    Book,
    Newsletter = getPrintMediaCode('newsletter'),
    Magazine = Newsletter * 3
}
// or
enum PrintMedia {
    Newsletter = getPrintMediaCode('newsletter'),
    Magazine = Newsletter * 3,
    Newspaper = 0,
    Book,
}
```

### String Enum

String enums are similar to numeric enums, except that the enum values are initialized with string values rather than numeric values.

The benefits of using string enums is that string enums offer better readability. If we were to debug a program, it is easier to read string values rather than numeric values.

Consider the same example of a numeric enum, but represented as a string enum:

### Example: String Enum

```ts
enum PrintMedia {
    Newspaper = "NEWSPAPER",
    Newsletter = "NEWSLETTER",
    Magazine = "MAGAZINE",
    Book = "BOOK"
}
// Access String Enum 
PrintMedia.Newspaper; //returns NEWSPAPER
PrintMedia['Magazine'];//returns MAGAZINE
```

In the above example, we have defined a string enum, PrintMedia, with the same values as the numeric enum above, with the difference that these enum values are initialized with string literals. The difference between numeric and string enums is that numeric enum values are auto-incremented, while string enum values need to be individually initialized.

### Heterogeneous Enum

Heterogeneous enums are enums that contain both string and numeric values.

### Example: Heterogeneous Enum

```ts
enum Status { 
    Active = 'ACTIVE', 
    Deactivate = 1, 
    Pending
}
```

### Reverse Mapping

Enum in TypeScript supports reverse mapping. It means we can access the value of a member and also a member name from its value. Consider the following example.

### Example: Reverse Mapping

```ts
enum PrintMedia {
  Newspaper = 1,
  Newsletter,
  Magazine,
  Book
}

PrintMedia.Magazine;   // returns  3
PrintMedia["Magazine"];// returns  3
PrintMedia[3];         // returns  Magazine
```

As you can see in the above example, `PrintMedia[3]` returns its member name "Magazine". This is because of reverse mapping. Let's see how TypeScript implements reverse mapping using the following example.

```ts
enum PrintMedia {
  Newspaper = 1,
  Newsletter,
  Magazine,
  Book
}
console.log(PrintMedia)
```

The above example gives the following output in the browser console.

```ts
{
  '1': 'Newspaper',
  '2': 'Newsletter',
  '3': 'Magazine',
  '4': 'Book',
  Newspaper: 1,
  Newsletter: 2,
  Magazine: 3,
  Book: 4 
}

```

You will see that each value of the enum appears twice in the internally stored enum object. We know that num values can be retrieved using the corresponding enum member value. But it is also true that enum members can be retrieved using their values. This is called reverse mapping.

TypeScript can compile the above enum into the following JavaScript function.

### Example: Compiled JavaScript of Enum

```ts
var PrintMedia;
(function (PrintMedia) {
    PrintMedia[PrintMedia["Newspaper"] = 1] = "Newspaper";
    PrintMedia[PrintMedia["Newsletter"] = 2] = "Newsletter";
    PrintMedia[PrintMedia["Magazine"] = 3] = "Magazine";
    PrintMedia[PrintMedia["Book"] = 4] = "Book";
})(PrintMedia || (PrintMedia = {}));
```

`PrintMedia` is an object in JavaScript which includes both value and name as properties and that's why enum in TypeScript supports reverse mapping.

So, both the following mappings are true to enums: `name` -> `value`, and `value` -> `name`.
 

 ---
created: 2022-09-24T09:56:12 (UTC +02:00)
tags: [union,union type,typescript,typescript tutorials,free typescript tutorials,online typescript tutorial,Free typescript Tutorials for beginners,learn typescript,learn nodejs,learn,typescript for beginners,typescript tutorials,typescript Basics,typescript step by step,learn typescript from scratch,what is typescript]
source: https://www.tutorialsteacher.com/typescript/typescript-union
author: 
---

# Union Type in TypeScript

> ## Excerpt
> Learn about union type in TypeScript. TypeScript allows us to use more than one data type for a variable or a function parameter. This is called union type.

---
## `Union` Type

TypeScript allows us to use more than one data type for a variable or a function parameter. This is called union type.

Syntax:

```ts
(type1 | type2 | type3 | .. | typeN)

```

Consider the following example of union type.

### Example: Union

```ts
let code: (string | number);
code = 123;   // OK
code = "ABC"; // OK
code = false; // Compiler Error

let empId: string | number;
empId = 111; // OK
empId = "E111"; // OK
empId = true; // Compiler Error
```

In the above example, variable `code` is of union type, denoted using `(string | number)`. So, you can assign a string or a number to it.

The function parameter can also be of union type, as shown below.

Example: Function Parameter as `Union` Type

```ts
function displayType(code: (string | number)) {
    if(typeof(code) === "number")
        console.log('Code is number.')
    else if(typeof(code) === "string")
        console.log('Code is string.')
}

displayType(123); // Output: Code is number.
displayType("ABC"); // Output: Code is string.
displayType(true); //Compiler Error: Argument of type 'true' is not assignable to a parameter of type string | number
```

In the above example, parameter `code` is of union type. So, you can pass either a string value or a number value. If you pass any other type of value e.g. boolean, then the compiler will give an error.


---
created: 2022-09-24T09:57:31 (UTC +02:00)
tags: [void,typescript,typescript tutorials,free typescript tutorial,online typescript tutorials,typescript for beginners,learn typescript,learn,typescript tutorials,typescript Basics,typescript step by step,learn typescript from scratch,what is typescript]
source: https://www.tutorialsteacher.com/typescript/typescript-void
author: 
---

## `Void` TypeScript `Type`


Similar to languages like Java, void is used where there is no data. For example, if a function does not return any value then you can specify void as return type.

```ts
function sayHi(): void { 
    console.log('Hi!')
} 

let speech: void = sayHi(); 
console.log(speech); //Output: undefined
```

There is no meaning to assign void to a variable, as only null or undefined is assignable to void.

```ts
let nothing: void = undefined;
let num: void = 1; // Error
```

## TypeScript `never` Data Type

TypeScript introduced a new type `never`, which indicates the values that will never occur.

The never type is used when you are sure that something is never going to occur. For example, you write a function which will not return to its end point or always throws an exception.

### Example: never

```ts
function throwError(errorMsg: string): never { 
            throw new Error(errorMsg); 
} 

function keepProcessing(): never { 
            while (true) { 
         console.log('I always does something and never ends.')
     }
}
```

In the above example, the `throwError()` function throws an error and `keepProcessing()` function is always executing and never reaches an end point because the while loop never ends. Thus, never type is used to indicate the value that will never occur or return from a function.

### Difference between never and void

The void type can have undefined or null as a value where as never cannot have any value.

### Example: never vs void

```ts
let something: void = null;
let nothing: never = null; // Error: Type 'null' is not assignable to type 'never'
```

In TypeScript, a function that does not return a value, actually returns undefined. Consider the following example.

```ts
function sayHi(): void { 
    console.log('Hi!')
}

let speech: void = sayHi();
console.log(speech); // undefined
```

- As you can see in the above example, `speech` is undefined, because the `sayHi` function internally returns undefined even if return type is void. If you use never type, `speech:never` will give a compile time error, as void is not assignable to never.

## Type Assertion in TypeScript

Here, you will learn about how TypeScript infers and checks the type of a variable using some internal logic mechanism called Type Assertion.

Type assertion allows you to set the type of a value and tell the compiler not to infer it. This is when you, as a programmer, might have a better understanding of the type of a variable than what TypeScript can infer on its own. Such a situation can occur when you might be porting over code from JavaScript and you may know a more accurate type of the variable than what is currently assigned. It is similar to type casting in other languages like C# and Java. However, unlike C# and Java, there is no runtime effect of type assertion in TypeScript. It is merely a way to let the TypeScript compiler know the type of a variable.

### Example: Type Assertion

```ts
let code: any = 123; 
let employeeCode = <number> code; 
console.log(typeof(employeeCode)); //Output: number
```

In the above example, we have a variable `code` of type `any`. We assign the value of this variable to another variable called `employeeCode`. However, we know that code is of type number, even though it has been declared as 'any'. So, while assigning `code` to `employeeCode`, we have asserted that code is of type number in this case, and we are certain about it. Now, the type of `employeeCode` is number.

Similarly, we might have a situation where we have an object that has been declared without any properties yet.

### Example: Type Assertion with Object

```ts
let employee = { };
employee.name = "John"; //Compiler Error: Property 'name' does not exist on type '{}'
employee.code = 123; //Compiler Error: Property 'code' does not exist on type '{}'
```

The above example will give a compiler error, because the compiler assumes that the type of employee is {} with no properties. But, we can avoid this situation by using type assertion, as shown below.

### Example: Type Assertion with Object

```ts
interface Employee { 
    name: string; 
    code: number; 
} 

let employee = <Employee> { }; 
employee.name = "John"; // OK
employee.code = 123; // OK
```

In the above example, we created an interface `Employee` with the properties name and code. We then used this type assertion on employee. Interfaces are used to define the structure of variables. Learn more about this in [interface](https://www.tutorialsteacher.com/typescript/typescript-interface) chapter.

Be careful while using type assertion. The TypeScript compiler will autocomplete `Employee` properties, but it won't show any compile time error if you forgot to add the properties. For example:

```ts
interface Employee { 
    name: string; 
    code: number; 
} 

let employee = <Employee> { 
    // Compiler will provide autocomplete properties,
    but will not give an error if you forgot to add the properties
}; 
console.log(employee.name); // undefined; 
```

You can also use the JavaScript library in TypeScript for some existing functions.

```ts
let employeeCode = <number> myJSLib.GetEmployeeCode('Steve');
console.log(typeof(employeeCode)); // number
```

In the above example, we assume that `myJSLib` is a separate JavaScript library and we call its `GetEmployeeCode()` function. So, we set the type of return value as number because we know that it returns a number.

There are two ways to do type assertion in TypeScript:

**1.** Using the angular bracket <> syntax. So far in this section, we have used angular brackets to show type assertion.

```ts
let code: any = 123; 
let employeeCode = <number> code; 
```

However, there is another way to do type assertion, using the 'as' syntax.

**2.** Using as keyword

### Example: as syntax

```ts
let code: any = 123; 
let employeeCode = code as number;
```

Both the syntaxes are equivalent and we can use any of these `type` assertions syntaxes. However, while dealing with `JSX` in `TypeScript`, only the `as` syntax is allowed, because JSX is `embeddable` in XML like a syntax. And since XML uses angular brackets, it creates a conflict while using type assertions with angular brackets in `JSX`.


