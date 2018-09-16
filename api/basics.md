# Basics

## Creating Stamps

Stampit gives you several ways to create your stamps.

> _**NOTE!**_
>
> Any time you compose - you create a **NEW** stamp. Every function below always returns a **NEW** stamp.

### Pass plain descriptor

You can pass standard [stamp descriptor](../essentials/what-is-a-stamp.md#stamps-metadata-descriptor) to stampit.

```javascript
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
  deepConfiguration: ...,
  name: ...
}

const MyStamp = stampit(descriptor) // passing the descriptor
```

Or you can pass _sampit-flavoured_ descriptor \(shorter version of the standard descriptor\).

```javascript
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
  deepConf: ...,
  name: ...
}

const MyStamp = stampit(shorterDescriptor)
```

### Shortcut functions

Stampit has 18 shortcut functions attached to it. For example:

```javascript
const { props, methods, init } = stampit
```

You can create stamps from them too.

```javascript
const DefaultFoo = props({ foo: null })
const PrintFoo = methods({ printFoo() { console.log(this.foo) } })
const PassFoo = init({ foo }) { this.foo = foo })

const Foo = DefaultFoo.compose(PrintFoo, PassFoo)
```

The full list of the shortcut functions matches the list of keys you can pass as stamp descriptor \(see above\).

```javascript
const {     // destructuring
  methods,

  props, properties,

  deepProps, deepProperties,

  init, initializers,

  propertyDescriptors,

  statics, staticProperties,

  deepStatics, staticDeepProperties,

  staticPropertyDescriptors,

  conf, configuration,

  deepConf, deepConfiguration,

  composers
} = stampit
```

### Creating same stamp in few ways

All the examples below create _exactly the same stamp_.

#### Classic way

```javascript
const logger = require('bunyan').createLogger({ name: 'log' })

const HasLog = stampit({
  properties: {
    log: logger
  }
})
```

#### Classic way using shorter API

```javascript
const HasLog = stampit({
  props: {
    log: logger
  }
})
```

#### Shortcut way

```javascript
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

```javascript
const HasLog = stampit().props({
  log: logger
})
```

### Chaining

You can chain all the shortcut functions \(see the full list above\).

Below is just an example of what you can do. It is a utility stamp. If you compose it to any of your stamps then it will _count the number of object instances_ created from your stamp. Feel free to remove unwanted parts.

{% code-tabs %}
{% code-tabs-item title="InstanceCounter.js" %}
```javascript
const InstanceCounter = stampit()
.conf({
  instanceCounter: 0 // number of instances of a particular stamp, incremental
})
.props({
  instanceIndex: -1 // an instance number, incremental
})
.methods({
  printInstanceIndex() {
    console.log('I am instance #', this.instanceIndex)
  } 
})
.init(function (_, { stamp }) {
  this.instanceIndex = stamp.compose.configuration.instanceCounter // instance number
  stamp.compose.configuration.instanceCounter += 1 // increment the counter
})
.staticPropertyDescriptors({ // give the Stamp.name a non-default name. ES6 only.
  name: { value: 'InstanceCounter' }
})
.statics({ 
  printTotalInstanceCount() {
    console.log(this.compose.configuration.instanceCounter)
  } 
})
.composers(function ({ stamp }) {
  // We need to reset the counter each time the InstanceCounter is composed with.
  stamp.compose.configuration.instanceCounter = 0
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}

