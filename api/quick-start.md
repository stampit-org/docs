# Quick start

## Quick start

[Install](../essentials/installation.md) the JavaScript module from the NPM registry.

```bash
npm i @stamp/it
# or
npm i stampit
```

Create an empty stamp.

```javascript
const stampit = require('@stamp/it')

let Stamp = stampit() // creates new stamp
```

[Compose](../essentials/what-is-a-stamp.md#composing-stamps) it with another stamp.

```javascript
const HasLog = require('./HasLog')

Stamp = Stamp.compose(HasLog) // creates a new stamp composed from the two
```

Add default [properties](properties.md) to it.

```javascript
Stamp = Stamp.props({ // creates a new derived stamp
  S3: require('aws-sdk').S3
})
```

Add an [initializer](initializers.md) \(aka constructor\).

```javascript
Stamp = Stamp.init(function ({ bucket }, { stamp }) { // creates a new derived stamp
  this.s3instance = new this.S3({ 
    apiVersion: '2006-03-01', 
    params: { Bucket: bucket || stamp.compose.configuration.bucket } // using configuration.bucket, see below
  })
})
```

Add a [method](methods.md).

```javascript
Stamp = Stamp.methods({ // creates a new derived stamp
  upload({ fileName, stream }) {
    this.log.info({ fileName }, 'Uploading file') // .log property from the HasLog stamp

    return this.s3instance.upload({ Key: fileName, Body: stream }).promise() // see above
  }
})
```

Add a [configuration](configuration.md).

```javascript
Stamp = Stamp.conf({ // creates a new derived stamp
  bucket: process.env.UPLOAD_BUCKET
})
```

Add a [static](static-properties.md) method.

```javascript
Stamp = Stamp.statics({ // creates a new derived stamp
  setDefaultBucket(bucket) {
    return this.conf({ bucket })
  }
})
```

Make the `.s3instance` , `.log`, and `.S3` properties [private](../ecosystem/stamp-privatize.md).

```javascript
const Privatize = require('@stamp/privatize')

Stamp = Stamp.compose(Privatize) // creates a new derived stamp
```

Give it a proper name.

```javascript
const FileStore = Stamp
```

Create objects from your stamp.

```javascript
const store = FileStore()
```

### Shorter API

Here is the same `FileStore` stamp but written in a more concise way.

{% code-tabs %}
{% code-tabs-item title="S3FileStore.js" %}
```javascript
const HasLog = require('./HasLog')
const Privatize = require('@stamp/privatize')

const FileStore = HasLog.compose(Privatize, {
  props: {
    S3: require('aws-sdk').S3
  },
  methods: {
    upload({ fileName, stream }) {
      this.log.info({ fileName }, 'Uploading file')
      return this.s3instance.upload({ Key: fileName, Body: stream }).promise()
    }
  },
  init({ bucket }, { stamp }) {
    this.s3instance = new this.S3({ 
      apiVersion: '2006-03-01', 
      params: { Bucket: bucket || stamp.compose.configuration.bucket }
    })
  },
  conf: {
    bucket: process.env.UPLOAD_BUCKET
  },
  statics: {
    setDefaultBucket(bucket) {
      return this.conf({ bucket })
    }
  }
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Using the stamp

Use your stamp ad-hoc.

```javascript
async function uploadTo(req, res) {
  const fileStore = FileStore({ bucket: req.params.bucket }) // create instance

  await fileStore.upload({ fileName: req.query.file, stream: req }) // use the method of the stamp

  res.sendStatus(201)
}
```

Or preconfigure it.

```javascript
const CatGifStore = FileStore.setDefaultBucket('cat-gifs') // pre-configuring the bucket name

const catGifStore = CatGifStore() // create an instance of the store

await catGifStore.upload({ fileName: 'cat.gif', stream: readableStream })
```

If you want to silence the shameful fact that you are collecting cat gifs then here is how you disable log. Just overwrite the `log` property with a silent one.

```javascript
const SilentCatGifStore = CatGifStore.props({
  log: {
    info() {} // silence!
  }
})

await SilentCatGifStore().upload({ fileName: 'cat.gif', stream: readableStream })
```

Alternatively, you can have a generic silent logger stamp. Compose with it to override the `HasLog`.

```javascript
const HasSilentLog = stampit.props({ log: { info: () => {} } })

const SilentCatGifStore = CatGifStore.compose(HasSilentLog)
```

The `new` keyword also works with any stamp.

```javascript
const catGifStore = new CatGifStore()
```

Another way to create an object is to use `.create()` static method.

```javascript
const catGifStore = CatGifStore.create()
```

## Mocking I/O in unit tests

Replacing actual `S3` with a fake one.

```javascript
const MockedFileStore = FileStore.props({
  S3() {
    return {
      upload() {
        return { promise() { return Promise.resolve() } }
      }
  }
})

// Same as above but a one-liner:

const MockedFileStore = FileStore.props({
  S3: () => ({ upload: () => ({ async promise() {} }) })
})
```

## Basic API

Head to the [Basics](basics.md) page to understand what other API there are.

