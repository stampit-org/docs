# Static deep properties

The static deep properties are properties attached to stamp itself. But unlike regular \(shallow\) static properties these are deeply merged together by stamp [deep merging algorithm](../essentials/specification/merging-algorithm.md).

```javascript
const SchemaTypes = stampit.deepStatics({
  Types: {
    String,
    Number,
    Array
  }
})

const ObjectIdSchemaType = stampit.deepStatics({
  Types: {
    ID: String,
    ObjectId: require('./object-id')
  }
})

const Schema = stampit(SchemaTypes, ObjectIdSchemaType, { // stamp deep merging kicks in this line
  init(data, { stamp }) {
    // stamp.Types.String, stamp.Types.ID, ...
  }
})

// Schema.Types.String
// Schema.Types.Number
// Schema.Types.Array
// as well as
// Schema.Types.ID
// Schema.Types.ObjectId
```

## Descriptor merging algorithm

The staticDeepProperties are **deeply merged** using stamp [deep merging algorithm](../essentials/specification/merging-algorithm.md). See below - the `Types` value is always a new object.

```javascript
SchemaTypes.Types !== Schema.Types // NEVER EQUAL! NO MATTER WHAT!
```

Sometimes a regular \(shallow\) static property has the same name as a deep static property. The shallow static property always wins.

```javascript
const NullTypes = stampit.statics({
  Types: null
})

const NoTypesSchema = Schema.compose(NullTypes)

NoTypesSchema.Types === null // the regular property overwrites deep property
```

## Other ways to add static deep properties

Exactly the same stamp can be created in few ways. Here they all are.

```javascript
const myTypes = { String, ID: String, Number, Array }

const AuthServiceToken = stampit({
  deepStatics: {
    Types: myTypes
  }
})

const HasLog = stampit({
  staticDeepProperties: {
    Types: myTypes
  }
})

const HasLog = stampit.deepStatics({
  Types: myTypes
})

const HasLog = stampit.staticDeepProperties({
  Types: myTypes
})

const HasLog = stampit().deepStatics({
  Types: myTypes
})

const HasLog = stampit().staticDeepProperties({
  Types: myTypes
})
```

