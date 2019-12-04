## Автоматичний підрахунок посилань

У Swift для відслідковування й керування використанням пам'яті ваших додатків використовується механізм *автоматичного підрахунку посилань* (*Automatic Reference Counting, або ARC)*. В більшості випадків, це означає що управління пам'яттю у Swift “просто працює”, і вам не потрібно замислюватись про управління пам'яттю самостійно. ARC автоматично звільняє пам'ять, що використовується екземплярами класів, коли ці екземпляри більше не потрібні. 

Однак, у невеликій кількості випадків ARC вимагає додаткової інформації про взаємозв'язки між частинами вашого коду для автоматичного керування пам'яттю. У даному розділі описуються ці ситуації, і показано, як ви можете дати можливість ARC керувати всією пам'яттю вашого додатку. Використання ARC у Swift є дуже подібним до підходу, описаного в статті *Transitioning to ARC Release Notes* for using ARC для Objective-C.

> **Примітка**
> 
> Reference counting only applies to instances of classes. Structures and enumerations are value types, not reference types, and are not stored and passed by reference.

### Як працює ARC

Щоразу при створенні нового екземпляру класу, ARC виділяє блок пам'яті для зберігання інформації про цей екземпляр. У цьому блоці зберігається інформація про тип екземпляру, а також значення всіх властивостей, що зберігаються, асоційованих із даним екземпляром. 

Додатково, коли екземпляр більше не потрібен, ARC звілняє пам'ять, котра використовувалась даним екземпляром, щоб її можна було використовувати для інших потреб. Це гарантує, що екземпляри класів не займають місце в пам'яті, коли вони більше не потрібні. 

Однак, якби ARC деалокував екземпляри, котрі ще використовуються, було би більше не можливо звертатись до властивостей цього екземпляру, чи викликати його методи. Справді, якщо спробувати звернутись до такого екземпляру, додаток найімовірніше буде закритим через помилку. 

Щоб гарантувати, що екземпляри не зникають, поки вони ще потрібні, ARC відслідковує, скільки властивостей, констант та змінних в даний момент посилаються на кожен екземпляр класу. ARC не буде деалокувати екземпляр допоки існує хоча б одне активне посилання на цей екземпляр. 

Щоб це було можливим, щоразу при присвоєнні екземпляру класу властивості, константі, чи змінній, ця властивість, константа чи змінна утворює *сильне посилання* на екземпляр. Це посилання називають “сильним”, бо воно тримається міцно за цей екземпляр, і не дає йому бути деалокованим допоки існує це сильне посилання. 

### ARC у дії

Ось приклад роботи автоматичного підрахунку посилань. Даний приклад починається з простого класу, що називається `Person`, котрий визначає властивість, що зберігається, на ім'я `name`:

```swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
        print("\(name) ініціалізується")
    }
    deinit {
        print("\(name) деініціалізується")
    }
}
```

Клас `Person` має ініціалізатор, котрий присвоює значення властивості екземпляру `name` та друкує повідомлення про те, що триває ініціалізація. Клас `Person` також має деініціалізатор, котрий друкує повідомвлення про те, що екземпляр класу деалокується. 

У наступному фрагменті коду оголошено три змінні типу `Person?`, котрі використовуватимуться для створення кількох посилань на новий екземпляр `Person` у наступних фрагментах коду. Оскільки ці змінні мають опціональний тип (`Person?`, не `Person`), вони автоматично ініціалізуються значенням `nil`, і на даний момент не посилаються на екземпляр `Person`.

```swift
var reference1: Person?
var reference2: Person?
var reference3: Person?
```

Тепер можна створити новий екземпляр `Person` та присвоїти його одній з цих трьох змінних:

```swift
reference1 = Person(name: "Дмитро Клюшин")
// Надрукує "Дмитро Клюшин ініціалізується"
```

Слід зауважити, що повідомлення `"Дмитро Клюшин ініціалізується"` друкується в момент виклику ініціалізатора класу `Person`. Це підтверджує факт ініціалізації. 

Оскільки новий екземпляр `Person` було присвоєно змінній `reference1`, тепер є сильне посилання змінної `reference1` на новий екземпляр `Person`. Оскільки є хоча б одне сильне посилання, ARC тримає цей екземпляр `Person` у пам'ятті та не деалокує його.

