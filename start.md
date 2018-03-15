# Quick start

```bash
npm i @stamp/it
```

Create an empty stamp.

```js
const stampit = require('@stamp/it')

let Stamp = stampit() // creates new stamp
```

Compose it with another stamp.

```js
const HasLog = require('./HasLog')

Stamp = Stamp.compose(HasLog) // creates a new stamp composed from the two
```

Add default properties to it.

```js
Stamp = Stamp.props({ // creates a new derived stamp
  S3: require('aws-sdk').S3
})
```

Add a method.

```js
Stamp = Stamp.methods({ // creates a new derived stamp
  upload({ fileName, stream }) {
    this.log.info({ fileName }, 'Uploading file')

    return this.s3instance.upload({ Key: fileName, Body: stream }).promise()
  }
})
```

Add an initializer \(aka constructor\).

```js
Stamp = Stamp.init(function ({ bucket }, { stamp }) {
  this.s3instance = new this.S3({ 
    apiVersion: '2006-03-01', 
    params: { Bucket: bucket || stamp.compose.configuration.bucket }
  })
})
```

Add a configuration.

```js
Stamp = Stamp.conf({
  bucket: process.env.UPLOAD_BUCKET
})
```

Add a static method.

```js
Stamp = Stamp.statics({
  setDefaultBucket(bucket) {
    return this.conf({ bucket })
  }
})
```

Make the `.s3instance` , `.log`, and `.S3` properties private.

```js
const Privatize = require('@stamp/privatize')

Stamp = Stamp.compose(Privatize)
```

Give it a proper name.

```js
const FileStore = Stamp
```

## Using the stamp

Use your stamp ad-hoc.

```js
function uploadTo(req, res, next) {
  FileStore({ bucket: req.params.bucket }).upload({ fileName: req.query.file, stream: req })
  .then(() => res.sendStatus(201))
  .catch(next)
}
```

or

```js
let CatGifStore = FileStore.setDefaultBucket('cat-gifs')

const catGifStore = CatGifStore() // create an instance of the store

await catGifStore.upload({ fileName: 'cat.gif', stream: readableStream })
```

If you want to silence the shameful fact that you are collecting cat gifs then here is how you disable logs.

```js
const SilentCatGifStore = CatGifStore.props({
  log: {
    info() {} // silence!
  }
})

await SilentCatGifStore().upload({ fileName: 'cat.gif', stream: readableStream })
```

The `new` keyword also works with any stamp.

```js
const catGifStore = new CatGifStore()
```

Another way to create an object is to use `.create()` static method.

```js
const catGifStore = CatGifStore.create()
```

## Shorter API

Here is the same `FileStore` stamp but written in a more concise way.

```js
const FileStore = require('./HasLog').compose(require('@stamp/privatize'), {
  conf: {
    bucket: process.env.UPLOAD_BUCKET
  },
  statics: {
    setDefaultBucket(bucket) {
      return this.conf({ bucket })
    }
  },
  props: {
    S3: require('aws-sdk').S3
  },
  init({ bucket }, { stamp }) {
    this.s3instance = new this.S3({ 
      apiVersion: '2006-03-01', 
      params: { Bucket: bucket || stamp.compose.configuration.bucket }
    })
  },
  methods: {
    upload({ fileName, stream }) {
      this.log.info({ fileName }, 'Uploading file')
      return this.s3instance.upload({ Key: fileName, Body: stream }).promise()
    }
  }
})
```



