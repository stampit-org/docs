# Ecosystem Overview

## Main modules

* **@stamp/compose** - The standard compose function implementation to create stamps
* **stampit** - A nice, handy API implementation of the compose standard. The original `stampit` module where it all began
* **@stamp/it** - Same as above

## Utility stamps

* **@stamp/arg-over-prop** - Assign properties passed to the stamp factory
* [**@stamp/collision**](stamp-collision.md) - Detect and manipulate method collisions
* **@stamp/configure** - Access configuration of your stamps anywhere
* **@stamp/eventemittable** - Node.js' EventEmitter as a stamp
* **@stamp/fp-constructor** - Adds the Functional Programming capabilities to your stamps
* **@stamp/init-property** - Replaces properties which reference stamps with objects created from the stamps
* [**@stamp/instanceof**](stamp-instanceof.md) - Enables `obj instanceof MyStamp` in ES6 environments
* [**@stamp/named**](stamp-named.md) - \(Re\)Name a stamp factory function
* [**@stamp/privatize**](stamp-privatize.md) - Protect private properties
* [**@stamp/required**](stamp-required.md) - Insist on a method/property/staticProperty/configuration presence
* **@stamp/shortcut** - Adds handy shortcuts for stamp composition. Used in **@stamp/it**

## Other

* **@stamp/is** - Various validation functions used with stamps. Used in **@stamp/it**
* **@stamp/core** - Core functions for creating stamps. Used in **@stamp/it**
* **@stamp/check-compose** - Command line tool and library to test your `compose` function implementation

