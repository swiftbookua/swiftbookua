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

На практиці, писати `self` у коді потрібно не дуже часто. Якщо вседерині певного методу не вказати явно `self`, Swift припускає, що відбувається посилання на властивість чи метод поточного екземпляру при будь-якому використанні відомої назви властивості чи методу. Це припущення демонтсрується використанням назви властивості `count` (замість `self.count`) всередині трьох методів класу `Counter` у прикладі вище.

Головним виключенням з цього правила є колізія імен: наприклад, коли ім'я параметру методу екземпляру співпадає з іменем властивості цього екземпляру. У цій ситуації, ім'я параметру має пріорітет, і стає потрібним звертатись до властивості більш точним способом. В подібних випадках влатсивість `self` використовується для розрізнення імені параметру та імені властивості.

Тут властивсіть `self` допомагає відрізнити параметр методу на ім'я `x` від властивості екземпляру, що теж називається `x`:

```swiftstruct Point {    var x = 0.0, y = 0.0    func isToTheRightOf(x: Double) -> Bool {        return self.x > x    }}let somePoint = Point(x: 4.0, y: 5.0)if somePoint.isToTheRightOf(x: 1.0) {
    print("Ця точка знаходиться справа від лінії, де x == 1.0")}// Надрукує "Ця точка знаходиться справа від лінії, де x == 1.0"
```

Без префіксу `self`, компілятор Swift би припустив би, що обидва `x` посилаються на параметр методу на ім'я `x`.

#### Зміни типів-значень в методах екземплярів

Структури та перечислення є *типами-значеннями*. За замовчанням, властивості типів-значень не можуть бути змінені всередині методів екземпляру. 

Однак, якщо потрібно змінити значення властивості структури чи перечислення всередині певного методу, це можна зробити шлляхом увімкнення *мутуючої* поведінки для цього методу. Після цього метод може змінювати властивості екземпляру, і ці зміни записуються назад в оригінальну структуру після закінчення методу. Метод може навіть присвоїти повністю новий екземпляр неявній властивості `self`, і цей новий екземпляр замінить собою існуючий екземпляр по закінченню метода.

Мутуюча поведінка методу вмикається шляхом вказування ключового слова `mutating` перед ключовим словом `mutating` в оголошенні цього метода:

```swiftstruct Point {    var x = 0.0, y = 0.0    mutating func moveBy(x deltaX: Double, y deltaY: Double) {        x += deltaX        y += deltaY    }}var somePoint = Point(x: 1.0, y: 1.0)somePoint.moveBy(x: 2.0, y: 3.0)print("Точка тепер має координати (\(somePoint.x), \(somePoint.y))")// Надрукує "Точка тепер має координати (3.0, 4.0)"
```

Структура `Point` моделює точку на площині з координатами `x` та `y`; вона визначає мутуючий метод `moveBy(x:y:)`, що пересовує точку на певну величину. Замість того, щоб повернути нову точку, цей метод фактично змінює саму точку, на якій він був викликаний. Ключове слово `mutating` додається до оголошення цього методу, щоб дозволити йому модифікувати властивості свого екземпляру. 

Слід помітити, що не можна викликати мутуючий метод на константі типу-структури, оскільки її властивості не можна змінювати, навіть якщо вони і оголошені як змінні властивості. Це детально описано в підрозділі [Властивості константних структур, що зберігаються](9_properties.md#Властивості-константних-структур,-що-зберігаються). Наприклад:

```swiftlet fixedPoint = Point(x: 3.0, y: 3.0)fixedPoint.moveBy(x: 2.0, y: 3.0)// тут буде повідомлення про помилку компіляції
```

#### Присвоєння self у мутуючому методі

У мутуючому методі можна присвоїти цілком новий екземпляр неявній властивості `self`. Приклад зі структурою `Point` вище можна переписати у наступний спосіб:

```swiftstruct Point {    var x = 0.0, y = 0.0    mutating func moveBy(x deltaX: Double, y deltaY: Double) {        self = Point(x: x + deltaX, y: y + deltaY)    }}
```

У даній версії мутуючого методу `moveBy(x:y:)` створюється новий екземпляр структури зі значеннями `x` та `y`, що дорівнюють результату перенесення. Кінцевий результат виклику цієї альтернативної версії методу буде точно таким же, як і результат виклику попередньої версії.

Мутуючі методи перечислень можуть присвоїти неявному параметру `self` інший елемент цього ж перечислення:

```swiftenum TriStateSwitch {    case off, low, high    mutating func next() {        switch self {        case .off:            self = .low        case .low:            self = .high        case .high:            self = .off        }    }}var ovenLight = TriStateSwitch.lowovenLight.next()// ovenLight тепер дорівнює .highovenLight.next()// ovenLight тепер дорівнює .off
```

У даному прикладі визначено перечислення, що моделює вимикач із трьома станами. Вимикач циклічно змінює свій стан (поміж `off`, `low` та `high`) щоразу коли викликається його метод `next()`.
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

```swiftvar player = Player(name: "Argyrios")player.complete(level: 1)print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")// Надрукує "highest unlocked level is now 2"
```
If you create a second player, whom you try to move to a level that is not yet unlocked by any player in the game, the attempt to set the player’s current level fails:

```swiftplayer = Player(name: "Beto")if player.tracker.advance(to: 6) {    print("player is now on level 6")} else {    print("level 6 has not yet been unlocked")}// Надрукує "level 6 has not yet been unlocked"
```