Якщо присвоєти той же екземпляр `Person` двом іншим змінним, буде утворено ще два сильних посилання на цей екземпляр:

```swift
reference2 = reference1
reference3 = reference1
```

Тепер є *три* сильних посилання на цей єдиний екземпляр `Person`.

Якщо прибрати два з цих трьох сильних посилань (включно з початковим посиланням) шляхом присвоєння `nil` двом змінним, одне сильне посилання залишиться, і екземпляр `Person` не буде деалоковано:

```swift
reference1 = nil
reference2 = nil
```

ARC не деалокуватиме екземпляр `Person` допоки не буде прибрано третє й останнє сильне посилання, і в цей момент стає ясно, що екземпляр `Person` більше ніде не використовується:

```swift
reference3 = nil
// Надрукує "Дмитро Клюшин деініціалізується"
```

### Цикли сильних посилань між екземплярами класів

У прикладах вище, ARC дозволяє відслідковувати кількість посилань на новий екземпляр `Person` та деалокувати екземпляр `Person`, коли він більше не потрібен. 

Однак, можливо написати код, у котрому екземпляр класу ніколи не добирається до точки, коли він має нуль сильних посилань. Це може статись, коли два екземпляри класу тримають сильні посилання один на одного, таким чином тримаючи один одного живим. Такі ситуації називають *циклами сильних посилань*.

Цикли сильних посилань можна вирішити шляхом використання слабких (`weak`) чи безхазяйних (`unowned`) посилань замість сильних. Даний процес описаний у підрозділі [Вирішення циклів сильних посилань між екземплярами класів](15_automatic_reference_counting.md#Вирішення-циклів-сильних-посилань-між-екземплярами-класів). Однак, перед тим як вирішувати цикли сильних посилань, корисно зрозуміти, як такі цикли утворюються. 

Ось приклад випадкового створення циклу сильних посилань. У даному прикладі оголошено два класи, `Person` та `Apartment`, котрі моделюють особу та її помешкання відповідно:

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) деініціалізується") }
}
 
