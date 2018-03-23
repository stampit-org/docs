# Properties

You can add properties to the objects created from your stamps.

```js
const HasLog = stampit({
  props: {
    log: require('bunyan').createLogger({ name: 'my logger' })
  }
})

const loggerObject = HasLog()
loggerObject.log.debug('I can log')
```

If you compose the stamp above with any other stamp, then object instances created from it will also have the `.log` property.

```js
const RequestHandler = stampit().methods({
  handle(req, res, next) {
    this.log.info({ originalUrl: req.originalUrl }, 'handling request') // using the .log
    res.sendStatus(200)
  }
})
.compose(HasLog) // composing with HasLog

const handler = RequestHandler()
handler.log.debug('Created a handler')
```

The properties are copied **by assignment**. In other words - **by reference **using `Object.assign`.

```js
HasLog().log === RequestHandler().log
```

In case of conflicts the last composed property wins.

```js
const MyValue = stampit.props({
  props: {
    myValue: 1
  }
})
.compose({
  props: {
    myValue: 2
  }
})
.compose({
  props: { 
    myValue: 3
  }
})

MyValue.compose.properties.myValue === 3

MyValue().myValue === 3
```

## Other ways to add properties

Exactly the same stamp can be created in few other ways. Here they all are.

```js
const HasLog = stampit({
  props: {
    log: require('bunyan').createLogger({ name: 'my logger' })
  }
})

const HasLog = stampit({
  properties: {
    log: require('bunyan').createLogger({ name: 'my logger' })
  }
})

const HasLog = stampit.props({
  log: require('bunyan').createLogger({ name: 'my logger' })
})

const HasLog = stampit.properties({
  log: require('bunyan').createLogger({ name: 'my logger' })
})

const HasLog = stampit().props({
  log: require('bunyan').createLogger({ name: 'my logger' })
})

const HasLog = stampit().properties({
  log: require('bunyan').createLogger({ name: 'my logger' })
})
```



