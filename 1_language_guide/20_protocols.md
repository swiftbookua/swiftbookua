## Протоколи

*Протокол* визначає список методів, властивостей, та інших вимог, котрі підходять для певної задачі чи частини функціональності. Класи, структури та перечислення можуть підпорядковуватись протоколам, надаючи реалізації до цих вимог. Якщо тип виконує всі вимоги певного протоколу, про нього кажуть, що тип *підпорядковується* протоколу. 

Окрім визначення вимог, яким повинні відповідати підпорядковані типи, протоколи можуть бути розширеними для реалізації цих вимог, або для реалізації додаткової функціональності, котрою можуть скористатись підпорядковані типи. 

### Синтаксис протоколів

Протоколи оголошуються за допомогою синтаксису, дуже схожого на синтаксис класів, структур та перечислень:

```swift
protocol SomeProtocol {
    // тут йде оголошення протоколу
}
```

Типи можуть зазначати, що вони є підпорядкованими до певного протоколу, за допомогою назви протоколу, що йде після назви типу та двокрапки в місці оголошенні типу. Тип може бути підпорядкованим одразу кільком протоколам: в такому випадку їх слід розділяти комами:

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // тут йде оголошення структури
}
```

Якщо клас має батьківський клас, то назва батьківського класу має йти перед усіма протоколами, до яких підпорядкований тип, відділяючись від них комою:

```swift
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // тут йде оголошення класу
}
```

### Вимоги властивостей

Протокол може вимагати, щоб підпорядкований тип мав властивість екземпляру або властивість типу з певною назвою та типом. Протокол не вказує, чи повинна бути ця властивість зберігатись чи обчислюватись – він лише вказує назву та тип. Протокол також вказує, чи повинна це бути властивість тільки для читання, чи для читання й запису.

Якщо протокол вимагає, щоб властивість була для читання й запису, цю вимогу не можна задовольнити константною властивістю чи властивістю лише для читання. Якщо протокол вимагає властивість лише для читання, цю вимогу можна задовольнити будь-якою доречною для вашого коду властивістю, як тільки для читання, так і для читання й запису, і навіть константною. 

Вимоги властивостей завжди оголошуються так само, як і самі властивості, за допомогою ключового слова `var`. Вимоги властивості для читання й запису при цьому помічаються конструкцією `{ get set }` після їх оголошення, вимоги властивості тільки для читання – конструкцією `{ get }`.

```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```

Вимоги властивостей типу завжди помічаються в протоколі ключовим словом `static`. При цьому, якщо протоколу підпорядковується клас, такі вимоги задовольняються як властивостями типу (`static`), та і властивостями класу (`class`).

```swift
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}
```

Ось приклад протоколу з єдиною вимогою щодо властивості екземпляру:

```swift
protocol FullyNamed {
    var fullName: String { get }
}
```

Протокол `FullyNamed` вимагає, щоб підпорядкований тип мав повне ім'я. Цей протокол не ставить будь-яких вимог щодо природи підпорядкованого типу – він вимагає лише того, щоб тип міг надати інформацію про повне ім'я. Протокол зазначає, що будь-який піпорядкований `FullyNamed` тип мав властивість екземпляру для читання на ім'я `fullName`, котра має тип `String`.

Ось приклад простої структури, що підпорядковується до протоколу `FullyNamed` та реалізовує його:

```swift
struct Person: FullyNamed {
    var fullName: String
}
let stepan = Person(fullName: "Степан Яблучко")
// stepan.fullName дорівнює "Степан Яблучко"
```

У даному прикладі визначено структуру на ім'я `Person`, котра представляє конкретну особу, що має повне ім'я. Ця структура оголошує себе підпорядкованою протоколу `FullyNamed` в першому рядку свого оголошення.

Кожен екземпляр структури `Person` має єдину властивість та ім'я `fullName`, типу `String`. Це задовольняє єдину вимогу протоколу `FullyNamed`, та означає, що структура `Person` коректно підпорядкувалась до цього протоколу. (У випадках, коли тип не відповідає вимогам протоколу, Swift повідомляє про помилку компіляції).

Ось приклад складнішого класу, що також підпорядкований протоколу `FullyNamed` та реалізовує його вимоги:

```swift
class Starship: FullyNamed {
    var prefix: String?
    var name: String
    init(name: String, prefix: String? = nil) {
        self.name = name
        self.prefix = prefix
    }
    var fullName: String {
        return (prefix != nil ? prefix! + " " : "") + name
    }
}
var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
// ncc1701.fullName дорівнює "USS Enterprise"
```

Цей клас реалізовує вимогу властивості `fullName` за допомогою властивості тільки для читання, що обчислюється. Кожен екземпляр `Starship` зберігає обов'язкове значення `name` та опціональне значення `prefix`. Властивість  `fullName` використовує значення `prefix`, якщо воно присутнє, та додає його до `name` зліва, щоб створити повне ім'я космічного корабля.

### Вимоги методів

Протоколи можуть вимагати, щоб підпорядковані типи реалізовували певні методи екземплярів чи методи типів. Ці вимоги записуються в оголошенні протоколу точно так само, як і самі методи, тільки без фігурних дужок та тіла методу. Дозволяються варіативні параметри, котрі регулюються тими ж правилами, що й звичайні методи. Однак, нажаль у визначені протоколу не можна вказувати значення параметрів методу за замовчанням.

Як і з вимогами властивостей, слід завжди позначати вимоги методів типів за допомогою ключового слова `static`. При цьому реалізації цих методів у класах можуть позначатись як ключовим словом `static`, так і ключовим словом  `class`:

```swift
protocol SomeProtocol {
    static func someTypeMethod()
}
```

У наступному прикладі визначено протокол з єдиною вимогою методу екземпляру:

```swift
protocol RandomNumberGenerator {
    func random() -> Double
}
```

Даний протокол `RandomNumberGenerator` вимагає, щоб підпорядковані типи мали метод екземпляру на ім'я `random`, котрий при виклику повертає випадкове значення типу `Double`. Хоч в протоколі це не зазначено явно, будемо вважати, що метод має повертати числове значення в діапазоні від  `0.0` включно до `1.0` невключно.

Протокол `RandomNumberGenerator` не робить жодних припущень щодо того, як буде згенеровано кожне випадкове число – він просто вимагає, щоб генератор випадкових чисел, котрий до нього підпорядкований, мав  стандартний спосіб генерації випадкового числа. 

Ось реалізація класу, підпорядкованого до протоколу `RandomNumberGenerator`. Цей клас реалізовує алгоритм геренації псевдовипадкових чисел, відомий як *лінійний конгруентний метод*:

```swift
class LinearCongruentialGenerator: RandomNumberGenerator {
    var lastRandom = 42.0
    let m = 139968.0
    let a = 3877.0
    let c = 29573.0
    func random() -> Double {
        lastRandom = ((lastRandom * a + c).truncatingRemainder(dividingBy:m))
        return lastRandom / m
    }
}
let generator = LinearCongruentialGenerator()
print("Ось випадкове число: \(generator.random())")
// Надрукує "Ось випадкове число: 0.37464991998171"
print("А ось іще одне: \(generator.random())")
// Надрукує "А ось іще одне: 0.729023776863283"
```

### Вимоги мутуючих методів

У методах часом буває потрібно змінити (або *мутувати*) екземпляр, до якого належить даний метод. Методи екземплярів типів-значень (тобто структур та перечислень), що можуть змінювати свій екземпляр або будь-яку з його властивостей, повинні позначатись ключовим словом `mutating` перед ключовим словом `func`. Цей процес детально описаний у підрозділі [Зміни типів-значень в методах екземплярів](10_methods.md#Зміни-типів-значень-в-методах-екземплярів).

При визначенні у протоколі вимог методів екземплярів, котрі призначені для зміни цих екземплярів, слід позначати такі вимоги ключовим словом `mutating` у оголошенні протоколу. Це дозволяє структурам та перечисленням підпорядковуватись протоколу та задовольняти вимоги мутуючих методів. 

> **Примітка**
> 
> Якщо позначити вимогу методу екземпляру ключовим словом `mutating`, при реалізації даного методу в класі, підпорядкованому даному протоколу, вказувати це ключове слово не потрібно. Ключове слово `mutating` використовується тільки для структур та перечислень. 

У прикладі нижче оголошено протокол на ім'я `Togglable`, котрий визначає єдину вимогу методу екземпляру на ім'я `toggle`. Метод `toggle()` призначений для інвертування стану підпорядкованого типу, тобто для переключення стану на протилежний, шляхом зміни властивості цього типу. 

В оголошенні протоколу  `Togglable` вимогу методу `toggle()` позначено ключовим словом `mutating`, щоб відобразити, що метод `toggle()` призначений для зміни стану підпорядкованого екземпляру: 

```swift
protocol Togglable {
    mutating func toggle()
}
```

Якщо реалізовувати протокол `Togglable` у структурі чи перечисленні, ця структура чи перечислення повинна мати реалізацію методу `toggle()`, що також є позначеною ключовим словом `mutating`. 

У прикладі далі оголошено перечислення на ім'я `OnOffSwitch`, котре моделює стан перемикача світла. Це перечислення переключається між двома станами, що виражаються елеметнами перечислення `on` (увімкнено) та `off` (вимкнено). Реалізація методу `toggle` є позначеною ключовим словом `mutating`, як того вимагає протокол `Togglable`:

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
// lightSwitch тепер дорівнює .on
```

