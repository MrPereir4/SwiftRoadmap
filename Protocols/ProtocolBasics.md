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

> **Note** You don’t need to mark protocol initializer implementations with the required modifier on classes that are marked with the final modifier, because final classes can’t subclassed.

If a subclass overrides a designated initializer from a superclass, and also implements a matching initializer requirement from a protocol, mark the initializer implementation with both the required and override modifiers:

```Swift
protocol SomeProtocol {
    init()
}


class SomeSuperClass {
    init() {
        // initializer implementation goes here
    }
}


class SomeSubClass: SomeSuperClass, SomeProtocol {
    // "required" from SomeProtocol conformance; "override" from SomeSuperClass
    required override init() {
        // initializer implementation goes here
    }
}
```

+ When implementing a init from protocols you always have to mark it with *required*.
+ So as the subClass is overriding the initializer of the superClass, it should be marked with *override*


## Protocol as Types
Protocols don’t actually implement any functionality themselves. Regardless, you can use a protocol as a type in your code.

The most common way to use a protocol as a type is to use a protocol as a generic constraint. Code with generic constraints can work with any type that conforms to the protocol, and the specific type is chosen by the code that uses the API. For example, when you call a function that takes an argument and that argument’s type is generic, the caller chooses the type.

Code with an opaque type works with some type that conforms to the protocol. The underlying type is known at compile time, and the API implementation chooses that type, but that type’s identity is hidden from clients of the API. Using an opaque type lets you prevent implementation details of an API from leaking through the layer of abstraction — for example, by hiding the specific return type from a function, and only guaranteeing that the value conforms to a given protocol.

Code with a boxed protocol type works with any type, chosen at runtime, that conforms to the protocol. To support this runtime flexibility, Swift adds a level of indirection when necessary — known as a box, which has a performance cost. Because of this flexibility, Swift doesn’t know the underlying type at compile time, which means you can access only the members that are required by the protocol. Accessing any other APIs on the underlying type requires casting at runtime.

### Generic Constraints With Protocol

```Swift
protocol Drawable {
    func draw()
}

struct Circle: Drawable {
    func draw() {
        print("Drawing a circle")
    }
}

struct Square: Drawable {
    func draw() {
        print("Drawing a square")
    }
}

func drawShape<T: Drawable>(_ shape: T) {
    shape.draw()
}

let circle = Circle()
let square = Square()

drawShape(circle)  // Output: Drawing a circle
drawShape(square)  // Output: Drawing a square
```
Allows that a generic functions works with any type that conforms to the protocol, with the type choosen by the caller.

### Opaque Type

```Swift
protocol Shape {
    func area() -> Double
}

struct Rectangle: Shape {
    var width: Double
    var height: Double

    func area() -> Double {
        return width * height
    }
}

func makeRectangle() -> some Shape {
    return Rectangle(width: 10, height: 5)
}

let shape = makeRectangle()
print(shape.area())  // Output: 50.0
```
Allows to hide the specific return type of a function, ensuring that it conforms to the protocol

### Boxed Protocol Type

```Swift
protocol Animal {
    func sound() -> String
}

struct Dog: Animal {
    func sound() -> String {
        return "Woof"
    }
}

struct Cat: Animal {
    func sound() -> String {
        return "Meow"
    }
}

func makeSound(animal: Animal) {
    print(animal.sound())
}

let dog = Dog()
let cat = Cat()

makeSound(animal: dog)  // Output: Woof
makeSound(animal: cat)  // Output: Meow
```
Allows to work with any type that conforms to a protocol choosen in runtime, with a performance cost.

## Delegation
Delegation is a design pattern that enables a class or structure to hand off (or delegate) some of its responsibilities to an instance of another type. This design pattern is implemented by defining a protocol that encapsulates the delegated responsibilities, such that a conforming type (known as a delegate) is guaranteed to provide the functionality that has been delegated. Delegation can be used to respond to a particular action, or to retrieve data from an external source without needing to know the underlying type of that source.

The example below defines a dice game and a nested protocol for a delegate that tracks the game’s progress:

```Swift
class DiceGame {
    let sides: Int
    let generator = LinearCongruentialGenerator()
    weak var delegate: Delegate?


    init(sides: Int) {
        self.sides = sides
    }


    func roll() -> Int {
        return Int(generator.random() * Double(sides)) + 1
    }


    func play(rounds: Int) {
        delegate?.gameDidStart(self)
        for round in 1...rounds {
            let player1 = roll()
            let player2 = roll()
            if player1 == player2 {
                delegate?.game(self, didEndRound: round, winner: nil)
            } else if player1 > player2 {
                delegate?.game(self, didEndRound: round, winner: 1)
            } else {
                delegate?.game(self, didEndRound: round, winner: 2)
            }
        }
        delegate?.gameDidEnd(self)
    }


    protocol Delegate: AnyObject {
        func gameDidStart(_ game: DiceGame)
        func game(_ game: DiceGame, didEndRound round: Int, winner: Int?)
        func gameDidEnd(_ game: DiceGame)
    }
}
```
The DiceGame class implements a game where each player takes a turn rolling dice, and the player who rolls the highest number wins the round. It uses a linear congruential generator from the example earlier in the chapter, to generate random numbers for dice rolls.

