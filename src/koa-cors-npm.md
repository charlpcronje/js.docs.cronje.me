---
created: 2022-09-13T06:16:37 (UTC +02:00)
tags: [cors,koa,koajs]
source: https://www.npmjs.com/package/koa-cors
author:  Charl Cronje
---

# Koa CORS - NPM

> ## Excerpt
> CORS middleware for Koa. Latest version: 0.0.16, last published: 7 years ago. Start using koa-cors in your project by running `npm i koa-cors`. There are 253 other projects in the npm registry using koa-cors.

---
# Koa CORS

CORS middleware for Koa

Inspired by the great [node-cors](https://github.com/troygoode/node-cors) module.

## [](https://www.npmjs.com/package/koa-cors#installation-via-npm-)Installation (via [npm](https://npmjs.org/package/koa-cors))

```
$ npm install koa-cors
```

## [](https://www.npmjs.com/package/koa-cors#usage)Usage

```
var koa = require('koa');var route = require('koa-route');var cors = require('koa-cors');var app = koa(); app.use(cors()); app.use(route.get('/', function() {  this.body = { msg: 'Hello World!' };})); app.listen(3000);
```

## [](https://www.npmjs.com/package/koa-cors#options)Options

### [](https://www.npmjs.com/package/koa-cors#origin)origin

Configures the **Access-Control-Allow-Origin** CORS header. Expects a string (ex: [http://example.com](http://example.com)). Set to `true` to reflect the [request origin](http://tools.ietf.org/html/draft-abarth-origin-09), as defined by `req.header('Origin')`. Set to `false` to disable CORS. Can also be set to a function, which takes the request as the first parameter.

### [](https://www.npmjs.com/package/koa-cors#expose)expose

Configures the **Access-Control-Expose-Headers** CORS header. Expects a comma-delimited string (ex: 'WWW-Authenticate,Server-Authorization') or an array (ex: `['WWW-Authenticate', 'Server-Authorization]`). Set this to pass the header, otherwise it is omitted.

### [](https://www.npmjs.com/package/koa-cors#maxage)maxAge

Configures the **Access-Control-Max-Age** CORS header. Set to an integer to pass the header, otherwise it is omitted.

### [](https://www.npmjs.com/package/koa-cors#credentials)credentials

Configures the **Access-Control-Allow-Credentials** CORS header. Set to `true` to pass the header, otherwise it is omitted.

### [](https://www.npmjs.com/package/koa-cors#methods)methods

Configures the **Access-Control-Allow-Methods** CORS header. Expects a comma-delimited string (ex: 'GET,PUT,POST') or an array (ex: `['GET', 'PUT', 'POST']`).

### [](https://www.npmjs.com/package/koa-cors#headers)headers

Configures the **Access-Control-Allow-Headers** CORS header. Expects a comma-delimited string (ex: 'Content-Type,Authorization') or an array (ex: `['Content-Type', 'Authorization]`). If not specified, defaults to reflecting the headers specified in the request's **Access-Control-Request-Headers** header.

For details on the effect of each CORS header, [read this article on HTML5 Rocks](http://www.html5rocks.com/en/tutorials/cors/).
