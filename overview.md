# History

The original idea of Stamps belongs to [Eric Elliott](https://ericelliottjs.com/). See his [first commit](https://github.com/stampit-org/stampit/commit/ac330e8537e349a9640bbe4a34c63150db445a20) from 11 Feb 2013 in the stampit repository. Since then the idea evolved to [specification](/specification.md) and a whole [ecosystem](/ecosystem.md) of modules.

# Using Stamps

Head straight to the [Quick start](/start.md) for code examples.

# Ecosystem

Stampit have a lot of helper modules. You can always find the list of official NPM packages here: [https://www.npmjs.com/~stamp](https://www.npmjs.com/~stamp)

See more information on the [Ecosystem](/ecosystem.md) page.

# Stampit and ES6

Often people ask why there is no ES6 bundle. There are few reasons.

* The main advantage of the ES6 is the [import/export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) syntax which enables easy [treeshaking](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking). But.
  * There is nothing to treeshake in `stampit` or `@stamp/it`.
  * `stampit` is already 1.3KB gzipped.
* Maintaining ES6-&gt;ES5 is quite hard in such a specific project.

We will rethink this policy as soon as there is a need which cannot be solved with ES5.

