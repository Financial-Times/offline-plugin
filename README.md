<div align="center">
<h2>Forked from <a href="https://github.com/LeComptoirDesPharmacies/offline-plugin" alt="original-offline-plugin">LeComptoirDesPharmacies offline-plugin</a></h2>

  <h1><code>offline-plugin</code> for webpack</h1>

</div>
<br>

This plugin is intended to provide an offline experience for **webpack** projects. It uses **ServiceWorker**, and **AppCache** as a fallback under the hood. Simply include this plugin in your ``webpack.config``, and the accompanying runtime in your client script, and your project will become offline ready by caching all (or some) of the webpack output assets.

If you are looking for the original `v5` from <a href="https://github.com/LeComptoirDesPharmacies/offline-plugin" alt="original-offline-plugin">LeComptoirDesPharmacies offline-plugin</a> we have stored it on <a href="https://github.com/Financial-Times/offline-plugin/tree/lcdp">lcdp branch</a>.

## Webpack5 Warnings

There are two warnings when building using Webpack5:

The first one is in regards to usage of Compilation.cache. The reference is in `service-worker.js` and it should be pretty safe to remove it because the offline-plugin is not using Hot Module Replacement currently.
```
(node:20486) [DEP_WEBPACK_COMPILATION_CACHE] DeprecationWarning: Compilation.cache was removed in favor of Compilation.getCache()
```

This second warning is a little bit more complex because it requires a change on how the offline-plugin interact with the internal plugins, in this case: service-worker.js
The solution is to change the updates on `.assets` to use a tapable way of adding assets during compilation time.
For reference, Google Workbox had similar issue: https://github.com/webpack/webpack/issues/11425
So, `index.js` won't emit the EntryPlugin as it is currently doing. It will need to emit via tap instead of adding the entry, emitting and forcing the change during compilation time.

```
(node:20486) [DEP_WEBPACK_COMPILATION_ASSETS] DeprecationWarning: Compilation.assets will be frozen in future, all modifications are deprecated.
BREAKING CHANGE: No more changes should happen to Compilation.assets after sealing the Compilation.
        Do changes to assets earlier, e. g. in Compilation.hooks.processAssets.
        Make sure to select an appropriate stage from Compilation.PROCESS_ASSETS_STAGE_*.
```

Those warnings are important to address because Webpack5 will break compatibility at some point.

## Install

`npm install @financial-times/offline-plugin [--save-dev]`

## Setup

First, instantiate the plugin in your `webpack.config`:

```js
// webpack.config.js example

var OfflinePlugin = require('@financial-times/offline-plugin');

module.exports = {
  // ...

  plugins: [
    // ... other plugins
    // it's always better if OfflinePlugin is the last plugin added
    new OfflinePlugin()
  ]
  // ...
}
```
_(and optionally configure with [options](docs/options.md))_  

Then, add the [runtime](docs/runtime.md) into your entry file (typically main entry):

```js
require('@financial-times/offline-plugin/runtime').install();
```

ES6/Babel/TypeScript
```js
import * as OfflinePluginRuntime from '@financial-times/offline-plugin/runtime';
OfflinePluginRuntime.install();
```

> For more details of usage with `TypeScript` see [here](docs/typescript.md)

### `offline-plugin` isn't working?

:information_source: **[Troubleshooting](docs/troubleshooting.md)** | **[FAQ](docs/FAQ.md)**

## Docs

* [Options](docs/options.md)
* [Caches](docs/caches.md)
* [Update process](docs/updates.md)
* [Cache Maps](docs/cache-maps.md)
* [Runtime API](docs/runtime.md)

## Examples

* [Single Page Application](docs/examples/SPA.md)

## Articles

* [Easy Offline First Apps With Webpack's Offline Plugin](https://dev.to/kayis/easy-offline-first-apps-with-webpacks-offline-plugin)
* [Handling Client Side App Updates (with Service Workers)](https://zach.codes/handling-client-side-app-updates-with-service-workers/)

## Options

All options are optional and `offline-plugin` can be used without specifying them.

#### [See all available options here.](docs/options.md)

## Who is using `offline-plugin`

<div align="center">
  <strong>Demo:<br><a href="https://offline-plugin.now.sh/"> Progressive Web App built with <code>offline-plugin</code></a></strong><br>
  <div>(<a href="https://github.com/NekR/offline-plugin-pwa"><i>source code</i></a>)</div>
</div>

### Projects

* [React Boilerplate](https://github.com/mxstbr/react-boilerplate)
* [Phenomic](https://phenomic.io)
* [React, Universally](https://github.com/ctrlplusb/react-universally)

### PWAs

* [`offline-plugin` PWA](https://offline-plugin.now.sh/)
* [Omroep West](https://m.omroepwest.nl/)
* [Preact](https://preactjs.com/) ([source](https://github.com/developit/preact-www))
* [CodePan](https://codepan.net) ([source](https://github.com/egoist/codepan))
* [Offline Kanban](https://offline-kanban.herokuapp.com) ([source](https://github.com/sarmadsangi/offline-kanban))
* [Online Board](https://onlineboard.sonnywebdesign.com/) ([source](https://github.com/andreasonny83/online-board))
* [Fluid Outliner](https://fluid-notion.github.io/fluid-outliner/) ([source](https://github.com/fluid-notion/fluid-outliner))

_If you are using `offline-plugin`, feel free to submit a PR to add your project to this list._


## Contribution

See [CONTRIBUTING](CONTRIBUTING.md)

## License

[MIT](LICENSE.md)  

## CHANGELOG

[CHANGELOG](CHANGELOG.md)