### Вимоги ініціалізаторів

Протоколи можуть вимагати, щоб підпорядковані типи мали певні ініціалізатори. Ці вимоги ініціалізаторів записуються як частина оголошення протоколу, в точно такий же спосіб, як і звичайні ініціалізатори, але без фігурних дужок та тіла ініціалізатора:

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
```

#### Реалізація вимог ініціалізаторів у класах

Вимоги ініціалізаторів у класах можна реалізовувати як у вигляді призначених ініціалізаторів, так і у вигляді ініціалізаторів для зручності. В обох випадках, слід реалізацію ініціалізатору слід позначати ключовим словом `required`:

```swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // тут йде реалізація ініціалізатора
    }
}
```

Використання модифікатора `required` гарантує, що явну чи успадковану реалізацію ініціалізатора буде надано *в усіх нащадках* підпорядкованого класу, і таким чином вони також підпорядковуються до даного протоколу.  

Детальніше з обов'язковими ініціалізаторами можна ознайомитись у підрозділі [Обов'язкові ініціалізатори](13_initialization.md#Обов'язкові-ініціалізатори).и

> **Примітка**
>
> Для класів, позначених ключовим словом `final`, позначати реалізацію ініціалізатора протоколу не потрібно, оскільки фінальні класи не можна наслідувати. Детальніше з ключовим словом `final` можна ознайомитись у підрозділі [Запобігання заміщенню](12_inheritance.md#Запобігання-заміщенню).

Якщо клас-нададок заміщує призначений ініціалізатор батьківського класу, і він також реалізовує вимогу ініціалізатора протоколу, реалізацію ініціалізатора в нащадку слід позначати одночасно ключовими словами  `required` та `override`:

```swift
protocol SomeProtocol {
    init()
}
 
