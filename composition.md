# What is a Stamp?

Stamp is a **composable** factory function.

> _NOTE_
>
> Factory functions create and return new objects.

Go to [API Quick start](/start.md) to learn how to create a Stamp.

```js
const object = Stamp() // creating a new object from a stamp
```

## Stamp's metadata

But unlike plain functions, Stamps carry around the metadata about the object they are going to produce. The metadata is attached to the `Stamp.compose` object. Another name for that metadata is "stamp descriptor" or just "descriptor".

```js
const descriptor = Stamp.compose // retrieving a stamp's metadata
```

There are 11 [standardized](/specification.md) types of metadata:

```ts
interface Descriptor {
  methods: Object,
  properties: Object,
  deepProperties: Object,
  propertyDescriptors: Object,
  initializers: Function[],
  staticProperties: Object,
  staticDeepProperties: Object,
  staticPropertyDescriptors: Object,
  composers: Function[],
  configuration: Object,
  deepConfiguration: Object
}
```

You, the developer, is allowed and encouraged to operate that metadata manually if needed.

For example, you can always check which methods an object instance will have:

```js
console.log('Object will have these methods:', Object.keys(Stamp.compose.methods))
```

## Ducktyping a stamp

To understand if your object is a stamp you need to check its property `.compose` like this:

```js
function isStamp (object) {
  return typeof object === 'function' && object.compose === 'function'
}
```

## Composing stamps

The main reason why stamps exist is the ability to freely compose stamps together.

```js
const ComposedStamp = compose(Stamp1, Stamp2, Stamp3)
```

The line above is identical to these:

```js
const ComposedStamp = Stamp1.compose(Stamp2, Stamp3)
const ComposedStamp = Stamp1.compose(Stamp2).compose(Stamp3)
const ComposedStamp = Stamp1.compose(Stamp2.compose(Stamp3))
const ComposedStamp = compose().compose(Stamp1, Stamp2, Stamp3)
```

The `compose`  and `.compose` functions are doing only one thing: **merge stamp descriptors according to the **[**stamp specification**](/specification.md).

> _NOTE_
>
> Every time you call `compose` or `.compose` you create a **NEW** stamp.

## Creating stamps

To create a new stamp from scratch you would need to use one of the JavaScript modules available:

* [`@stamp/compose`](https://www.npmjs.com/package/@stamp/compose) - the [**standardized compose function**](/specification.md)
* [`@stamp/it`](https://www.npmjs.com/package/@stamp/it) - the same as the compose function above, but provides some additional nicer API
* [`stampit`](https://www.npmjs.com/package/stampit) - the same as the `@stamp/it` above, but optimized for browsers

You can use both `stampit`s same way as if it was the `compose` function. But you cannot use `compose` function same way as `stampit`. For example, these lines generate same stamp:

```js
const StampFromCompose = compose(Stamp1, Stamp2, Stamp3)
const StampFromStampit = stampit(Stamp1, Stamp2, Stamp3)
```

However, `stampit` adds few more handy APIs to your stamp:

```js
const NewStamp1 = StampFromStampit.methods({ myMethod () {} }) // add a method metadata using chaining API
const NewStamp2 = stampit({ props: { myProperty: 'my value' } }) // using "props" metadata shortcut
// etc
```

Whereas `compose` does not have the handy API:

```js
StampFromCompose.methods === undefined // THERE IS NO .methods CHAINING API
compose({ props: { myProperty: 'my value' } }) // WRONG! YOU MUST USE FULL `properties` KEY
```



