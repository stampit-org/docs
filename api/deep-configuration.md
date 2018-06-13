# Deep configuration

Deep Configuration is a storage of an arbitrary data in the `.deepConfiguration` metadata of stamp's descriptor. But unlike regular \(shallow\) configuration these are deeply merged together by stamp [deep merging algorithm](../essentials/specification/merging-algorithm.md#deep-merging-algorithm).

```javascript
let Kue = stampit.deepConf({
  Kue: {
    name: 'kue',
    priority: 'normal',
    attempts: 3,
    delay: 500,
    ttl: 5,
    events: false
  }
})

const HighPriorityKue = stampit.deepConf({
  Kue: {
    priority: 'high'
  }
})

const MyKue = stampit(Kue, HighPriorityKue) // composing them together

MyKue.compose.deepConfiguration.Kue.attempts === 3
MyKue.compose.deepConfiguration.Kue.priority === 'high'
```

You can set deep configuration in [static methods](static-properties.md):

```javascript
Kue = Kue.statics({
  configureKue(options) {
    return this.deepConf({Kue: options}) // create new stamp by composing parent stamp with some configuration
  },

  connectionString(connectionString) { return this.configureKue({connectionString} },
  priority(priority)                 { return this.configureKue({priority} },
  attempts(attempts)                 { return this.configureKue({attempts}) },
  delay(delay)                       { return this.configureKue({delay}) },
  ttl(ttl)                           { return this.configureKue({ttl}) },
  events(events)                     { return this.configureKue({events}) }
})

const MyRegularJobKue = Kue.attempts(5).delay(1000).ttl(10).events(true).priority('low')
```

You can access the configuration in stamp [initializers](initializers.md) \(aka constructors\).

```javascript
Kue = Kue.init(function (payload, { stamp }) {
  const conf = stamp.compose.deepConfiguration.Kue
  this.conf = conf
  this.redisClient = require('redis').createClient(conf.connectionString)
})
.methods({
  put(data) {
    return this.redisClient.put(this.queueName, { ...this.conf, data })
  }
})

const myKue = Kue()
myKue.put({ title: 'Vasyl Boroviak', email: 'vasyl@example.com', template: 'welcome-email' })
```

## Descriptor merging algorithm

The deepConfiguration is **deeply merged** using stamp [deep merging algorithm](../essentials/specification/merging-algorithm.md#deep-merging-algorithm).

See below - the `Kue` value is always a new object.

```javascript
Kue.compose.deepConfiguration.Kue !== MyKue.compose.deepConfiguration.Kue
// NEVER EQUAL! NO MATTER WHAT!
```

## Other ways to add deep configuration

Exactly the same stamp can be created in few ways. Here they all are.

```javascript
const myKueConf = { Kue: { priority: 'normal' } }

const MyKue = stampit({
  deepConf: {
    Kue: myKueConf
  }
})

const MyKue = stampit({
  deepConfiguration: {
    Kue: myKueConf
  }
})

const MyKue = stampit.deepConf({
  Kue: myKueConf
})

const MyKue = stampit.deepConfiguration({
  Kue: myKueConf
})

const MyKue = stampit().deepConf({
  Kue: myKueConf
})

const MyKue = stampit().deepConfiguration({
  Kue: myKueConf
})
```

