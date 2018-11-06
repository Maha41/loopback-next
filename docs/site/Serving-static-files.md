---
lang: en
title: 'Serving static files in LoopBack 4'
keywords: LoopBack
sidebar: lb4_sidebar
permalink: /doc/en/lb4/Serving-static-files.html
---

One of the basic requirements of a web app is the ability to serve static files.
Serving static files from a LoopBack is very simple - just call the
`app.static(path, rootDir[, options])` method. The variables in the API are
explained below.

- `app`: An instance of a LoopBack application.
- `path`: The path where the static assets are to be served from. Refer to
  https://expressjs.com/en/4x/api.html#path-examples for possible values.
- `rootDir`: The directory where the static assets are located on the file
  system.
- `options`: An otional object for configuring the underlying
  [express.static](https://expressjs.com/en/4x/api.html#express.static).

Here is an example of configuring an app to serve the static assets from a
directory named `public` at `/`.

```ts
import {TodoListApplication} from './application';
import {ApplicationConfig} from '@loopback/core';
const path = require('path');

export async function main(options?: ApplicationConfig) {
  const app = new TodoListApplication(options);
  app.static('/', path.join(__dirname, 'public'));
  await app.boot();
  await app.start();

  const url = app.restServer.url;
  console.log(`Server is running at ${url}`);
  return app;
}
```

You can call `app.static()` multiple times to configure the app to serve static
assets from different drectories.

```ts
app.static('/files', path.join(__dirname, 'files'));
app.static('/downloads', path.join(__dirname, 'mp3s'));
```

You can also call `app.static()` multiple times on the same mounth path to serve
static assets from different directories on the same path.

```ts
app.static('/files', path.join(__dirname, 'files'));
app.static('/files', path.join(__dirname, 'other-files'));
```

And `app.static()` can be called even after the app have started.

```ts
await app.boot();
await app.start();
app.static('/files', path.join(__dirname, 'files'));
```
