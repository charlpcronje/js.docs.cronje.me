---
title: TypeScript Functions | CRONje.ME
label: TypeScript Functions
order: 60
authors:
  - name: Charl Cronje
    email: charl@cronje.me
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
<script type="text/javascript">(function(w,s){var e=document.createElement("script");e.type="text/javascript";e.async=true;e.src="https://cdn.pagesense.io/js/webally/f2527eebee974243853bcd47b32631f4.js";var x=document.getElementsByTagName("script")[0];x.parentNode.insertBefore(e,x);})(window,"script");</script>

`TypeScript` is a bit more strict about how you declare functions and what params you send the function and what it returns

Here is a function that does some kind of calculation

## Unused Parameters

```ts
function calc(income: number): number {
    return 0;
}
```

The above `function` has an unused `param`, I don't like to have any unused `params` so I like to change the following setting in `jsconfig.json`

```json
"noUnusedParameters": true,
```

## Implicit Returns

Also switch on the following settings:

```json
    "noImplicitReturns": true,      // When the function does not always return a value
    "noUnusedLocals": true          // when there are local variables declared inside a function but they are never used
```

## Default values for parameters

```ts
function calc(income = 0): number {
    return income * 2;
}
```

## Optional value for parameters

Below the tax param is optional because of the `?` before the `:` at the second param

```ts
function calc(income = 0, tax?: number): number {
    tax = tax || 14;
    return income * tax;
}
```


