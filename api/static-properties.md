# Static properties

Static properties are properties attached to the stamp itself.

```javascript
const PrintMyself = stampit({
  statics: {
    myProperty: 'foo',

    printMyself() {
      console.log(this)
    }
  }
})

PrintMyself.myProperty === 'foo'

PrintMyself.printMyself() // call the function without creating an instance of the stamp
```

If you compose the stamp above with any other stamp, then it will have the `printMyself` method too.

```javascript
const Server = stampit(PrintMyself, {
  ...
})

Server.printMyself()
```

> Fun fact
>
> Stampit's chain methods are all regular static properties: Stamp.props\(\).methods\(\).init\(\).statics\(\).conf\(\)... etc

## Using statics to compose in your own way

Often static methods are used to tweak a stamp, to compose more metadata in a domain specific way.

```javascript
const HasFactor = stampit()
.statics({
  allowFactorSetter(allow) {
    return this.conf({ HasFactor: { allowSetter: allow } }) // tweaking the stamp. Using conf() static method
  }
})
.init(({ factor }, { instance, stamp }) => {
  let factor = factor || 1 // private (scoped) variable
  instance.getFactor = () => factor

  const { configuration } = stamp.compose
  if (configuration && configuration.HasFactor && configuration.HasFactor.addFactorSetter) {
    instance.setFactor = f => { factor = f }
  }
})
```

The example above exposes a static function which controls access to the `setFactor` method of your object instances.

```javascript
HasFactor().setFactor === undefined // there is no .setFactor() method

const HasFactorAllowed = HasFactor.allowFactorSetter(true)
const instance = HasFactor2()
instance.setFactor(5); // we have access to the setter!
instance.getFactor() === 5 // the factor was set
```

## Descriptor merging algorithm

The static properties are copied **by assignment**. In other words - **by reference** using `Object.assign`.

```javascript
HasFactor.allowFactorSetter === HasFactorAllowed.allowFactorSetter
```

In case of conflicts the last composed property wins.

## Other ways to add static properties

Exactly the same stamp can be created in few ways. Here they all are.

```javascript
function func(allow) {
  return this.conf({ HasFactor: { allowSetter: allow } })
}

const HasFactor = stampit({
  statics: {
    allowFactorSetter: func
  }
})

const HasFactor = stampit({
  staticProperties: {
    allowFactorSetter: func
  }
})

const HasFactor = stampit.statics({
  allowFactorSetter: func
})

const HasFactor = stampit.staticProperties({
  allowFactorSetter: func
})

const HasFactor = stampit().statics({
  allowFactorSetter: func
})

const HasFactor = stampit().staticProperties({
  allowFactorSetter: func
})
```