class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    var tenant: Person?
    deinit { print("Помешкання \(unit) деініціалізується") }
}
```

Кожен екземпляр `Person` має властивість `name` типу `String` та опціональну властивість `apartment`, котра спершу дорівнює `nil`. Властивість `apartment` є опціональною, бо особа не завжди має помешкання. 

Аналогічно, кожен екземпляр `Apartment` має властивість `unit` типу `String`, та опціональну властивість `tenant`, що спершу дорівнює `nil`. Властивість `tenant` є опціональним, бо помешкання не завжди має мешканця.

Обидва цих класи мають ініціалізатори, котрі друкують повідомлення для підтвердження факту деініціалізації їх екземлярів. Це дозволяє бачити, чи дійсно екземпляри класів `Person` та `Apartment` деалокуються так, як ми цього очікуємо. 

У наступному фрагмені коду оголошено дві змінні на ім'я `john` та `unit4A`, котрим будуть пізніше присвоєні конкретні екземпляри `Apartment` та `Person`. Обидві змінні автоматично ініціалізуються значенням `nil`, через свою опціональну природу. 

```swift
var john: Person?
var unit4A: Apartment?
```

Тепер можна створити конкретні екземпляри класів `Person` та `Apartment` та присвоїти ці нові екземпляри змінним `john` та `unit4A`:

```swift
john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")
```

Ось як виглядають сильні посилання після створення та присвоєння цих двох екземплярів. Змінна `john` зараз тримає сильне посилання на новий екземпляр `Person`, а змінна `unit4A` – на екземпляр `Apartment`:

![](images/referenceCycle01_2x.png)
￼

Тепер можна поєднати два екземпляри разом, так, щоб у особи було помешкання, а у помешкання - мешканець. Слід звернути увагу на знак оклику (`!`), котрий використовується для доступу до екземплярів, що зберігаються всередині опціональних змінних `john` та `unit4A`, таким чином дозволяючи звертатись до властивостей даних екземплярів:

```swift
john!.apartment = unit4A
unit4A!.tenant = john
```

Ось як виглядаються сильні посилання після поєднання двох екземплярів:

![](images/referenceCycle02_2x.png)

￼Нажаль, поєднання двох екземплярів створює цикл сильних посилань між ними. Екземпляр `Person` тепер має сильне посилання на екземпляр `Apartment`, а він у свою чергу має сильне посилання на екземпляр `Person`. Більше того, якщо прибрати сильні посилання, що тримають змінні `john` та `unit4A`, лічильники посилань не знизяться до нуля, і дані екземпляри не будуть деалоковані ARC:

```swift
john = nil
unit4A = nil
```

Жоден з деініціалізаторів не буде викликано при присвоєнні цим двом змінним значення `nil`. Цикл сильних посилань перешкоджає екземплярам `Person` та `Apartment` бути будь-коли деалокованими, спричиняючи витік пам'яті у даному додатку.

Ось так виклядають сильні посилання після того, як змінним `john` та `unit4A` було присвоєно значення `nil`:

![](images/referenceCycle03_2x.png)
￼

Цикл сильних посилань між екземпляром `Person` та екземпляром `Apartment` житиме, і його ніяк не можна розв'язати.

#### Вирішення циклів сильних посилань між екземплярами класів

У Swift є два способи вирішувати цикли сильних посилань при роботі з властивостями типів-класів: слабкі посилання та безхазяйні посилання. 

Слабкі та безхазяйні посилання дозволяють одному екземпляру в циклі посилатись на інший екземпляр *не утримуючи* його сильно. Екземпляри таким чином можуть посилатись один на одного не утворюючи при цьому циклу сильних посилань. 

Слабкі посилання слід створювати тоді, коли інший екземпляр має коротший час існування; іншими словами, коли інший екземпляр може бути деалоковано раніше. У прикладі з помешканням вище, для помешкання нормально не мати мешканця в якийсь момент його існування, тому слабке посилання є доречним способом розірвати циклічне посилання у даному випадку. Безхазяйні посилання слід застосовувати у протилежних випадках: коли інший екземпляр має такий же або довший час існування.

#### Слабкі посилання

*Слабим посиланням* є посилання, котре не тримається міцно за екземпляр, на котрий воно посилається, і таким  не заважає ARC деалокувати цей екземпляр. Така поведінка не дає посиланню стати частиною циклу сильних посилань. Слабкі посилання позначаються ключовим словом `weak` перед оголошенням змінної чи властивості. 

Оскільки слабкі посилання не трибаються міцно за свій екземпляр, стає можливою деалокація цього екземпляру доки слабке посилання все ще на нього посилається. Тому ARC автоматично присвоює слабкому посиланню значення `nil` під час деалокації екземпляру, на який воно посилається. І тому, оскільки слабкі посилання повинні дозволяти своєму значенню змінитись на  `nil` під час виконання, вони завжди оголошуються як змінні, а не як константи, і завжди мають опціональний тип.

При зверненні до слабкого посилання слід завжди перевіряти, чи існує його значення, так само як і при зверненні будь-якого іншого опціонального значення. Це дозволить запобігти зверненню до екземпляру, котрого вже не існує. 

> **Примітка**
>
> Коли ARC присвоює слабкій властивості значення `nil`, спостерігачі за цією властивістю не викликаються.

Прикладі нижче є майже ідентичним до прикладу про `Person` та `Apartment` вище, однак має одну важливу відмінність. Цього разу, властивість `tenant` класу `Apartment` оголошено слабкою:

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) деініціалізується") }
}
 
class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    weak var tenant: Person?
    deinit { print("Помешкання \(unit) деініціалізується") }
}
```

Сильні посилання з двох змінних (`john` та `unit4A`) та зв'язки між цими двома екземплярами створюються так само, як і раніше:

```swift
var john: Person?
var unit4A: Apartment?
 
john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")
 
john!.apartment = unit4A
unit4A!.tenant = john
```

Ось як тепер виклядаються посилання після зв'язування цих екземплярів разом:

![](images/weakReference01_2x.png)
￼
Клас `Person` все ще має сильне посилання на екземпляр `Apartment`, однак екземпляр `Apartment` тепер має *слабке* посилання на екземпляр `Person`. Це означає, що при розірванні сильного посилання внаслідок присвоєнні змінній `john` значення `nil`, більше не залишиться сильних посилань на екземпляр `Person`:

```swift
john = nil
// Надрукує "John Appleseed деініціалізується"
```

Оскільки більше немає сильних посилань на екземпляр `Person`, його буде деалоковано і властивості `tenant` буде задано значення `nil`:

![](images/weakReference02_2x.png)

￼Тепер єдиним сильним посиланням на екземпляр `Apartment` лишається змінна `unit4A`. Якщо його розірвати, більше не залишиться сильних посилань на екземпляр `Apartment`:

```swift
unit4A = nil
// Надрукує "Помешкання 4A деініціалізується"
```

Оскільки більше не залишилось сильних посилань на екземпляр `Apartment`, його теж буде деалоковано:

![](images/weakReference03_2x.png)
￼
> **Примітка**
>
> У системах з прибиранням сміття, слабкі посиланні іноді використовуються для реалізації простого механізму кешування: там об'єкти, на які немає сильних посилань, деалокуються лише тоді, коли при закінченні пам'яті в системі буде розпочато збірку сміття. Однак, при роботі з ARC, значення деалокуються одразу як тільки вони втрачають останнє сильне посилання, і тому створення слабких посилань не підходить для даної цілі. 

#### Безхазяйні посилання

Як і слабкі посилання, *безхазяйні посилання* не тримаються міцно за екземпляр, на котрий вони посилаються. Але на відміну від слабких посилань, безхазяйні посилання використовуються тоді, коли час життя іншого екземпляру такий же, як і наш, або довший. Безхазяйні посилання позначаються ключовим словом `unowned` перед оголошенням змінної чи властивості. 

Вважається, що безхазяйне посилання завжди має значення. Як результат, ARC ніколи не присвоює безхазяйному посиланню значення `nil`, що значить, що безхазяйні посилання можуть мати неопціональний тип.

> **Важливо**
> 
> Use an unowned reference only when you are sure that the reference *always refers* to an instance that has not been deallocated.
> 
> If you try to access the value of an unowned reference after that instance has been deallocated, you’ll get a runtime error.

The following example defines two classes, `Customer` and `CreditCard`, which model a bank customer and a possible credit card for that customer. These two classes each store an instance of the other class as a property. This relationship has the potential to create a strong reference cycle.

The relationship between `Customer` and `CreditCard` is slightly different from the relationship between `Apartment` and `Person` seen in the weak reference example above. In this data model, a customer may or may not have a credit card, but a credit card will *always* be associated with a customer. A `CreditCard` instance never outlives the `Customer` that it refers to. To represent this, the `Customer` class has an optional `card` property, but the `CreditCard` class has an unowned (and nonoptional) `customer` property.

Furthermore, a new `CreditCard` instance can *only* be created by passing a `number` value and a `customer` instance to a custom `CreditCard` initializer. This ensures that a `CreditCard` instance always has a `customer` instance associated with it when the `CreditCard` instance is created.

Because a credit card will always have a `customer`, you define its customer property as an unowned reference, to avoid a strong reference cycle:

```swift
class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) {
        self.name = name
    }
    deinit { print("\(name) is being deinitialized") }
}
 
class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    init(number: UInt64, customer: Customer) {
        self.number = number
        self.customer = customer
    }
    deinit { print("Card #\(number) is being deinitialized") }
}
```

> **Примітка**
> 
> The `number` property of the `CreditCard` class is defined with a type of `UInt64` rather than `Int`, to ensure that the number property’s capacity is large enough to store a 16-digit card number on both 32-bit and 64-bit systems.

This next code snippet defines an optional `Customer` variable called `john`, which will be used to store a reference to a specific customer. This variable has an initial value of `nil`, by virtue of being optional:

```swift
var john: Customer?
```

You can now create a `Customer` instance, and use it to initialize and assign a new `CreditCard` instance as that customer’s `card` property:

```swift
john = Customer(name: "John Appleseed")
john!.card = CreditCard(number: 1234_5678_9012_3456, customer: john!)
```

Here’s how the references look, now that you’ve linked the two instances:
￼
![](images/unownedReference01_2x.png)

The `Customer` instance now has a strong reference to the `CreditCard` instance, and the `CreditCard` instance has an unowned reference to the `Customer` instance.

