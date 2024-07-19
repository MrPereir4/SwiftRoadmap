# Protocol Basics
Protocol defines a blueprint of methods, properties, and others requirements that suits a particular task ou piece of functionality. The protocol can be addoped by a class, structure, or enumaration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to conform to that protocol.

## Protocol Syntax
They are defined very similar to classes, structs and enumarations:
```swift
protocol someProtocol {
  // protocol defifinition goes here
}
```
Custom types state that requires that they adopt a particular protocol by placing the protocols's name after the type's name, separate by a column, as part of their definition. Multiple protocols can be listed, and separeted by commas:
```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // structure definition goes here
}
```
If the class has a superclass, list the superclass **before** any protocol it adops, also followed by a comma:
```swift
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // class definition goes here
}
```
> **Note:** Because protocols are types, begin their names with a capital letter (such as FullyNamed and RandomNumberGenerator) to match the names of other types in Swift (such as Int, String, and Double).


## Property Requirements
+ Protocols can require any conforming type to provide an instance of that property.
+ Protocols doesn't specify whether the property should be a stored property or a computed property.
+ Also protocols require that you specify if the property should be gettable or gettable and settable (Get & Set).

If a protocol requires a property to be gettable and settable, that property requirement can’t be fulfilled by a constant stored property or a read-only computed property. If the protocol only requires a property to be gettable, the requirement can be satisfied by any kind of property, and it’s valid for the property to be also settable if this is useful for your own code.

```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```

### Example
```swift
protocol Vehicle {
    var make: String { get }        // Read-only property
    var model: String { get }       // Read-only property
    var year: Int { get set }       // Read-write property
    var isElectric: Bool { get set } // Read-write property

    func startEngine()
    func stopEngine()
}
```

```swift
class Motorcycle: Vehicle {
    var make: String
    var model: String
    var year: Int
    var isElectric: Bool

    init(make: String, model: String, year: Int, isElectric: Bool) {
        self.make = make
        self.model = model
        self.year = year
        self.isElectric = isElectric
    }

    func startEngine() {
        print("Motorcycle engine started")
    }

    func stopEngine() {
        print("Motorcycle engine stopped")
    }
}

let harley = Motorcycle(make: "Harley-Davidson", model: "Street 750", year: 2020, isElectric: false)
print(harley.make)       // Output: Harley-Davidson
print(harley.model)      // Output: Street 750
print(harley.year)       // Output: 2020
print(harley.isElectric) // Output: false

harley.year = 2021
harley.isElectric = true
print(harley.year)       // Output: 2021
print(harley.isElectric) // Output: true

harley.startEngine()     // Output: Motorcycle engine started
harley.stopEngine()      // Output: Motorcycle engine stopped
```
The example above shows an implementation of a protocol to create a Motorcycle class. In this scenario, the Motorcycle class conforms to the Vehicle protocol, which forces the class to implement the properties defined in the protocol. It is important to note that when defining the properties in the protocol, make and model are configured to be get-only ({ get }), and year and isElectric are gettable and settable ({ get set }). With this implementation, we can only change the values for year and isElectric after the instantiation of the class, which shows us that get-only properties behave like constants (let).

> ⚠️ **Warning:** If you attempt to change a read-only property (get) in a protocol, the Swift compiler will raise an error because read-only properties do not have a setter.

Always prefix type properties requirement with the *static* keyworld when you difine then in a protocol.

```swift
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}
```

The difference between using and not using stastic is that when you dont use the static keyword, is creates a instance of the object for each instance of the type has its separate copy of the propery. By using static you shared the same propery for all instances of the type, and that is called type property.

## Method Requirements
Protocols can require a specific instance methods and type methods to be implemented by conforming types. These methods are written as part of the protocol's definition in exactly the same way as for normal instances and type methods, but without curly braces or a method body. Variadic parameters are allowed subject to the same rule as for normal methods. Default values, however, can’t be specified for method parameters within a protocol’s definition.

As with type property requirements, you always prefix type method requirements with the static keyword when they’re defined in a protocol. This is true even though type method requirements are prefixed with the class or static keyword when implemented by a class:

```swift
protocol SomeProtocol {
    static func someTypeMethod()
}
```

The same idea of using static in properties is used in methods. You can have a type method using static and instance method not using it.


The following example defines a protocol with a single instance method requirement:

```swift
protocol RandomNumberGenerator {
    func random() -> Double
}
```

This protocol, RandomNumberGenerator, requires any conforming type to have an instance method called random, which returns a Double value whenever it’s called. Although it’s not specified as part of the protocol, it’s assumed that this value will be a number from 0.0 up to (but not including) 1.0.

The RandomNumberGenerator protocol doesn’t make any assumptions about how each random number will be generated — it simply requires the generator to provide a standard way to generate a new random number.

## Mutating Method Requirements
It's sometimes necessary for a method to modify (or mutate) the instance it belongs to. For instance methods on value type (that is, structures and enumarations) you place the mutating keyworld before a method's func keyword to indicate that the method is allowed to modify the instance it belongs to and any property of that instance.

If you define a protocol instance method requirement that’s intended to mutate instances of any type that adopts the protocol, mark the method with the mutating keyword as part of the protocol’s definition. This enables structures and enumerations to adopt the protocol and satisfy that method requirement.

> **Note:** If you mark a protocol instance method requirement as mutating, you don’t need to write the mutating keyword when writing an implementation of that method for a class. The mutating keyword is only used by structures and enumerations.

The example below defines a protocol called Togglable, which defines a single instance method requirement called toggle. As its name suggests, the toggle() method is intended to toggle or invert the state of any conforming type, typically by modifying a property of that type.

The toggle() method is marked with the mutating keyword as part of the Togglable protocol definition, to indicate that the method is expected to mutate the state of a conforming instance when it’s called:

```swift
protocol Togglable {
    mutating func toggle()
}
```

If you implement the Togglable protocol for a structure or enumeration, that structure or enumeration can conform to the protocol by providing an implementation of the toggle() method that’s also marked as mutating.

The example below defines an enumeration called OnOffSwitch. This enumeration toggles between two states, indicated by the enumeration cases on and off. The enumeration’s toggle implementation is marked as mutating, to match the Togglable protocol’s requirements:

```swift
enum OnOffSwitch: Togglable {
    case off, on
    mutating func toggle() {
        switch self {
        case .off:
            self = .on
        case .on:
            self = .off
        }
    }
}
var lightSwitch = OnOffSwitch.off
lightSwitch.toggle()
// lightSwitch is now equal to .on
}
```

## Initializer Requirements
Protocols can require specific initializers to be implemented by conforming types. You write these initializers as part of the protocol's definition in exactly the same way as for normal initializers, but without curly braces or an initializer body:

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
```

## Class Implementations of Protocol Initializer Requirements
You can implement a protocol intializer on a conforming class as either a designated or a convenience initializer. In both cases, you must mark the initializer implementation with the required modifier:

```Swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // initializer implementation goes here
    }
}
```

The use of the required modifier ensures that you provide an explicit or inherited implementation of the initializer requirements on all subclasses of the conforming class, such that they also conform to the protocol.
