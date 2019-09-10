## Методи

*Методи* – це функції, що асоційовані із певним типом. Як класи, так і структури й перечислення можуть визначати методи екземплярів, що інкапсулюють певні завдання та функціональність для роботи із екземпляром даного типу. Класи, структури та перечислення також можуть визначати методи типу, котрі асоціюються із самим типом. Методи типу є аналогічними до методів класу у мові Objective-C.

Можливість стуктур та перечислень визначати методи у Swift є значною відмінністю цієї мови від мов C та Objective-C. У Objective-C, класи є єдиними типами, що можуть визначати методи. У Swift можна обирати між класом, структурою та перечисленням, маючи при цьому гнучкість для визначення методів у типі, що ви створюєте. 

### Методи екземплярів

*Метод екземпляру* – це функція, що належить до екземпляру певного класу, структури чи перечислення. Методи екземплярів формують функціональність цих екземплярів, або надаючи способи доступу та модифікації властивостей екземпляру, або надаючи функціональність, що відноситься до призначення екземпляру. Методи екземплярів мають точно такий же синтаксис, що й функції, які описані в розділі [Функції](5_functions.md).

Методи екземплярів записуються поміж фігурних дужок типу, до якого вони належать. Метод екземпляру має доступ до всіх інших методів та властивостей цього типу. Метод екземпляру можна викликати лише на певному екземплярі типу, до якого він належить. Його не можна викликати окремо від існуючого екземпляру.

Ось приклад простого класу `Counter`, котрий можна використовувати для підрахунку кількості наставань певної події:

```swiftclass Counter {    var count = 0    func increment() {        count += 1    }    func increment(by amount: Int) {        count += amount    }    func reset() {        count = 0    }}
```

Клас `Counter` визначає три методи екземпляру: 

 + `increment()` збільшує лічильник на `1`. 
 + `increment(by: Int)` збільшує лічильник на визначену цілочисельну величину.
 + `reset()` скидує лічильник до нуля.

Клас `Counter` також оголошує змінну властисіть `count` для відслідковування поточного значення лічильника. 

Методи екземпляру викликаються за допомогою синтаксису крапки, аналогічно до звернень до властивостей:

```swiftlet counter = Counter()// початкове значення лічильника 0counter.increment()// значення лічильника тепер 1counter.increment(by: 5)// значення лічильника тепер 6counter.reset()// значення лічильника тепер 0
```

