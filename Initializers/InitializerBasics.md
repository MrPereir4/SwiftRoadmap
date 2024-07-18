### Initializers
An initializer in Swift is a special method that sets up a new instance of a class, struct, or enum by assigning initial values to its properties.

## Normal initializer
The most basic for initialization in swift. It is used to ensure that the instance of a class, struct or enumaration is completed initialized before been used.

```Swift
class Person {
    var name: String
    var age: Int

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
```

As seen in the example above, the initializer requests a name and a age. This ensures that this variables ll be set when ever a instance is created.

## Required initializer
A required initializer is a initializer that must be implemented by all its subclasses. It is necessary in classes that needs to ensure that its subclasses have a certain type of initilization. The keyword *required* is written befere the keyword *init*.

```Swift
class Person {
    var name: String
    var age: Int

    required init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}

class Student: Person {
    var studentID: String

    required init(name: String, age: Int) {
        self.studentID = ""
        super.init(name: name, age: age)
    }
}
```
So, what requires is used for is when we have a subclass with at least on initializes, and this forces all initializes to initialize the required init from the super class.

Example with multiple required initializers: 

```Swift
class Person {
    var name: String
    var age: Int

    // Primeiro inicializador required
    required init(name: String, age: Int) {
        self.name = name
        self.age = age
    }

    // Segundo inicializador required
    required init(name: String) {
        self.name = name
        self.age = 0  // Valor padrão
    }
}

class Student: Person {
    var studentID: String

    // Implementando o primeiro inicializador required
    required init(name: String, age: Int) {
        self.studentID = ""
        super.init(name: name, age: age)
    }

    // Implementando o segundo inicializador required
    required init(name: String) {
        self.studentID = ""
        super.init(name: name)
    }
}
```

Real world example: 

```Swift
class Manager: Employee {
    var department: String

    // Implements the required initializer from Employee
    required init(name: String, employeeID: String) {
        self.department = ""
        super.init(name: name, employeeID: employeeID)
    }

    // Can have other additional initializers, if necessary
    init(name: String, employeeID: String, department: String) {
        self.department = department
        super.init(name: name, employeeID: employeeID)
    }
}

// Attempting to create a Manager without providing an employeeID would cause a compilation error
// let manager = Manager(name: "Alice", department: "Engineering")  // Error: Missing arguments

// Creating a Manager with a valid employeeID
let manager = Manager(name: "Alice", employeeID: "M123", department: "Engineering")
print(manager.employeeID)  // Output: "M123"
```

As seen above, once i use required, it make obligatory the initializition of the super class initializer marqued with required.

## Convenient Initializer
A convenient initializer is a secondary initializer that is user for provide alternatives initializations for a class. 

```Swift
class Person {
    var name: String
    var age: Int

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }

    convenience init(name: String) {
        self.init(name: name, age: 0)
    }
}
```

In the example above, the convenient initializer provides a way of initialize the whole object only setting the name with a default value for age.

## Failable Initializer
A initializer that returns nil when the initialization fails. It's defined using init?. This is useful when initialization might not succeed due to invalid parameters or other reasons.

```Swift
struct Temperature {
    var celsius: Double

    init?(fahrenheit: Double) {
        if fahrenheit < -459.67 {
            return nil  // The temperature is below absolute zero, which is invalid.
        }
        self.celsius = (fahrenheit - 32) * 5 / 9
    }
}

if let temp = Temperature(fahrenheit: -500) {
    print("Temperature is \(temp.celsius) degrees Celsius")
} else {
    print("Invalid temperature")
}
```
Tha example above the failable initializer checks if the input is valid. If no, it returns nil, indicating the initialization failed.

