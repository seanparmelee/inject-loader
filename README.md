<div align="center">
  <h3>💉📦<br><br><code>inject-loader</code></h3>
  <h4>A Webpack loader for injecting code into modules via their dependencies</h4>
  <a href="https://travis-ci.org/plasticine/inject-loader"><img src="https://img.shields.io/travis/plasticine/inject-loader/master.svg?style=flat-square" alt="build status" /></a> <a href="https://www.npmjs.com/package/inject-loader"><img src="https://img.shields.io/npm/v/inject-loader.svg?style=flat-square" alt="npm version" /></a> <a href="https://www.npmjs.com/package/inject-loader"><img src="https://img.shields.io/npm/dm/inject-loader.svg?style=flat-square" alt="npm downloads" /></a>
</div>

***

### Why

This is particularly useful for writing tests where mocking things inside your module-under-test is sometimes necessary before execution.

`inject-loader` was inspired by, and builds upon ideas introduced in [jauco/webpack-injectable](https://github.com/jauco/webpack-injectable).

### Usage

[Documentation: Using loaders](http://webpack.github.io/docs/using-loaders.html)

Use the inject loader by adding the `inject-loader!` [inline loader](https://webpack.js.org/concepts/loaders/#inline) when you use `require`, this will return a function that can used in test code to modify the injected module.

By default all `require` statements in an injected module will be altered to be replaced with an injector, though if a replacement it not specified the default values will be used.

### Examples

Given some code in a module like this:

```javascript
// MyStore.js

var Dispatcher = require('lib/dispatcher');
var EventEmitter = require('events').EventEmitter;
var handleAction = require('lib/handle_action');

Dispatcher.register(handleAction, 'MyStore');
```

You can manipulate it’s dependencies when you come to write tests as follows:

```javascript
// If no flags are provided when using the loader then
// all require statements will be wrapped in an injector
MyModuleInjector = require('inject-loader!MyStore')
MyModule = MyModuleInjector({
  'lib/dispatcher': DispatcherMock,
  'events': EventsMock,
  'lib/handle_action': HandleActionMock
})
```

There are a few examples of complete test setups for Webpack 2, 3 & 4 in the [`example`](./example) folder.

## License

MIT (http://www.opensource.org/licenses/mit-license.php)
