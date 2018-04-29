# @stamp/instanceof

_Enables _`obj instanceof MyStamp`_ in ES6 environments_

```javascript
const InstanceOf = require('@stamp/InstanceOf');
// or
import InstanceOf from '@stamp/InstanceOf';
```

## Example

Create a stamp:

```javascript
let MyStamp = compose({
  properties: { ... },
  initializers: [function () { ... }]
});
```

The following doesn't work:

```javascript
const obj = MyStamp();
obj instanceof MyStamp === false;
```

Compose the `InstanceOf` to your stamp:

```javascript
MyStamp = MyStamp.compose(InstanceOf);
```

Now it works:

```javascript
const myObject = MyStamp();
obj instanceof MyStamp === true;
```

## Notes

* We do not recommend to use `instanceof` in JavaScript in general.

