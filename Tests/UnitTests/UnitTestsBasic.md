# Unit Tests
Unit tests are automated tests written and run by software developers to ensure that a specific section of an application (known as a "unit") behaves as expected. A unit can be a single function, method, procedure, module, or object. The primary goal of unit testing is to validate that each unit of the software performs as designed. Unit tests help to detect issues early in the development process and ensure that individual parts of the application work correctly in isolation from the rest of the application.

### Specific Parts
+ **Isolated Testing**: Unit tests focus on a small, specific part of the application, ensuring that it works correctly independently of other parts.
+ **Automated and Repeatable**: Unit tests are automated, meaning they can be run multiple times with the same results, making it easier to catch and fix bugs.
+ **Fast Feedback**: Since unit tests are automated and quick to execute, they provide immediate feedback to developers, helping to maintain code quality throughout the development cycle.
+ **Documentation**: Unit tests serve as a form of documentation for the code, explaining what the code is supposed to do and how it is expected to behave.Continuous Integration: Unit tests are often integrated into the continuous integration (CI) pipeline to ensure that new code changes do not break existing functionality.

+ ## Example:
+ Lets build a simple structure for the calculations that a simple calculator can perform:

```Swift
struct CalculatorOperations {
  func add(_ a: Double, _ b: Double) -> Double {
    return a + b
  }

  func division(_ a: Double, _ b: Double) -> Double? {
    guard b != 0 else {return nil}
    return a / b
  }
}  
```

For example, only two of all operation methods were implemented. Now we create the unit tests for each functions.

A important thing to notice is that the tests folder is not inside the project folder, so we need to import the projet folder. This is how we do it:

```Swift
@testable import ProjectName
```

In the add function, we first need to check if it sums, then, some "edge cases". So we should test:
+ The sum of 2 positive numbers.
+ Two negative numbers.
+ A positive and negative number.
+ Two numbers with decimal places.

```Swift
...
  func test_sumPositiveNumbers {
    //Given
    let firstNumber: Double = 1.0
    let secondsNumber: Double = 5.0
    let calculatorOperation = CalculatorOperation()
    //When
    let sut = calculatorOperation.add(firstNumber, secondNumber)
    //Then
    XCTAssertEqual(sut, 6.0)
  }

func test_sumNegativeNumbers {
    //Given
    let firstNumber: Double = -1.0
    let secondsNumber: Double = -5.0
    let calculatorOperation = CalculatorOperation()
    //When
    let sut = calculatorOperation.add(firstNumber, secondNumber)
    //Then
    XCTAssertEqual(sut, -6.0)
  }

func test_sumPositiveAndNegativeNumbers {
    //Given
    let firstNumber: Double = -1.0
    let secondsNumber: Double = 5.0
    let calculatorOperation = CalculatorOperation()
    //When
    let sut = calculatorOperation.add(firstNumber, secondNumber)
    //Then
    XCTAssertEqual(sut, 4.0)
  }

func test_sumDecimalNumbers {
    //Given
    let firstNumber: Double = 1.35
    let secondsNumber: Double = 5.65
    let calculatorOperation = CalculatorOperation()
    //When
    let sut = calculatorOperation.add(firstNumber, secondNumber)
    //Then
    XCTAssertEqual(sut, 7.0)
  }
...
```
With all tests above, built respectively bases on the list above, we ensure to the developer that the function is when after he refactor the base function.

Also, a common thing i do while programming tests is using the comments *Given/When/Then*. Given is the parameters of given values that we need to use in the test. When is the perform of everything, or when something happens. Then is used to indicate what we ll do after everything above, that is the real test.

Now we need to test the division method, and this is what we should test:
+ 2 positive numbers.
+ 2 negative numbers.
+ A positive and negative number.
+ Divisor is 0, it returns nil.
+ 2 Numbers with decimals places.

```Swift
  func test_dividePositiveNumbers() {
    //Given
    let divisor: Double = 10.0
    let dividend: Double = 5.0
    let calculatorOperation = CalculatorOperation()
    //When
    let sut = calculatorOperation.division(divisor, dividend)
    //Then
    XCTAssertEqual(sut, 2.0)
  }

  func test_divideNegativeNumbers() {
      //Given
      let divisor: Double = -10.0
      let dividend: Double = -5.0
      let calculatorOperation = CalculatorOperation()
      //When
      let sut = calculatorOperation.division(divisor, dividend)
      //Then
      XCTAssertEqual(sut, 2.0)
    }
  
  func test_dividePositiveAndNegativeNumbers() {
      //Given
      let divisor: Double = 10.0
      let dividend: Double = -5.0
      let calculatorOperation = CalculatorOperation()
      //When
      let sut = calculatorOperation.division(divisor, dividend)
      //Then
      XCTAssertEqual(sut, -2.0)
    }
  
    func test_dividentZero() {
      //Given
      let divisor: Double = 10.0
      let dividend: Double = 0.0
      let calculatorOperation = CalculatorOperation()
      //When
      let sut = calculatorOperation.division(divisor, dividend)
      //Then
      XCTAssertNil(sut)
    }
  
    func test_divideWithDecimalPlaces() {
        //Given
        let divisor: Double = 10.5
        let dividend: Double = 5.6
        let calculatorOperation = CalculatorOperation()
        //When
        let sut = calculatorOperation.division(divisor, dividend)
        //Then
        XCTAssertEqual(sut, 1.875)
      }
```
