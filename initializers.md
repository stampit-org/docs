# Initializers

You can add initializers \(aka _constructors_\) to your stamps.

```js
const Logger = stampit({
  init({level = 50}) {
    this.level = level
  }
})

const logger = Logger({ level: 42 })

logger.level === 42
```

**Each initializer will be executed** on object creation.

```js
const Server = stampit(Logger, {
  init({ port = process.env.PORT }) {
    this.port = port
  }
})

const server = Server({ port: 6666 })

server.level === 50
server.port === 6666
```

If you return anything from an initializer then it becomes the object instance.

```js
const NullServer = Server.init(function () {
  return null
})

const server = NullServer()

server === null
```

The initializer are concatenated into a deduplicated array. As the result, the order of composition becomes the order of initializer execution.

```js
const {init} = stampit

const Log1 = init(() => console.log(1))
const Log2 = init(() => console.log(1))
const Log3 = init(() => console.log(1))

const MultiLog = stampit(Log1, Log2, Log3)

MultiLog() // Will print three times:
// 1
// 1
// 1
```

Stamps remove duplicate initializers.

```js
const {init} = stampit

const func = () => console.log(1)
const Log1 = init(func)
const Log2 = init(func)
const Log3 = init(func)

const MultiLog = stampit(Log1, Log2, Log3)

MultiLog() // Will print only once:
// 1
```

## Initializer arguments

You can pass multiple parameters to your stamp while creating an object. First parameter passed as is to all initializers. But the rest of the parameters are available inside the `{args}` property of the second initializer argument.

```js
const MultiArg = stampit({
  init(arg1, { args }) {
    arg1 === 'foo'
    args[0] === arg1
    args[0] === 'foo'
    args[1] === 'BAR'
    args[2] === 'thing'
  }
})

MultiArg('foo', 'BAR', 'thing')
```

If there is no first parameter then an empty object is passed as the first argument.

```js
const NoException = stampit({
  init({ iAmUndefined }) { // won't throw exception in this line
    iAmUndefined === undefined
    console.log(arguments[0]) // will print empty object: {}
  }
})

NoException(/* nothing here! */)
```

Every initializer second argument is always this object: `{ stamp, args, instance }`. Where:

* `stamp` is the stamp which was used to create this object.
* `args` the parameters passed to the stamp while creating the object.
* `instance` the object instance itself. Always equals `this` context of the initializer.

```js
const PrintMyArgs = stampit({
  init(_, { stamp, args, instance }) {
    console.log('Creating object from this stamp:', stamp)
    console.log('List of arguments:', args)
    console.log('Object instance:', instance)
    
    this === instance
  }
})
```

## Other ways to add initializers

Exactly the same stamp can be created in few other ways. Here they all are.

```js
function myInitializer ({ level = 50 }) {
  this.level = level
}

const Logger = stampit({
  init: myInitializer
})

const Logger = stampit({
  init: [myInitializer]
})

const Logger = stampit({
  initializers: myInitializer
})

const Logger = stampit({
  initializers: [myInitializer]
})

const Logger = stampit.init(myInitializer)
const Logger = stampit.init([myInitializer])
const Logger = stampit.initializers(myInitializer)
const Logger = stampit.initializers([myInitializer])

const Logger = stampit().init(myInitializer)
const Logger = stampit().init([myInitializer])
const Logger = stampit().initializers(myInitializer)
const Logger = stampit().initializers([myInitializer])
```



