## Ланцюжки опціоналів

*Ланцюжки опціоналів* - це процес звернення до властивостей, індексів та виклику методів на опціоналі, що може в цей момент мати значення `nil`. Якщо опціонал має значення, звернення до властивості, методу чи індексу є успішним; якщо опціонал дорівнює `nil`, звернення до властивості, методу чи індексу повертає `nil`. Кілька таких звернень можна об'єднати разом у ланцюжок, і тоді весь ланцюжок поверне `nil`, якщо хоча б одна ланка у ньому є `nil`. 

> **Примітка**
>
> Ланцюжки опціоналів у Swift є схожими на відправку повідомлення до `nil` у мові Objective-C, однак у спосіб, що працює для будь-якого типу, і в якому можна дізнатись, чи було звернення успішним. 

### Ланцюжки опціоналів як альтернатива примусовому розгортанню

Ланцюжок опціоналів створюється шляхом вказування знаку питання (`?`) після опціонального значення, на якому потрібно зробити виклик методу чи звернутись до властивості або індексу, якщо цей опціонал не `nil`. Це є дуже схожим на вказування знаку оклику (`!`) після опціонального значення для його примусового розгортання. Головною відмінністю є те, що ланцюжок опціоналів не призводить до помилки часу виконання, якщо опціонал є `nil`, в той час як примусове розгортання в такому випадку – призводить. 

Щоб відобразити той факт, що ланцюжок опціоналів може виклакатись на значенні `nil`, результатом виклику ланцюжка опціоналів завжди є опціональне значення, навіть якщо властивість, метод чи індекс повертає неопціональне значення. Опціональне значення, що повертається, завжди можна перевірити на `nil`, щоб з'ясувати, виклик всього ланцюжка був успішним (в такому випадку опціонал, що повертається, містить значення), чи був невдалим через наявність значення `nil` в ланцюжку (тоді опціонал, що повертається, матиме значення `nil`).

Якщо говорити точніше, результатом виклику ланцюжка опціоналів є очікуване значення, що повертається, але загорнуте в опціонал. Властивість, що зазвичай повертає значення типу `Int`, поверне значення типу `Int?`, якщо до неї звертатись через ланцюжок опціоналів. 

Наступні кілька фрагментів коду демонструють, як ланцюжки опціоналів відрізняються від примусового розгортання та дозволяють перевіряти успішність виклику.

Спершу оголошуємо два класи, `Person` та `Residence`, котрі моделюють особу та місце проживання відповідно:

```swift
class Person {
    var residence: Residence?
}
 
class Residence {
    var numberOfRooms = 1
}
```

Еземпляри `Residence` мають єдину властивість типу `Int` на ім'я `numberOfRooms`, котра моделює кількість кімнат та має значення за замовчанням `1`. Екземпляри `Person` мають опціональну властивість `residence` типу `Residence?`.

Якщо створити новий екземпляр `Person`, його властивість `residence` має значення за замовчанням `nil`, через те, що вона опціональна. В коді нижче, `ostap` має властивість `residence` зі значенням `nil`:

```swift
let ostap = Person()
```

Якщо спробувати звернутись до властивості `numberOfRooms` властивості `residence` цієї особи, використавши примусове розгортання через знак оклику після `residence`, ми отримаємо помилку часу виконання, оскільки `residence` не має значення, яке можна було б розгорнути:

```swift
let roomCount = ostap.residence!.numberOfRooms
// це призводить до помилки часу виконання
```

Якщо `ostap.residence` матиме значення, а не дорівнюватиме `nil`, код вище відпрацює успішно та присвоїть константі `roomCount` цілочисельне значення, що буде містити кількість кімнат. Однак, цей код завжди призводить до помилки часу виконання, коли `residence` дорівнює `nil`, що й показано вище. 

Ланцюжки опціоналів дають альтернативний спосід звертання до властивості `numberOfRooms`. Щоб скористатись ланцюжком опціоналів, ми використаємо знак питання замість знаку оклику:

```swift
if let roomCount = ostap.residence?.numberOfRooms {
    print("Остапова хата має \(roomCount) кімнат(у).")
} else {
    print("Неможливо отримати кількість кімнат.")
}
// Надрукує "Неможливо отримати кількість кімнат."
```

Це каже Swift “покласти в ланцюжок” опціональну властивість `residence` та дістати з неї значення `numberOfRooms`, якщо `residence` існує.