The DiceGame.Delegate protocol can be adopted to track the progress of a dice game. Because the DiceGame.Delegate protocol is always used in the context of a dice game, it’s nested inside of the DiceGame class. Protocols can be nested inside of type declarations like structures and classes, as long as the outer declaration isn’t generic.

To prevent strong reference cycles, delegates are declared as weak references. Marking the protocol as class-only lets the DiceGame class declare that its delegate must use a weak reference. A class-only protocol is marked by its inheritance from AnyObject, as discussed in Class-Only Protocols.

DiceGame.Delegate provides three methods for tracking the progress of a game. These three methods are incorporated into the game logic in the play(rounds:) method above. The DiceGame class calls its delegate methods when a new game starts, a new turn begins, or the game ends.

Because the delegate property is an optional DiceGame.Delegate, the play(rounds:) method uses optional chaining each time it calls a method on the delegate. If the delegate property is nil, these delegate calls are ignored. If the delegate property is non-nil, the delegate methods are called, and are passed the DiceGame instance as a parameter.

This next example shows a class called DiceGameTracker, which adopts the DiceGame.Delegate protocol:

```Swift
class DiceGameTracker: DiceGame.Delegate {
    var playerScore1 = 0
    var playerScore2 = 0
    func gameDidStart(_ game: DiceGame) {
        print("Started a new game")
        playerScore1 = 0
        playerScore2 = 0
    }
    func game(_ game: DiceGame, didEndRound round: Int, winner: Int?) {
        switch winner {
            case 1:
                playerScore1 += 1
                print("Player 1 won round \(round)")
            case 2: playerScore2 += 1
                print("Player 2 won round \(round)")
            default:
                print("The round was a draw")
        }
    }
    func gameDidEnd(_ game: DiceGame) {
        if playerScore1 == playerScore2 {
            print("The game ended in a draw.")
        } else if playerScore1 > playerScore2 {
            print("Player 1 won!")
        } else {
            print("Player 2 won!")
        }
    }
}
```

The DiceGameTracker class implements all three methods that are required by the DiceGame.Delegate protocol. It uses these methods to zero out both players’ scores at the start of a new game, to update their scores at the end of each round, and to announce a winner at the end of the game.

Here’s how DiceGame and DiceGameTracker look in action:

```Swift
let tracker = DiceGameTracker()
let game = DiceGame(sides: 6)
game.delegate = tracker
game.play(rounds: 3)
// Started a new game
// Player 2 won round 1
// Player 2 won round 2
// Player 1 won round 3
// Player 2 won!
```

My implementation to test my knowledge:

```Swift
//: A UIKit based Playground for presenting user interface
  
import UIKit
import PlaygroundSupport

//Air Traffic Control System
//Implementation of protocols with delegates


protocol FlightDelegate {
    func airplaneDidTakeOff(_ airplane: Airplane)
    
    func airplaneDidLand(_ airplane: Airplane)
    
    func airplaneDidReportEmergency(_ airplane: Airplane)
}

class Airplane {
    
    var flightNumber: String
    var flightDelegate: FlightDelegate?
    
    init(flightNumber: String, flightDelegate: FlightDelegate? = nil) {
        self.flightNumber = flightNumber
        self.flightDelegate = flightDelegate
    }
    
    convenience init(flightNumber: String) {
        self.init(flightNumber: flightNumber, flightDelegate: nil)
    }
    
    func takeOff() {
        
        guard let flightDelegate = flightDelegate else {return}
        flightDelegate.airplaneDidTakeOff(self)
    }
    
    func land() {
        guard let flightDelegate = flightDelegate else {return}
        flightDelegate.airplaneDidLand(self)
    }
    
    func reportEmergency() {
        guard let flightDelegate = flightDelegate else {return}
        flightDelegate.airplaneDidReportEmergency(self)
    }
    
}

class ControlTower: FlightDelegate {
    
    var monitoredAiplanes: [Airplane] = []
    
    func addAirplane(_ airplane: Airplane) {
        self.monitoredAiplanes.append(airplane)
    }
    
    func removeAiplane(_ airplane: Airplane) {
        self.monitoredAiplanes.removeAll(where: {$0.flightNumber == airplane.flightNumber})
        
        print("Airplane \(airplane.flightNumber) removed.")
    }
    
    func airplaneDidTakeOff(_ airplane: Airplane) {
        print("Control tower reports: Airplaine - \(airplane.flightNumber) did take off.")
    }
    
    func airplaneDidLand(_ airplane: Airplane) {
        print("Control tower reports: Airplaine - \(airplane.flightNumber) did land.")
    }
    
    func airplaneDidReportEmergency(_ airplane: Airplane) {
        print("Control tower reports: Airplaine - \(airplane.flightNumber) did report emergency.")
    }
}


let tower = ControlTower()

let airplane1 = Airplane(flightNumber: "ABC123", flightDelegate: tower)
let airplane2 = Airplane(flightNumber: "XYZ789", flightDelegate: tower)

tower.addAirplane(airplane1)
tower.addAirplane(airplane2)

airplane1.takeOff()
airplane1.reportEmergency()
airplane1.land()

airplane2.takeOff()
airplane2.land()

tower.removeAiplane(airplane1)
tower.removeAiplane(airplane2)

```


Reference: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/protocols/
