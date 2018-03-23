# Static properties

Static properties are properties attached to the stamp itself.

```js
const PrintMyself = stampit({
  statics: {
    printMyself() {
      console.log(this)
    }
  }
})

PrintMyself.printMyself()
```

If you compose the stamp above with any other stamp, then it will have the `printMyself` method too.

```js
const Server = stampit(PrintMyself, {
  ...
})

Server.printMyself()
```

> Fun fact
>
> Stampit's chain methods are all regular static properties: Stamp.props\(\).methods\(\).init\(\).statics\(\).conf\(\)... etc



