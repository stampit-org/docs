# @stamp/named

## @stamp/named

_Changes the_ `Stamp.name` _property using the_ [_new ES6 feature_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/name)_._

Supported platforms: node&gt;=4, iOS&gt;=10, Edge, FF, Chrome, Safari

If used in a non-supported environment \(node &lt;v4, or IE any version\) then nothing will throw. But the `Stamp.name` will always be `"Stamp"`.

## Example

Default behaviour \(without this stamp\):

```javascript
const MyRegularStamp = compose(...);
console.log(MyRegularStamp.name); // 'Stamp'
```

New behaviour:

```javascript
import Named from '@stamp/named';

const MyNamedStamp = MyRegularStamp.compose(Named).setName('MyNamedStamp');
```

Or if you don't want to import the stamp you can import only the method:

```javascript
import {setName} from '@stamp/named';
const MyNamedStamp = MyRegularStamp.compose(setName('MyNamedStamp'));
```

Then stamp receives a different name instead of the default "Stamp":

```javascript
console.log(MyNamedStamp.name); // 'MyNamedStamp'
```

Derived stamps behaviour:

```javascript
// All derived stamps will also be named 'MyNamedStamp' until changed:
let Stamp2 = compose(..., MyNamedStamp, ...);
console.log(Stamp2.name); // WARNING! Still 'MyNamedStamp' !!!

// Overwriting the name
Stamp2 = Stamp2.setName('Stamp2');
console.log(Stamp2.name); // 'Stamp2' :)
```

