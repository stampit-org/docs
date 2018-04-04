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

PrintMyself.printMyself() // call the function without creating an instance of the stamp
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

Often static methods are used to tweak a stamp.

```js
const HasFactor = stampit()
.statics({
  allowFactorSetter(allow) {
    return this.conf({ HasFactor: { allowSetter: allow } }) // tweaking the stamp
  }
})
.init(({ factor }, { instance, stamp }) => {
  let factor = factor || 1; // private (scoped) variable
  instance.getFactor = () => factor;

  const { configuration } = stamp.compose
  if (configuration && configuration.HasFactor && configuration.HasFactor.addFactorSetter) {
    instance.setFactor = f => { factor = f }
  }
})
```

The example above exposes a static function which controls access to the `setFactor` method of your object instances.

```js
console.log(HasFactor().setFactor); // undefined

const HasFactorAllowed = HasFactor.allowFactorSetter(true);
const instance = HasFactor2()
instance.setFactor(5); // we have access to the setter!
console.log(instance.getFactor()); // 5
```