Оскільки спроба звернутись до `numberOfRooms` може потенційно невдатись, виклик ланцюжку опціоналів повертає значення `Int?`, або “опціональний `Int`”. Коли `residence` дорівнює `nil`, як у прикладі вище, цей опціонал матиме значення `nil`, що відображає той факт, що неможливо звернутись до значення `numberOfRooms`. Далі відбувається прив'язування опцінала для розгортання цілочисельного значення та присвоєння неопціонального значення константі `roomCount`.

Слід помітити, що це працює незважаючи на те, що властивість `numberOfRooms` має неопціональний тип `Int`. Той факт, що звернення до цієї властивості відбувається через ланцюжок опціоналів, означає, що виклик завжди буде повертати значення типу `Int?` замість `Int`.

Можемо присвоїти екземпляр `Residence` властивості `ostap.residence`, щоб вона більше не мала значення `nil`:

```swift
ostap.residence = Residence()
```

`ostap.residence` тепер містить екземпляр `Residence` замість `nil`. Якщо звернутись до властивості `numberOfRooms` у цьому ж ланцюжку опціоналів, що й раніше, повернеться значення типу `Int?`, котре тепер буде містити значення `numberOfRooms` за замовчанням – `1`:

```swift
if let roomCount = ostap.residence?.numberOfRooms {
    print("Остапова хата має \(roomCount) кімнат(у).")
} else {
    print("Неможливо отримати кількість кімнат.")
}
// Надрукує "Остапова хата має 1 кімнат(у)."
```

### Defining Model Classes for Optional Chaining

You can use optional chaining with calls to properties, methods, and subscripts that are more than one level deep. This enables you to drill down into subproperties within complex models of interrelated types, and to check whether it is possible to access properties, methods, and subscripts on those subproperties.

The code snippets below define four model classes for use in several subsequent examples, including examples of multilevel optional chaining. These classes expand upon the `Person` and `Residence` model from above by adding a `Room` and `Address` class, with associated properties, methods, and subscripts.

The `Person` class is defined in the same way as before:

```swift
class Person {
    var residence: Residence?
}
```

The `Residence` class is more complex than before. This time, the `Residence` class defines a variable property called `rooms`, which is initialized with an empty array of type `[Room]`:

```swift
class Residence {
    var rooms = [Room]()
    var numberOfRooms: Int {
        return rooms.count
    }
    subscript(i: Int) -> Room {
        get {
            return rooms[i]
        }
        set {
            rooms[i] = newValue
        }
    }
    func printNumberOfRooms() {
        print("The number of rooms is \(numberOfRooms)")
    }
    var address: Address?
}
```

Because this version of `Residence` stores an array of `Room` instances, its `numberOfRooms` property is implemented as a computed property, not a stored property. The computed `numberOfRooms` property simply returns the value of the count property from the rooms array.

As a shortcut to accessing its rooms array, this version of `Residence` provides a read-write subscript that provides access to the room at the requested index in the rooms array.

This version of `Residence` also provides a method called `printNumberOfRooms`, which simply prints the number of rooms in the residence.

Finally, `Residence` defines an optional property called address, with a type of `Address?`. The `Address` class type for this property is defined below.

The `Room` class used for the `rooms` array is a simple class with one property called `name`, and an initializer to set that property to a suitable room name:

```swift
class Room {
    let name: String
    init(name: String) { self.name = name }
}
```

The final class in this model is called `Address`. This class has three optional properties of type `String?`. The first two properties, `buildingName` and `buildingNumber`, are alternative ways to identify a particular building as part of an address. The third property, street, is used to name the street for that address:

```swift
class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?
    func buildingIdentifier() -> String? {
        if buildingNumber != nil && street != nil {
            return "\(buildingNumber) \(street)"
        } else if buildingName != nil {
            return buildingName
        } else {
            return nil
        }
    }
}
```

The `Address` class also provides a method called `buildingIdentifier()`, which has a return type of `String?`. This method checks the properties of the address and returns `buildingName` if it has a value, or `buildingNumber` concatenated with street if both have values, or `nil` otherwise.

### Accessing Properties Through Optional Chaining

