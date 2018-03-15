# Basics

## Creating Stamps

Stampit gives you several ways to create your objects.

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

Or you can pass _sampit-flavoured_ descriptor \(short version of the standard descriptor\).

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

Stampit has shortcut functions attached to it. For example:

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
const logger = require('bunyan').createLogger({name: 'log'})

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

You can chain