Параметри функцій можуть мати як назву (для використання всередині тіла функції), так і мітку аргумента (для використання при виклику функціїи), як описано в підрозділі [Мітки аргументів функцій та імена їх параметрів](5_functions.md#Мітки-аргументів-функцій-та-імена-їх-параметрів). Це ж саме працює і для параметрів методів, оскільки методи є просто функціями, що асоційовані з певним типом.

#### Властивість self

Кожен екземпляр типу має неявну властивість на ім'я `self`, котра завжди дорівнює самому екземпляру. Властивість `self` використовується для посилання на поточний екземпляр всередині його власних методів.

Метод `increment()` з прикладу вище може бути записаний наступним чином:

```swiftfunc increment() {    self.count += 1}
```

На практиці, писати `self` у коді потрібно не дуже часто. 
In practice, you don’t need to write `self` in your code very often. If you don’t explicitly write `self`, Swift assumes that you are referring to a property or method of the current instance whenever you use a known property or method name within a method. This assumption is demonstrated by the use of `count` (rather than `self.count`) inside the three instance methods for `Counter`.
The main exception to this rule occurs when a parameter name for an instance method has the same name as a property of that instance. In this situation, the parameter name takes precedence, and it becomes necessary to refer to the property in a more qualified way. You use the `self` property to distinguish between the parameter name and the property name.
Here, `self` disambiguates between a method parameter called `x` and an instance property that is also called `x`:

```swiftstruct Point {    var x = 0.0, y = 0.0    func isToTheRightOf(x: Double) -> Bool {        return self.x > x    }}let somePoint = Point(x: 4.0, y: 5.0)if somePoint.isToTheRightOf(x: 1.0) {    print("This point is to the right of the line where x == 1.0")}// Prints "This point is to the right of the line where x == 1.0"
```
Without the `self` prefix, Swift would assume that both uses of `x` referred to the method parameter called `x`.
#### Modifying Value Types from Within Instance Methods
Structures and enumerations are *value types*. By default, the properties of a value type cannot be modified from within its instance methods.
However, if you need to modify the properties of your structure or enumeration within a particular method, you can opt in to *mutating* behavior for that method. The method can then mutate (that is, change) its properties from within the method, and any changes that it makes are written back to the original structure when the method ends. The method can also assign a completely new instance to its implicit `self` property, and this new instance will replace the existing one when the method ends.
You can opt in to this behavior by placing the `mutating` keyword before the `func` keyword for that method:

```swiftstruct Point {    var x = 0.0, y = 0.0    mutating func moveBy(x deltaX: Double, y deltaY: Double) {        x += deltaX        y += deltaY    }}var somePoint = Point(x: 1.0, y: 1.0)somePoint.moveBy(x: 2.0, y: 3.0)print("The point is now at (\(somePoint.x), \(somePoint.y))")// Prints "The point is now at (3.0, 4.0)"
```
The `Point` structure above defines a mutating `moveBy(x:y:)` method, which moves a `Point` instance by a certain amount. Instead of returning a new point, this method actually modifies the point on which it is called. The `mutating` keyword is added to its definition to enable it to modify its properties.
Note that you cannot call a mutating method on a constant of structure type, because its properties cannot be changed, even if they are variable properties, as described in [Stored Properties of Constant Structure Instances](9_properties.md#Stored-Properties-of-Constant-Structure-Instances):

```swiftlet fixedPoint = Point(x: 3.0, y: 3.0)fixedPoint.moveBy(x: 2.0, y: 3.0)// this will report an error
```
#### Assigning to self Within a Mutating Method
Mutating methods can assign an entirely new instance to the implicit `self` property. The `Point` example shown above could have been written in the following way instead:

```swiftstruct Point {    var x = 0.0, y = 0.0    mutating func moveBy(x deltaX: Double, y deltaY: Double) {        self = Point(x: x + deltaX, y: y + deltaY)    }}
```
This version of the mutating `moveBy(x:y:)` method creates a brand new structure whose `x` and `y` values are set to the target location. The end result of calling this alternative version of the method will be exactly the same as for calling the earlier version.
Mutating methods for enumerations can set the implicit `self` parameter to be a different case from the same enumeration:

```swiftenum TriStateSwitch {    case off, low, high    mutating func next() {        switch self {        case .off:            self = .low        case .low:            self = .high        case .high:            self = .off        }    }}var ovenLight = TriStateSwitch.lowovenLight.next()// ovenLight is now equal to .highovenLight.next()// ovenLight is now equal to .off
```
This example defines an enumeration for a three-state switch. The switch cycles between three different power states (`off`, `low` and `high`) every time its `next()` method is called.
### Type MethodsInstance methods, as described above, are methods that are called on an instance of a particular type. You can also define methods that are called on the type itself. These kinds of methods are called *type methods*. You indicate type methods by writing the `static` keyword before the method’s `func` keyword. Classes may also use the `class` keyword to allow subclasses to override the superclass’s implementation of that method.
> **Note**
> 
> In Objective-C, you can define type-level methods only for Objective-C classes. In Swift, you can define type-level methods for all classes, structures, and enumerations. Each type method is explicitly scoped to the type it supports.
Type methods are called with dot syntax, like instance methods. However, you call type methods on the type, not on an instance of that type. Here’s how you call a type method on a class called `SomeClass`:

```swiftclass SomeClass {    class func someTypeMethod() {        // type method implementation goes here    }}SomeClass.someTypeMethod()
```
Within the body of a type method, the implicit `self` property refers to the type itself, rather than an instance of that type. This means that you can use `self` to disambiguate between type properties and type method parameters, just as you do for instance properties and instance method parameters.
More generally, any unqualified method and property names that you use within the body of a type method will refer to other type-level methods and properties. A type method can call another type method with the other method’s name, without needing to prefix it with the type name. Similarly, type methods on structures and enumerations can access type properties by using the type property’s name without a type name prefix.The example below defines a structure called `LevelTracker`, which tracks a player’s progress through the different levels or stages of a game. It is a single-player game, but can store information for multiple players on a single device.
All of the game’s levels (apart from level one) are locked when the game is first played. Every time a player finishes a level, that level is unlocked for all players on the device. The `LevelTracker` structure uses type properties and methods to keep track of which levels of the game have been unlocked. It also tracks the current level for an individual player.

```swiftstruct LevelTracker {    static var highestUnlockedLevel = 1    var currentLevel = 1        static func unlock(_ level: Int) {        if level > highestUnlockedLevel { highestUnlockedLevel = level }    }        static func isUnlocked(_ level: Int) -> Bool {        return level <= highestUnlockedLevel    }        @discardableResult    mutating func advance(to level: Int) -> Bool {        if LevelTracker.isUnlocked(level) {            currentLevel = level            return true        } else {            return false        }    }}
```
The `LevelTracker` structure keeps track of the highest level that any player has unlocked. This value is stored in a type property called `highestUnlockedLevel`.
`LevelTracker` also defines two type functions to work with the `highestUnlockedLevel` property. The first is a type function called `unlock(_:)`, which updates the value of `highestUnlockedLevel` whenever a new level is unlocked. The second is a convenience type function called `isUnlocked(_:)`, which returns true if a particular level number is already unlocked. (Note that these type methods can access the `highestUnlockedLevel` type property without your needing to write it as `LevelTracker.highestUnlockedLevel`.)
In addition to its type property and type methods, `LevelTracker` tracks an individual player’s progress through the game. It uses an instance property called `currentLevel` to track the level that a player is currently playing.
To help manage the `currentLevel` property, `LevelTracker` defines an instance method called `advance(to:)`. Before updating `currentLevel`, this method checks whether the requested new level is already unlocked. The `advance(to:)` method returns a Boolean value to indicate whether or not it was actually able to set `currentLevel`. Because it’s not necessarily a mistake for code that calls the `advance(to:)` method to ignore the return value, this function is marked with the `@discardableResult` attribute. For more information about this attribute, see [Attributes](2_language_reference/07_attributes.md).
The `LevelTracker` structure is used with the Player class, shown below, to track and update the progress of an individual player:

```swiftclass Player {    var tracker = LevelTracker()    let playerName: String    func complete(level: Int) {        LevelTracker.unlock(level + 1)        tracker.advance(to: level + 1)    }    init(name: String) {        playerName = name    }}
```
The `Player` class creates a new instance of `LevelTracker` to track that player’s progress. It also provides a method called `complete(level:)`, which is called whenever a player completes a particular level. This method unlocks the next level for all players and updates the player’s progress to move them to the next level. (The Boolean return value of `advance(to:)` is ignored, because the level is known to have been unlocked by the call to `LevelTracker.unlock(_:)` on the previous line.)
You can create an instance of the `Player` class for a new player, and see what happens when the player completes level one:

```swiftvar player = Player(name: "Argyrios")player.complete(level: 1)print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")// Prints "highest unlocked level is now 2"
```
If you create a second player, whom you try to move to a level that is not yet unlocked by any player in the game, the attempt to set the player’s current level fails:

```swiftplayer = Player(name: "Beto")if player.tracker.advance(to: 6) {    print("player is now on level 6")} else {    print("level 6 has not yet been unlocked")}// Prints "level 6 has not yet been unlocked"
```