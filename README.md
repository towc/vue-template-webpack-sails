# Custom vue webpack-sails template

assumes the following structure

```
app
 - backend (sails new backend)
   - assets
     - index.html
 - frontend (vue init towc/vue-template-webpack-sails frontend)
```

The `assets/index.html` can then be used for any purpose.

Watching has been enabled with `aggregateTimeout: 0` and `poll: 100` for fast changing, without a hot reload. This means that after, ideally in a separate terminal, you can call `npm run build` and the various files in `backend/assets` will be automatically updated, so hard-refreshing the page is all that is needed, without having to run `sails lift` again.

In addition, ESLint has been disabled by default due to it being buggy, so you can just press `ENTER` during the vue setup, but still change anything you might want. And finally `mode: 'history'` has been enabled by default on the vue-router

In order to call the file using different paths, it is recommended to use this structure in sails' `backend/config/routes.js`:

```js
const paths = [ '/', '/login', '/profile', ... ]
    , vue = 'Vue.serve'
    , pathObject = {};

paths.map( path => pathObject[ path ] = vue );

module.exports.routes = Object.assign( pathObject, {
  
  'post /api/stuff': 'Whatever.action'
  ...
});
```

Where `Vue` is just a controller (`sails generate controller vue`) to which a `serve` action is called in `backend/api/controllers/VueController.js`:

```js
const fs = require( 'fs' )
    , vuePath = __dirname + '/../../assets/index.html';

module.exports = {
  serve( req, res ) {
    fs.exists( vuePath, ( exists ) => {
      if( !exists )
        return res.notFound( 'Vue hasn\'t been built yet' );

      fs.createReadStream( vuePath ).pipe( res );
    })
  }
}
```

The rest of the readme is from the original [vuejs-templates/webpack](https://github.com/vuejs-templates/webpack)

# vue-webpack-boilerplate

> A full-featured Webpack setup with hot-reload, lint-on-save, unit testing & css extraction.

> This template is Vue 2.0 compatible. For Vue 1.x use this command: `vue init webpack#1.0 my-project`

## Documentation

- [For this template](http://vuejs-templates.github.io/webpack): common questions specific to this template are answered and each part is described in greater detail
- [For Vue 2.0](http://vuejs.org/guide/): general information about how to work with Vue, not specific to this template

## Usage

This is a project template for [vue-cli](https://github.com/vuejs/vue-cli). **It is recommended to use npm 3+ for a more efficient dependency tree.**

``` bash
$ npm install -g vue-cli
$ vue init webpack my-project
$ cd my-project
$ npm install
$ npm run dev
```

If port 8080 is already in use on your machine you must change the port number in `/config/index.js`. Otherwise `npm run dev` will fail.

## What's Included

- `npm run dev`: first-in-class development experience.
  - Webpack + `vue-loader` for single file Vue components.
  - State preserving hot-reload
  - State preserving compilation error overlay
  - Lint-on-save with ESLint
  - Source maps

- `npm run build`: Production ready build.
  - JavaScript minified with [UglifyJS](https://github.com/mishoo/UglifyJS2).
  - HTML minified with [html-minifier](https://github.com/kangax/html-minifier).
  - CSS across all components extracted into a single file and minified with [cssnano](https://github.com/ben-eb/cssnano).
  - All static assets compiled with version hashes for efficient long-term caching, and a production `index.html` is auto-generated with proper URLs to these generated assets.
  - Use `npm run build --report`to build with bundle size analytics.

- `npm run unit`: Unit tests run in PhantomJS with [Karma](http://karma-runner.github.io/0.13/index.html) + [Mocha](http://mochajs.org/) + [karma-webpack](https://github.com/webpack/karma-webpack).
  - Supports ES2015+ in test files.
  - Supports all webpack loaders.
  - Easy mock injection.

- `npm run e2e`: End-to-end tests with [Nightwatch](http://nightwatchjs.org/).
  - Run tests in multiple browsers in parallel.
  - Works with one command out of the box:
    - Selenium and chromedriver dependencies automatically handled.
    - Automatically spawns the Selenium server.

### Fork It And Make Your Own

You can fork this repo to create your own boilerplate, and use it with `vue-cli`:

``` bash
vue init username/repo my-project
```
