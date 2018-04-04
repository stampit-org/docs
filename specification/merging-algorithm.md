# Stamp merging algorithm

When composing stamps you actually merge their [descriptors](/descriptor.md) \(aka metadata\).

The `Object.assign` is used for these descriptor properties:

* [methods](/methods.md)
* [properties](/properties.md)
* propertyDescriptors
* [staticProperties](/static-properties.md)
* staticPropertyDescriptors
* [configuration](/configuration.md)

The special deep merging algorithm \(see below\) is used for these descriptor properties:

* deepProperties
* staticDeepProperties
* deepConfiguration

The array concatenation and deduplication \(see below\) is used for these descriptor properties:

* [initializers](/initializers.md)
* composers

## Array concatenation and deduplication

This is how initializers and composers metadata gets merged.

```js
const _ = require('lodash')

descriptor.initializers = _.uniq(_.concat(descriptor1.initializers, descriptor2.initializers))
descriptor.composers = _.uniq(_.concat(descriptor1.composers, descriptor2.composers))
```

See the [exact line](https://github.com/stampit-org/stamp-specification/blob/8a5448806da286b2f840c33cb618ddbcdb40182d/compose.js#L162) of the reference implementation source code.

## Deep merging algorithm

The [stamp specification](/specification.md) standardized the deep merging algorithm to, basically, this:

* Plain objects are \(recursively\) deep merged, including ES6 Symbol keys.
* **Arrays are concatenated**.
* Functions, Symbols, RegExp, etc. values are copied by reference.
* The last object type overwrites previous object type.

```js
const MyStamp1 = stampit()

const deepObject1 = {
  [Symbol('foo')]: { one: 'first' },                 // this plain object will be deep merged
  
  array: [0, 'bar', () => {}, { obj: 'my object' }], // this array will be concatenated with
  
  func: MyStamp1,                                    // this stamp will be replaced with another stamp
  
  something: [42],                                   // this array will be replaced with an object

  oldKey: 'some value'
}

const MyStamp2 = stampit()

const deepObject2 = {
  [Symbol('foo')]: { two: 'second' },
  
  array: [0, 'bar', { another: 'object' }],
  
  func: MyStamp2,
  
  something: { [0]: 42 },
  
  newKey: 'some value'
}
```

The merged result of the two objects above will be this:

```js
const deepResult = {
  [Symbol('foo')]: { one: 'first', two: 'second' }, // contains properties from both objects
  
  array: [0, 'bar', () => {}, { obj: 'my object' }, 0, 'bar', { another: 'object' }], // both arrays are here
  
  func: MyStamp2,                                   // Stamp2 replaced the Stamp1
  
  something: { [0]: 42 },                           // the array was replaced with the object
  
  oldKey: 'some value',                             // these two were simply carried across
  newKey: 'some value'
}
```

See the [exact line](https://github.com/stampit-org/stamp-specification/blob/master/compose.js#L70) of the reference implementation source code.

