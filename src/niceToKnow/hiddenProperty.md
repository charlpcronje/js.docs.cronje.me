# Hiding a property on an object

In any font-end web code we expose most of what we do and there is no real hiding of anything, but not being able to see some properties of an object when you console log the object is at least something we can do

Normally we create an object like this:

```js
const obj = {
    name: 'John',
    surname: 'Smith',
    idNumber: '0123120339323'
};
console.log(obj);

#output
{name: 'John', surname: 'Smith', idNumber: '0123120339323'}
```

So say for instance you want to not have the idNumber to show as part of the object when you `console.log(obj)` you can declare the object like this:

```js
const obj = {
    name: 'John',
    surname: 'Smith'
};
Object.defineProperty(obj,'secret',{
    enumerable: false,
    value: '0123120339323'
});
console.log(obj);

#output
{name: 'John', surname: 'Smith'}
```