Because of the unowned `customer` reference, when you break the strong reference held by the `john` variable, there are no more strong references to the `Customer` instance:

![](images/unownedReference02_2x.png)
￼
Because there are no more strong references to the `Customer` instance, it is deallocated. After this happens, there are no more strong references to the `CreditCard` instance, and it too is deallocated:

```swift
john = nil
// Надрукує "John Appleseed is being deinitialized"
// Надрукує "Card #1234567890123456 is being deinitialized"
```

The final code snippet above shows that the deinitializers for the `Customer` instance and `CreditCard` instance both print their “deinitialized” messages after the `john` variable is set to `nil`.

> **Примітка**
> 
> The examples above show how to use *safe* unowned references. Swift also provides unsafe unowned references for cases where you need to disable runtime safety checks—for example, for performance reasons. As with all unsafe operations, you take on the responsiblity for checking that code for safety.
> 
> You indicate an unsafe unowned reference by writing `unowned(unsafe)`. If you try to access an unsafe unowned reference after the instance that it refers to is deallocated, your program will try to access the memory location where the instance used to be, which is an unsafe operation.

#### Unowned References and Implicitly Unwrapped Optional Properties

The examples for weak and unowned references above cover two of the more common scenarios in which it is necessary to break a strong reference cycle.

The `Person` and `Apartment` example shows a situation where two properties, both of which are allowed to be `nil`, have the potential to cause a strong reference cycle. This scenario is best resolved with a weak reference.

The `Customer` and `CreditCard` example shows a situation where one property that is allowed to be `nil` and another property that cannot be `nil` have the potential to cause a strong reference cycle. This scenario is best resolved with an unowned reference.

However, there is a third scenario, in which *both* properties should always have a value, and neither property should ever be `nil` once initialization is complete. In this scenario, it is useful to combine an unowned property on one class with an implicitly unwrapped optional property on the other class.

This enables both properties to be accessed directly (without optional unwrapping) once initialization is complete, while still avoiding a reference cycle. This section shows you how to set up such a relationship.

The example below defines two classes, `Country` and `City`, each of which stores an instance of the other class as a property. In this data model, every country must always have a capital city, and every city must always belong to a country. To represent this, the `Country` class has a `capitalCity` property, and the `City` class has a `country` property:

```swift
class Country {
    let name: String
    var capitalCity: City!
    init(name: String, capitalName: String) {
        self.name = name
        self.capitalCity = City(name: capitalName, country: self)
    }
}
 
class City {
    let name: String
    unowned let country: Country
    init(name: String, country: Country) {
        self.name = name
        self.country = country
    }
}
```

To set up the interdependency between the two classes, the initializer for `City` takes a `Country` instance, and stores this instance in its `country` property.

