## Властивості

*Властивості* асоціюють значення із певним класом, структурою чи перечисленням. Властивості бувають двох видів: ті, що зберігаються, та ті, що обчислюються. Ті, що зберігаються, зберігають константу чи змінну як частину екземпляру, в той час як ті, що обчислюються, лише обчислюють значення, не зберігаючи нічого. Властивості, що обчислюються, можуть міститись у класах, структурах та перечисленнях. Властивості, що зберігаються, можуть міститись лише у класах та структурах. 

Властивості, що зберігаються та що обчислюються, зазвичай є асоційованими із екземпляром певного типу. Однак, властивості такожи можуть бути асоційовані із самим типом. Такі властивості називають властивостями типу. 

Окрім властивостей, можна також створювати спостерігачі за властивостями, щоб стежити за значенням властивості і реагувати на їх зміни. Спостерігачі за властивостями можна додавати до властивостей, що зберігаються і що створені вами, а також до властивостей, які клас-нащадок успадковує від батьківського класу. 

### Властивості, що зберігаються

У найпростішій формі, властивість, що зберігається – це константа чи змінна, що зберігається як частина певного класу чи структури. Властивості, що зберігаються, можуть бути або *змінними властивостями, що зберігаються* (оголошуються за допомогою ключового слова `var`), або *константними  властивостями, що зберігаються* (оголошуються за допомогою ключового слова `let`).

