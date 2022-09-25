# Debug TypeScript

To debug your TypeScript files you will need a way to map the code being generated back to the `TypeScript` file. To do that, edit the following setting in `tsconfig.json`

```json
"sourceMap": true
```

Now to make the `index.ts` file more debug-able, I will add some more logic, so open `./src/index.ts` and the following code:

```ts
let age: number = 20;

if (age < 50>)
  age += 10;
```

- To start debugging, click on the `Run and Debug` button to the left of the file explorer or press `Ctrl+Shift+D` 
- Then click on the line's number to and add a breakpoint
- Then Click on `Create launch.json` amd select `Node.js`

That will create a `launch.json` file that will contain the following

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
      {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        "program": "${workspaceFolder}/src/workspace.ts",
        "outFiles": ["${workspaceFolder}/**/*.js"]
      }
    ]
  }
```

Update the `program line and add a  preLaunchTask` line, so that the file looks like this:

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
      {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        "program": "${workspaceFolder}/src/index.ts",
        "preLaunchTask": "tsc: build - tsconfig.json",
        "outFiles": ["${workspaceFolder}/**/*.js"]
      }
    ]
}
  
```

- Now the debugger knows what config is being used and at the debug panel on the left top right there will be a dropdown, and one of the options will be `Launch Program`. like we Specified in the `launch.json` at the `name` setting.
- Now when you press `F5` the debugger will run, just make sure you have a breakpoint somewhere to make the debugger stop, otherwise you might not even notice that it ran through the code.
