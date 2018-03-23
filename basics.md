# Basics

## Creating Stamps

Stampit gives you several ways to create your objects.

> _**NOTE!**_
>
> Any time you compose - you create a **NEW** stamp. Every function below always returns a **NEW** stamp.

### Pass plain descriptor

You can pass standard [stamp descriptor](/composition.md) to stampit.

```js
const descriptor = {
  methods: ...,
  properties: ...,
  deepProperties: ...,
  propertyDescriptors: ...,
  initializers: ...,
  staticProperties: ...,
  staticDeepProperties: ...,
  staticPropertyDescriptors: ...,
  composers: ...,
  configuration: ...,
  deepConfiguration: ...
}

const MyStamp = stampit(descriptor)
```

Or you can pass _sampit-flavoured_ descriptor \(shorter version of the standard descriptor\).

```js
const shorterDescriptor = {
  methods: ...,
  props: ...,
  deepProps: ...,
  propertyDescriptors: ...,
  init: ...,
  statics: ...,
  staticDeepProps: ...,
  staticPropertyDescriptors: ...,
  composers: ...,
  conf: ...,
  deepConf: ...
}

const MyStamp = stampit(shorterDescriptor)
```

### Shortcut functions

Stampit has 18 shortcut functions attached to it. For example:

```js
const { props, methods, init } = stampit
```

You can create stamps from them too.

```js
const DefaultFoo = props({ foo: null })
const PrintFoo = methods({ printFoo() { console.log(this.foo) } })
const PassFoo = init({ foo }) { this.foo = foo })

const Foo = DefaultFoo.compose(PrintFoo, PassFoo)
```

The full list of the shortcut functions matches the list of keys you can pass as stamp descriptor \(see above\).

```js
const {
  methods,

  props,
  properties,

  deepProps,
  deepProperties,

  init,  
  initializers,

  propertyDescriptors,

  statics,
  staticProperties,

  deepStatics,
  staticDeepProperties,

  staticPropertyDescriptors,

  conf,
  configuration,

  deepConf,
  deepConfiguration,

  composers
} = stampit
```

### Creating same stamp in few ways

All the examples below create _exactly the same stamp_.

#### Classic way

```js
const logger = require('bunyan').createLogger({ name: 'log' })

const HasLog = stampit({
  properties: {
    log: logger
  }
})
```

#### Classic way using shorter API

```js
const HasLog = stampit({
  props: {
    log: logger
  }
})
```

#### Shortcut way

```js
const HasLog = stampit.props({
  log: logger
})

// OR

const { props } = stampit

const HasLog = props({
  log: logger
})
```

#### Composition way

```js
const HasLog = stampit().props({
  log: logger
})
```

### Chaining

You can chain all the shortcut functions \(see list above\).

```js
const InstanceCounter = stampit()
.conf({
  instanceCounter: 0 // number of instances of a particular stamp
})
.props({
  instanceIndex: -1 // an instance number, incremental
})
.methods({
  printInstanceIndex() { console.log('I am instance #', this.instanceIndex) } 
})
.init(function ({ foo }, { stamp }) {
  this.instanceIndex = stamp.compose.configuration.instanceCounter // assign instance number
  stamp.compose.configuration.instanceCounter += 1 // increment the counter
})
.staticPropertyDescriptors({ 
  name: { value: 'InstanceCounter' }
})
.statics({ 
  printTotalInstanceCount() { console.log(this.compose.configuration.instanceCounter) } 
})
.composers(function ({ stamp }) {
  // We need to reset the counter each time the InstanceCounter is composed with.
  stamp.compose.configuration.instanceCounter = 0
})
```

The stamp above is a utility stamp. If you compose it to any of your stamps it will _count the number of object instances_ created from your stamp.

