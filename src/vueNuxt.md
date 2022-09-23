# Vue.js and Nuxt 3, Notion API

First of all, I know it is not always possible to just switch over from Nuxt 2 to Nuxt 3. But there are some steps to take to at least work towards that direction. The first most important step is using the Nuxt Bridge.

## What does the nuxt bridge give you

- New CLi
- The composition API (Nuxi)
- Nuxt 3 modules compatible with Nuxt and Vue 2

## What can I do with Nuxi

- Create a new application
- Add Composible
- Add Component

## Directory Structure

### Build directory

By default, many tools assume that `.nuxt` is a hidden directory, because its name starts with a dot. You can use the `buildDir` option to prevent that. If you do change the name remember to add the new name to your `.gitignore` file.

To update the build directory edit the 

## Create new Nuxt 3 App

- Using NPX

```sh
npx nuxi init appName
cd appName

# Install Dependencies
npm install
```

## Working work .env and Runtime Config

### .env file and key value pairs

- Create a .env file and add some `key` - `value` pairs

```env
API_KEY="super secret key"
```

### RuntimeConfig in nuxt.config.tx file

- Edit `./nuxt.config.ts` and add a `runtimeConfig` object. Inside the object you can reference your .env `key` - `value` pairs with `process.env.KEY`

```ts
// nuxt.config.ts
runtimeConfig: {
    apiKey : process.env.API_KEY,
    public : {
        pubKey : "No such a big secret"
    }

}
```

- The values specified within the `public` object will be exposed to the fontend.
- To reference the `env` variables in your project you can use the following methods

### Referencing Config Variables in Pages

Create `./pages/index.vue` file and add the following

```vue
<template>
    <h1>Public and Private runtimeConfig Variables</h1>

    <h2>Notice the Private key not showing on the frontend</h2>
    <h3>Private key</h3>
    <p>
        API Key: {{ config.apiKey ? config.apiKey : "It's a secret" }}  
    </p>
    <h3>Public Key</h3>
    <p>
        Pub Key: {{ config.public.pubKey }}
    </p>
</template>
<script setup>
    const config = useRuntimeConfig();

    console.log(config.apiKey);
    console.log(config.public.pubKey);
</script>
```

### Referencing Config Variables in Composables

- Create the following file: `composables/useConfigVariables.ts` and add the following content

```ts
export const useConfigVariables = () => {
  const config = useRuntimeConfig();

  // Notice how server side both the pubic and private keys are logged but on the client side only the public key is logged.
  console.log("Runtime Config in Composables: ",config);
}
```

- Then use the composable inside a page so that it is being used
- Open `index.vue` and add the following:

```vue
# index.vue

<script setup>
  const configComposable = useConfigVariables();
<script/>
```

### Referencing Config Variables in Middleware

- Route middleware is called every time a route is accessed. 
- Create `./middleware/auth.ts` and add the following

```ts
export default defineNuxtRouteMiddleware((to, from) {
  const config = useRuntimeConfig();
  // Notice how server side both the pubic and private keys are logged but on the client side only the public key is logged.
  
  console.log("Runtime Config in Route (auth) Middleware",config);
```

### Referencing Config Variables in a Plugin

All plugins run on startup, in the first plugin below, we are adding route middleware. So rge plugin gets called on startup, the we add route middleware, and routeMiddleware gets called every time a route is changed. So a plugin that only run on startup can instantiate middleware that run on each route

- First create `globalMiddleware` plugin
- Here we can create a some `globalMiddleware`
- Create file `./plugins/globalMiddleware.ts` and add the following

```ts
export default defineNuxtPlugin(() => {
  const config = useRuntimeConfig();
  
  addRouteMiddleware(
    "global",
    () => {
      console.log("global middleware was added in a plugin and will be run on every route change");
      
      // Notice how server side both the pubic and private keys are logged but on the client side only the public key is logged.
      console.log("Runtime Config in global Middleware",config);
    },
    { global: true }
  );
});
```

- We can also create `runtimeConfig` plugin that will run in `startup`
- Create `./plugins/runtimeConfig.ts` file and add the following

