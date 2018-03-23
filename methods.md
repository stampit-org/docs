# Methods

You can add methods \(aka _functions_\) to the objects created from your stamps.

```js
const Logger = stampit({
  methods: {
    debug(str) {
      console.log(str)
    }
  }
})

const logger = Logger()

logger.debug('Server started')
```

All the methods are **attached to object's prototype**.

```js
logger.__proto__.debug === Logger.compose.methods.debug
```

Moreover, the entire `methods` [descriptor metadata](/composition.md) becomes the prototype.

```js
logger.__proto__ === Logger.compose.methods
```

If you compose the stamp above with any other stamp, then object instances created from it will also have the `.debug` method.

```js
const Server = Logger.compose({
  methods: {
    async start() {
      this.debug('Server started')
    }
  }
})

const server = Server()

server.debug('Starting server')
server.start()
```

The methods are copied **by assignment**. In other words - **by reference **using `Object.assign`.

```js
Logger().debug === Server().debug
```

In case of conflicts the last composed method wins. To avoid method collision use the [@stamp/collision](/stampcollision.md) stamp. To make sure a method is implemented use the [@stamp/required](/stamprequired.md) stamp.

## Other ways to add methods

Exactly the same stamp can be created in few other ways. Here they all are.

```js
const Logger = stampit({
  methods: {
    debug(str) {
      console.log(str)
    }
  }
})

const Logger = stampit.methods({
  debug(str) {
    console.log(str)
  }
})

const Logger = stampit().methods({
  debug(str) {
    console.log(str)
  }
})
```