The initializer for `City` is called from within the initializer for `Country`. However, the initializer for `Country` cannot pass self to the `City` initializer until a new `Country` instance is fully initialized, as described in [Two-Phase Initialization](13_initialization.md#Two-Phase-Initialization).

To cope with this requirement, you declare the `capitalCity` property of `Country` as an implicitly unwrapped optional property, indicated by the exclamation mark at the end of its type annotation (`City!`). This means that the `capitalCity` property has a default value of `nil`, like any other optional, but can be accessed without the need to unwrap its value as described in [Implicitly Unwrapped Optionals](0_the_basics.md#Implicitly-Unwrapped-Optionals).

Because `capitalCity` has a default `nil` value, a new `Country` instance is considered fully initialized as soon as the `Country` instance sets its name property within its initializer. This means that the `Country` initializer can start to reference and pass around the implicit `self` property as soon as the `name` property is set. The `Country` initializer can therefore pass `self` as one of the parameters for the `City` initializer when the `Country` initializer is setting its own capitalCity property.

All of this means that you can create the `Country` and `City` instances in a single statement, without creating a strong reference cycle, and the `capitalCity` property can be accessed directly, without needing to use an exclamation mark to unwrap its optional value:

```swift
var country = Country(name: "Canada", capitalName: "Ottawa")
print("\(country.name)'s capital city is called \(country.capitalCity.name)")
// Надрукує "Canada's capital city is called Ottawa"
```

In the example above, the use of an implicitly unwrapped optional means that all of the two-phase class initializer requirements are satisfied. The `capitalCity` property can be used and accessed like a nonoptional value once initialization is complete, while still avoiding a strong reference cycle.

### Сильні циклічні посилання в замиканнях
### Strong Reference Cycles for Closures

You saw above how a strong reference cycle can be created when two class instance properties hold a strong reference to each other. You also saw how to use weak and unowned references to break these strong reference cycles.

A strong reference cycle can also occur if you assign a closure to a property of a class instance, and the body of that closure captures the instance. This capture might occur because the closure’s body accesses a property of the instance, such as `self.someProperty`, or because the closure calls a method on the instance, such as `self.someMethod()`. In either case, these accesses cause the closure to “capture” `self`, creating a strong reference cycle.

This strong reference cycle occurs because closures, like classes, are *reference* types. When you assign a closure to a property, you are assigning a *reference* to that closure. In essence, it’s the same problem as above — two strong references are keeping each other alive. However, rather than two class instances, this time it’s a class instance and a closure that are keeping each other alive.

Swift provides an elegant solution to this problem, known as a *closure capture list*. However, before you learn how to break a strong reference cycle with a closure capture list, it is useful to understand how such a cycle can be caused.

The example below shows how you can create a strong reference cycle when using a closure that references `self`. This example defines a class called `HTMLElement`, which provides a simple model for an individual element within an HTML document:

```swift
class HTMLElement {
    
    let name: String
    let text: String?
    
    lazy var asHTML: () -> String = {
        if let text = self.text {
            return "<\(self.name)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }
    
    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }
    
    deinit {
        print("\(name) is being deinitialized")
    }
    
}
```

The `HTMLElement` class defines a name property, which indicates the name of the element, such as `"h1"` for a heading element, `"p"` for a paragraph element, or `"br"` for a line break element. `HTMLElement` also defines an optional `text` property, which you can set to a string that represents the text to be rendered within that HTML element.

In addition to these two simple properties, the `HTMLElement` class defines a lazy property called `asHTML`. This property references a closure that combines `name` and `text` into an HTML string fragment. The `asHTML` property is of type `() -> String`, or “a function that takes no parameters, and returns a `String` value”.

By default, the `asHTML` property is assigned a closure that returns a string representation of an HTML tag. This tag contains the optional `text` value if it exists, or no text content if `text` does not exist. For a paragraph element, the closure would return `"<p>some text</p>"` or `"<p />"`, depending on whether the text property equals `"some text"` or `nil`.

The `asHTML` property is named and used somewhat like an instance method. However, because `asHTML` is a closure property rather than an instance method, you can replace the default value of the `asHTML` property with a custom closure, if you want to change the HTML rendering for a particular HTML element.

For example, the `asHTML` property could be set to a closure that defaults to some text if the `text` property is `nil`, in order to prevent the representation from returning an empty HTML tag:

```swift
let heading = HTMLElement(name: "h1")
let defaultText = "some default text"
heading.asHTML = {
    return "<\(heading.name)>\(heading.text ?? defaultText)</\(heading.name)>"
}
print(heading.asHTML())
// Надрукує "<h1>some default text</h1>"
```

> **Примітка**
> 
> The `asHTML` property is declared as a lazy property, because it is only needed if and when the element actually needs to be rendered as a string value for some HTML output target. The fact that `asHTML` is a lazy property means that you can refer to `self` within the default closure, because the lazy property will not be accessed until after initialization has been completed and `self` is known to exist.

The `HTMLElement` class provides a single initializer, which takes a `name` argument and (if desired) a `text` argument to initialize a new element. The class also defines a deinitializer, which prints a message to show when an `HTMLElement` instance is deallocated.

Here’s how you use the `HTMLElement` class to create and print a new instance:

```swift
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// Надрукує "<p>hello, world</p>"
```

> **Примітка**
> 
> The paragraph variable above is defined as an *optional* `HTMLElement`, so that it can be set to nil below to demonstrate the presence of a strong reference cycle.

Unfortunately, the `HTMLElement` class, as written above, creates a strong reference cycle between an `HTMLElement` instance and the closure used for its default `asHTML` value. Here’s how the cycle looks:

![](images/closureReferenceCycle01_2x.png)
￼
The instance’s `asHTML` property holds a strong reference to its closure. However, because the closure refers to `self` within its body (as a way to reference `self.name` and `self.text`), the closure *captures* `self`, which means that it holds a strong reference back to the `HTMLElement` instance. A strong reference cycle is created between the two. (For more information about capturing values in a closure, see [Capturing Values](6_closures.md#Capturing-Values).)

> **Примітка**
> 
> Even though the closure refers to `self` multiple times, it only captures one strong reference to the `HTMLElement` instance.

If you set the `paragraph` variable to `nil` and break its strong reference to the `HTMLElement` instance, neither the `HTMLElement` instance nor its closure are deallocated, because of the strong reference cycle:

```swift
paragraph = nil
```

Note that the message in the `HTMLElement` deinitializer is not printed, which shows that the `HTMLElement` instance is not deallocated.

### Resolving Strong Reference Cycles for Closures

You resolve a strong reference cycle between a closure and a class instance by defining a *capture list* as part of the closure’s definition. A capture list defines the rules to use when capturing one or more reference types within the closure’s body. As with strong reference cycles between two class instances, you declare each captured reference to be a weak or unowned reference rather than a strong reference. The appropriate choice of weak or unowned depends on the relationships between the different parts of your code.

> **Примітка**
> 
> Swift requires you to write `self.someProperty` or `self.someMethod()` (rather than just `someProperty` or `someMethod()`) whenever you refer to a member of `self` within a closure. This helps you remember that it’s possible to capture `self` by accident.

#### Defining a Capture List

Each item in a capture list is a pairing of the `weak` or `unowned` keyword with a reference to a class instance (such as `self`) or a variable initialized with some value (such as `delegate = self.delegate!`). These pairings are written within a pair of square braces, separated by commas.

Place the capture list before a closure’s parameter list and return type if they are provided:

```swift
lazy var someClosure: (Int, String) -> String = {
    [unowned self, weak delegate = self.delegate!] (index: Int, stringToProcess: String) -> String in
    // closure body goes here
}
```

If a closure does not specify a parameter list or return type because they can be inferred from context, place the capture list at the very start of the closure, followed by the `in` keyword:

```swift
lazy var someClosure: () -> String = {
    [unowned self, weak delegate = self.delegate!] in
    // closure body goes here
}
```

#### Weak and Unowned References

Define a capture in a closure as an unowned reference when the closure and the instance it captures will always refer to each other, and will always be deallocated at the same time.

Conversely, define a capture as a weak reference when the captured reference may become `nil` at some point in the future. Weak references are always of an optional type, and automatically become `nil` when the instance they reference is deallocated. This enables you to check for their existence within the closure’s body.

> **Примітка**
> 
> If the captured reference will never become `nil`, it should always be captured as an unowned reference, rather than a weak reference.

An unowned reference is the appropriate capture method to use to resolve the strong reference cycle in the `HTMLElement` example from earlier. Here’s how you write the `HTMLElement` class to avoid the cycle:

```swift
class HTMLElement {
    
    let name: String
    let text: String?
    
    lazy var asHTML: () -> String = {
        [unowned self] in
        if let text = self.text {
            return "<\(self.name)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }
    
    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }
    
    deinit {
        print("\(name) is being deinitialized")
    }
    
}
```

This implementation of `HTMLElement` is identical to the previous implementation, apart from the addition of a capture list within the `asHTML` closure. In this case, the capture list is `[unowned self]`, which means “capture self as an unowned reference rather than a strong reference”.

You can create and print an `HTMLElement` instance as before:

```swift
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// Надрукує "<p>hello, world</p>"
```

Here’s how the references look with the capture list in place:

![](images/closureReferenceCycle02_2x.png)
￼
This time, the capture of `self` by the closure is an unowned reference, and does not keep a strong hold on the `HTMLElement` instance it has captured. If you set the strong reference from the `paragraph` variable to `nil`, the `HTMLElement` instance is deallocated, as can be seen from the printing of its deinitializer message in the example below:

```swift
paragraph = nil
// Надрукує "p is being deinitialized"
```

For more information about capture lists, see [Capture Lists](04_expressions.md#Capture-Lists).