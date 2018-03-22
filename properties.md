# Properties

You can add properties to your objects.

```js
const HasLog = stampit({
  props: {
    log: require('bunyan').createLogger({ name: 'my logger' })
  }
})

const logger = HasLog()
logger.log.debug('I can log')
```

If you compose the stamp above with any other stamp, then object instances created from it will have the `.log` property.

```js
const RequestHandler = stampit().methods({
  handle(req, res, next) {
    this.log.info({ originalUrl: req.originalUrl }, 'handling request') // using the .log
    res.sendStatus(200)
  }
})
.compose(HasLog)

const handler = RequestHandler()
handler.log.debug('Created a handler')
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

## Mechanics

In case of conflicts the last composed property wins.

```js
const MyValue = stampit({
  props: {
    myValue: 1
  }
})
.compose({
  props: {
    myValue: 2
  }
})
.compose(stampit.props({ 
  myValue: 3
}))

MyValue.compose.properties.myValue === 3

MyValue().myValue === 3
```

All the properties are **copied by reference** using `Object.assign`.

```js
const originalObject = { myValue: 0 }

const MyObject = stampit().props({
  myObject: originalObject
})

MyObject.compose.properties.myObject.myValue === 0

const obj = MyObject()
obj.myObject.myValue = 622 // changing the originalObject

MyObject.compose.properties.myObject.myValue === 622 // we have changed the originalObject
originalObject.myValue === 622 // we have changed the originalObject
```



