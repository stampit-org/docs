# Configuration

Configuration is a storage of an arbitrary data. It is stored in the `.configuration` metadata of stamp's descriptor.

```js
let HaveApiKey = stampit({
  conf: {
    apiKey: process.env.API_KEY
  }
})

HaveApiKey.compose.configuration.apiKey === process.env.API_KEY
```

You can set it in [static methods](/static-properties.md):

```js
HaveApiKey = HaveApiKey.statics({
  setApiKey(key) {
    return this.conf({ apiKey: key }) // create new stamp by composing parent stamp with some configuration
  }
})

HaveApiKey = HaveApiKey.setApiKey('abcd1234')

HaveApiKey.compose.configuration.apiKey === 'abcd1234'
```

You can access the configuration in stamp [initializers](/initializers.md) \(aka constructors\).

```js
HaveApiKey = HaveApiKey.init(function (opts, { stamp }) {
  const configuration = stamp.compose.configuration
  
  console.log(configuration.apiKey) // It is perfectly safe to log secret API keys. Right?
})

HaveApiKey() // this will execute the initializer, and will print "abcd1234" to the console
```





