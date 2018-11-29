# Static property descriptors

Static property descriptors are [standard JavaScript property descriptors](https://mdn.io/defineProperties). They are applied last thing when a stamp is being composed.

```javascript
let MyStamp = stampit() // empty stamp creates empty objects

MyStamp.name === 'Stamp' // every stamp default name is "Stamp"

MyStamp = MyStamp.staticPropertyDescriptors({
  name: { value: 'MyStamp' } // this meta data will be applied just after composition, i.e. immediately
})

MyStamp.name === 'MyStamp'
```

The code above adds some more metadata to the `MyStamp` stamp. It overwrites function name. \(Property descriptors is the only way to change a function name.\)

{% hint style="info" %}
NOTE

The `stampit` and `@stamp/it` modules have the ["name" feature](name.md) built in.

```text
const MyStamp = stampit({ name: 'MyStamp' })
MyStamp.name === 'MyStamp'
```
{% endhint %}

## Other ways to add static property descriptors

Exactly the same stamp can be created in few ways. Here they all are.

```javascript
const myStampName = {
  name: { value: 'MyStamp }
}

const NamedStamp = stampit({
  staticPropertyDescriptors: myStampName
})

const NamedStamp = stampit.staticPropertyDescriptors(myStampName)

const NamedStamp = stampit().staticPropertyDescriptors(myStampName)
```