class SomeSuperClass {
    init() {        
        // тут йде реалізація ініціалізатора
    }
}
 
class SomeSubClass: SomeSuperClass, SomeProtocol {
    // "required" через підпорядкованість до протоколу SomeProtocol; 
    // "override" через наслідування SomeSuperClass:
    required override init() {
        // тут йде реалізація ініціалізатора
    }
}
```

#### Вимоги ненадійних ініціалізаторів

Протоколи можуть визначати вимоги ненадійних ініціалізаторів, котрі описані у підрозділі [Ненадійні ініціалізатори](13_initialization.md#Ненадійні ініціалізатори).

Вимогу ненадійного ініціалізатору можна задовольнити як ненадійним, так і звичайним, надійним ініціалізатором підпорядкованого типу. Вимогу надійного ініціалізатору можна задовольнити або надійним ініціалізатором, або ненадійним ініціалізатором `init!`.

### Протоколи як типи

Протоколи самі по собі фактично не реалізовують жодної функціональності. Тим не менше, кожен протокол можна використовувати у коді як повноцінний тип.

Оскільки протокол є типом, його можна використовувати у багатьох місцях, де дозволяються інші типи, в тому числі:

 + Як тип параметра чи тип, що повертається у функціях, методах чи ініціалізаторах.
 + Як тип константи, змінної чи властивості
 + Як тип елементів масиву, словнику чи іншої колекції.

> **Примітка**
>
> Оскільки протоколи є типами, слід записувати їх назви з великої літери (як, наприклад, `FullyNamed` та `RandomNumberGenerator`), щоб відповідати іншим назвам типів у Swift (як, наприклад, `Int`, `String`, та `Double`).

Ось приклад використання протоколу як типу:

```swift
class Dice {
    let sides: Int
    let generator: RandomNumberGenerator
    init(sides: Int, generator: RandomNumberGenerator) {
        self.sides = sides
        self.generator = generator
    }
    func roll() -> Int {
        return Int(generator.random() * Double(sides)) + 1
    }
}
```

У даному прикладі оголошено новий клас на ім'я `Dice`, котрий представляє *n*-сторонні гральні кості для використання в настільній грі. Екземпляри `Dice` мають цілочисельну властивість `sides`, котра представляє кількість сторін, що має кость, та властивість на ім'я `generator`, котра містить генератор випадкових чисел для моделювання значення кидка костей. 

Властивість `generator` має тип `RandomNumberGenerator`. Таким чином, їй можна присвоїти екземпляр *будь-якого типу*, що підпорядковується до протоколу `RandomNumberGenerator`. Для того, щоб екземпляр можна було присвоїти властивості `RandomNumberGenerator`, не вимагається нічого, крім підпорядкованості протоколу `RandomNumberGenerator`. 

Екземпляри `Dice` також має ініціалізатор, що задає їх початковий стан. Цей ініціалізатор має параметр на ім'я `generator`, котрий також має тип `RandomNumberGenerator`. Таким чином при ініціалізації нового екземпляру `Dice` можна передавати значення будь-якого підпорядкованого типу в якості цього параметра.

Клас `Dice` також має метод екземпляру, `roll()`, що повертає цілочисельне значення в проміжку від `1` до `sides` (кількості сторін гральної кості). Цей метод викликає метод `random()` властивості `generator`, котрий повертає випадкове число в діапазоні від `0.0` до `1.0`, після чого це число використовується для отримання значення кості всередині її діапазону значень. Оскільки екземпляр `generator` є підпорядкованим протоколу `RandomNumberGenerator`, він гарантовано має метод `random()`.

Ось приклад використання класу `Dice` для створення шестигранної гральної кості з екземпляром `LinearCongruentialGenerator` в якості її генератора випадкових чисел:

```swift
var d6 = Dice(sides: 6, generator: LinearCongruentialGenerator())
for _ in 1...5 {
    print("Випадковий кидок кості: \(d6.roll())")
}
// Випадковий кидок кості: 3
// Випадковий кидок кості: 5
// Випадковий кидок кості: 4
// Випадковий кидок кості: 5
// Випадковий кидок кості: 4
```

### Делегування

*Делегування* – це шаблон проектування, що дозволяє класу чи стурктурі передати (або делегувати) якісь із своїх відповідальностей до екземпляру іншого типу. Цей шаблон проектування реалізовується за допомогою протоколу, що інкапсулює делеговані відповідальності таким чином, що підпорядкований тип (котрий називають делегатом) гарантовано реалізовує делеговану функціональність. Делегування може використовуватись як відповідь на певну дію, чи для отримання даних із зовнішнього джерела, без необхідності знати фактичний тип цього джерела. 

У прикладі нижче визначено два протоколи для використання у настільних іграх, де потрібні гральні кості:

```swift
protocol DiceGame {
    var dice: Dice { get }
    func play()
}
protocol DiceGameDelegate {
    func gameDidStart(_ game: DiceGame)
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
    func gameDidEnd(_ game: DiceGame)
}
```
Протоколу `DiceGame` може підпорядковуватись будь-яка гра, що передбачає кидання костей. Протоколу `DiceGameDelegate` може підпорядковуватись будь-який тип, якому потрібно відстежувати прогрес у грі, що предаставляє `DiceGame`. 

Ось версія гри *Ліла* (або *Змії і сходи*), котру вперше було представлено у розділі [Потік керування](4_control_flow.md). Ця версія є адаптованою до використання екземпляру `Dice` для кидків костей, підпорядкованою до протоколу `DiceGame`, та повідомляє свій прогрес за допомогою протоколу `DiceGameDelegate`:

```swift
class SnakesAndLadders: DiceGame {
    let finalSquare = 25
    let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
    var square = 0
    var board: [Int]
    init() {
        board = Array(repeating: 0, count: finalSquare + 1)
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
    }
    var delegate: DiceGameDelegate?
    func play() {
        square = 0
        delegate?.gameDidStart(self)
        gameLoop: while square != finalSquare {
            let diceRoll = dice.roll()
            delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)
            switch square + diceRoll {
            case finalSquare:
                break gameLoop
            case let newSquare where newSquare > finalSquare:
                continue gameLoop
            default:
                square += diceRoll
                square += board[square]
            }
        }
        delegate?.gameDidEnd(self)
    }
}
```

Описання ігрового процесу гри Ліла можна знайти у секції [Інструкція Break](4_control_flow.md#Інструкція-Break) розділу [Потік керування](4_control_flow.md).

Ця версія гри є загорнутою в клас `SnakesAndLadders`, котрий реалізовує протокол `DiceGame`. Він має властивість для читання `dice` та метод `play()`, і таким чином підпорядковується до цього протоколу. (Властивість `dice` оголошено як константну властивість, оскільки вона не змінюється після ініціалізації, а протокол вимагає лише щоб цю властивість можна було прочитати).

Налаштування дошки в грі *Ліла* відбувається у ініціалізаторі класу `init()`. Вся ігрова логіка переїхала до методу реалізації методу протоколу `play()`, в котрій використовується властивість протоколу `dice`, для отримання значення кинутої кості. 

Слід помітити, що властивість `delegate` визначено як опціональний `DiceGameDelegate`, оскільки для гри не обов'язково мати делегат. Оскільки властивість `delegate` має опціональний тип, вона автоматично отримує початкове значення `nil`. Після цього, той, хто створив гру, має можливість присвоїти властивості `delegate` відповідне значення.

Протокол `DiceGameDelegate` має три методи для відслідковування прогресу гри. Ці методи було інкорпоровано в ігрову логіку всередині методу `play()` вище; вони викликаються на початку гри, на початку ходу та в кінці гри. 

Оскільки властивість `delegate` є опціональним `DiceGameDelegate`, метод `play()` викликає методи делегату за допомогою ланцюжку опціоналів. Якщо властивість `delegate` має значення `nil`, ці звернення до делегату будуть проігноровані, без помилок. Якщо властивість `delegate` має значення і воно не `nil`, будуть викликані методи делегату, і до них буде передано екземпляр `SnakesAndLadders` в якості параметра.

У наступному прикладі показано клас на ім'я `DiceGameTracker`, котрий підпорядковується до протоколу `DiceGameDelegate`:

```swift
class DiceGameTracker: DiceGameDelegate {
    var numberOfTurns = 0
    func gameDidStart(_ game: DiceGame) {
        numberOfTurns = 0
        if game is SnakesAndLadders {
            print("Розпочалась нова гра у Змії та Сходи")
        }
        print("У грі використовуються \(game.dice.sides)-сторонні гральні кості")
    }
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
        numberOfTurns += 1
        print("На костях випало \(diceRoll)")
    }
    func gameDidEnd(_ game: DiceGame) {
        print("Гра тривала \(numberOfTurns) ходи(ів)")
    }
}
```

Клас `DiceGameTracker` реалізовує всі три методи, що вимагаються протоколом `DiceGameDelegate`. Він використовує ці методи для відслідковування кількості ходів у грі. Він скидає значення властивості `numberOfTurns` до нуля на початку нової гри, збільшує його на одиницю при кожному ході, та друкує загальну кількість ходів у грі, як тільки завершиться гра.

Показана вище реалізація методу `gameDidStart(_:)` використовує параметр `game` для друку деякої вступної інформації про те, що гра починається. Параметр `game` має тип `DiceGame`, а не `SnakesAndLadders`, тому метод `gameDidStart(_:)` має доступ лише до методів та властивостей, що були оголошені як частина протоколу `DiceGame`. Однак, все ще можна використати приведення типів всередині методу, щоб отримати тип фактично переданого екземпляру. У даному прикладі, перевіряється, чи є параметр `game` фактично екземпляром класу `SnakesAndLadders` за лаштунками, і якщо так – друкує відповідне повідомлення. 

Метод `gameDidStart(_:)` також звертається до властивості `dice` переданого параметру `game`. Оскільки параметр `game` має будь-який тип, підпорядкований протоколу `DiceGame`, він гарантовано має властивість `dice`, і тому метод `gameDidStart(_:)` може звертатись до неї та друкувати її значення, незалежно від того, яка саме гра зараз відбувається. 

Ось як виглядає `DiceGameTracker` в дії:

```swift
let tracker = DiceGameTracker()
let game = SnakesAndLadders()
game.delegate = tracker
game.play()
// Розпочалась нова гра у Змії та Сходи
// У грі використовуються 6-сторонні гральні кості
// На костях випало  3
// На костях випало  5
// На костях випало  4
// На костях випало  5
// Гра тривала 4 ходи(ів)
```

### Підпорядкування протоколу за допомогою розширення

Можна розширити існуючий тип для підпорядкування його новому протоколу, навіть якщо у вас доступу до вихідного коду існуючого типу. Розширення можуть додавати нові властивості, методи та індекси до існуючих типів, і тому їм до снаги реалізувати будь-які вимоги, що можуть бути проголошені протоколом. Більше про розширення можна прочитати у розділі [Розширення](19_extensions.md).

> **Примітка**
>
> Коли тип підпорядковується протоколу за допомогою розширення, існуючі екземпляри цього типу підпорядковуються протоколу та реалізовують його автоматично.

Наприклад, цей протокол, що називається `TextRepresentable`, може бути реалізованим будь-яким типом, що може бути представленим у вигляді тексту. Це може бути опис себе, або текстова версія поточного стану:

```swift
protocol TextRepresentable {
    var textualDescription: String { get }
}
```

Клас `Dice` з попередніх прикладів може бути розширеним для підпорядкування до протоколу `TextRepresentable`:

```swift
extension Dice: TextRepresentable {
    var textualDescription: String {
        return "\(sides)-стороння гральна кість"
    }
}
```

Це розширення підпорядковує клас `Dice` новому протоколу так, ніби цей клас підпорядковувався би до ного у своїй оригінальній реалізації. Назва протоколу йде після імені типу, відділяючись комою, а реалізація всіх вимог протоколу йде далі у фігурних дужках.

До будь-який екземпляру класу `Dice` тепер можна звертатись як до `TextRepresentable`:

```swift
let d12 = Dice(sides: 12, generator: LinearCongruentialGenerator())
print(d12.textualDescription)
// Надрукує "12-стороння гральна кість"
```

Аналогічно, клас гри `SnakesAndLadders` може бути розширеним для підпорядкування протоколу `TextRepresentable`:

```swift
extension SnakesAndLadders: TextRepresentable {
    var textualDescription: String {
        return "Гра ліла на \(finalSquare) клітинок"
    }
}
print(game.textualDescription)
// Надрукує "Гра ліла на 25 клітинок"
```

#### Оголошення підпорядкування протоколу за допомогою розширення

Якщо тип вже реалізовує всі вимоги протоколу, але явно не вказує, що підпорядковується до цього протоколу, можна підпорядкувати його до протоколу за допомогою порожнього розширення:

```swift
struct Hamster {
    var name: String
    var textualDescription: String {
        return "Хом'як на ім'я \(name)"
    }
}
extension Hamster: TextRepresentable {}
```

Екземпляри класу `Hamster` тепер можна використовувати будь-де, де вимагається тип `TextRepresentable`:

```swift
let simonTheHamster = Hamster(name: "Сомко")
let somethingTextRepresentable: TextRepresentable = simonTheHamster
print(somethingTextRepresentable.textualDescription)
// Надрукує "Хом'як на ім'я Сомкои"
```

> **Примітка**
>
> Типи не підпорядковуються до протоколів автоматично, лише задовольняючи його вимоги. Вони мають явно оголошувати їх підпорядкування до протоколу.

### Collections of Protocol Types

### Колекції протоколів

A protocol can be used as the type to be stored in a collection such as an array or a dictionary, as mentioned in [Protocols as Types](#Protocols-as-Types). This example creates an array of `TextRepresentable` things:

```swift
let things: [TextRepresentable] = [game, d12, simonTheHamster]
```

It is now possible to iterate over the items in the array, and print each item’s textual description:

```swift
for thing in things {
    print(thing.textualDescription)
}
// A game of Snakes and Ladders with 25 squares
// A 12-sided dice
// A hamster named Simon
```

Note that the thing constant is of type `TextRepresentable`. It is not of type `Dice`, or `DiceGame`, or `Hamster`, even if the actual instance behind the scenes is of one of those types. Nonetheless, because it is of type `TextRepresentable`, and anything that is `TextRepresentable` is known to have a `textualDescription` property, it is safe to access `thing.textualDescription` each time through the loop.

### Наслідування протоколів

### Protocol Inheritance

A protocol can *inherit* one or more other protocols and can add further requirements on top of the requirements it inherits. The syntax for protocol inheritance is similar to the syntax for class inheritance, but with the option to list multiple inherited protocols, separated by commas:

```swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
    // protocol definition goes here
}
```

Here’s an example of a protocol that inherits the `TextRepresentable` protocol from above:

```swift
protocol PrettyTextRepresentable: TextRepresentable {
    var prettyTextualDescription: String { get }
}
```

This example defines a new protocol, `PrettyTextRepresentable`, which inherits from `TextRepresentable`. Anything that adopts `PrettyTextRepresentable` must satisfy all of the requirements enforced by `TextRepresentable`, *plus* the additional requirements enforced by `PrettyTextRepresentable`. In this example, `PrettyTextRepresentable` adds a single requirement to provide a gettable property called `prettyTextualDescription` that returns a `String`.

The `SnakesAndLadders` class can be extended to adopt and conform to `PrettyTextRepresentable`:

```swift
extension SnakesAndLadders: PrettyTextRepresentable {
    var prettyTextualDescription: String {
        var output = textualDescription + ":\n"
        for index in 1...finalSquare {
            switch board[index] {
            case let ladder where ladder > 0:
                output += "▲ "
            case let snake where snake < 0:
                output += "▼ "
            default:
                output += "○ "
            }
        }
        return output
    }
}
```

This extension states that it adopts the `PrettyTextRepresentable` protocol and provides an implementation of the `prettyTextualDescription` property for the `SnakesAndLadders` type. Anything that is `PrettyTextRepresentable` must also be `TextRepresentable`, and so the implementation of `prettyTextualDescription` starts by accessing the `textualDescription` property from the `TextRepresentable` protocol to begin an output string. It appends a colon and a line break, and uses this as the start of its pretty text representation. It then iterates through the array of board squares, and appends a geometric shape to represent the contents of each square:

If the square’s value is greater than 0, it is the base of a ladder, and is represented by ▲.
If the square’s value is less than 0, it is the head of a snake, and is represented by ▼.
Otherwise, the square’s value is 0, and it is a “free” square, represented by ○.
The prettyTextualDescription property can now be used to print a pretty text description of any SnakesAndLadders instance:

```swift
print(game.prettyTextualDescription)
// A game of Snakes and Ladders with 25 squares:
// ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
```

### Class-Only Protocols

You can limit protocol adoption to class types (and not structures or enumerations) by adding the `AnyObject` protocol to a protocol’s inheritance list.

```swift
protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
    // class-only protocol definition goes here
}
```

In the example above, `SomeClassOnlyProtocol` can only be adopted by class types. It is a compile-time error to write a structure or enumeration definition that tries to adopt `SomeClassOnlyProtocol`.

> **Примітка**
>
> Use a class-only protocol when the behavior defined by that protocol’s requirements assumes or requires that a conforming type has reference semantics rather than value semantics. For more on reference and value semantics, see [Структури і перечислення як типи-значення](8_classes_and_structures.md#Структури-і-перечислення-як-типи-значення) and [Класи як типи-посилання](8_classes_and_structures.md#Класи-як-типи-посилання).

### Protocol Composition

### Композиція протоколів

It can be useful to require a type to conform to multiple protocols at the same time. You can combine multiple protocols into a single requirement with a *protocol composition*. Protocol compositions behave as if you defined a temporary local protocol that has the combined requirements of all protocols in the composition. Protocol compositions don’t define any new protocol types.

Protocol compositions have the form `SomeProtocol & AnotherProtocol`. You can list as many protocols as you need, separating them with ampersands (`&`). In addition to its list of protocols, a protocol composition can also contain one class type, which you can use to specify a required superclass.

Here’s an example that combines two protocols called `Named` and `Aged` into a single protocol composition requirement on a function parameter:

```swift
protocol Named {
    var name: String { get }
}
protocol Aged {
    var age: Int { get }
}
struct Person: Named, Aged {
    var name: String
    var age: Int
}
func wishHappyBirthday(to celebrator: Named & Aged) {
    print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
}
let birthdayPerson = Person(name: "Malcolm", age: 21)
wishHappyBirthday(to: birthdayPerson)
// Надрукує "Happy birthday, Malcolm, you're 21!"
```

In this example, the `Named` protocol has a single requirement for a gettable `String` property called `name`. The `Aged` protocol has a single requirement for a gettable Int property called `age`. Both protocols are adopted by a structure called `Person`.

The example also defines a `wishHappyBirthday(to:)` function. The type of the celebrator parameter is `Named & Aged`, which means “any type that conforms to both the `Named` and `Aged` protocols.” It doesn’t matter which specific type is passed to the function, as long as it conforms to both of the required protocols.

The example then creates a new `Person` instance called `birthdayPerson` and passes this new instance to the `wishHappyBirthday(to:)` function. Because `Person` conforms to both protocols, this call is valid, and the `wishHappyBirthday(to:)` function can print its birthday greeting.

Here’s an example that combines the `Named` protocol from the previous example with a `Location` class:

```swift
class Location {
    var latitude: Double
    var longitude: Double
    init(latitude: Double, longitude: Double) {
        self.latitude = latitude
        self.longitude = longitude
    }
}
class City: Location, Named {
    var name: String
    init(name: String, latitude: Double, longitude: Double) {
        self.name = name
        super.init(latitude: latitude, longitude: longitude)
    }
}
func beginConcert(in location: Location & Named) {
    print("Hello, \(location.name)!")
}