В оголошенні властивості, що зберігається, можна надавати значення за замовчанням, як описано у підрозділі [Значення властивостей за замовчанням](13_initialization.md#Значення-властивостей-за-замовчанням). Можна задавати та змінювати початкове значення властивості, що зберігається, під час ініціалізації. Це працює навіть для константних властивостей, що зберігаються, як описано у підрозділі [Присвоєння значень константним властивостям під час ініціалізації](13_initialization.md#Присвоєння-значень-константним-властивостям-під-час-ініціалізації).

У прикладі нижче оголошено стурктуру із назвою `FixedLengthRange`, котра описує діапазон цілих чисел фіксованої довжини:

```swiftstruct FixedLengthRange {    var firstValue: Int    let length: Int}var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// діапазон представляє цілі числа 0, 1 та 2rangeOfThreeItems.firstValue = 6
// діапазон тепер представляє цілі числа 6, 7, та 8
```

Екземпляри структури `FixedLengthRange` мають змінну властивість, що зберігається на ім'я `firstValue` та константну властивість, що зберігається на ім'я `length`. У прикладі вище, властивість `length` було ініціалізовано при створенні нового діапазону, і вона не може надалі змінюватись, оскільки це – константна властивість. 

#### Властивості константних структур, що зберігаються

Якщо створити екземпляр структури та присвоїти його константі, буде неможливо змінити його властивості, не дивлячись навіть на те, що їх було оголошено змінними властивостями:

```swiftlet rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// цей діапазон представляє цілі числа 0, 1, 2, та 3rangeOfFourItems.firstValue = 6
// тут буде повідомлення про помилку, хоч firstValue і є змінною властивістю
```

Оскільки `rangeOfFourItems` оголошено як константу (за допомогою ключового слова `let`), неможливо змінити її властивість `firstValue`, хоч `firstValue` і є змінною властивістю.

Ця поведінка властива структурам внаслідок того, що вони – *типи-значення*. Коли екземпляр типу-значення позначений як константа, такими ж стають і всі його властивості. 

Це ж саме не є справедливим для класів, оскільки вони є *типами-посиланнями*. Якщо присвоїти екземпляр типу-посилання константі, все ще можна модифікувати змінні властивості цього екземпляру. 

#### Ліниві властивості, що зберігаються

*Лінива властивість, що зберігається* – це властивість, чиє початкове значення не обчислюється до її першого використання. Щоб позначити властивість, що зберігається, лінивою, перед її оголошенням вживають ключове слово `lazy`.
> **Примітка**
> 
> Слід завжди оголошувати ліниві властивості як змінні (за допомогою ключового слова `var`), оскільки їх початкове значення не можна отримати допоки ініціалізація екземпляру не закінчиться. Константні властивості повинні завжди мати значення до завершення ініціалізації екземпляру, тому їх не можна оголошувати лінивими. 

Ліниві властивості доцільно використовувати, коли початкове значення властивості залежить від зовнішніх чинників, чиї значення невідомі до завершення ініціалізації екземпляру. Ліниві властивості також доцільні в ситуаціях, коли обчислення початкового значення влативості вимагає складних чи обчислювально дорогих операцій, котрі не бажано виконувати без реальної необхідності. 

У прикладі нижче ліниву властивость, що зберігаються, застосовано для уникнення зайвої ініціалізації в складному класі. У цьому прикладі оголошено два класи із назвами `DataImporter` та `DataManager`, жоден з яких не показаний повністю:

```swiftclass DataImporter {    /*
     DataImporter – це клас, що імпортує дані із зовнішнього файлу.
     Будемо вважати, що клас ініціалізація класу DataImporter займає суттєву кільківть часу.     */    var fileName = "data.txt"
    // клас DataImporter буде реалізовувати функціональність імпорту даних тут} class DataManager {    lazy var importer = DataImporter()    var data = [String]()
    // клас DataManager буде реалізовувати функціональність управління даними тут} let manager = DataManager()manager.data.append("Some data")manager.data.append("Some more data")
// на даний момент досі не створено екземпляру класу DataImporter, що зберігається у властивості importer
```

Клас `DataManager` містить властивість, що зберігається, на ім'я `data`, котру ініціалізовано новим порожнім масивом рядків. Хоч решта функціональності класу `DataManager` не приводитья, метою цього класу є управління та надання доступу до цього масиву даних у вигляді рядків.

Частиною функціональності класу `DataManager` є можливість імпортувати дані із файлу. Ця функціональність реалізовується в класі `DataImporter`, і ми будемо вважати, що його ініціалізація займає значну кількість часу. Це може бути через те, що екземпляр `DataImporter` при ініціалізації повинен відкрити файл і прочитати його вміст у пам'ять.

Екземпляр класу `DataManager` може керувати своїми даними, жодного разу не імпортувавши ці дані із файлу, тому нема необхідності створювати новий екземпляр `DataImporter` щоразу при створенні екземпляру самого `DataManager`. Натомість, більше сенсу має створення екземпляру `DataImporter` при першому використанні.

Оскільки властивість `importer` позначено модифікатором `lazy`, екземпляр `DataImporter` буде створено лише при першому звертанні до властивості `importer`, наприклад, при звертанні до властивості `fileName`:

```swiftprint(manager.importer.fileName)// на даний момент створено екземпляр DataImporter, що зберігається у властивості importer// Надрукує "data.txt"
```
> **Примітка**
> 
> Якщо властивість позначено модифікатором `lazy`, до неї звертаються із різних потоків одночасно, і при цьому дану властивість ще не було проініціалізовано, немає гарантії, що властивість буде проініціалізовано тільки раз.

#### Властивості, що зберігаються, та змінні екземпляру

Якщо ви маєте досвід роботи з Objective-C, вам може бути відомо, що в цій мові є *два* способи зберігати значення та посилання як частину екземпляру класу. Окрім властивостей, там можна створювати змінні екземпляру, як внутрішнє вмістилище для значень, що зберігаються у властивості. 

У мові Swift ці концепції об'єднано в єдине оголошення властивості. Властивості у Swift не мають відповідної змінної екземпляру, і до внутрішнього вмістилища значення властивості нема прямого доступу. Цей підхід дозволяє уникати плутанини щодо того, як звертатись до значення у різних контекстах та спрощує оголошення властивості до єдиної вичерпної інструкції. Вся інформація про властивість, включаючи її назву, тип, та характеристики управління пам'яттю, оголошується в єдиному місці як частина оголошення типу. 

### Властивості, що обчислюються

Окрім властивостей, що зберігаються, у класах, структурах та перечисленнях можна створювати *властивості, що обчислюються*, котрі фактично не зберігають значеннь, Натомість, вони визначають спосіб звертатись до інших властивостей чи змінювати їх непрямо. Звертатись до інших властивостей можна через спеціальну функцію: "геттер". Змінювати їх – через спеціальну функцію "сеттер". Для властивостей, що обчислюються, обов'язковою є лише наявність геттера.

```swiftstruct Point {    var x = 0.0, y = 0.0}struct Size {    var width = 0.0, height = 0.0}struct Rect {    var origin = Point()    var size = Size()    var center: Point {        get {            let centerX = origin.x + (size.width / 2)            let centerY = origin.y + (size.height / 2)            return Point(x: centerX, y: centerY)        }        set(newCenter) {            origin.x = newCenter.x - (size.width / 2)            origin.y = newCenter.y - (size.height / 2)        }    }}var square = Rect(origin: Point(x: 0.0, y: 0.0),                  size: Size(width: 10.0, height: 10.0))let initialSquareCenter = square.centersquare.center = Point(x: 15.0, y: 15.0)print("square.origin тепер знаходиться в точці (\(square.origin.x), \(square.origin.y))")// Надрукує "square.origin тепер знаходиться в точці (10.0, 10.0)"
```

У даному прикладі визначаються три структури для роботи із геометричними фігурами:

 + `Point` інкапсулює точку з координатами x та y. + `Size` інкапсулює розмір із шириною `width` та висотою `height`.
 + `Rect` визначає прямокутник за лівою нижньою вершиною `origin` та розміром `size`.

Структура `Rect` також містить властивість, що обчислюється, на ім'я `center` (центр). Поточне значення центру прямокутника `Rect` завжди можна визначити за його розміром `size` та одною з вершин, зокрема з  `origin`, тому немає необхідності зберігати значення центру у вигляді точки `Point` явно. Натомість, `Rect` визначає властні геттер та сеттер для властивості, що обчислюється, на ім'я `center`, щоб працювати із центром прямокутника так, ніби це насправді властивість, що зберігається. 

У попередньому прикладі створено нову змінну типу `Rect` на ім'я `square`. Змінну `square` проініціалізовано лівою нижньою вершиною `(0, 0)`, та розміром `10` на `10`. Цей квадрат зображено як синій квадрат на графіку нижче. 

Далі йде звернення до властивості `center` змінної `square` за допомогою синтаксису крапки  (`square.center`), що спричиняє виклик геттера властивості `center`, і повернення поточного значення властивості. Замість повернення існуючого значення, геттер обчислює та повертає нове значення `Point`, що відповідає центру квадрата. Як видно вище, геттер коректно обчислює центральну точку `(5, 5)`.

Потім властивості `center` присвоєно нове значення `(15, 15)`, таким чином даний квадрат рухається вверх та вправо, на нове місце, яке зображено як помаранчевий квадрат на графіку нижче. Присвоєння значення властивості `center` спричиняє виклик її сеттера, котрий змінює значення `x` та `y` у властивості `origin`, що зберігається в `Rect`, що, власне, і пересуває даний квадрат на нову позицію. 

![](images/computedProperties_2x.png)￼#### Shorthand Setter Declaration
If a computed property’s setter does not define a name for the new value to be set, a default name of `newValue` is used. Here’s an alternative version of the `Rect` structure, which takes advantage of this shorthand notation:

```swiftstruct AlternativeRect {    var origin = Point()    var size = Size()    var center: Point {        get {            let centerX = origin.x + (size.width / 2)            let centerY = origin.y + (size.height / 2)            return Point(x: centerX, y: centerY)        }        set {            origin.x = newValue.x - (size.width / 2)            origin.y = newValue.y - (size.height / 2)        }    }}
```

#### Властивості тільки для читання

Властивість, що має геттер, але не має сеттера, називають *властивістю тільки для читання*. Тільки властивості, що обчислюються, можуть бути властивостями тільки для читання. Такі властивості завжди повертають значення, яке можна дістати шляхом використання синтаксису крапки, але їм не можна присвоїти інше значення. 
> **Примітка**
> 
> Слід оголошувати властивості, що обчислюються (включно із властивостями тільки для читання), як змінні властивості, за допомогою ключового слова `var`, бо їх значення не є фіксованими. Ключове слово `let` використовуються лише для константних властивостей, щоб позначити, що їх значення не можуть змінюватись після присвоєння у ході ініціалізації. 

Можна спростити оголошення властивості тільки для читання, опустивши ключове слово `get` та відповідні фігурні дужки:

```swiftstruct Cuboid {    var width = 0.0, height = 0.0, depth = 0.0    var volume: Double {        return width * height * depth    }}let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)print("об'єм куба fourByFiveByTwo дорівнює \(fourByFiveByTwo.volume)")// Надрукує "об'єм куба fourByFiveByTwo дорівнює 40.0"
```

У даному прикладі створено нову структуру на ім'я `Cuboid`, що моделює тривимірний прямокутний паралелепіпед із властивостями `width` для ширини, `height` для висоти, та `depth` для глибини. Ця структура також має властивість тільки для читання на ім'я `volume`, котра обчислює та повертає об'єм даного паралелепіпеда. Немає сенсу створювати сеттер у властивості `volume`, бо тоді буде незрозуміло, як саме змінювати значення `width`, `height`, та `depth` при зміні об'єму. Тим не менше, дана властивість тільки для читання структури `Cuboid` є корисною: користувачі даної структури можуть дізнаватись обчислений об'єм. 
### Property Observers
Property observers observe and respond to changes in a property’s value. Property observers are called every time a property’s value is set, even if the new value is the same as the property’s current value.
You can add property observers to any stored properties you define, except for lazy stored properties. You can also add property observers to any inherited property (whether stored or computed) by overriding the property within a subclass. You don’t need to define property observers for nonoverridden computed properties, because you can observe and respond to changes to their value in the computed property’s setter. Property overriding is described in [Overriding](12_inheritance.md#Overriding).
You have the option to define either or both of these observers on a property: + `willSet` is called just before the value is stored.
 + `didSet` is called immediately after the new value is stored.
 If you implement a `willSet` observer, it’s passed the new property value as a constant parameter. You can specify a name for this parameter as part of your `willSet` implementation. If you don’t write the parameter name and parentheses within your implementation, the parameter is made available with a default parameter name of `newValue`.
Similarly, if you implement a `didSet` observer, it’s passed a constant parameter containing the old property value. You can name the parameter or use the default parameter name of `oldValue`. If you assign a value to a property within its own didSet observer, the new value that you assign replaces the one that was just set.
> **Примітка**
> > The `willSet` and `didSet` observers of superclass properties are called when a property is set in a subclass initializer, after the superclass initializer has been called. They are not called while a class is setting its own properties, before the superclass initializer has been called.
> > For more information about initializer delegation, see [Initializer Delegation for Value Types](13_initialization.md#Initializer-Delegation-for-Value-Types) and [Initializer Delegation for Class Types](13_initialization.md#Initializer-Delegation-for-Class-Types).
Here’s an example of `willSet` and `didSet` in action. The example below defines a new class called `StepCounter`, which tracks the total number of steps that a person takes while walking. This class might be used with input data from a pedometer or other step counter to keep track of a person’s exercise during their daily routine.

```swiftclass StepCounter {    var totalSteps: Int = 0 {        willSet(newTotalSteps) {            print("About to set totalSteps to \(newTotalSteps)")        }        didSet {            if totalSteps > oldValue  {                print("Added \(totalSteps - oldValue) steps")            }        }    }}let stepCounter = StepCounter()stepCounter.totalSteps = 200// About to set totalSteps to 200// Added 200 stepsstepCounter.totalSteps = 360// About to set totalSteps to 360// Added 160 stepsstepCounter.totalSteps = 896// About to set totalSteps to 896// Added 536 steps
```
The `StepCounter` class declares a `totalSteps` property of type `Int`. This is a stored property with `willSet` and `didSet` observers.
The `willSet` and `didSet` observers for `totalSteps` are called whenever the property is assigned a new value. This is true even if the new value is the same as the current value.
This example’s `willSet` observer uses a custom parameter name of `newTotalSteps` for the upcoming new value. In this example, it simply prints out the value that is about to be set.
The `didSet` observer is called after the value of `totalSteps` is updated. It compares the new value of `totalSteps` against the old value. If the total number of steps has increased, a message is printed to indicate how many new steps have been taken. The `didSet` observer does not provide a custom parameter name for the old value, and the default name of `oldValue` is used instead.
> **Примітка**
> 
> If you pass a property that has observers to a function as an in-out parameter, the `willSet` and `didSet` observers are always called. This is because of the copy-in copy-out memory model for in-out parameters: The value is always written back to the property at the end of the function. For a detailed discussion of the behavior of in-out parameters, see [In-Out Parameters](2_language_reference/06_declarations.md#In-Out-Parameters).
### Global and Local Variables
The capabilities described above for computing and observing properties are also available to *global variables* and *local variables*. Global variables are variables that are defined outside of any function, method, closure, or type context. Local variables are variables that are defined within a function, method, or closure context.
The global and local variables you have encountered in previous chapters have all been *stored variables*. Stored variables, like stored properties, provide storage for a value of a certain type and allow that value to be set and retrieved.
However, you can also define *computed variables* and define observers for stored variables, in either a global or local scope. Computed variables calculate their value, rather than storing it, and they are written in the same way as computed properties.
> **Примітка**
> > Global constants and variables are always computed lazily, in a similar manner to [Lazy Stored Properties](9_properties.md#Lazy-Stored-Properties). Unlike lazy stored properties, global constants and variables do not need to be marked with the `lazy` modifier.
> > Local constants and variables are never computed lazily.
### Type Properties
Instance properties are properties that belong to an instance of a particular type. Every time you create a new instance of that type, it has its own set of property values, separate from any other instance.
You can also define properties that belong to the type itself, not to any one instance of that type. There will only ever be one copy of these properties, no matter how many instances of that type you create. These kinds of properties are called *type properties*.
Type properties are useful for defining values that are universal to all instances of a particular type, such as a constant property that all instances can use (like a static constant in C), or a variable property that stores a value that is global to all instances of that type (like a static variable in C).
Stored type properties can be variables or constants. Computed type properties are always declared as variable properties, in the same way as computed instance properties.
> **Примітка**
> 
> Unlike stored instance properties, you must always give stored type properties a default value. This is because the type itself does not have an initializer that can assign a value to a stored type property at initialization time.
> > Stored type properties are lazily initialized on their first access. They are guaranteed to be initialized only once, even when accessed by multiple threads simultaneously, and they do not need to be marked with the `lazy` modifier.
 #### Type Property Syntax
In C and Objective-C, you define static constants and variables associated with a type as *global* static variables. In Swift, however, type properties are written as part of the type’s definition, within the type’s outer curly braces, and each type property is explicitly scoped to the type it supports.
You define type properties with the `static` keyword. For computed type properties for class types, you can use the `class` keyword instead to allow subclasses to override the superclass’s implementation. The example below shows the syntax for stored and computed type properties:

```swiftstruct SomeStructure {    static var storedTypeProperty = "Some value."    static var computedTypeProperty: Int {        return 1    }}enum SomeEnumeration {    static var storedTypeProperty = "Some value."    static var computedTypeProperty: Int {        return 6    }}class SomeClass {    static var storedTypeProperty = "Some value."    static var computedTypeProperty: Int {        return 27    }    class var overrideableComputedTypeProperty: Int {        return 107    }}
```
> **Примітка**
> > The computed type property examples above are for read-only computed type properties, but you can also define read-write computed type properties with the same syntax as for computed instance properties.
 #### Querying and Setting Type Properties
Type properties are queried and set with dot syntax, just like instance properties. However, type properties are queried and set on the *type*, not on an instance of that type. For example:

```swiftprint(SomeStructure.storedTypeProperty)// Надрукує "Some value."SomeStructure.storedTypeProperty = "Another value."print(SomeStructure.storedTypeProperty)// Надрукує "Another value."print(SomeEnumeration.computedTypeProperty)// Надрукує "6"print(SomeClass.computedTypeProperty)// Надрукує "27"
```
The examples that follow use two stored type properties as part of a structure that models an audio level meter for a number of audio channels. Each channel has an integer audio level between `0` and `10` inclusive.
The figure below illustrates how two of these audio channels can be combined to model a stereo audio level meter. When a channel’s audio level is `0`, none of the lights for that channel are lit. When the audio level is 10, all of the lights for that channel are lit. In this figure, the left channel has a current level of `9`, and the right channel has a current level of `7`:￼
![](images/staticPropertiesVUMeter_2x.png)
The audio channels described above are represented by instances of the `AudioChannel` structure:

```swiftstruct AudioChannel {    static let thresholdLevel = 10    static var maxInputLevelForAllChannels = 0    var currentLevel: Int = 0 {        didSet {            if currentLevel > AudioChannel.thresholdLevel {                // cap the new audio level to the threshold level                currentLevel = AudioChannel.thresholdLevel            }            if currentLevel > AudioChannel.maxInputLevelForAllChannels {                // store this as the new overall maximum input level                AudioChannel.maxInputLevelForAllChannels = currentLevel            }        }    }}
```
The `AudioChannel` structure defines two stored type properties to support its functionality. The first, `thresholdLevel`, defines the maximum threshold value an audio level can take. This is a constant value of `10` for all `AudioChannel` instances. If an audio signal comes in with a higher value than `10`, it will be capped to this threshold value (as described below).
The second type property is a variable stored property called `maxInputLevelForAllChannels`. This keeps track of the maximum input value that has been received by *any* `AudioChannel` instance. It starts with an initial value of `0`.
The `AudioChannel` structure also defines a stored instance property called `currentLevel`, which represents the channel’s current audio level on a scale of `0` to `10`.
The `currentLevel` property has a `didSet` property observer to check the value of `currentLevel` whenever it is set. This observer performs two checks:
 + If the new value of `currentLevel` is greater than the allowed `thresholdLevel`, the property observer caps `currentLevel` to `thresholdLevel`. + If the new value of `currentLevel` (after any capping) is higher than any value previously received by *any* `AudioChannel` instance, the property observer stores the new `currentLevel` value in the `maxInputLevelForAllChannels` type property.
 > **Примітка**
> > In the first of these two checks, the `didSet` observer sets `currentLevel` to a different value. This does not, however, cause the observer to be called again.
You can use the `AudioChannel` structure to create two new audio channels called `leftChannel` and `rightChannel`, to represent the audio levels of a stereo sound system:

```swiftvar leftChannel = AudioChannel()var rightChannel = AudioChannel()
```
If you set the currentLevel of the *left* channel to `7`, you can see that the `maxInputLevelForAllChannels` type property is updated to equal `7`:

```swiftleftChannel.currentLevel = 7print(leftChannel.currentLevel)// Надрукує "7"print(AudioChannel.maxInputLevelForAllChannels)// Надрукує "7"
```
If you try to set the `currentLevel` of the *right* channel to `11`, you can see that the right channel’s `currentLevel` property is capped to the maximum value of `10`, and the `maxInputLevelForAllChannels` type property is updated to equal `10`:

```swiftrightChannel.currentLevel = 11print(rightChannel.currentLevel)// Надрукує "10"print(AudioChannel.maxInputLevelForAllChannels)// Надрукує "10"
```