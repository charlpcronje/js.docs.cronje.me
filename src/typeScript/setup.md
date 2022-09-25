# Setup TypeScript

TypeScript needs to be compiled by something, there are extensions for `VS Code` that does that, `Node.js` can compile `TypeScript` and other `JavaScript` runTimes like `Demo` or `Bun` does it natively without you having to install anything.

## Install TypeScript on `Node.js`

Make sure to install this package with the `-g` flag so that it is available globally.

```sh
npm i -g typescript
```

After the installation you can enter the following command in the terminal to get the version of the installed TypeScript compiler

```sh
# tsc stands for TypeScript Compiler

tsc -c
```

## Create first TypeScript project

Create a new folder for your project, in this case I will create `/var/www/ts/examples/1-start`

```sh
mkdir -p /var/www/ts/examples/1-start/
cd /var/www/ts/examples/1-start
code index.ts
```

### Create first `.ts` file and declare a variable

Now just to get us started, edit `index.ts` and add the following

```ts
let age: number = 20;
```

### Compile `.ts`  to `.js` file

Then to compile this code to JavaScipt, run the following command

```sh
tsc index.ts
```

This will output a file called `index.js`, with the following content

```js
var age = 20;
```

You will notice that it compiled to an old version of JavaScript (`ES5`). The default for TypeScript is to compile to `ES5`, but we can change this 

### Change version of `js` that `tsc` outputs

To achieve this we have to create a `TypeScript` config file

```sh
tsc --init
```

That would have created a file called `tsconfig.json` with some of the following the following content

```json
{
  "compilerOptions": {
    /* Language and Environment */
    "target": "es2017",                                  /* Set the JavaScript language version for emitted JavaScript and include compatible library declarations. */
   
    /* Modules */
    "module": "commonjs",                                /* Specify what module code is generated. */
    "rootDir": "./src",                                  /* Specify the root folder within your source files. */

    /* Emit */
    "outDir": "./dist",                                   /* Specify an output folder for all emitted files. */
    "removeComments": true,                           /* Disable emitting comments. */
    "noEmitOnError": true,                            /* Disable emitting files if any type checking errors are reported. */

    /* Interop Constraints */
    "esModuleInterop": true,                             /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */
    "forceConsistentCasingInFileNames": true,            /* Ensure that casing is correct in imports. */

    /* Type Checking */
    "strict": true,                                      /* Enable all strict type-checking options. */
    
    /* Completeness */
    "skipLibCheck": true                                 /* Skip type checking all .d.ts files. */
  }
}
```

I'm highlighting a few settings above...

- `target` from `es2015` to `es2017` to get more modern `js` output. The reason I am doing this even though it will effect the compatibility of of my JavaScript code, is because the newer versions of javascript have a few functions that executes a lot faster than some of the older ones and the newer code could be a lot less code, because of some new features of JavaScript, making it less code to download for the users.
- `rootDir` from `./` to `./src`
- `outDir` from `./` to `./dist`

By setting `rootDir` and the `outDir` makes it simpler to compile all the `.ts` files, now we only need to run the following:

```sh
tsc 
```

Instead of 

```sh
tsc index.ts
```

- `removeComments`. Will remove the comments to make the file smaller
- `noEmitOnError`. Will not create a JavaScript file if there were errors.