let seattle = City(name: "Seattle", latitude: 47.6, longitude: -122.3)
beginConcert(in: seattle)
// Надрукує "Hello, Seattle!"
```

The `beginConcert(in:)` function takes a parameter of type `Location & Named`, which means “any type that’s a subclass of `Location` and that conforms to the `Named` protocol.” In this case, `City` satisfies both requirements.

Passing `birthdayPerson` to the `beginConcert(in:)` function is invalid because `Person` isn’t a subclass of `Location`. Likewise, if you made a subclass of `Location` that didn’t conform to the `Named` protocol, calling `beginConcert(in:)` with an instance of that type is also invalid.

### Checking for Protocol Conformance

### Перевірка на підпорядкованість протоколу

You can use the is and as operators described in [Type Casting]() to check for protocol conformance, and to cast to a specific protocol. Checking for and casting to a protocol follows exactly the same syntax as checking for and casting to a type:

- The is operator returns true if an instance conforms to a protocol and returns false if it does not.
- The `as?` version of the downcast operator returns an optional value of the protocol’s type, and this value is nil if the instance does not conform to that protocol.
- The `as!` version of the downcast operator forces the downcast to the protocol type and triggers a runtime error if the downcast does not succeed.

This example defines a protocol called `HasArea`, with a single property requirement of a gettable `Double` property called area:

```swift
protocol HasArea {
    var area: Double { get }
}
```

Here are two classes, `Circle` and `Country`, both of which conform to the `HasArea` protocol:

```swift
class Circle: HasArea {
    let pi = 3.1415927
    var radius: Double
    var area: Double { return pi * radius * radius }
    init(radius: Double) { self.radius = radius }
}
class Country: HasArea {
    var area: Double
    init(area: Double) { self.area = area }
}
```

The `Circle` class implements the area property requirement as a computed property, based on a stored radius property. The `Country` class implements the area requirement directly as a stored property. Both classes correctly conform to the `HasArea` protocol.
Here’s a class called `Animal`, which does not conform to the `HasArea` protocol:

```swift
class Animal {
    var legs: Int
    init(legs: Int) { self.legs = legs }
}
```

The `Circle`, `Country` and `Animal` classes do not have a shared base class. Nonetheless, they are all classes, and so instances of all three types can be used to initialize an array that stores values of type `AnyObject`:

```swift
let objects: [AnyObject] = [
    Circle(radius: 2.0),
    Country(area: 243_610),
    Animal(legs: 4)
]
```

The objects array is initialized with an array literal containing a `Circle` instance with a radius of 2 units; a `Country` instance initialized with the surface area of the United Kingdom in square kilometers; and an `Animal` instance with four legs.

The objects array can now be iterated, and each object in the array can be checked to see if it conforms to the `HasArea` protocol:

```swift
for object in objects {
    if let objectWithArea = object as? HasArea {
        print("Area is \(objectWithArea.area)")
    } else {
        print("Something that doesn't have an area")
    }
}
// Area is 12.5663708
// Area is 243610.0
// Something that doesn't have an area
```

Whenever an object in the array conforms to the `HasArea` protocol, the optional value returned by the `as?` operator is unwrapped with optional binding into a constant called `objectWithArea`. The `objectWithArea` constant is known to be of type `HasArea`, and so its area property can be accessed and printed in a type-safe way.

Note that the underlying objects are not changed by the casting process. They continue to be a `Circle`, a `Country` and an `Animal`. However, at the point that they are stored in the `objectWithArea` constant, they are only known to be of type `HasArea`, and so only their area property can be accessed.

### Optional Protocol Requirements

You can define *optional requirements* for protocols, These requirements do not have to be implemented by types that conform to the protocol. Optional requirements are prefixed by the optional modifier as part of the protocol’s definition. Optional requirements are available so that you can write code that interoperates with Objective-C. Both the protocol and the optional requirement must be marked with the `@objc` attribute. Note that `@objc` protocols can be adopted only by classes that inherit from Objective-C classes or other `@objc` classes. They can’t be adopted by structures or enumerations.

When you use a method or property in an optional requirement, its type automatically becomes an optional. For example, a method of type `(Int) -> String` becomes `((Int) -> String)?`. Note that the entire function type is wrapped in the optional, not the method’s return value.

An optional protocol requirement can be called with optional chaining, to account for the possibility that the requirement was not implemented by a type that conforms to the protocol. You check for an implementation of an optional method by writing a question mark after the name of the method when it is called, such as `someOptionalMethod?(someArgument)`. For information on optional chaining, see [Optional Chaining](15_optional_chaining.md).

The following example defines an integer-counting class called `Counter`, which uses an external data source to provide its increment amount. This data source is defined by the `CounterDataSource` protocol, which has two optional requirements:

```swift
@objc protocol CounterDataSource {
    @objc optional func increment(forCount count: Int) -> Int
    @objc optional var fixedIncrement: Int { get }
}
```

The `CounterDataSource` protocol defines an optional method requirement called `increment(forCount:)` and an optional property requirement called `fixedIncrement`. These requirements define two different ways for data sources to provide an appropriate increment amount for a `Counter` instance.

> **Примітка**
>
> Strictly speaking, you can write a custom class that conforms to CounterDataSource without implementing either protocol requirement. They are both optional, after all. Although technically allowed, this wouldn’t make for a very good data source.

The `Counter` class, defined below, has an optional `dataSource` property of type `CounterDataSource?`:

```swift
class Counter {
    var count = 0
    var dataSource: CounterDataSource?
    func increment() {
        if let amount = dataSource?.increment?(forCount: count) {
            count += amount
        } else if let amount = dataSource?.fixedIncrement {
            count += amount
        }
    }
}
```

The `Counter` class stores its current value in a variable property called count. The `Counter` class also defines a method called increment, which increments the count property every time the method is called.

The `increment()` method first tries to retrieve an increment amount by looking for an implementation of the `increment(forCount:)` method on its data source. The `increment()` method uses optional chaining to try to call `increment(forCount:)`, and passes the current count value as the method’s single argument.

Note that two levels of optional chaining are at play here. First, it is possible that `dataSource` may be nil, and so `dataSource` has a question mark after its name to indicate that `increment(forCount:)` should be called only if `dataSource` isn’t `nil`. Second, even if `dataSource` does exist, there is no guarantee that it implements `increment(forCount:)`, because it is an optional requirement. Here, the possibility that `increment(forCount:)` might not be implemented is also handled by optional chaining. The call to `increment(forCount:)` happens only if `increment(forCount:)` exists—that is, if it isn’t `nil`. This is why `increment(forCount:)` is also written with a question mark after its name.

Because the call to `increment(forCount:)` can fail for either of these two reasons, the call returns an optional Int value. This is true even though `increment(forCount:)` is defined as returning a nonoptional Int value in the definition of `CounterDataSource`. Even though there are two optional chaining operations, one after another, the result is still wrapped in a single optional. For more information about using multiple optional chaining operations, see [Linking Multiple Levels of Chaining](15_optional_chaining.md#Linking-Multiple-Levels-of-Chaining).

After calling `increment(forCount:)`, the optional `Int` that it returns is unwrapped into a constant called amount, using optional binding. If the optional `Int` does contain a value — that is, if the delegate and method both exist, and the method returned a value — the unwrapped amount is added onto the stored count property, and incrementation is complete.

If it is not possible to retrieve a value from the `increment(forCount:)` method — either because `dataSource` is `nil`, or because the data source does not implement `increment(forCount:)` — then the `increment()` method tries to retrieve a value from the data source’s fixedIncrement property instead. The fixedIncrement property is also an optional requirement, so its value is an optional `Int` value, even though fixedIncrement is defined as a nonoptional Int property as part of the `CounterDataSource` protocol definition.

Here’s a simple `CounterDataSource` implementation where the data source returns a constant value of `3` every time it is queried. It does this by implementing the optional fixedIncrement property requirement:

```swift
class ThreeSource: NSObject, CounterDataSource {
    let fixedIncrement = 3
}
```

You can use an instance of `ThreeSource` as the data source for a new `Counter` instance:

```swift
var counter = Counter()
counter.dataSource = ThreeSource()
for _ in 1...4 {
    counter.increment()
    print(counter.count)
}
// 3
// 6
// 9
// 12
```

The code above creates a new `Counter` instance; sets its data source to be a new `ThreeSource` instance; and calls the counter’s increment() method four times. As expected, the counter’s count property increases by three each time increment() is called.

Here’s a more complex data source called TowardsZeroSource, which makes a Counter instance count up or down towards zero from its current count value:

```swift
@objc class TowardsZeroSource: NSObject, CounterDataSource {
    func increment(forCount count: Int) -> Int {
        if count == 0 {
            return 0
        } else if count < 0 {
            return 1
        } else {
            return -1
        }
    }
}
```

The `TowardsZeroSource` class implements the optional increment(forCount:) method from the CounterDataSource protocol and uses the count argument value to work out which direction to count in. If count is already zero, the method returns 0 to indicate that no further counting should take place.
You can use an instance of TowardsZeroSource with the existing Counter instance to count from -4 to zero. Once the counter reaches zero, no more counting takes place:

```swift
counter.count = -4
counter.dataSource = TowardsZeroSource()
for _ in 1...5 {
    counter.increment()
    print(counter.count)
}
// -3
// -2
// -1
// 0
// 0
```

### Розширення протоколів

### Protocol Extensions

Protocols can be extended to provide method, initializer, subscript, and computed property implementations to conforming types. This allows you to define behavior on protocols themselves, rather than in each type’s individual conformance or in a global function.

For example, the `RandomNumberGenerator` protocol can be extended to provide a randomBool() method, which uses the result of the required random() method to return a random Bool value:

```swift
extension RandomNumberGenerator {
    func randomBool() -> Bool {
        return random() > 0.5
    }
}
```

By creating an extension on the protocol, all conforming types automatically gain this method implementation without any additional modification.

```swift
let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// Надрукує "Here's a random number: 0.3746499199817101"
print("And here's a random Boolean: \(generator.randomBool())")
// Надрукує "And here's a random Boolean: true"
```

Protocol extensions can add implementations to conforming types but can’t make a protocol extend or inherit from another protocol. Protocol inheritance is always specified in the protocol declaration itself.

#### Providing Default Implementations

You can use protocol extensions to provide a default implementation to any method or computed property requirement of that protocol. If a conforming type provides its own implementation of a required method or property, that implementation will be used instead of the one provided by the extension.

> **Примітка**
>
> Protocol requirements with default implementations provided by extensions are distinct from optional protocol requirements. Although conforming types don’t have to provide their own implementation of either, requirements with default implementations can be called without optional chaining.
>

For example, the PrettyTextRepresentable protocol, which inherits the TextRepresentable protocol can provide a default implementation of its required prettyTextualDescription property to simply return the result of accessing the textualDescription property:

```swift
extension PrettyTextRepresentable  {
    var prettyTextualDescription: String {
        return textualDescription
    }
}
```

#### Adding Constraints to Protocol Extensions

When you define a protocol extension, you can specify constraints that conforming types must satisfy before the methods and properties of the extension are available. You write these constraints after the name of the protocol you’re extending by writing a generic where clause. For more about generic where clauses, see Generic Where Clauses.

For example, you can define an extension to the Collection protocol that applies to any collection whose elements conform to the Equatable protocol. By constraining a collection’s elements to the Equatable protocol, a part of the standard library, you can use the == and != operators to check for equality and inequality between two elements.

```swift
extension Collection where Element: Equatable {
    func allEqual() -> Bool {
        for element in self {
            if element != self.first {
                return false
            }
        }
        return true
    }
}
```

The allEqual() method returns true only if all the elements in the collection are equal.

Consider two arrays of integers, one where all the elements are the same, and one where they aren’t:

```swift
let equalNumbers = [100, 100, 100, 100, 100]
let differentNumbers = [100, 100, 200, 100, 200]
Because arrays conform to Collection and integers conform to Equatable, equalNumbers and differentNumbers can use the allEqual() method:

print(equalNumbers.allEqual())
// Надрукує "true"
print(differentNumbers.allEqual())
// Надрукує "false"
```

> **Примітка**
>
> If a conforming type satisfies the requirements for multiple constrained extensions that provide implementations for the same method or property, Swift uses the implementation corresponding to the most specialized constraints.
>