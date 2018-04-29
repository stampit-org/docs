# Property descriptors

Property descriptors are [standard JavaScript property descriptors](https://mdn.io/defineProperties). They are applied last thing when an object instance is being created. See more info in [stamp internals](../essentials/specification/object-creation-internals.md).

```javascript
const Key = stampit.props({
  key: 'my secret key'
})

const auth = Key()
auth.key === 'my secret key'
auth.key = 'ha ha ha'
auth.key === 'ha ha ha' // oops :(

const FrozenKey = stampit.propertyDescriptors(Key, { // composing the two
  key: { // this is a standard JavaScript property descriptor
    configurable: false,
    writable: false,
  }
})

const auth = FrozenKey()
auth.key === 'my secret key'
auth.key = 'ha ha ha'
auth.key === 'my secret key' // OK!!! The "key" was not overwritten.
```

The code above adds some more metadata to the `Key` stamp. It protects the `key` property from overwrites

## Other ways to add property descriptors

Exactly the same stamp can be created in few ways. Here they all are.

```javascript
const keyPropertyDescriptor = {
  key: {
    configurable: false,
    writable: false,
  }
}

const FrozenKey = stampit({
  propertyDescriptors: keyPropertyDescriptor
})

const FrozenKey = stampit.propertyDescriptors(keyPropertyDescriptor)

const FrozenKey = stampit().propertyDescriptors(keyPropertyDescriptor)
```

