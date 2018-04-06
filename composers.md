# Composers

Composers are callback functions executed each time a composition happens. There can be as many as you wish composers on each stamp.

> Composers are a very powerful feature. If you can implement something without composers then it's a good idea to avoid them.

It is highly recommended to **not create new stamps** in the composers, but **mutate the given stamp** if necessary.

```js
const InstanceOf = stampit.composers(({ stamp, composables }) => {
  if (!stamp.compose.methods) stamp.compose.methods = {} // mutating stamp
  stamp.compose.methods.getStamp = () => stamp           // mutating stamp

  Object.defineProperty(stamp, Symbol.hasInstance, {     // mutating stamp
    value(obj) {
      return obj && typeof obj.getStamp === 'function' && obj.getStamp() === stamp
    }
  })
})

console.log(InstanceOf() instanceof InstanceOf)     // true
console.log(InstanceOf().getStamp() === InstanceOf) // true

const Stamp = InstanceOf.props({ anything: 1 })
console.log(Stamp() instanceof Stamp)               // true
console.log(Stamp().getStamp() === Stamp)           // true

console.log(Stamp() instanceof InstanceOf)          // false
console.log(InstanceOf() instanceof Stamp)          // false
```

The above `InstanceOf` stamp overrides the default `instanceof` behavior using ES6 will-known symbol `Symbol.hasInstance`. Compose it with your stamps if you need the `instanceof` to work as expected for you.

## Descriptor merging algorithm

The composers are concatenated into a deduplicated array. As the result, the order of composition becomes **the order of composer execution**.

```js
const {composers} = stampit

const Log1 = composers(() => console.log(1))
const Log2 = composers(() => console.log(1))
const Log3 = composers(() => console.log(1))

const MultiLog = stampit(Log1, Log2, Log3)

MultiLog() // Will print three times:
// 1
// 1
// 1

// because there are 3 composers
MultiLog.compose.composers.length === 3
```

Stamps remove duplicate composers.

```js
const {composers} = stampit

const func = () => console.log(1)
const Log1 = composers(func)
const Log2 = composers(func)
const Log3 = composers(func)

const MultiLog = stampit(Log1, Log2, Log3)

MultiLog() // Will print only once:
// 1

// because there is only one composer
MultiLog.compose.composers.length === 1
```

## Composer arguments

Every composer always receive this object: `{ stamp, composables }`. Where:

* `stamp` is the stamp which was used to create this object. Useful to retrieve stamp's metadata \(aka descriptor\).
* `composables` is the array of stamps and descriptors the above stamp was composed with.

## Other ways to add composers

Exactly the same stamp can be created in few other ways. Here they all are.

```js
function myDebugComposer ({ stamp, composables }) {
  console.log(`Stamp ${stamp} was composed of ${composables}`)
}

const Logger = stampit({
  composers: myDebugComposer
})

const Logger = stampit({
  composers: [myDebugComposer]
})

const Logger = stampit.composers(myDebugComposer)
const Logger = stampit.composers([myDebugComposer])

const Logger = stampit().composers(myDebugComposer)
const Logger = stampit().composers([myDebugComposer])
```



