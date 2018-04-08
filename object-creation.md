# Stampit and ES6

Stampit is ES5. Often people ask why there is no ES6 bundle. There are few reasons.

* The main advantage of the ES6 is the [import/export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) syntax which enables easy [treeshaking](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking). But.
  * There is nothing to treeshake in `stampit` or `@stamp/it`.
  * `stampit` is already 1.3KB gzipped.
* Maintaining ES6-&gt;ES5 is quite hard in such a specific project.

We will rethink this policy as soon as there is a need which cannot be solved with ES5.

