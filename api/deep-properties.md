# Deep properties

The deep properties add properties to the objects created from your stamps. These are default properties of your objects. But unlike regular \(shallow\) properties these are deeply merged together by stamp [deep merging algorithm](../essentials/specification/merging-algorithm.md#deep-merging-algorithm).

```javascript
const AuthServiceToken = stampit.deepProps({
  Secrets: {
    authServiceToken: process.env.AUTH_SERVICE_TOKEN
  }
})

const S3KeyIdAndSecret = stampit.deepProps({
  Secrets: {
    S3: {
      keyId: process.env.AWS_ACCESS_KEY_ID,
      secret: process.env.AWS_SECRET_ACCESS_KEY
    }
  }
})

const DefaultSecrets = stampit(AuthServiceToken, S3KeyIdAndSecret) // composing them together

DefaultSecrets().Secrets.authServiceToken === process.env.AUTH_SERVICE_TOKEN
DefaultSecrets().Secrets.S3.keyId === process.env.AWS_ACCESS_KEY_ID
DefaultSecrets().Secrets.S3.secret === process.env.AWS_SECRET_ACCESS_KEY
```

## Descriptor merging algorithm

The deepProperties are **deeply merged** using stamp [deep merging algorithm](../essentials/specification/merging-algorithm.md#deep-merging-algorithm). See below - the `Secrets` value is always a new object.

```javascript
S3KeyIdAndSecret().Secrets !== DefaultSecrets().Secrets // NEVER EQUAL! NO MATTER WHAT!
```

Sometimes a regular \(shallow\) property has the same name as a deep property. While creating an object instance the shallow property wins. See [stamp internals](../essentials/specification/object-creation-internals.md).

```javascript
const NullSecrets = stampit.props({
  Secrets: null
})

const AuthServiceToken = stampit.deepProps({
  Secrets: {
    authServiceToken: process.env.AUTH_SERVICE_TOKEN
  }
})

const Result = NullSecrets.compose(AuthServiceToken)

Result().Secrets === null // the regular property overwrites deep property while creating an object
```

## Other ways to add deep properties

Exactly the same stamp can be created in few ways. Here they all are.

```javascript
const mySecrets = { authServiceToken: process.env.AUTH_SERVICE_TOKEN }

const AuthServiceToken = stampit({
  deepProps: {
    Secrets: mySecrets
  }
})

const HasLog = stampit({
  deepProperties: {
    Secrets: mySecrets
  }
})

const HasLog = stampit.deepProps({
  Secrets: mySecrets
})

const HasLog = stampit.deepProperties({
  Secrets: mySecrets
})

const HasLog = stampit().deepProps({
  Secrets: mySecrets
})

const HasLog = stampit().deepProperties({
  Secrets: mySecrets
})
```

