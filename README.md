## Webpack Boilerplate :100:

[![Build Status](https://travis-ci.org/code-for-coffee/webpack-boilerplate.svg?branch=master)](https://travis-ci.org/code-for-coffee/webpack-boilerplate)

This boilerplate is designed to get you up and running with Webpack 4. It is minimally configured but supports native ES6 and ES6 modules using Babel.

#### Development Quickstart

1. Clone this repository to your local machine.
2. Run `yarn install` / `npm i` to install dependencies from your `package.json`.
3. Place source files for your client side code inside of the `./src` directory.
4. Open the `webpack.config.js` and add starting source files to your `entry` file. Note the current entry file is `app.js` and there is currently an `./src/app.js`; you can add other source files as well.
5. Run `npm run start` / `yarn start` to begin running Webpack in production mode. Browse to `localhost:8080` to see your website.

#### Releasing for Production

- You can build an _unminified_ version of your source code by running `yarn build`. This will place the output files in your `/dist` directory. You'll just need to include those files in your HTML file(s) as needed.
- You can also build a _minified_ version of your source code by running `yarn prod`. This also places your code in the `/dist` folder. This is the best option to use for performance.

#### Debugging in the Browser

Webpack 2 provides source maps in the debug window of your browser. In the **sources** tab where all of your code is loaded, you'll notice a `webpack://` domain. Inside of that domain, a `~` folder exists - this is your raw Javascript code. You can place breakpoints here and debug as usual. It is awesome!

#### Lazy Loading

Webpack can load/import individual Javascript files as needed. In `/src/js/app.js` you'll notice that there is a `System.import()` for `LazyLoadingComponent.js`. This allows the developer to only load code as needed providing a better user experience. _Note_: you should provide some sort of user interface experience such as a loading bar to let the user know that there is something being performed in the background.

#### Tree Shaking

Webpack 2 removes ES6 modules (anything using the `import ...` syntax) that aren't actually imported into your project through a technique called _tree shaking_. Notice that in your `build` or `prod` output, `/src/js/components/NotUseComponent.js` is not included in the output.

#### window.Promise Polyfill for older browsers

In Webpack 1.x, you had to specify a Promsie library. In Webpack 2, a `Promise` polyfill must be included for browsers that do not support it. [Check to see if you need a Polyfill on caniuse](http://caniuse.com/#feat=promises). If you do, in the `/src/js/app.js` file, you could include the following snippet to utilize Lazy Loading (code splitting) to import in your Polyfill. This way, you only load the additional script if needed.

First, add `es6-promise` to your dependencies.

`yarn add es6-promise`

Then, import via code-spliting/lazy loading:

```javascript
// make sure this is hoisted first!
if (!window.Promise) {
  System.import("es6-promise").polyfill();
}
```

##### Usage in IE<9

`catch` is a reserved word in IE<9, meaning `promise.catch(func)` throws a syntax error. To work around this, you can use a string to access the property as shown in the following example.

However, please remember that such technique is already provided by most common minifiers, making the resulting code safe for old browsers and production:

```js
promise["catch"](function(err) {
  // ...
});
```

Or use `.then` instead:

```js
promise.then(undefined, function(err) {
  // ...
});
```

#### window.fetch Polyfill

If your browser audience does not include support for ES6 `fetch()` (the ES6 alternative to \$.ajax), you will need a polyfill. [Check to see if you need a Polyfill on caniuse](http://caniuse.com/#feat=fetch). If you do, in the `/src/js/app.js` file, you could include the following snippet to utilize Lazy Loading (code splitting) to import in your Polyfill. This way, you only load the additional script if needed.

Add the dependency first.

`yarn add whatwg-fetch`

Then, import via code-splitting/lazy loading.

```javascript
// make sure this is hoisted first!
if (!window.fetch) {
  System.import("es6-promise").polyfill();
}
```

Then, `fetch()` away and [read the Mozilla documentation on using fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch).

```javascript
const input = document.querySelector('input[type="file"]');

let data = new FormData();
data.append("file", input.files[0]);
data.append("user", "hubot");

fetch("/avatars", {
  method: "POST",
  body: data
});
```

#### async + await support

This Webpack config now supports async+await. The following example is included in the `./helpers/` directory:

```js
/**
 * request
 * @param URI URL for an API, such as http://api.gogoair.com
 * @param hook a function that is called when response JSON is provided
 */
const request = async (URI, hook) => {
  const response = await fetch(URI);
  const json = await response.json();
  //console.log(json);
  if (typeof hook === "function") hook(json);
};

/**
 * jsonRequest
 * Async fetch method that fetches a URI and passes JSON to a hook function
 */
export const jsonRequest = request;
```
