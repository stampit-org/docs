# Stamp object creation internals

This is a plain factory function. It creates and returns a plain object.

```js
function myFactory () {
  return { myProperty: 'my value' }
}
```

But Stamps are a little different. They carry around the metadata about the object they are going to produce. The metadata is attached to the `Stamp.compose` object:

```js
function Stamp () {
  const metadata = Stamp.compose // retrieving metadata from itself

  ... SNIP ...

  return newObject
}
```

There are 11 [standardized](/specification.md) types of metadata, such as **properties** and **deepProperties**.

```js
function Stamp () {
  const metadata = Stamp.compose // retrieving metadata from itself

  const properties = metadata.properties // retrieving properties from the metadata
  let deepProperties = metadata.deepProperties // retrieving deepProperties from the metadata
  deepProperties = JSON.parse(JSON.stringify(deepProperties)) // we need to clone deepProperties
  const newObject = Object.assign({}, deepProperties, properties) // creating new object using the properties

  ... SNIP ...

  return newObject
}
```

There is also **methods** object which becomes the new object's prototype.

```js
function Stamp () {
  const metadata = Stamp.compose // retrieving metadata from itself

  const properties = metadata.properties // retrieving properties from the metadata
  let deepProperties = metadata.deepProperties // retrieving deepProperties from the metadata
  deepProperties = JSON.parse(JSON.stringify(deepProperties)) // we need to clone deepProperties
  const newObject = Object.assign({}, deepProperties, properties) // creating new object using the properties

  const methods = metadata.methods // retrieving methods from the metadata
  if (methods) Object.setPrototypeOf(newObject, methods) // assigning new object's prototype

  ... SNIP ...

  return newObject
}
```

The **propertyDescriptors** metadata is an object applied to the resulting object using [`Object.defineProperties`](https://mdn.io/defineProperties) JavaScript standard function.

```js
function Stamp () {
  const metadata = Stamp.compose // retrieving metadata from itself

  const properties = metadata.properties // retrieving properties from the metadata
  let deepProperties = metadata.deepProperties // retrieving deepProperties from the metadata
  deepProperties = JSON.parse(JSON.stringify(deepProperties)) // we need to clone deepProperties
  const newObject = Object.assign({}, deepProperties, properties) // creating new object using the properties

  const methods = metadata.methods // retrieving methods from the metadata
  if (methods) Object.setPrototypeOf(newObject, methods) // assigning new object's prototype

  Object.defineProperties(newObject, metadata.propertyDescriptors)

  ... SNIP ...

  return newObject
}
```

Last, but not least, the **initializers** are kind of constructors. With classic classes you would execute only one constructor when creating an object. With stamps you **execute **_**all**_** the initializers** of a stamp.

```js
function Stamp () {
  const metadata = Stamp.compose // retrieving metadata from itself

  const properties = metadata.properties // retrieving properties from the metadata
  let deepProperties = metadata.deepProperties // retrieving deepProperties from the metadata
  deepProperties = JSON.parse(JSON.stringify(deepProperties)) // we need to clone deepProperties
  const newObject = Object.assign({}, deepProperties, properties) // creating new object using the properties

  const methods = metadata.methods // retrieving methods from the metadata
  if (methods) Object.setPrototypeOf(newObject, methods) // assigning new object's prototype

  Object.defineProperties(newObject, metadata.propertyDescriptors)

  for (const init of metadata.initializers || []) { // run every initializer one by one
    const firstArg = argument[0]
    const secondArg = { args: [...arguments], instance: newObject, stamp: Stamp }
    const returnedValue = init.call(newObject, firstArg, secondArg)
    if (returnedValue !== undefined) {
      newObject = returnedValue
    }
  }

  return newObject
}
```

If an initializer returns a non-undefined value then it becomes the new object instance.

See [Stamp Specification](/specification.md) for more details.

> _NOTE_
>
> The code above is not the actual stampit code. It was optimized for readability purposes.





