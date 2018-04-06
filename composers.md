# Composers

Composers are callback functions executed each time a composition happens. There can be as many as you wish composers on each stamp.

> Composers are a very powerful feature. If you can implement something without composers then it's a good idea to avoid them.

It is highly recommended to **not create new stamps** in the composers, but **mutate the given stamp**.

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



