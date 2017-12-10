# koa-mockjs

> mockjs api server middleware for Koa2

[![Linux Build][travis-image]][travis-url]
[![Coverage Status][coveralls-image]][coveralls-url]
[![Dependencies][dependencies-image]][dependencies-url]
[![node][node-image]][node-url]
[![license MIT][license-image]][license-url]


[中文文档](README.zh-CN.md)


## How to use it

### Installation

``` sh
$ yarn add -D koa-mockjs
# or
$ npm i -D koa-mockjs
```

----


### Quick start


1. Create a directory `api-server`, then create the file `app.js`, the content is：

``` js
const { join } = require('path');
const Koa = require('koa');
const mock = require('koa-mockjs');

const app = new Koa();

// Use the default path '/' (Not recommended)
// app.use(mock('/', join(__dirname, 'mocks')));

// Use a custom path '/api'
app.use(mock('/api', join(__dirname, 'mocks')));

// Here you can add any code.

app.listen(3000, () => {
  console.log('mock server run at http://localhost:3000/api/');
});
```

2. Create a `mocks` directory under `api-server` and create `data.json` as follows:

```js
/**
 * api interface description
 *
 * @url /test-api
 */
{
  "code": 0,
  "result|5": [
    {
      "uid|+1": 1,
      "name": "@name",
      "email": "@email"
    }
  ]
}
```

3. Install dependent modules

```sh
$ yarn add -D koa koa-mockjs
```

4. Start

```sh
$ node app.js
```

> You can then access the <http://localhost:3000/api> to view API documents.


----


### Mock JSON

* [Mock.js 0.1 doc](https://github.com/nuysoft/Mock/wiki)  
* [Mock Sample](http://mockjs-lite.js.org/docs/examples.html)  


### Examples

```
.
├── mocks
    ├── home
    ⎪   ├── data.json
    ├── user
    ⎪   ├── data.js
    ⎪   ├── data.json
    ├── game
        ├── data.json
```



## data.json

`Mock JSON` here is not a real JSON file, and more like a JS file, so you want to use the following format.

> Hypothetical file in 'mocks/home/test.json'

``` js
/**
 * Interface function description
 *
 * @url /api-access-path
 *
 * Parameter description and other instructions.
 * uid: user ID
 * name: username
 * email: the email
 * etc.
 */

{
  "code": 0,
  "result|5": [
    {
      "uid|+1": 1,
      "name": "@name",
      "email": "@email"
    }
  ]
}
```

Then you can access the <http://localhost:3000/api/api-access-path> through the browser.

Of course, you can also use the JS file directly.

``` js
/**
 * home page links
 *
 * @url /home-links
 *
 * Here you can write a detailed description
 * of the parameters of the information.
 */

module.exports = {
  code() { // simulation error code, 1/10 probability of error code 1.
    return Math.random() < 0.1 ? 1 : 0;
  },
  "list|5-10": [
    {"title": "@title", "link": "@url"}
  ]
};
```

Or export function.

``` js
/**
 * user page - user info
 *
 * @url /user?uid=233
 *
 * GET: Request method and parameter
 *   uid This is the requested userID
 *
 * Here you can write a detailed description
 * of the parameters of the information.
 */

module.exports = function (req) {
  const { uid } = req.query;

  if (!uid) {
    return {
      code: -1,
      msg: 'no uid',
    }
  }

  return {
    code: 0,
    data: {
      "uid": +uid,
      "name": "@name",
      "age|20-30": 1,
      "email": "@email",
      "date": "@date",
    },
  };
};
```


[travis-url]: https://travis-ci.org/52cik/koa-mockjs
[travis-image]: https://img.shields.io/travis/52cik/koa-mockjs/master.svg?label=linux

[coveralls-url]: https://coveralls.io/github/52cik/koa-mockjs?branch=master
[coveralls-image]: https://coveralls.io/repos/52cik/koa-mockjs/badge.svg?branch=master&service=github

[license-url]: https://opensource.org/licenses/MIT
[license-image]: https://img.shields.io/badge/license-MIT-blue.svg

[dependencies-url]: https://david-dm.org/52cik/koa-mockjs
[dependencies-image]: https://img.shields.io/david/52cik/koa-mockjs.svg?style=flat

[node-url]: https://nodejs.org
[node-image]: https://img.shields.io/badge/node-%3E=%208-brightgreen.svg
