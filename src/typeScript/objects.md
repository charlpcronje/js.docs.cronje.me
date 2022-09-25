# TypeScript Objects

Lets declare a `user` object, in JavaScript the object properties are dynamic, so you can add properties after declaring the object, at a later stage.

But in TypeScript you must declare you object with all of it's properties at the beginning 

```ts
// User Object
const user = {
    id: 1,
    name: "Shaun"
}
```

### To be more concise, the same object should be declared like this:

```ts
const user = {
    id: number,
    name: string
} = {
    id: 1,
    name: "Shaun"
}
```

## But you must define both fields...

```ts
// This will produce an error because both params must be set
const user = {
    id: number,
    name: string
} = {
    id: 1
}
```

## You can make the param optional

```ts
// This will produce an error because both params must be set
const user = {
    id: number,
    name?: string
} = {
    id: 1
}
```

### Readonly property

This will prevent us from accidently changing a propery that we shouldn't change

```ts
const user = {
    readonly id: number,
    name?: string
} = {
    id: 1,
    name: 'Charl'
}
```

### Add return method to object

```ts
const user : {
    readonly id: number,
    name?: string,
    retire: (date: Date) => void
} = {
    id: 1,
    name: 'Charl',
    retire: (date: Date) => {
        console.log(date);
    }
};
```



// But the following will not be accepted
const user = {
    id: 1
}
user.name = "Shaun";  //  This will produce an error
```



