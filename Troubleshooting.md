## Duplicate versions

Many packages are published with the same version, and almost all of them depend on the `@codingame/monaco-vscode-api` main package (with strict version range).

It is VERY important that only a single version of ALL the packages is installed, otherwise weird things can happen.

You can check that `npm list vscode` only lists a single version.

## If you use Webpack

### monaco-editor-webpack-plugin

Starting from v2, [monaco-editor-webpack-plugin](https://www.npmjs.com/package/monaco-editor-webpack-plugin) can't be used

Here's the alternative for each options:

- `filename`: it can be configured at the webpack level directly
- `publicPath`: it can be configured at the webpack level or by hands when redistering the worker in `window.MonacoEnvironment`.
- `languages`: Import VSCode language extensions (`@codingame/monaco-vscode-xxx-default-extension`) or (`@codingame/@codingame/monaco-vscode-standalone-*`). Please obey: VSCode extensions can only be used if `themes` and `textmate` service overrides are configured and monaco languages can only be used if those two services are not configured (see [here](#monaco-standalone-services) for further details).
- `features`: With this lib, you can't remove editor features.
- `globalAPI`: you can set `window.MonacoEnvironment.globalAPI` to true

### exclude assets from loaders

Webpack makes all file go through all matching loaders. This libraries need to load a lot of internals resources, including HTML, svg and javascript files (for default extension codes).

We need webpack to let those file untouched:
- the babel loader shouldn't transform extension javascript files
- the html loader shouldn't transform the worker extension host iframe html
- ...

Fortunately, all the assets are loaded via the `new URL('asset.extension', import.meta.url)` syntax, and webpack provide a way to exclude the file loaded that way: `dependency: { not: ['url'] }` see https://webpack.js.org/guides/asset-modules/

## If you use Vite
### Asset management

This library uses a lot the `new URL('asset.extension', import.meta.url)` syntax which [is supported by vite](https://vitejs.dev/guide/assets.html#new-url-url-import-meta-url)

While it works great in `build` mode (because rollup is used), there is some issues in `watch` mode:

- `import.meta.url` is not replaced while creating bundles, it is an issue when the syntax is used inside a dependency
- Vite is still trying to inject/transform javascript assets files, breaking the code by injecting ESM imports in commonjs files

There are workarounds for both:

- We can help vite by replacing `import.meta.url` by the original module path:

```typescript
import importMetaUrlPlugin from '@codingame/esbuild-import-meta-url-plugin'

{
  ...
  optimizeDeps: {
    esbuildOptions: {
      plugins: [importMetaUrlPlugin]
    }
  }
}
```
#### Commonjs dependencies

Some dependencies are published only as CommonJS, and vite fails by default to work with them when used in a worker.

Those libraries are concerned:
- `vscode-textmate`
- `vscode-oniguruma` (textmate dependency)
- `@vscode/vscode-languagedetection`

If you use the corresponding service override, you can force vite to optimize them, which will make vite properly handle commonjs, by adding them in the vite config in `optimizeDeps.include`:

```typescript
export default defineConfig({
  optimizeDeps: {
    include: [
      'vscode-textmate',
      'vscode-oniguruma',
      '@vscode/vscode-languagedetection'
    ]
  }
})
```

## If using Angular and getting `Not allowed to load local resource:` errors

*The short version*: set up and use a custom webpack config file and add this under `module`:

```typescript
parser: {
  javascript: {
    url: true,
  }
}
```

See [this issue](https://github.com/CodinGame/monaco-vscode-api/issues/186) or this [StackOverflow answer](https://stackoverflow.com/a/75252098) for more details, and [this discussion](https://github.com/angular/angular-cli/issues/24617) for more context.

## The typescript language features extension is not providing project-wide intellisense

The typescript language features extensions requires [SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) to enable project wide intellisense or only a per-file intellisense is provided.

It requires [crossOriginIsolated](https://developer.mozilla.org/en-US/docs/Web/API/crossOriginIsolated) to be true, which requires assets files to be servers with some specific headers:

- [Cross-Origin-Opener-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Opener-Policy): `same-origin`
- [Cross-Origin-Embedder-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Embedder-Policy): `require-corp` or `credentialless`

At least thoses files should have the headers:

- the main page html
- the worker extension host iframe html: `webWorkerExtensionHostIframe.html`
- the worker extension host worker javascript: `extensionHost.worker.js`

If adding those headers is not an options, you can have a look at <https://github.com/gzuidhof/coi-serviceworker>, but only if you are not using webviews as it introduces problems then.