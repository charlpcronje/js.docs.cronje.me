# Getting Started with `Flow Types`

## Installing

Have a look at teh different ways installing `Flow` by looking at their getting started section, found here: [https://flow.org/en/docs/install/](https://flow.org/en/docs/install/)

- You can choose if you want to use NPM or Yarn, and if you want to use `Babel` or `FLow-Remove-Types` to compile you code

### Setup Compiler

First you’ll need to setup a compiler to strip away Flow types. You can choose between [Babel](http://babeljs.io/) and [flow-remove-types](https://github.com/facebook/flow/tree/master/packages/flow-remove-types).

[flow-remove-types](https://github.com/facebook/flow/tree/master/packages/flow-remove-types) is a small CLI tool for stripping Flow type annotations from files. It’s a lighter-weight alternative to Babel for projects that don’t need everything Babel provides.


### Installation | Flow

> Installing and setting up Flow for a project

First install `flow-remove-types` with either [Yarn](https://yarnpkg.com/) or [npm](https://www.npmjs.com/).


### Add dev dependency

```sh
yarn add --dev flow-remove-types
```

### Setting up Flow for your project

If you then put all your source files in a `src` directory you can compile them to another directory by running:

```sh
yarn run flow-remove-types src/ -- -d lib/
```

You can add this to your `package.json` scripts easily.

```json
{
  "name": "my-project",
  "main": "lib/index.js",
  "scripts": {
    "build": "flow-remove-types src/ -d lib/",
    "prepublish": "yarn run build"
  }
}
```

> **Note:** You’ll probably want to add a `prepublish` script that runs this transform as well, so that it runs before you publish your code to the npm registry.

### Setup Flow

Flow works best when installed per-project with explicit versioning rather than globally.

Luckily, if you’re already familiar with `npm` or `yarn`, this process should be pretty familiar!

**Add a `devDependency` on the `flow-bin` npm package:**

```sh
yarn add --dev flow-bin
```

**Run Flow:**

```sh
yarn run flow
```

```sh
yarn run v0.15.1
$ flow
No errors!
✨  Done in 0.17s.
```
