# TypeScript Generics

Okay to quickly show and example of a generic, I am doing to declare a number array

```ts
type NumArray = Array<number>
```

Here is a function where I explain, why to use generics

```ts
const last = (arr: Array<number>) => {
    // Then I will send back the last element in the array
    return arr[arr.length - 1];
} 

const l = last([1,2,3]);

// But the following will cause an error
const l2 = last(["a","b","c"]);
```

The function above is for retrieving the last element from an array. The function should be able to work on both strings and numbers. To it should not just take a number array and but also a string array. 

So for that we could for the following

```ts
const last = (arr: Array<any>) => {
    // Then I will send back the last element in the array
    return arr[arr.length - 1];
} 

// Both of these below will work because the function above now accepts `any`

const l = last([1,2,3]);
// But the following will cause an error
const l2 = last(["a","b","c"]);
```

But still above we are losing types. By using generics below we can keep the types


```ts
const last = <T>(arr: Array<T>) => {
    // Then I will send back the last element in the array
    return arr[arr.length - 1];
} 

// Both of these below will work because the function above now accepts `any`

const l = last([1,2,3]);
// But the following will cause an error
const l2 = last(["a","b","c"]);
```

The function above now takes either string or numbers or in effect `any` but we keep the type going to the function, `l` will be a `number` and `l2` will be a `string`
