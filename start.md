# Quick start

```bash
npm i @stamp/it
```

Create an empty stamp.

```js
const stampit = require('@stamp/it')

let Stamp = stampit() // creates new stamp
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
  upload({fileName, stream}) {
    return this.s3.upload({Key: fileName, Body: stream}).promise()
  }
})
```

Add an initializer \(aka constructor\).

```js
Stamp = Stamp.init(function ({bucket}, {stamp}) {
  const bucket = bucket || stamp.compose.configuration.bucket
  this.s3 = new S3({ apiVersion: '2006-03-01', params: { Bucket: bucket } })
})
```

Add configuration.

```js
Stamp = Stamp.conf({
  bucket: process.env.UPLOAD_BUCKET
})
```

Add static method.

```js
Stamp = Stamp.statics({
  bucket(bucket) {
    return this.conf({bucket})
  }
})
```



