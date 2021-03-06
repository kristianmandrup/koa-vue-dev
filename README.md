# [koa](http://koajs.com)-[vue](http://vuejs.org)-dev

> Vue.js development server middleware for Koa.js.

This is a middleware for [Koa.js](http://koajs.com) and provides a fully featured development server built on top of [Webpack](http://webpack.github.io) which renders the [Vue.js](http://vuejs.org) application for client-side and server-side. The middleware is configurable and supports hot module replacement out of the box. This middleware should be used in **development only**. Please check the [koa-vue-builder](https://github.com/kristianmandrup/koa-vue-builder) middleware for a production ready alternative.

This is an open source package for [Vue.js](http://vuejs.org/) and [Koa.js](http://koajs.com). The source code is available on [GitHub](https://github.com/kristianmandrup/koa-vue-dev) where you can also find our [issue tracker](https://github.com/kristianmandrup/koa-vue-dev/issues).

Note: This project was forked with permission from [express-vue-dev](forked from xpepermint/express-vue-dev)

## Related Projects

* [vue-webpack](https://github.com/xpepermint/vue-webpack): Webpack configuration object generator for Vue.js.
* [vue-builder](https://github.com/xpepermint/vue-builder): Server-side and client-side rendering for Vue.js.
* [koa-vue-builder](https://github.com/kristianmandrup/koa-vue-builder): Vue.js server-side rendering middleware for Koa.js.
* [vue-cli-template](https://github.com/xpepermint/vue-cli-template): A simple server-side rendering CLI template for Vue.js.

## Install

Run the command below to install the package.

```
$ npm install vue-builder webpack@2.1.0-beta.25
$ npm install --save-dev koa-vue-dev
```

## Usage

To create a middleware, create a Webpack configuration objects for client-side and server-side, then pass it the the `devServer` method. Use the [vue-webpack](https://github.com/xpepermint/vue-webpack) package to simplify the setup.

```js
const {build} = require('vue-webpack');
const {devServer} = require('koa-vue-dev');

let middleware = devServer({
  server: build({
    mode: 'server',
    inputFilePath: `./app/server-entry.js` // Vue application entry file for server-side
  }),
  client: build({
    mode: 'client',
    inputFilePath: `./app/client-entry.js` // Vue application entry file for client-side
  })
}); // pass this to app.use() of your Express application
```

Check the included `./example` directory or run the `npm run example` command to start the application.

## API

**devServer({server, client, verbose})**

> Development server middleware for serving Vue.js application.

| Option | Type | Required | Default | Description
|--------|------|----------|---------|------------
| server | Object | Yes | - | Webpack configuration object for server-side rendering.
| client | Object | Yes | - | Webpack configuration object for client-side rendering.
| verbose | Boolean | No | false | When `true` detailed logging is enabled.

### Webpack middleware

Uses Koa specific webpack middleware:

- [koa-webpack-dev-middleware](https://www.npmjs.com/package/koa-webpack-dev-middleware)
- [koa-webpack-hot-middleware](https://www.npmjs.com/package/koa-webpack-hot-middleware)

The middlewares are wrapped using `convert` and `compose` in order to work with Koa2 async/await promises:

```js
const convert = require('koa-convert');
const compose = convert.compose;

// ...

return compose([
  convert(webpackDevMiddleware(clientCompiler, {
    //...
  }),
  convert(webpackHotMiddleware(clientCompiler, {
    //...
  }
  //...
]);
```

## License (MIT)

```
Copyright (c) 2016 Kristian Mandrup <kmandrup@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```
