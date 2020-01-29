## Узагальнення

*Узагальнений код* дозволяє писати гнучкі функції та типи, котрі можна повторно використовувати, що працюють із будь-якими типами відповідно до ваших вимог. Таким способом можна писати код, що уникає повторень, та виражає намір автора у зрозумілій та абстрагованій манері. 

Узагальнення є однією з найбільш потужних можливостей мови Swift, і велика частина стандартної бібліотеки Swift побудована за допомогою узагальненого коду. Фактично, ми користувались узагальненнями всюди у *Керівництві з мови*, навіть якщо ми не усвідомлювали цього. Наприклад, типи `Array` та `Dictionary` у Swift є узагальненими колекціями. Можна створити масив, що зберігає значення типу `Int`, або типу `String`, або будь-якого іншого типу, що можна створити у Swift. Аналогічно, можна створити словник, що зберігає значення будь-якого вказаного типу, і немає жодних обмежень щодо типу цих значень. 

### Проблема, котру вирішують узагальнення

Ось стандартна, неузагальнена функція, що називається `swapTwoInts(_:_:)`, та міняє місцями два значення типу `Int`:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

Ця функція для обміну значень `a` та `b` використовує двонаправлені параметри, що описані у підрозділі [Двонаправлені параметри](5_functions.md#Двонаправлені-параметри).

Функція `swapTwoInts(_:_:)` підставляє початкове значення `b` у змінну `a`, і початкове значення `a` у змінну  `b`. Можна викликати цю функцію для обміну двох значень типу `Int`:

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt тепер дорівнює \(someInt), а anotherInt тепер дорівнює \(anotherInt)")
// Надрукує "someInt тепер дорівнює 107, а anotherInt тепер дорівнює 3"
```

Функція `swapTwoInts(_:_:)` є корисною, але її можна застосувати лише до пари значень типу `Int`. Якщо потрібно обміняти значеннями дві змінні типу `String` чи `Double`, потрібно написати ще функцій, наприклад `swapTwoStrings(_:_:)` та `swapTwoDoubles(_:_:)`:

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

Слід помітити, що тіла функцій `swapTwoInts(_:_:)`, `swapTwoStrings(_:_:)`, та `swapTwoDoubles(_:_:)` повністю співпадаються. Єдиною відмінністю є тип значень, що приймають ці функції (`Int`, `String`, та `Double`).

Було б ефективніше та набагато більш гнучко написати одну функцію, що обмінює значеннями два значення будь-якого типу. Узагальнений код дозволяє написати таку функцію. (Узагальнена версія цих трьох функцій оголошується далі).

> **Примітка**
>
> У всіх цих трьох функціях, тип у змінних `a` та `b` має бути однаковим. Якщо тип у `a` та `b` не співпадає, неможливо обміняти їх значеннями. Swift є типобезпечною мовою, і тому не дозволяє (наприклад) обміняти значеннями змінні типу `String` та `Double`. Спроба зробити це призводить до помилки часу компіляції.

### Узагальнені функції

Узагальнені функції можуть працювати з будь-яким типом. Ось приклад узагальненої версії функції `swapTwoInts(_:_:)` з прикладу вище, котра називається `swapTwoValues(_:_:)`:

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

Тіло функції `swapTwoValues(_:_:)` є ідентичним до тіла функції `swapTwoInts(_:_:)`. Однак, перший рядок функції `swapTwoValues(_:_:)` трохи відрізняється від `swapTwoInts(_:_:)`. Ось перші два рядки в порівнянні:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int)
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```

Узагальнена функція використовує замісник назви типу (що в даному випадку називається `T`) замість фактичної назви типу (на кшталт `Int`, `String`, чи `Double`). Замісник назви типу нічого не каже про то, чим може бути  `T`, але він каже, що `a` та `b` повинні мати однаковий тип `T`, яким би він не був. Фактичний тип для використання замість `T` встановлюється в момент виклику функції `swapTwoValues(_:_:)`.

Різниця між узагальненою та неузагальненою функцією полягає також у тому, що після назви узагальненої функції (`swapTwoValues(_:_:)`) йде замісник назви типу (`T`) у фігурних дужках (`<T>`). Фігурні дужки підказують  компілятору Swift, що `T` – це замісник назви типу у визначенні функції `swapTwoValues(_:_:)`. Оскільки `T` – це замісник назви типу, компілятор Swift не шукатиме фактичний тип, що називається `T`.

Функція `swapTwoValues(_:_:)` може тепер викликатись так само, як і `swapTwoInts(_:_:)`, але їй можна передавати пару значень будь-якого типу, головне, щоб це були значення одного й того ж типу. При кожному виклику `swapTwoValues(_:_:)`, тип, який слід використовувати замість `T`, визначається з типів значень, що передаються до функції.

У двох прикладах нижче, `T` визначається як `Int`  та  `String` відповідно:

```swift
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
// someInt тепер дорівнює 107, а anotherInt тепер дорівнює 3

var someString = "hello"
var anotherString = "world"
swapTwoValues(&someString, &anotherString)
// someString тепер дорівнює "world", а anotherString тепер дорівнює "hello"
```

> **Примітка**
>
> Визначена вище функція `swapTwoValues(_:_:)` є аналогом узагальненої функції `swap(::)`, що є частиною стандартної бібліотеки Swift, та є автоматично доступною для використання у ваших додатках. Якщо вам потрібна поведінка функції `swapTwoValues(_:_:)` у вашому коді, слід використовувати існуючу функцію `swap(::)` замість того, щоб реалізовувати її самотужки.

### Параметри типів

У прикладі з `swapTwoValues(_:_:)`, замісник назви типу `T` є прикладом параметра типу. Параметри типів визначають та іменують замісник типу, і записуються одназу після назви функції, всередині пари кутових дужок (наприклад, `<T>`).

Як тільки вказано параметр типу, його можна використовувати для визначення типу параметрів функції (наприклад, типи параметрів `a` та `b` функції `swapTwoValues(_:_:)`), або типу, що повертається функцією, або як анотацію типу всередині тіла функції. В кожному випадку, тип параметра замінюється фактичним типом при кожному виклику функції. (У прикладі з `swapTwoValues(_:_:)` вище, `T` замінювався на `Int` у першому виклику функції, та на `String` у другому виклику функції).

Функція може мати декілька параметрів типів, їх так само записують у кутових дужках, розділюючи комами. 

#### Іменування параметрів типів

У більшості випадків, параметри типів мають змісновті назви, як наприклад `Key` (ключ) та `Value` (значення) у `Dictionary<Key, Value>`, котрі вказують читачу на зв'язкок між параметром типу та узагальненим типом або функцією, в якому він використовується. Однак, у випадках, коли між ними нема змістовного зв'язку, традиційно типи параметрів однією великою літерою, зазвичай `T`, `U`, чи `V`, як `T` у функції `swapTwoValues(_:_:)` вище.

> **Примітка**
>
> Слід завжди давати параметрам типів назви, що пишуться [ВерхнімВерблюжимРегістром](https://uk.wikipedia.org/wiki/Верблюжий_регістр) (наприклад, `T` або `MyTypeParameter`), щоб позначити, що це замісник типу, а не значення.

### Узагальнені типи

Окрім узагальнених функцій, Swift дає можливість створювати власні *узагальнені типи*. Ними можуть бути власні класи, структури та перечислення, що можуть працювати з будь-яким типом, аналогічно до вбудованих `Array` та `Dictionary`.

Цей підрозділ покаже, як написати власну узагальнену колекцію на ім'я `Stack`. Стек - це впорядкована множина значень, аналогічна до масиву, але із більш обмеженим набором операцій, ніж у масивів у Swift. Масиви у Swift дозволяють вставляти чи видаляти елементи в будь-якій позиції. Стек дозволяють новим елементам додаватись лише в кінець колекції (про що кажуть: "заштовхнути елемент у стек", або push). Стек також дозволяє елементам видалятись, але лише з кінця колекції (про що кажуть: "виштовхнути елемент зі стека", або pop).

> **Примітка**
>
> Концепція стеку використовується класом `UINavigationController` для моделювання в'ю-контоллерів у ієрархії навігації в додатках на iOS. Щоб додати в'ю-контроллер до навігаційного стеку, слід викликати метод `pushViewController(_:animated:)` класу `UINavigationController`, а щоб прибрати (тобто виштовхнути) в'ю-контроллер з навігаційного стеку – метод `popViewControllerAnimated(_:)`. Стек є корисною моделлю колекції в тих випадках, де для керування колекцією потрібне строге правило “останнім прийшов — першим пішов” (або LIFO, від англійсього “last in, first out”).

Ілюстрація нижче демонструє поведінку стека і його операції push та pop:

![](images/stackPushPop_2x.png)

1. У стеку зберігаються три значення.
2. До стеку заштовхується четверте значення.
3. Стек тепер тримає чотири значення, при цьому останнє лежить на його вершині.
4. Зі стеку виштовхується значення.
5. Після виштовхування значення, стек знову містить три початкові значення.

Ось як можна записати неузагальнену версію стеку, в даному випадку для стеку значень типу `Int`:

```swift
struct IntStack {
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
}
```

У даній струкрурі для зберігання значень у стеку використовується масив значень типу `Int`, що зберігається у властивості на ім'я `items`. Цей стек має два методи, `push()` та  `pop()`, що відповідно заштовхують та виштовхують елементи стеку. Ці методи позначені модифікатором `mutating`, оскільки їм потрібно змінити масив, що зберігається у даній структурі.

Однак тип `IntStack` може використовуватись лише для зберігання значень типу `Int`. Було б набагато більш ефективно створити узагальнений клас `Stack`, котрий би керував стеком значень будь-якого типу. 

Ось узагальнена версію того ж коду:

```swift
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```

Слід помітити, що узагальнена версія `Stack` є практично такою ж, як і неузагальнена версія, єдиною відмінністю є параметр типу на ім'я `Element` замість фактичного типу `Int`. Параметр типу записується всередині пари кутових дужок (`<Element>`) одразу після назви структури.

`Element` визначає замісник назви типу, що буде надана пізніше. До цього майбутнього типу далі можна звертатись як `Element` будь-де у визначенні структури. В даному випадку, `Element` використовується як замісник у трьох місцях:

- Для створення властивості `items`, котра ініціалізується порожнім масивом значень типу `Element`.
- Щоб вказати, що метод `push(_:)` має єдиний параметр на ім'я `item`, котрий повинен мати тип `Element`.
- Щоб вказати, що метод `pop()` повертає значення типу `Element`.

Оскільки `Stack` є узагальненим типом, його можна використовувати для створення стеку будь-якого типу Swift, аналогічно до `Array` та `Dictionary`.

Щоб створити новий екземпляр `Stack`, слід записати тип значень, що зберігатиметься у стеку, у фігурних дужках. Наприклад, щоб створити стек рядків, слід написати `Stack<String>()`:

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
stackOfStrings.push("cuatro")
// стек містить 4 рядки
```

Ось як виглядає стек `stackOfStrings` після заштовхування в нього чотирьох значень:

![](images/stackPushedFourStrings_2x.png)

Виштовхування значень зі стеку видаляє та повертає значення на вершині стеку, `"cuatro"`:

```swift
let fromTheTop = stackOfStrings.pop()
// fromTheTop тепер дорівнює "cuatro", а стек тепер містить 3 рядки
```

Ось як виглядає стек після виштовхування верхнього значення:

![](images/stackPoppedOneString_2x.png)

### Розширення узагальнених типів

При розширенні узагальненого типу не потрібно вказувати список параметрів типів у оголошенні розширення. При цьому список параметрів типів з оригінального оголошення типу є доступним для тіла розширення, і оригінальні назви параметрів типів можна використовувати для посилання на параметри в оригінальному оголошенні. 

Наступний приклад розширює узагальнений тип `Stack`, додаючи до нього властивість тільки для читання, що обчислюється і називається `topItem`, котра повертає елемент на вершині стеку, без виштовхування цього елементу зі стеку:

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```

Властивість `topItem` повертає опціональне значення типу `Element`. Якщо стер порожній, `topItem` поверне `nil`; якщо стек не порожній, `topItem` поверне останній елемент у масиві `items`.

Варто помітити, що розширення не оголошує список параметрів типів. При цьому назва існуючого параметра типу `Element` використовується всередині розширення для позначення опціонального типу властивості `topItem`.

Властивість, що обчислюється, `topItem` тепер може бути використаною з будь-яким екземпляром `Stack` для отримання елемента на вершині стеку без його видалення:

```swift
if let topItem = stackOfStrings.topItem {
    print("Елементом на вершині стеку є '\(topItem)'.")
}
// Надрукує "Елементом на вершині стеку є 'tres'."
```

Розширення узагальненого типу можуть також включати вимоги, котрі мають задовольняти типи, що отримають нову функціональність. Це детальніше описано далі, в підрозділі [Розшинення з інструкцією узагальнення Where](#Розшинення-з-інструкцією-узагальнення-Where).

### Обмеження типів

Функція  `swapTwoValues(_:_:)` та тип `Stack` можуть працювати із будь-яким типом. Однак, часто буває потрібно накласти певні обмеження на типи, які можуть використовуватись в узагальнених функціях та типах. Обмеження типів вказують, що параметр типу повинен наслідуватись від певного класу, або підпорядковуватись певному протоколу, чи композиції протоколів.

Наприклад, тип `Dictionary` у Swift вводить обмеження на типи, що можуть використовуватись в якості ключів у словнику. Як описано у підрозділі [Словники](3_collection_types.md#Словники), тип ключа у словнику має підпорядковуватись протоколу `Hashable`, тобто вміти обчислити свій хеш. Іншими словами, ключ повинен надавати своє однозначне представлення. Словнику потрібно знаходити хеш ключа для того, щоб перевіряти, чи містить він для цього ключа значення. Без цього обмеження, словник не мав би змоги з'ясувати, потрібно вставляти нове значення чи заміняти існуючи, а також не мав би змоги з'ясувати, чи міститься у ньому значення за заданим ключем. 

Дана вимога ставиться за допомогою обмеження типу ключа для `Dictionary`, котра визначає, що ключ має підпорядковуватись протоколу `Hashable`, спеціальному протоколу зі стандартної бібліотеки Swift. Всі базові типи Swift (такі, як `String`, `Int`, `Double`, та `Bool`) є за замовчанням підпорядкованими цьому протоколу. 

Створюючи власні узагальнені типи, можна ставити власні обмеження типів, і ці обмеження містять у собі велику силу узагальненого програмування. Абстрактні концепції на кшталт `Hashable` характеризуть типи з точки зору їх концептуальних характеристик, допомагаючи уникати використання конкретних типів. 

#### Синтаксис обмежень типів

Обмеження типів записуються шляхом вказування назви класу чи протоколу у списку параметрів типів, після назви параметру типу, відділяючись від нього комою. Базовий синтаксис обмеження типів узагальненої функції показано нижче (синтаксис для узагальнених функцій та типів однаковий):

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // тут йде тіло функції
}
```

Гіпотетична функція вище має два параметри типів. Перший параметр типу, `T`, має обмеження типу, що вимагає `T` бути класом-нащадком класу  `SomeClass`. Другий параметр, `U`, має обмеження типу, що вимагає `U` бути підпорядкованим протоколу `SomeProtocol`.

#### Обмеження типів у дії

Ось приклад неузагальненої функції на ім'я `findIndex(ofString:in:)`, котра шукає зананий рядок у заданому масиві рядків. Функція `findIndex(ofString:in:)` повертає опціональне цілочисельне значення, котре виражає індекс першого рядка в масиві, що співпав із заданим, або `nil`, якщо не співпав жоден:

```swift
func findIndex(ofString valueToFind: String, in array: [String]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

Функцію `findIndex(ofString:in:)` можна використовувати для знаходження рядку у масиві рядків:

```swift
let strings = ["кіт", "пес", "лама", "папуга", "черепаха"]
if let foundIndex = findIndex(ofString: "лама", in: strings) {
    print("Рядок 'лама' має індекс \(foundIndex)")
}
// Надрукує "Рядок 'лама' має індекс 2"
```

Однак, принцип знаходження індексу значення в масиві може стати в нагоді не лише для рядків. Можна написати аналогічну функціональність у вигляді узагальненої функції, замінюючи згадки типу `String` на параметр типу `T`.

Ось як можла б виглядати узагальнена версія функції `findIndex(ofString:in:)`, що носила б назву `findIndex(of:in:)`. Слід помітити, що тип, що повертається цією функцією, все ще є `Int?`, оскільки функція повертає опціональний індекс у масиві, а не опціональне значення з масиву. Однак, маємо попередити: дана функція не скомпілюється через причини, які ми пояснимо далі після цього прикладу:

```swift
func findIndex<T>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

Функція вище не скомпілюється. Проблема лежить в перевірці на рівність,  “`if value == valueToFind`”. Не кожен тип у Swift можна порівнювати за допомогою оператору рівності (`==`). Якщо, наприклад, створити власний клас чи структуру, що представляє якусь модель даних, тоді значення операції “дорівнює” для цього класу чи структури не є очевидним для Swift, і компілятор не може його вгадати. Через це, неможливо гарантувати, що даний код буде працювати для будь-якого можливого типу `T`, і компілятор вкаже на це у повідомленні про помилку. 

Однак, не все втрачено. У стандартній бібліотеці Swift є протокол, що носить назву `Equatable`, котрий вимагає в підпорядкованого типу реалізовувати оператори рівності (`==`) та нерівності (`!=`) для порівняння двох значень даного типу. Всі стандартні типи Swift автоматично підпорядковані протоколу `Equatable`.

Будь-який тип, підпорядкований протоколу `Equatable`, можна безпечно використовувати у функції `findIndex(of:in:)`, оскільки він гарантовано підтриимує оператор рівності. Для вираження цього факту, вимога підпорядковуватись протоколу `Equatable` записується як обмеження типу `T` у визначенні функції:

```swift
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

Єдиний параметр типу функції `findIndex(of:in:)` записуєтсья як `T: Equatable`, що означає “будь-який `T`, що підпорядковується протоколу `Equatable`.

Функція `findIndex(of:in:)` тепер успішно компілюється та може використовуватись із будь-яким підпорядкованим протоколу `Equatable` типом, наприклад, `Double` чи `String`:

```swift
let doubleIndex = findIndex(of: 9.3, in: [3.14159, 0.1, 0.25])
// doubleIndex є опціональним Int без значення, бо значення 9.3 відсутнє в масиві
let stringIndex = findIndex(of: "Антоніна", in: ["Михайло", "Максим", "Антоніна"])
// stringIndex є опціональним Int зі значенням 2
```

### Associated Types

When defining a protocol, it’s sometimes useful to declare one or more associated types as part of the protocol’s definition. An associated type gives a placeholder name to a type that is used as part of the protocol. The actual type to use for that associated type isn’t specified until the protocol is adopted. Associated types are specified with the associatedtype keyword.

#### Associated Types in Action

Here’s an example of a protocol called Container, which declares an associated type called Item:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

The Container protocol defines three required capabilities that any container must provide:

- It must be possible to add a new item to the container with an append(_:) method.

- It must be possible to access a count of the items in the container through a count property that returns an Int value.

- It must be possible to retrieve each item in the container with a subscript that takes an Int index value.

This protocol doesn’t specify how the items in the container should be stored or what type they’re allowed to be. The protocol only specifies the three bits of functionality that any type must provide in order to be considered a Container. A conforming type can provide additional functionality, as long as it satisfies these three requirements.

Any type that conforms to the Container protocol must be able to specify the type of values it stores. Specifically, it must ensure that only items of the right type are added to the container, and it must be clear about the type of the items returned by its subscript.

To define these requirements, the Container protocol needs a way to refer to the type of the elements that a container will hold, without knowing what that type is for a specific container. The Container protocol needs to specify that any value passed to the append(_:) method must have the same type as the container’s element type, and that the value returned by the container’s subscript will be of the same type as the container’s element type.

To achieve this, the Container protocol declares an associated type called Item, written as `associatedtype Item`. The protocol doesn’t define what Item is — that information is left for any conforming type to provide. Nonetheless, the Item alias provides a way to refer to the type of the items in a Container, and to define a type for use with the append(_:) method and subscript, to ensure that the expected behavior of any Container is enforced.

Here’s a version of the nongeneric IntStack type from [Generic Types](#Generic-Types) above, adapted to conform to the Container protocol:

```swift
struct IntStack: Container {
    // original IntStack implementation
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    // conformance to the Container protocol
    typealias Item = Int
    mutating func append(_ item: Int) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
}
```

The IntStack type implements all three of the Container protocol’s requirements, and in each case wraps part of the IntStack type’s existing functionality to satisfy these requirements.

Moreover, IntStack specifies that for this implementation of Container, the appropriate Item to use is a type of Int. The definition of typealias Item = Int turns the abstract type of Item into a concrete type of Int for this implementation of the Container protocol.

Thanks to Swift’s type inference, you don’t actually need to declare a concrete Item of Int as part of the definition of IntStack. Because IntStack conforms to all of the requirements of the Container protocol, Swift can infer the appropriate Item to use, simply by looking at the type of the append(_:) method’s item parameter and the return type of the subscript. Indeed, if you delete the typealias Item = Int line from the code above, everything still works, because it’s clear what type should be used for Item.

You can also make the generic Stack type conform to the Container protocol:

```swift
struct Stack<Element>: Container {
    // original Stack<Element> implementation
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
    // conformance to the Container protocol
    mutating func append(_ item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}
```

«This time, the type parameter Element is used as the type of the append(_:) method’s item parameter and the return type of the subscript. Swift can therefore infer that Element is the appropriate type to use as the Item for this particular container.

#### Extending an Existing Type to Specify an Associated Type

You can extend an existing type to add conformance to a protocol, as described in [Adding Protocol Conformance with an Extension](). This includes a protocol with an associated type.

Swift’s Array type already provides an append(_:) method, a count property, and a subscript with an Int index to retrieve its elements. These three capabilities match the requirements of the Container protocol. This means that you can extend Array to conform to the Container protocol simply by declaring that Array adopts the protocol. You do this with an empty extension, as described in [Declaring Protocol Adoption with an Extension]():

```swift
extension Array: Container {}
```

Array’s existing append(_:) method and subscript enable Swift to infer the appropriate type to use for Item, just as for the generic Stack type above. After defining this extension, you can use any Array as a Container.

#### Adding Constraints to an Associated Type

You can add type constraints to an associated type in a protocol to require that conforming types satisfy those constraints. For example, the following code defines a version of Container that requires the items in the container to be equatable.

```swift
protocol Container {
    associatedtype Item: Equatable
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

To conform to this version of Container, the container’s Item type has to conform to the Equatable protocol.

#### Using a Protocol in Its Associated Type’s Constraints

A protocol can appear as part of its own requirements. For example, here’s a protocol that refines the Container protocol, adding the requirement of a suffix(_:) method. The suffix(_:) method returns a given number of elements from the end of the container, storing them in an instance of the Suffix type.

```swift
protocol SuffixableContainer: Container {
    associatedtype Suffix: SuffixableContainer where Suffix.Item == Item
    func suffix(_ size: Int) -> Suffix
}
```

In this protocol, Suffix is an associated type, like the Item type in the `Container` example above. Suffix has two constraints: It must conform to the `SuffixableContainer` protocol (the protocol currently being defined), and its Item type must be the same as the container’s Item type. The constraint on Item is a generic where clause, which is discussed in [Associated Types with a Generic Where Clause]() below.

Here’s an extension of the Stack type from Generic Types above that adds conformance to the `SuffixableContainer` protocol:

```swift
extension Stack: SuffixableContainer {
    func suffix(_ size: Int) -> Stack {
        var result = Stack()
        for index in (count-size)..<count {
            result.append(self[index])
        }
        return result
    }
    // Inferred that Suffix is Stack.
}
var stackOfInts = Stack<Int>()
stackOfInts.append(10)
stackOfInts.append(20)
stackOfInts.append(30)
let suffix = stackOfInts.suffix(2)
// suffix contains 20 and 30
```

In the example above, the Suffix associated type for Stack is also Stack, so the suffix operation on Stack returns another Stack. Alternatively, a type that conforms to SuffixableContainer can have a Suffix type that’s different from itself—meaning the suffix operation can return a different type. For example, here’s an extension to the nongeneric IntStack type that adds SuffixableContainer conformance, using Stack<Int> as its suffix type instead of IntStack:

```swift
extension IntStack: SuffixableContainer {
    func suffix(_ size: Int) -> Stack<Int> {
        var result = Stack<Int>()
        for index in (count-size)..<count {
            result.append(self[index])
        }
        return result
    }
    // Inferred that Suffix is Stack<Int>.
}
```

### Generic Where Clauses

### Інструкція узагальнення Where

«Type constraints, as described in [Type Constraints](), enable you to define requirements on the type parameters associated with a generic function, subscript, or type.

It can also be useful to define requirements for associated types. You do this by defining a generic where clause. A generic where clause enables you to require that an associated type must conform to a certain protocol, or that certain type parameters and associated types must be the same. A generic where clause starts with the where keyword, followed by constraints for associated types or equality relationships between types and associated types. You write a generic where clause right before the opening curly brace of a type or function’s body.

The example below defines a generic function called allItemsMatch, which checks to see if two Container instances contain the same items in the same order. The function returns a Boolean value of true if all items match and a value of false if they don’t.

The two containers to be checked don’t have to be the same type of container (although they can be), but they do have to hold the same type of items. This requirement is expressed through a combination of type constraints and a generic where clause:

```swift
func allItemsMatch<C1: Container, C2: Container>
    (_ someContainer: C1, _ anotherContainer: C2) -> Bool
    where C1.Item == C2.Item, C1.Item: Equatable {

        // Check that both containers contain the same number of items.
        if someContainer.count != anotherContainer.count {
            return false
        }
    
        // Check each pair of items to see if they're equivalent.
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
             }
        }

        // All items match, so return true.
        return true
}
```

This function takes two arguments called someContainer and anotherContainer. The someContainer argument is of type C1, and the anotherContainer argument is of type C2. Both C1 and C2 are type parameters for two container types to be determined when the function is called.

The following requirements are placed on the function’s two type parameters:

- C1 must conform to the Container protocol (written as C1: Container).

- C2 must also conform to the Container protocol (written as C2: Container).

- The Item for C1 must be the same as the Item for C2 (written as C1.Item == C2.Item).

- The Item for C1 must conform to the Equatable protocol (written as C1.Item: Equatable).


The first and second requirements are defined in the function’s type parameter list, and the third and fourth requirements are defined in the function’s generic where clause.

These requirements mean:

- someContainer is a container of type C1.

- anotherContainer is a container of type C2.

- someContainer and anotherContainer contain the same type of items.

- The items in someContainer can be checked with the not equal operator (!=) to see if they’re different from each other.


The third and fourth requirements combine to mean that the items in anotherContainer can *also* be checked with the != operator, because they’re exactly the same type as the items in someContainer.

These requirements enable the `allItemsMatch(_:_:)` function to compare the two containers, even if they’re of a different container type.

The `allItemsMatch(_:_:)` function starts by checking that both containers contain the same number of items. If they contain a different number of items, there’s no way that they can match, and the function returns false.

After making this check, the function iterates over all of the items in someContainer with a for-in loop and the half-open range operator (..<). For each item, the function checks whether the item from someContainer isn’t equal to the corresponding item in anotherContainer. If the two items aren’t equal, then the two containers don’t match, and the function returns false.

If the loop finishes without finding a mismatch, the two containers match, and the function returns true.

Here’s how the allItemsMatch(_:_:) function looks in action:

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")

var arrayOfStrings = ["uno", "dos", "tres"]

if allItemsMatch(stackOfStrings, arrayOfStrings) {
    print("All items match.")
} else {
    print("Not all items match.")
}
// Надрукує "All items match."
```

The example above creates a Stack instance to store String values, and pushes three strings onto the stack. The example also creates an Array instance initialized with an array literal containing the same three strings as the stack. Even though the stack and the array are of a different type, they both conform to the Container protocol, and both contain the same type of values. You can therefore call the allItemsMatch(_:_:) function with these two containers as its arguments. In the example above, the allItemsMatch(_:_:) function correctly reports that all of the items in the two containers match.

#### Extensions with a Generic Where Clause

#### Розшинення з інструкцією узагальнення Where

You can also use a generic `where` clause as part of an extension. The example below extends the generic Stack structure from the previous examples to add an isTop(_:) method.

```swift
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item
    }
}
```

This new `isTop(_:)` method first checks that the stack isn’t empty, and then compares the given item against the stack’s topmost item. If you tried to do this without a generic where clause, you would have a problem: The implementation of isTop(_:) uses the == operator, but the definition of Stack doesn’t require its items to be equatable, so using the == operator results in a compile-time error. Using a generic where clause lets you add a new requirement to the extension, so that the extension adds the isTop(_:) method only when the items in the stack are equatable.

Here’s how the isTop(_:) method looks in action:

```swift
if stackOfStrings.isTop("tres") {
    print("Top element is tres.")
} else {
    print("Top element is something else.")
}
// Надрукує "Top element is tres.
```

If you try to call the isTop(_:) method on a stack whose elements aren’t equatable, you’ll get a compile-time error.

```swift
struct NotEquatable { }
var notEquatableStack = Stack<NotEquatable>()
let notEquatableValue = NotEquatable()
notEquatableStack.push(notEquatableValue)
notEquatableStack.isTop(notEquatableValue)  // Error
```

«You can use a generic where clause with extensions to a protocol. The example below extends the Container protocol from the previous examples to add a startsWith(_:) method.

```swift
extension Container where Item: Equatable {
    func startsWith(_ item: Item) -> Bool {
        return count >= 1 && self[0] == item
    }
}
```

The startsWith(_:) method first makes sure that the container has at least one item, and then it checks whether the first item in the container matches the given item. This new startsWith(_:) method can be used with any type that conforms to the Container protocol, including the stacks and arrays used above, as long as the container’s items are equatable.

```swift
if [9, 9, 9].startsWith(42) {
    print("Starts with 42.")
} else {
    print("Starts with something else.")
}
// Надрукує "Starts with something else."
```

The generic where clause in the example above requires Item to conform to a protocol, but you can also write a generic where clauses that require Item to be a specific type. For example:

```swift
extension Container where Item == Double {
    func average() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += self[index]
        }
        return sum / Double(count)
    }
}
print([1260.0, 1200.0, 98.6, 37.0].average())
// Надрукує "648.9"
```

This example adds an average() method to containers whose Item type is Double. It iterates over the items in the container to add them up, and divides by the container’s count to compute the average. It explicitly converts the count from Int to Double to be able to do floating-point division.

You can include multiple requirements in a generic where clause that is part of an extension, just like you can for a generic where clause that you write elsewhere. Separate each requirement in the list with a comma.

### Associated Types with a Generic Where Clause

You can include a generic where clause on an associated type. For example, suppose you want to make a version of Container that includes an iterator, like what the Sequence protocol uses in the standard library. Here’s how you write that:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }

    associatedtype Iterator: IteratorProtocol where Iterator.Element == Item
    func makeIterator() -> Iterator
}
```

The generic where clause on Iterator requires that the iterator must traverse over elements of the same item type as the container’s items, regardless of the iterator’s type. The `makeIterator()` function provides access to a container’s iterator.

For a protocol that inherits from another protocol, you add a constraint to an inherited associated type by including the generic where clause in the protocol declaration. For example, the following code declares a ComparableContainer protocol that requires Item to conform to Comparable:

```swift
protocol ComparableContainer: Container where Item: Comparable { }
```

### Generic Subscripts

Subscripts can be generic, and they can include generic where clauses. You write the placeholder type name inside angle brackets after subscript, and you write a generic where clause right before the opening curly brace of the subscript’s body. For example:

```swift
extension Container {
    subscript<Indices: Sequence>(indices: Indices) -> [Item]
        where Indices.Iterator.Element == Int {
            var result = [Item]()
            for index in indices {
                result.append(self[index])
            }
            return result
    }
}
```

This extension to the Container protocol adds a subscript that takes a sequence of indices and returns an array containing the items at each given index. This generic subscript is constrained as follows:

- The generic parameter Indices in angle brackets has to be a type that conforms to the Sequence protocol from the standard library.

- The subscript takes a single parameter, indices, which is an instance of that Indices type.
- The generic where clause requires that the iterator for the sequence must traverse over elements of type Int. This ensures that the indices in the sequence are the same type as the indices used for a container.

Taken together, these constraints mean that the value passed for the indices parameter is a sequence of integers.

