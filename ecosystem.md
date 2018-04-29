# Stamp modules ecosystem

## Main modules

* **@stamp/compose** - The standard compose function implementation to create stamps
* **stampit** - A nice, handy API implementation of the compose standard. The original `stampit` module where it all began
* **@stamp/it** - Same as above

## Utility stamps

* **@stamp/arg-over-prop** - Assign properties passed to the stamp factory
* [**@stamp/collision**](/stampcollision.md) - Detect and manipulate method collisions
* **@stamp/configure** - Access configuration of your stamps anywhere
* **@stamp/eventemittable** - Node.js' EventEmitter as a stamp
* **@stamp/fp-constructor** - Adds the Functional Programming capabilities to your stamps
* **@stamp/init-property** - Replaces properties which reference stamps with objects created from the stamps
* [**@stamp/instanceof**](/stampinstanceof.md) - Enables `obj instanceof MyStamp` in ES6 environments
* [**@stamp/named**](/stampnamed.md) - \(Re\)Name a stamp factory function
* [**@stamp/privatize**](/stampprivatize.md) - Protect private properties
* [**@stamp/required**](/stamprequired.md) - Insist on a method/property/staticProperty/configuration presence
* **@stamp/shortcut** - Adds handy shortcuts for stamp composition. Used in **@stamp/it**

## Other

* **@stamp/is** - Various validation functions used with stamps
* **@stamp/core** - Core functions for creating stamps. Used in **@stamp/it**
* **@stamp/check-compose** - Command line tool and library to test your `compose` function implementation



