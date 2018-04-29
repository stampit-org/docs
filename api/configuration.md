# Configuration

Configuration is a storage of an arbitrary data in the `.configuration` metadata of stamp's descriptor.

```javascript
let HaveApiKey = stampit({
  conf: {
    apiKey: process.env.API_KEY
  }
})

HaveApiKey.compose.configuration.apiKey === process.env.API_KEY
```

You can set configuration in [static methods](https://github.com/stampit-org/docs/tree/cb1b11dcef3e3b0b3aa5212adcf9047a2f882b06/static-properties.md):

```javascript
HaveApiKey = HaveApiKey.statics({
  setApiKey(key) {
    return this.conf({ apiKey: key }) // create new stamp by composing parent stamp with some configuration
  }
})

HaveApiKey = HaveApiKey.setApiKey('abcd1234')

HaveApiKey.compose.configuration.apiKey === 'abcd1234'
```

You can access the configuration in stamp [initializers](https://github.com/stampit-org/docs/tree/cb1b11dcef3e3b0b3aa5212adcf9047a2f882b06/initializers.md) \(aka constructors\).

```javascript
const ApiKeyPrinter = HaveApiKey.init(function (opts, { stamp }) {
  const configuration = stamp.compose.configuration

  console.log(configuration.apiKey) // It is perfectly safe to log secret API keys. Right?
})

HaveApiKey() // this will execute the initializer, and will print "abcd1234" to the console
```

## Descriptor merging algorithm

The configuration properties are copied **by assignment**. In other words - **by reference **using `Object.assign`.

```javascript
HaveApiKey.compose.configuration.apiKey === ApiKeyPrinter.compose.configuration.apiKey
```

In case of conflicts the last composed property wins.

## Other ways to add configuration

Exactly the same stamp can be created in few ways. Here they all are.

```javascript
const HasLog = stampit({
  conf: {
    apiKey: process.env.API_KEY
  }
})

const HasLog = stampit({
  configuration: {
    apiKey: process.env.API_KEY
  }
})

const HasLog = stampit.conf({
  apiKey: process.env.API_KEY
})

const HasLog = stampit.configuration({
  apiKey: process.env.API_KEY
})

const HasLog = stampit().conf({
  apiKey: process.env.API_KEY
})

const HasLog = stampit().configuration({
  apiKey: process.env.API_KEY
})
```

