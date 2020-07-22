# Introduction

## What is stampit

`stampit` is a JavaScript module which implements the [**stamp** specification](essentials/specification/). **Stamps** are composable factory functions.

**Think of** _**stamps**_ **as classic classes but without any limits, boundaries, or rules.**

The stamps brag large amount of features and things you can do with your objects and/or stamps.

Head straight to the [Quick Start](api/quick-start.md) page for code examples.

![Object instances created from classes vs stamps.](.gitbook/assets/class_vs_stamp.png)

## Differences from classes

* Inheritance
  * Classes
    * Classes do child+parent+parent+parent... inheritance chains. See picture above.
    * When you extend a class you link class prototypes.
  * Stamps
    * Stamp are single object + single prototype. See picture above.
    * When you "extend" you separately merge [methods](api/methods.md), separately merge [properties](api/properties.md), separately merge [static properties](api/static-properties.md), separately merge [initializers](api/initializers.md) \(aka constructors\), etc etc etc.
    * That's why it is not just inheritance, but special type of object composition. The stamp composition algorithm is [standardized](essentials/specification/merging-algorithm.md).
    * You can influence composition result with your code at runtime using the [composers](api/composers.md) feature.
* Object creation
  * Classes
    * In most programming languages you execute only one constructor per class.
    * To pass data to the parent constructor you have to manually call parent constructor `this.super(data)`.
    * To create an object you need to use the `new` keyword: `const object = new MyClass()`.
  * Stamps
    * Stamp executes every initializer \(aka constructor\) it has.
    * All initializers receive exactly the same set of arguments, no need to manually pass data.
    * The initializer execution sequence is the same as the stamp composition sequence.
    * To create an object you [call stamp](api/quick-start.md) as a function: `const object = MyStamp()`.

## History

The original idea of Stamps belongs to [Eric Elliott](https://ericelliottjs.com/). See his [first commit](https://github.com/stampit-org/stampit/commit/ac330e8537e349a9640bbe4a34c63150db445a20) from 11 Feb 2013 in the stampit repository. Since then the idea evolved to the [specification](essentials/specification/) and a whole [ecosystem](ecosystem/ecosystem-overview.md) of mini modules.

## Using Stamps

Head straight to the [Quick start ](api/quick-start.md)for code examples.

## Ecosystem

Stampit have a lot of helper modules. You can always find the list of official NPM packages here: [https://www.npmjs.com/~stamp](https://www.npmjs.com/~stamp)

See more information on the [Ecosystem](ecosystem/ecosystem-overview.md) page.

## Quick code example

The example below introduces `GraphPoint` and `GraphLine` stamps \(aka blueprints, aka factory functions, aka behaviours\).

Let's start by declaring the `Point` stamp.

```javascript
const stampit = require("stampit");

const Point = stampit({
  props: { // default property values
    x: 0,
    y: 0
  },
  init({ x, y }) { // kinda constructor
    if (x != null) this.x = x;
    if (y != null) this.y = y;
  },
  methods: { // that goes to the prototype
    distance(point) {
      return Math.sqrt(Math.abs(this.x - point.x)**2 + Math.abs(this.y - point.y)**2);
    }
  }
});
```

That's how you create instance of that stamp.

```javascript
// Creating instance of the Point.
const point = Point({ x: 12, y: 42 });
// Calling a method.
point.distance({ x: 13, y: 42 }) === 1.0;
```

Now let's "inherit" the `Point` to create a `Circle`. We will add `radius` property to the mix.

```javascript
// kinda inheritance, but not really
const Circle = stampit(Point, {
  props: {
    radius: 1
  },
  init({ radius }) {
    if (radius != null) this.radius = radius;
  },
  methods: { // that goes to the prototype
    distance(point) {
      return Point(point).distance(this) - this.radius;
    }
  }
});
```

When creating instance of the `Circle` you will actually call **TWO** different initialisers \(aka constructors\). Stamps have multiple initialisers. 

```javascript
// TWO different initializers will be executed here!!!
const circle = Circle({ x: 12, y: 42, radius: 1.5 });
circle.distance({ x: 14, y: 42 }) === 0.5;
```

Now, declaring couple of additional stamps. We'll use them to enrich JavaScript drawable objects.

```javascript
const Tagged = stampit({
  props: {
    tag: ""
  },
  init({ tag }) {
    this.tag = tag || this.tag;
  }
});

const Colored = stampit({
  props: {
    color: "#000000"
  },
  init({ color }) {
    this.color = color || this.color;
  }
});
```

The `Drawable` behaviour. We have to make sure that developers implement the `draw()` method when using this stamp.

```javascript
const Required = require("@stamp/required");

const Drawable = stampit(
  Tagged,
  Colored,
  Required.required({ methods: { draw: Required } })
);
```

Now let's implement the `GraphPoint` stamp.

```javascript
const GraphPoint = stampit(Drawable, Circle, {
  methods: {
    draw(ctx) {
      ctx.setColor(this.color);
      ctx.drawFilledCircle(this.x, this.y, this.radius);
      if (this.tag) 
        ctx.drawText(this.x + this.radius, this.y + this.radius, this.tag);
    }
  }
});
```

Please, note we are executing **FOUR** intializers here.

```javascript
// FOUR different initializers will be executed here.
const graphPoint = GraphPoint({ 
  x: 12, 
  y: 42, 
  radius: 1.5, 
  color: "red", 
  tag: "start" 
});
graphPoint.draw(someDrawingContext);
```

`Line` primitive.

```javascript
const Line = stampit({
  props: {
    point1: Point({ x: 0, y: 0 }),
    point2: Point({ x: 0, y: 0 })
  },
  init({ point1, point2 }) {
    this.point1 = Point(point1);
    this.point2 = Point(point2);
  },
  methods: {
    middlePoint() {
      return Point({ 
        x: (this.point1.x + this.point2.x) / 2, 
        y: (this.point1.y + this.point2.y) / 2
      });
    }
  }
});

const line = Line({ point1: { x: 12, y: 42 }, point2: { x: 34, y: 40 } });
```

Our final destination - a drawable graph line.

```javascript
const GraphLine = stampit(Drawable, Line, {
  methods: {
    draw(ctx) {
      ctx.setColor(this.color);
      ctx.drawLine(this.point1.x, this.point1.y, this.point2.x, this.point2.y);
      if (this.tag) 
        ctx.drawText(this.middlePoint().x, this.middlePoint().y, this.tag);
    }
  }
});
```

And the usage.

```javascript
const graphLine = GraphLine({ 
  point1: { x: 12, y: 42 }, 
  point2: { x: 34, y: 40 }, 
  color: "red", 
  tag: "March" 
});
graphLine.draw(someDrawingContext);
```

The idea is - separate concerns. We split our logic into several reusable highly decoupled primitive behaviours \(Point, Colored, Tagged, Circle, Line\).

The above has:

* `Point` concern and all related functionality in a _separate_ stamp.
* `Circle` concern and the related functionality _separated_. `Circle` **is** an extension of the `Point`.
* `Tagged` concern in a _separate_ stamp.
* `Colored` concern in a _separate_ stamp.
* `GraphPoint` concern **is** `Circle`, `Tagged` and `Colored` concerns combined.
* `Line` concern - it **has** two `Points`.
* `GraphLine` - **is** the `Line` but with additional `Tagged` and `Colored` functionalities.

 You can't do anything like that with classes. And the resulting code is more readable than classes.

