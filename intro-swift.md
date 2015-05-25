# An Introduction to Swift
This document represents the notes I took while learning Swift.

## Basics

Constants use `let`, values use `var`:

```
var myVariable = 62
let myConstant = 84
```

We can assign specific, non-inferred types:

```
var myDouble: Double = 65.0
```

Casting using types:

```
// First Way
let width = 54
let widthLabel = String(width)

// Second Way
let widthString = "The width of the string is \(width)"
```

Arrays and dictionaries using brackets:

```
var shoppingList = ["eggs", "milk", "butter"]
var jobs = [
    "Kanye West" : "Rapper",
    "Derek Jeter" : "Baseball Player",
]
jobs["Kanye West"] // "Rapper"
```

Or we can initialize:

```
let emptyArray = String[]()
let emptyDictionary = Dictionary<String,Float>()
```

Optionals make sure we don't need to do checks for nil:

```
var optionalName: String? = "Matt Piccolella"
```

Iterating over dictionaries is easy:

```
let patterns = [
    "Fibonacci": [1,1,2,3,5,8],
    "Square": [1,4,9,16,25],
]
for (key,value) in patterns {
    for number in values {
        println("\(key) + \(number)")
    }
}
```

Functions use `func`:

```
func greet(name: String, day: String) -> String{
    return "Hello \(name), today is \(day)."
}
greet("Bob", "Tuesday")
// Specify types of arguments and type of return
```

Tuples return multiple:

```
func getGasPrices() -> (Double, Double, Double) {
    return (3.59, 3.69, 3.79)
}
```

We can take arbitrary numbers of arguments of a given type:

```
func sumOf(numbers: Int...) -> Int {
    var sum = 0
    for number in numbers {
        sum += number
    }
    return sum
}
sumOf(42)
sumOf(42,58,12)

**Note:** nest functions if you have a function you want to call inside another.

Functions can also be passed and returned:

```
// Return a function
func makeIncrementer() -> (Int -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)

// Pass a function
func hasMatches(list: Int[], condition: Int -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasMatches(numbers, lessThanTen)
```

**Closures** using brackets are great:

```
numbers.map({
    (number: Int) -> Int in
    let result = 3 * number
    return result
    })
numbers.map({ number in 3 * number })
```

## Objects and Classes
Classes are pretty simple, using `init` and `deinit` for initializer and deinitializer:
```
class NamedShape {
    var numberOfSides = 0
    var name: String

    init(name: String) {
        self.name = name
    }

    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```

Inheritence uses the `override` keyword:

```
class Square: NamedShape {
    var sideLength: Double 

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }

    func area -> Double {
        return sideLength * sideLength
    }

    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "My Test")
test.area()
test.simpleDescription()
```

`get` and `set` can be placed on any property:

```
class Square: NamedShape {
    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            // This is implicit.
            sideLength = newValue / 3.0
        }
    } 
}
```

**Note**: we can use `willSet` and `didSet`:
    - `willSet` is called right before a value will be set
    - `didSet` is called right after a value is set

Specifying a second value for within the function:

```
class Counter {
    var count: Int = 0
    func incrementBy(amount: Int, numberOfTimes times: Int) {
        count += amount * times
    }
}
```

Using the `?` for optionals is very important:

```
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional")
let sideLength = optionalSquare?.sideLength
```

If the optional value is `nil`, everything after the `?` is ignored and the value of the expression is `nil`.

Enums are kinda simple:

```
enum Rank: Int {
    case Ace = 1
    case Two, Three, Four, Five, Six, Seven, Eight, Nine, Ten
    case Jack, Queen, King
    func simpleDescription() -> String {
        switch self {
        case .Ace:
            return "ace"
        case .Jack:
            return "jack"
        case .Queen:
            return "queen"
        case .King:
            return "king"
        default:
            return String(self.toRaw())
        }
    }
}
let ace = Rank.Ace
let aceRawValue = ace.toRaw()
```

Structs are similar to structs in other languages.

**Protocols** are like interfaces: use the `protocol` key word:

```
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}

class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 4102
    func adjust() {
        simpleDescription += " Now Adjusted."
    }
}
```

**Note**: structs may need to use the word `mutating` to note that the function changes a value within the class.

**Extensions** are like abstract classes: we can extend protocols to add additional functionality:

```
extension Int: ExampleProtocol {
    var simpleDescription: String {
        return  "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
}
7.simpleDescription()
```

**Generics** are similar to other languages as well:

```
func repeat<ItemType>(item: ItemType, times: Int) -> ItemType[] {
    var result = ItemType[]()
    for i in 0..times {
        result += item
    }
    return result
}
repeat("knock", 4)
```

We can also use `where` in our generics to require certain types:

```
func anyCommonElements <T, U where T: Sequence, U: Sequence> (lhs: T, rhs: U) -> Bool {
    for lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                return True
            }
        }
    }
    return false
}
anyCommonElements([1,2,3], [3])

**Subscripts** are shortcuts for accessing the member elements of a collection, list, or sequence. You use subscripts to set and retrieve values my index without needing separate methods. These include `get` and `set`.

```
struct Matrix {
    let rows: Int, columns: Int

    subscript(row: Int, column: Int) -> Double {
        get {
            return grid[(row * columns) + column]
        }
        set {
            grid[(row * columns) + column] = newValue
        }
    }
}












