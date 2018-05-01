# FAQ

## Q: What if composed stamps have same named property/method/etc?

The last composed property/method/etc wins. In other words, you can overwrite everything when composing stamps.

## Q: But what if there are names conflicts during stamp composition?

In our practice the names never collide. Because stamps tend to stay very small thanks to its absolute freedom.

But if you still need to avoid name collision or protect a method from overwriting then use [@stamp/collision](../ecosystem/stamp-collision.md) utility stamp.

## Q: Why stampit is ES5? Should it be ES6 or later?

* The main advantage of ES6 is the [import/export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) syntax which enables easy [treeshaking](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking). But:
  * There is nothing to treeshake in `stampit` or `@stamp/it`.
  * `stampit` is already 1.3KB gzipped.
* Maintaining ES6-&gt;ES5 is quite hard in such a specific project.
* Most projects still transpile from ESNext to ES5.

We will rethink this policy as soon as there is a need which cannot be solved with ES5.