```ts
export default
defineNuxtPlugin((nuxtApp) => {
  const config = useRuntimeConfig();
  // Notice how server side both the pubic and private keys are logged but on the client side only the public key is logged.
  console.log("Runtime Config in Plugin",config);
});
```

### Referencing Config Variables on the server

- All files in the `./server` directory will be executed on the server.
- Create file `./server/api/apisecret.get.ts` and add the following
- In this example we send the apiKey to the API in the headers and see if the apiKey we send matches the one in the runtimeConfig, and in this way we are doing some kind of authentication

```ts
export default defineEventHandler((event) => {
  const config = useRuntimeConfig() ;
  const headers = event.req.headers;
  
  console. log("Header Authorization: ",headers.authorization);
  if (headers.authorization = config.apiSecret) {
    return "You are authorized";
  } else {
    return "Access denied!";
  }
});
```

- There is also server middleware
- Every time an api endpoint is called the middleware wil run first
- So create file `./server/middleware/serverMiddleware.ts` and add the following

```ts
export default defineEventHand1er((event) => {
  const config = useRuntimeConfig();
  console.log("Config from Server Middleware:",config) ;
  event.req.headers.authorization = config.apiKey;
});
```

- So every time an endpoint is called we are adding the `apiKey` to the `authorization` header.
- Then the API endpoint is executed, so in this example the headers are read once again and compared to the same `apiKey`
- To access the API endpoint go to `http://localhost:3000/api/apisecret` and you should receive a response that says `You are authorized`

---

- For another example create file `./server/api/pubkey.get.ts` and add the following

```ts
export default defineEventHandler((event) => {
  const config = useRuntimeConfig();
  const headers = event.req.headers;
  
  console.log(
    "Header Authorization (PUBLIC KEY):",
    headers.pubKey,
    config.public.pubKey
  ); 

  if (headers.pubKey  == config.public.pubKey) {
    return "You are authorized";
  } else {
    return "Access denied!"
  }
});
```

- We will call the `pubkey` end point from the `index.vue` file to authenticate
- Open your `./pages/index.vue` file and add the following

```vue
<script setup>
---
// create a reactive variable
  const message = ref();

  onMounted(async() => {
    // notice the $ in front of fetch
    message.value = await $fetch("/api/pubkey", {
      headers: {
        pubKey: runtimeConfig.public.pubKey,
      }
    )
  })
</script>
```


## install tailwind v3 via npm

- Execute the following command

```sh
npm install -D tailwindcss postcss@latest autoprefixer@latest
npx tailwindcss init
```

- create a new file: `./assets/css/main.css` 
- Add the new file to the css array in the `nuxt.config.js` file.
- Here is the `assets/css/main.css`

```css
 /* assets/css/main.css */

@tailwind base;
@tailwind components;
@tailwind utilities;
```

- Next, you need to add `postcss` and `/assets/css/main.css` in `nuxt.config.ts`.

```ts
// nuxt.config.ts
// https://v3.nuxtjs.org/api/configuration/nuxt.config
export default defineNuxtConfig({
  build: {
    postcss: {
      postcssOptions: {
        plugins: {
          tailwindcss: {},
          autoprefixer: {},
        },
      },
    },
  },

  css: [
    '@/assets/css/main.css',
  ],
});
```

- Add the paths to all of your template files in your `tailwind.config.js` file.

```js
tailwind.config.js

module.exports = {
  content: [
    "./components/**/*.{js,vue,ts}",
    "./layouts/**/*.vue",
    "./pages/**/*.vue",
    "./plugins/**/*.{js,ts}",
    "./nuxt.config.{js,ts}",
    "./app.vue"
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

- Then finally use tailwind: `app.vue`

```vue
<template>
  <div class="flex justify-center items-center h-screen">
    <h1 class="text-3xl font-bold text-green-500">
      How to install Tailwind CSS 3 in Nuxt 3
    </h1>
  </div>
</template>
```

- Start your dev server

```sh
npm run dev
```