As demonstrated in [Optional Chaining as an Alternative to Forced Unwrapping](16_optional_chaining.md#Optional-Chaining-as-an-Alternative-to-Forced-Unwrapping), you can use optional chaining to access a property on an optional value, and to check if that property access is successful.

Use the classes defined above to create a new Person instance, and try to access its `numberOfRooms` property as before:

```swift
let john = Person()
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// Надрукує "Unable to retrieve the number of rooms."
```

Because `john.residence` is `nil`, this optional chaining call fails in the same way as before.

You can also attempt to set a property’s value through optional chaining:

```swift
let someAddress = Address()
someAddress.buildingNumber = "29"
someAddress.street = "Acacia Road"
john.residence?.address = someAddress
```

In this example, the attempt to set the address property of `john.residence` will fail, because `john.residence` is currently `nil`.

The assignment is part of the optional chaining, which means none of the code on the right hand side of the `=` operator is evaluated. In the previous example, it’s not easy to see that `someAddress` is never evaluated, because accessing a constant doesn’t have any side effects. The listing below does the same assignment, but it uses a function to create the address. The function prints “Function was called” before returning a value, which lets you see whether the right hand side of the `=` operator was evaluated.

```swift
func createAddress() -> Address {
    print("Function was called.")
    
    let someAddress = Address()
    someAddress.buildingNumber = "29"
    someAddress.street = "Acacia Road"
    
    return someAddress
}
john.residence?.address = createAddress()
```

You can tell that the `createAddress()` function isn’t called, because nothing is printed.

### Calling Methods Through Optional Chaining

You can use optional chaining to call a method on an optional value, and to check whether that method call is successful. You can do this even if that method does not define a return value.

The `printNumberOfRooms()` method on the `Residence` class prints the current value of `numberOfRooms`. Here’s how the method looks:

```swift
func printNumberOfRooms() {
    print("The number of rooms is \(numberOfRooms)")
}
```

This method does not specify a return type. However, functions and methods with no return type have an implicit return type of `Void`, as described in [Functions Without Return Values](5_functions.md#Functions-Without-Return-Values). This means that they return a value of `()`, or an empty tuple.

If you call this method on an optional value with optional chaining, the method’s return type will be `Void?`, not `Void`, because return values are always of an optional type when called through optional chaining. This enables you to use an if statement to check whether it was possible to call the `printNumberOfRooms()` method, even though the method does not itself define a return value. Compare the return value from the `printNumberOfRooms` call against `nil` to see if the method call was successful:

```swift
if john.residence?.printNumberOfRooms() != nil {
    print("It was possible to print the number of rooms.")
} else {
    print("It was not possible to print the number of rooms.")
}
// Надрукує "It was not possible to print the number of rooms."
```

The same is true if you attempt to set a property through optional chaining. The example above in [Accessing Properties Through Optional Chaining](16_optional_chaining.md#Accessing-Properties-Through-Optional-Chaining) attempts to set an address value for `john.residence`, even though the `residence` property is `nil`. Any attempt to set a property through optional chaining returns a value of type `Void?`, which enables you to compare against `nil` to see if the property was set successfully:

```swift
if (john.residence?.address = someAddress) != nil {
    print("It was possible to set the address.")
} else {
    print("It was not possible to set the address.")
}
// Надрукує "It was not possible to set the address."
```

### Accessing Subscripts Through Optional Chaining

You can use optional chaining to try to retrieve and set a value from a subscript on an optional value, and to check whether that subscript call is successful.

> **Примітка**
> 
> When you access a subscript on an optional value through optional chaining, you place the question mark *before* the subscript’s brackets, not after. The optional chaining question mark always follows immediately after the part of the expression that is optional.

The example below tries to retrieve the name of the first room in the `rooms` array of the `john.residence` property using the subscript defined on the `Residence` class. Because `john.residence` is currently `nil`, the subscript call fails:

```swift
if let firstRoomName = john.residence?[0].name {
    print("The first room name is \(firstRoomName).")
} else {
    print("Unable to retrieve the first room name.")
}
// Надрукує "Unable to retrieve the first room name."
```

The optional chaining question mark in this subscript call is placed immediately after `john.residence`, before the subscript brackets, because `john.residence` is the optional value on which optional chaining is being attempted.

Similarly, you can try to set a new value through a subscript with optional chaining:

```swift
john.residence?[0] = Room(name: "Bathroom")
```

This subscript setting attempt also fails, because `residence` is currently `nil`.

If you create and assign an actual `Residence` instance to `john.residence`, with one or more `Room` instances in its `rooms` array, you can use the `Residence` subscript to access the actual items in the rooms array through optional chaining:

```swift
let johnsHouse = Residence()
johnsHouse.rooms.append(Room(name: "Living Room"))
johnsHouse.rooms.append(Room(name: "Kitchen"))
john.residence = johnsHouse
 
if let firstRoomName = john.residence?[0].name {
    print("The first room name is \(firstRoomName).")
} else {
    print("Unable to retrieve the first room name.")
}
// Надрукує "The first room name is Living Room."
```

#### Accessing Subscripts of Optional Type

If a subscript returns a value of optional type — such as the key subscript of Swift’s `Dictionary` type — place a question mark after the subscript’s closing bracket to chain on its optional return value:

```swift
var testScores = ["Dave": [86, 82, 84], "Bev": [79, 94, 81]]
testScores["Dave"]?[0] = 91
testScores["Bev"]?[0] += 1
testScores["Brian"]?[0] = 72
// the "Dave" array is now [91, 82, 84] and the "Bev" array is now [80, 94, 81]
```

The example above defines a dictionary called `testScores`, which contains two key-value pairs that map a `String` key to an array of `Int` values. The example uses optional chaining to set the first item in the `"Dave"` array to `91`; to increment the first item in the `"Bev"` array by `1`; and to try to set the first item in an array for a key of `"Brian"`. The first two calls succeed, because the testScores dictionary contains keys for `"Dave"` and `"Bev"`. The third call fails, because the `testScores` dictionary does not contain a key for `"Brian"`.

### Linking Multiple Levels of Chaining

You can link together multiple levels of optional chaining to drill down to properties, methods, and subscripts deeper within a model. However, multiple levels of optional chaining do not add more levels of optionality to the returned value.

To put it another way:

 + If the type you are trying to retrieve is not optional, it will become optional because of the optional chaining.
 + If the type you are trying to retrieve is *already* optional, it will not become *more* optional because of the chaining.

Therefore:

 + If you try to retrieve an `Int` value through optional chaining, an `Int?` is always returned, no matter how many levels of chaining are used.
 + Similarly, if you try to retrieve an `Int?` value through optional chaining, an `Int?` is always returned, no matter how many levels of chaining are used.

The example below tries to access the `street` property of the `address` property of the `residence` property of `john`. There are *two* levels of optional chaining in use here, to chain through the `residence` and `address` properties, both of which are of optional type:

```swift
if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// Надрукує "Unable to retrieve the address."
```

The value of `john.residence` currently contains a valid `Residence` instance. However, the value of `john.residence.address` is currently `nil`. Because of this, the call to `john.residence?.address?.street` fails.

Note that in the example above, you are trying to retrieve the value of the `street` property. The type of this property is `String?`. The return value of `john.residence?.address?.street` is therefore also `String?`, even though two levels of optional chaining are applied in addition to the underlying optional type of the property.

If you set an actual `Address` instance as the value for `john.residence.address`, and set an actual value for the address’s `street` property, you can access the value of the `street` property through multilevel optional chaining:

```swift
let johnsAddress = Address()
johnsAddress.buildingName = "The Larches"
johnsAddress.street = "Laurel Street"
john.residence?.address = johnsAddress
 
if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// Надрукує "John's street name is Laurel Street."
```

In this example, the attempt to set the address property of `john.residence` will succeed, because the value of `john.residence` currently contains a valid `Residence` instance.

### Chaining on Methods with Optional Return Values

The previous example shows how to retrieve the value of a property of optional type through optional chaining. You can also use optional chaining to call a method that returns a value of optional type, and to chain on that method’s return value if needed.

The example below calls the `Address` class’s `buildingIdentifier()` method through optional chaining. This method returns a value of type `String?`. As described above, the ultimate return type of this method call after optional chaining is also `String?`:

```swift
if let buildingIdentifier = john.residence?.address?.buildingIdentifier() {
    print("John's building identifier is \(buildingIdentifier).")
}
// Надрукує "John's building identifier is The Larches."
```

If you want to perform further optional chaining on this method’s return value, place the optional chaining question mark *after* the method’s parentheses:

```swift
if let beginsWithThe =
    john.residence?.address?.buildingIdentifier()?.hasPrefix("The") {
    if beginsWithThe {
        print("John's building identifier begins with \"The\".")
    } else {
        print("John's building identifier does not begin with \"The\".")
    }
}
// Надрукує "John's building identifier begins with "The"."
```
> **Примітка**
> 
> In the example above, you place the optional chaining question mark *after* the parentheses, because the optional value you are chaining on is the `buildingIdentifier()` method’s return value, and not the `buildingIdentifier()` method itself.