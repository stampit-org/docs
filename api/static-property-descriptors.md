# Static property descriptors

Static property descriptors are [standard JavaScript property descriptors](https://mdn.io/defineProperties). They are applied last thing when an stamp is being composed.

```javascript
let MyStamp = stampit() // empty stamp creates empty objects

MyStamp.name === 'Stamp' // every stamp default name is "Stamp"

MyStamp = MyStamp.staticDeepProperties({
  name: { value: 'MyStamp' } // this meta data will be applied just after composition, i.e. immediately
})

MyStamp.name === 'MyStamp'
```

The code above adds some more metadata to the `MyStamp` stamp. It overwrites function name. \(Property descriptors is the only way to change a function name.\)

> NOTE
>
> Use the [@stamp/named](../ecosystem/stamp-named.md) utility stamp to name your stamps.

## Other ways to add static property descriptors

Exactly the same stamp can be created in few ways. Here they all are.

```javascript
const myStampName = {
  name: { value: 'MyStamp }
}

const NamedStamp = stampit({
  staticDeepProperties: myStampName
})

const NamedStamp = stampit.staticDeepProperties(myStampName)

const NamedStamp = stampit().staticDeepProperties(myStampName)
```

