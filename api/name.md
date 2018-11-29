# Name

By default all the stamps name is "Stamp".

```javascript
const MyStamp = stampit({})

console.log(MyStamp.name) // "Stamp"
```

But you can change it by passing the `name` property to stampit:

```javascript
const MyStamp = stampit({ name: "MyFactoryFunction" })

console.log(MyStamp.name) // "MyFactoryFunction"
```

This metadata does not have a chaining method. This is illegal: `MyStamp.name("MyFactoryFunction")`.

### **Gotchas**

* This feature is not part of the `compose` [specification](../essentials/specification/).
* Won’t work in ES5 environments \(like IE11\). The name will always be _“Stamp”_. Name of a function can be set only in &gt;=ES6 environments.
* This code doesn’t work in JavaScript in general: `SmsGateway.name = “bla”`. Because `Function.name` is a special protected property.
* If any of the stamps you compose have a name then all the derived stamps will have it too.
* The only way to clear the name \(reset to the defaults\) is to set the name to _"Stamp"_.

```javascript
MyStamp = MyStamp.compose({ name: "Stamp" })
```

