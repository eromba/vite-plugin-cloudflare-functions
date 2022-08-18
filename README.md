# vite-plugin-cloudflare-functions

[![CI](https://github.com/yjl9903/vite-plugin-cloudflare-functions/actions/workflows/ci.yml/badge.svg)](https://github.com/yjl9903/vite-plugin-cloudflare-functions/actions/workflows/ci.yml)

:construction: Work in progress.

Make Cloudflare Pages Functions works with Vite friendly.

## Features

+ **Dev**: Automatically start wrangler pages dev server
+ **Dev**: Automatically generate functions API type declaration
+ **Build**: Automatically move the functions source directory

## Installation

```bash
npm i -D vite-plugin-cloudflare-functions
```

```ts
// vite.config.ts

import { defineConfig } from 'vite';

import CloudflarePagesFunctions from 'vite-plugin-cloudflare-functions';

export default defineConfig({
  plugins: [
    CloudflarePagesFunctions()
  ]
});
```

## Usage

### Functions

Just write pages functions as usual, but you should use the following utility functions to make auto-generation work.

```ts
// /api/[msg].ts

import {
  makeRawPagesFunction,
  makePagesFunction,
  makeResponse
} from 'vite-plugin-cloudflare-functions/worker';

export const onRequestGet = makePagesFunction(({ params }) => ({
  status: 'OK',
  data: 'Hello, ' + params.msg + '!'
}));

export const onRequestPost = makeRawPagesFunction(({ params }) =>
  makeResponse({
    status: 'OK',
    data: 'Post ' + params.msg + ' OK!'
  })
);
```

#### Override environment

For example, you set the environment variable `PASS`.

```ts
// cloudflare.d.ts

import 'vite-plugin-cloudflare-functions/worker';

declare module 'vite-plugin-cloudflare-functions/worker' {
  interface PagesFunctionEnv {
    PASS: string;
  }

  interface PagesFunctionData {}
}
```

Then you can find the parameter `env` has corresponding type declarations.

```ts
// /api/index.ts

import { makePagesFunction } from 'vite-plugin-cloudflare-functions/worker';

export const onRequestGet = makePagesFunction(({ env }) => ({
  pass: env.PASS
}));
```

### Client

## Configuration

```ts
import { defineConfig } from 'vite';

import CloudflarePagesFunctions from 'vite-plugin-cloudflare-functions';

export default defineConfig({
  plugins: [
    CloudflarePagesFunctions({
      // Cloudflare Functions root directory
      root: '../functions',
      // Copy the functions directory to outDir
      outDir: '../../',
      // Wrangler configuration
      wrangler: {
        // Wrangler dev server port
        port: 8788,
        // Enable wrangler log
        log: true
      }
    })
  ]
});
```

## License

MIT License © 2021 [XLor](https://github.com/yjl9903)
