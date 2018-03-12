# Installation

## Browser usage

For browsers it is recommended to use the [`stampit`](https://github.com/stampit-org/stampit) module. It is a small \(v4.1 is 1.3KB gzipped\) and browser optimized. Also, it will work in other JavaScript environments and ES5-compatible engines.

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

The above will automatically install stampit to  `window.stampit` on web pages or to `self.stampit` in a [WebWorker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers) or [ServiceWorker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API).

### Compatibility note

The `stampit` v4 is compatible with any EcmaScript 5 environment, including IE9.

The detailed Release Notes are always available at the [GitHub Releases](https://github.com/stampit-org/stampit/releases) page.

## Node.js usage

For node.js it is recommended to use the [`@stamp/it`](https://github.com/stampit-org/stamp/tree/master/packages/it) module. Its source code is more readable than `stampit` module, so you can easily debug its internals if needed. Also, it will work in other JavaScript environments and ES5-compatible engines.

```bash
npm i -S @stamp/it
```

### Compatibility note

The `@stamp/it` is compatible with any EcmaScript 5 environment, including Node.js v0.10. However, some of the [Ecosystem](/ecosystem.md) modules are ES6 only.

The release notes can be found on the [project page](https://github.com/stampit-org/stamp).

# Ecosystem

Stampit have a lot of helper modules. You can always find the list of official NPM packages here: [https://www.npmjs.com/~stamp](https://www.npmjs.com/~stamp)

See more information on the [Ecosystem](/ecosystem.md) page.

# Stampit and ES6

Often people ask why there is no ES6 bundle. There are few reasons.

* The main advantage of the ES6 is the [import/export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) syntax which enables easy [treeshaking](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking).
  * There is nothing to treeshake in `stampit` or `@stamp/it`.
  * `stampit` is already 1.3KB.
* Maintaining ES6-&gt;ES5 is quite hard in such a specific project.

We will rethink this policy as soon as there is a need which cannot be solved with ES5.

