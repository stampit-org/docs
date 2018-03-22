# Installation

## Browser usage

For browsers it is recommended to use the [`stampit`](https://github.com/stampit-org/stampit) module. It's a small \(**1.3KB** gzipped\) and browser optimized. Also, it will work in other JavaScript environments and ES5-compatible engines.

### NPM

```bash
npm i -S stampit
```

### Bower

```bash
bower install stampit=https://npmcdn.com/stampit@4
bower install stampit=https://unpkg.com/stampit@4
bower install stampit=https://cdn.jsdelivr.net/npm/stampit@4
```

### Direct script

Use any of the links above. For example:

```html
<script src="https://cdn.jsdelivr.net/npm/stampit@4"></script>
```

The above will automatically install stampit to  `window.stampit`.

In a [WebWorker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers) or [ServiceWorker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) stampit would install itself to `self.stampit`.

### Compatibility note

The `stampit` v4 is compatible with any EcmaScript 5 environment, including IE9.

The detailed Release Notes are always available at the [GitHub Releases](https://github.com/stampit-org/stampit/releases) page.

## Node.js usage

For node.js it is recommended to use the [`@stamp/it`](https://github.com/stampit-org/stamp/tree/master/packages/it) module. Its source code is more readable than `stampit` module, so you can easily debug its internals if needed. In addition, it will work in any other ES5-compatible JavaScript environments.

```bash
npm i -S @stamp/it
```

### Compatibility note

The `@stamp/it` is compatible with any EcmaScript 5 environment, including Node.js v0.1. However, some of the [Ecosystem](/ecosystem.md) modules are ES6 only.

The release notes can be found on the [project page](https://github.com/stampit-org/stamp).



