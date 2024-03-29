---
title: Ланцюжки опціоналів
layout: default
parent: Посібник з мови
nav_order: 16
has_children: false
has_toc: false
---

# Ланцюжки опціоналів

_Ланцюжки опціоналів_ - це процес звернення до властивостей, індексів та виклику методів на опціоналі, що може в цей момент мати значення `nil`. Якщо опціонал має значення, звернення до властивості, методу чи індексу є успішним; якщо опціонал дорівнює `nil`, звернення до властивості, методу чи індексу повертає `nil`. Кілька таких звернень можна об'єднати разом у ланцюжок, і тоді весь ланцюжок поверне `nil`, якщо хоча б одна ланка у ньому є `nil`.

> **Примітка**
>
> Ланцюжки опціоналів у Swift є схожими на відправку повідомлення до `nil` у мові Objective-C, однак у спосіб, що працює для будь-якого типу, і в якому можна дізнатись, чи було звернення успішним.

## Ланцюжки опціоналів як альтернатива примусовому розгортанню

Ланцюжок опціоналів створюється шляхом вказування знаку питання \(`?`\) після опціонального значення, на якому потрібно зробити виклик методу чи звернутись до властивості або індексу, якщо цей опціонал не `nil`. Це є дуже схожим на вказування знаку оклику \(`!`\) після опціонального значення для його примусового розгортання. Головною відмінністю є те, що ланцюжок опціоналів не призводить до помилки часу виконання, якщо опціонал є `nil`, в той час як примусове розгортання в такому випадку – призводить.

Щоб показати той факт, що ланцюжок опціоналів може викликатись на значенні `nil`, результатом виклику ланцюжка опціоналів завжди є опціональне значення, навіть якщо властивість, метод чи індекс повертає не опціональне значення. Опціональне значення, що повертається, завжди можна перевірити на `nil`, щоб з'ясувати, виклик всього ланцюжка був успішним \(в такому випадку опціонал, що повертається, містить значення\), чи був невдалим через наявність значення `nil` в ланцюжку \(тоді опціонал, що повертається, матиме значення `nil`\).

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

Екземпляри `Residence` мають єдину властивість типу `Int` на ім'я `numberOfRooms`, котра моделює кількість кімнат та має значення за замовчанням `1`. Екземпляри `Person` мають опціональну властивість `residence` типу `Residence?`.

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

Ланцюжки опціоналів дають альтернативний спосіб звертання до властивості `numberOfRooms`. Щоб скористатись ланцюжком опціоналів, ми використаємо знак питання замість знаку оклику:

```swift
if let roomCount = ostap.residence?.numberOfRooms {
    print("Остапова хата має \(roomCount) кімнат(у).")
} else {
    print("Неможливо отримати кількість кімнат.")
}
// Надрукує "Неможливо отримати кількість кімнат."
```

Це каже Swift “покласти в ланцюжок” опціональну властивість `residence` та дістати з неї значення `numberOfRooms`, якщо `residence` існує.

Оскільки спроба звернутись до `numberOfRooms` може потенційно не вдатись, виклик ланцюжку опціоналів повертає значення `Int?`, або “опціональний `Int`”. Коли `residence` дорівнює `nil`, як у прикладі вище, цей опціонал матиме значення `nil`, що відображає той факт, що неможливо звернутись до значення `numberOfRooms`. Далі відбувається прив'язування опціоналу для розгортання цілочисельного значення та присвоєння не опціонального значення константі `roomCount`.

Слід помітити, що це працює попри те, що властивість `numberOfRooms` має не опціональний тип `Int`. Той факт, що звернення до цієї властивості відбувається через ланцюжок опціоналів, означає, що виклик завжди буде повертати значення типу `Int?` замість `Int`.

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

## Визначення модельних класів для ланцюжка опціоналів

Можна використовувати ланцюжки опціоналів зі зверненнями до властивостей, методів та індексів, в котрих є більш ніж один рівень вкладеності. Це дозволяє спускатись до підвластивостей у складних моделях взаємопов'язаних типів, і перевіряти, чи можливо звернутись до властивостей, методів та індексів у цих підвластивостях.

У фрагментах коду нижче визначено чотири модельні класи для використання у кількох наступних прикладах багаторівневих ланцюжків опціоналів. Ці класи розширюють модель, задану класами `Person` та `Residence` з прикладів вище, додаючи до неї класи `Room` та `Address` \(котрі моделюють кімнату та адресу відповідно\), з асоційованими властивостями, методами та індексами.

Клас `Person` визначено так само, як і раніше:

```swift
class Person {
    var residence: Residence?
}
```

Клас `Residence` тепер є складнішим, ніж раніше. Цього разу, клас `Residence` має змінну властивість на ім'я `rooms`, котра ініціалізується порожнім масивом типу `[Room]`:

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
        print("Кількість кімнат дорівнює \(numberOfRooms)")
    }
    var address: Address?
}
```

Оскільки дана версія класу `Residence` зберігає масив екземплярів `Room`, її властивість `numberOfRooms` реалізовано як властивість, що обчислюється, а не зберігається. Властивість `numberOfRooms` тепер просто повертає значення кількості елементів в масиві `rooms`.

Для швидкого доступу до масиву `rooms`, дана версія класу `Residence` реалізовує індекс для читання й запису, що дає доступ до кімнат по заданому індексу в масиві `rooms`.

Дана версія класу `Residence` також має метод на ім'я `printNumberOfRooms`, котрий просто друкує кількість кімнат у помешканні.

І нарешті, `Residence` має опціональну властивість на ім'я `address`, з типом `Address?`. `Address` – це клас, котрий буде оголошено далі.

Клас `Room`, котрий використовується для масиву `rooms`, є простим класом з єдиною властивістю на ім'я `name`, та ініціалізатором, що задає цій властивості доречну назву кімнати:

```swift
class Room {
    let name: String
    init(name: String) { self.name = name }
}
```

Останнім класом у даній моделі є клас `Address`. Цей клас має три опціональні властивості типу `String?`. Перші дві властивості, `buildingName` та `buildingNumber`, представляють назву будинку та номер будинку відповідно, та є альтернативними способами ідентифікувати певний будинок. Третя властивість, `street`, представляє назву вулиці в даній адресі:

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

Клас `Address` також має метод, що називається `buildingIdentifier()`, котрий повертає значення типу `String?`. Цей метод перевіряє властивості даної адреси та повертає `buildingName`, якщо та має значення, або значення властивості `buildingNumber`, конкатеноване зі значенням властивості `street`, якщо вони обидві мають значення, або `nil` в інших випадках.

## Доступ до властивостей через ланцюжок опціоналів

Як показано вище в підрозділі [Ланцюжки опціоналів як альтернатива примусовому розгортанню]({% link _book/1_language_guide/15_optional_chaining.md %}#ланцюжки-опціоналів-як-альтернатива-примусовому-розгортанню), ланцюжок опціоналів дозволяє звертатись до властивості опціонального значення, при цьому можна перевірити, чи було таке звернення успішним.

Використовуючи класи, оголошені вище, можна створити новий екземпляр `Person`, та спробувати звернутись до його властивості `numberOfRooms`, як ми це робили раніше:

```swift
let orest = Person()
if let roomCount = orest.residence?.numberOfRooms {
    print("Орестова хата має \(roomCount) кімнат(у).")
} else {
    print("Неможливо отримати кількість кімнат.")
}
// Надрукує "Неможливо отримати кількість кімнат."
```

Оскільки `orest.residence` дорівнює `nil`, це звернення до ланцюжка опціоналів буде невдалим з тих же причин, що і раніше.

Також можна спробувати присвоїти значення властивості за допомогою ланцюжка опціоналів:

```swift
let someAddress = Address()
someAddress.buildingNumber = "26"
someAddress.street = "Хрещатик"
orest.residence?.address = someAddress
```

В цьому прикладі, спроба присвоїти адресу властивості `orest.residence` провалиться, оскільки `orest.residence` в даний момент має значення `nil`.

Присвоєння є частиною ланцюжку опціоналів, і це значить, що жоден код справа від оператора `=` не буде виконано. У попередньому прикладі, не зовсім очевидним є те, що `someAddress` ніколи не виконається, бо звернення до константи не має ніяких побічних ефектів. У прикладі коду нижче відбувається аналогічне присвоєння, однак для створення адреси використовується функція. Функція друкує повідомлення “Функцію викликано” перед поверненням значення, щоб було видно, чи викликався код справа від оператора `=`:

```swift
func createAddress() -> Address {
    print("Функцію викликано.")

    let someAddress = Address()
    someAddress.buildingNumber = "26"
    someAddress.street = "Хрещатик"

    return someAddress
}
orest.residence?.address = createAddress()
```

Оскільки код вище нічого не надрукує, видно, що функція `createAddress()` в ньому не викликається.

## Виклик методів через ланцюжок опціоналів

Для виклику методів на опціональних значеннях також можна використовувати ланцюжки опціоналів, при цьому можна перевірити, чи був виклик методу успішним, навіть якщо метод не повертає значення.

Метод `printNumberOfRooms()` класу `Residence` друкує поточне значення властивості `numberOfRooms`. Ось як виглядає цей метод:

```swift
func printNumberOfRooms() {
    print("Кількість кімнат дорівнює \(numberOfRooms)")
}
```

Даний метод не визначає типу, що повертається. Однак, функції та методи, що не повертають значення, неявно повертають тип `Void`, як описано в підрозділі [Функції, що не повертають значення]({% link _book/1_language_guide/5_functions.md %}#функції-що-не-повертають-значення). Це означає, що вони повертають значення `()`, або порожній кортеж.

Якщо викликати даний метод на опціональному значенні за допомогою ланцюжку опціоналів, типом, що повертає цей метод буде `Void?`, а не `Void`, оскільки при виклику методу через ланцюжок опціоналів значення, що повертається, зажди буде загорнутим в опціонал. Це дозволяє використовувати інструкцію `if` для перевірки, чи було викликано метод `printNumberOfRooms()`, попри те, що цей метод сам по собі не повертає значення. Можна перевірити значення, що повертається методом `printNumberOfRooms()` на `nil` і побачити, чи був успішним цей виклик:

```swift
if john.residence?.printNumberOfRooms() != nil {
    print("Було надруковано кількість кімнат.")
} else {
    print("Неможливо надрукувати кількість кімнат.")
}
// Надрукує "Неможливо надрукувати кількість кімнат."
```

Це ж саме працює і для спроби присвоїти значення властивості через ланцюжок опціоналів. У прикладі вище в підрозділі [Доступ до властивостей через ланцюжок опціоналів]({% link _book/1_language_guide/15_optional_chaining.md %}#доступ-до-властивостей-через-ланцюжок-опціоналів) є спроба присвоїти значення адреси для `orest.residence`, незважаючи на те, що властивість `residence` дорівнює `nil`. Будь-яка спроба присвоїти значення властивості через ланцюжок опціоналів поверне значення типу `Void?`, що дозволяє дізнатись, чи було присвоєння успішним, перевіривши результат на `nil`:

```swift
if (john.residence?.address = someAddress) != nil {
    print("Адресу було присвоєно.")
} else {
    print("Неможливо присвоїти адресу.")
}
// Надрукує "Неможливо присвоїти адресу."
```

## Доступ до індексів через ланцюжок опціоналів

Через ланцюжок опціоналів можна отримувати та присвоювати значення через індекс на опціональному значенні, і перевіряти, чи була ця дія успішною.

> **Примітка**
>
> Щоразу при доступі до індексу на опціональному значенні через ланцюжок опціоналів, слід ставити знак питання _перед_ квадратними дужками індексу, а не після. Знак питання ланцюжку опціоналів має завжди йти одразу після частини виразу, що є опціональною.

У прикладі нижче відбувається спроба отримати назву першої кімнати в масиві `rooms` властивості `orest.residence` за допомогою індексу, оголошеному в класі `Residence`. Оскільки `orest.residence` в даний момент має значення `nil`, виклик індексу буде невдалим:

```swift
if let firstRoomName = orest.residence?[0].name {
    print("Навзою першої кімнати є \(firstRoomName).")
} else {
    print("Неможливо отримати назву першої кімнати.")
}
// Надрукує "Неможливо отримати назву першої кімнати."
```

Знак питання ланцюжку опціоналів у індексі йде одразу після `orest.residence`, перед квадратними дужками індексу, оскільки `orest.residence` є опціональним значенням, на якому застосовується ланцюжок опціоналів.

Аналогічно, можна спробувати присвоїти нове значення за допомогою індексу через ланцюжок опціоналів:

```swift
orest.residence?[0] = Room(name: "Ванна")
```

Присвоєння за допомогою індексу буде невдалим, оскільки властивість `residence` у даний момент дорівнює `nil`.

Якщо створити екземпляр `Residence` та присвоїти його властивості `orest.residence`, з одним або більше екземпляром `Room` в його масиві `rooms`, можна буде використовувати індекс `Residence` для доступу до фактичних елементів у масиві `rooms` через ланцюжок опціоналів:

```swift
let orestsHouse = Residence()
orestsHouse.rooms.append(Room(name: "Вітальня"))
orestsHouse.rooms.append(Room(name: "Кухня"))
orest.residence = orestsHouse

if let firstRoomName = orest.residence?[0].name {
    print("Навзою першої кімнати є \(firstRoomName).")
} else {
    print("Неможливо отримати назву першої кімнати.")
}
// Надрукує "Навзою першої кімнати є Вітальня."
```

### Доступ до індексів опціонального типу

Якщо індекс повертає значення опціонального типу – як, наприклад, індекс ключа у типі `Dictionary` у Swift – слід ставити знак питання після закритої квадратної дужки для створення ланцюжку опціоналів з опціонального типу, що повертається:

```swift
var testScores = ["Василь": [86, 82, 84], "Петро": [79, 94, 81]]
testScores["Василь"]?[0] = 91
testScores["Петро"]?[0] += 1
testScores["Сергій"]?[0] = 72
// масив "Василь" тепер дорівнює [91, 82, 84]
// а масив "Петро" тепер дорівнює [80, 94, 81]
```

У прикладі вище оголошено словник на ім'я `testScores`, котрий містить дві пари ключ-значення, що ставлять у відповідність рядковому ключу масив цілочисельних значень, моделюючи результати тестів групи студентів. В даному прикладі за допомогою ланцюжку опціоналів першому елементу масиву оцінок студента `"Василь"` присвоєно значення `91`; перший елемент масиву оцінок студента `"Петро"` збільшено на `1`; відбувається спроба присвоїти перший елемент масиву оцінок студента `"Сергій"`. Перші два виклики є успішними, оскільки словник `testScores` містить значення для ключів `"Василь"` та `"Петро"`. Третій виклик є невдалим, оскільки у словнику `testScores` немає елементу з ключем `"Сергій"`.

## Зв'язування кількох рівнів ланцюжків опціоналів

Кілька рівнів ланцюжків опціоналів можна поєднати разом, щоб спуститись до властивостей, методів та індексів, що лежать глибоко всередині моделі. Однак, кілька рівнів ланцюжку опціоналів не додає кількох рівнів опціональності значення, що повертається.

Іншими словами:

* Якщо тип, що повертається, не є опціональним, результат буде опціональним через використання ланцюжку опціоналів.
* Якщо тип, що повертається, _уже_ є опціональним, він не стане _більш_ опціональним через використання ланцюжку опціоналів.

Таким чином:

* Якщо звертатись до значення типу `Int` через ланцюжок опціоналів, завжди повертатиметься значення типу `Int?`, і нема різниці, скільки рівнів ланцюжка було використано. 
* Аналогічно, якщо звертатись до значення типу `Int?` через ланцюжок опціоналів, завжди повертатиметься значення типу `Int?`, скільки б не було використано рівнів ланцюжка.

У прикладі далі відбувається звернення до властивості `street` властивості `address` властивості `residence` змінної `orest`. Тут використовуються _два_ рівні ланцюжку опціоналів, щоб пройтись по властивостях `residence` та `address`, кожна з яких має опціональний тип:

```swift
if let orestsStreet = orest.residence?.address?.street {
    print("Орест живе на вулиці \(orestsStreet).")
} else {
    print("Неможливо отримати адресу.")
}
// Надрукує "Неможливо отримати адресу."
```

Значення `orest.residence` наразі містить конкретний екземпляр `Residence`. Однак, поточне значення `orest.residence.address` дорівнює `nil`. Через це, виклик `orest.residence?.address?.street` є невдалим.

Зауважимо, що у прикладі вище йде спроба отримати значення властивості `street`. Типом цієї властивості є `String?`. Значення, що повертає вираз `orest.residence?.address?.street` є таким чином теж `String?`, хоч на додачу до опціонального типу властивості й застосовуються кілька рівнів ланцюжку опціоналів.

Якщо присвоїти екземпляр `Address` як значення `orest.residence.address`, та присвоїти фактичне значення властивості `street` адреси, можна отримати значення властивості `street` через багаторівневий ланцюжок опціоналів:

```swift
let orestsAddress = Address()
orestsAddress.buildingName = "Дім з химерами"
orestsAddress.street = "Банкова"
orest.residence?.address = orestsAddress

if let orestsStreet = orest.residence?.address?.street {
    print("Орест живе на вулиці \(orestsStreet).")
} else {
    print("Неможливо отримати адресу.")
}
// Надрукує "Орест живе на вулиці Банкова."
```

У цьому прикладі, спроба присвоїти властивість `address` властивості `orest.residence` буде вдалою, оскільки значення `orest.residence` в даний момент містить коректний екземпляр `Residence`.

## Ланцюжки опціоналів на методах з опціональним значенням, що повертається

У попередніх прикладах показано, як отримати значення властивості опціонального типу через ланцюжок опціоналів. Ланцюжок опціоналів також можна використовувати для виклику методу, що повертає опціональне значення, та, за потреби, подальших звернень до значення, що повертається.

В прикладі нижче викликається метод `buildingIdentifier()` класу `Address` через ланцюжок опціоналів. Цей метод повертає значення типу `String?`. Як описано вище, результуючий тип виклику цього методу після застосування ланцюжку опціоналів також є `String?`:

```swift
if let buildingIdentifier = orest.residence?.address?.buildingIdentifier() {
    print("Назва будинку Ореста - \(buildingIdentifier).")
}
// Надрукує "Назва будинку Ореста - Дім з химерами."
```

Якщо потрібно побудувати подальший ланцюжок опціоналів на значенні, що повертає метод, слід ставити знак питання _після_ дужок виклику методу:

```swift
if let beginsWithThe =
    orest.residence?.address?.buildingIdentifier()?.hasPrefix("Дім") {
    if beginsWithThe {
        print("Назва будинку Ореста починається з \"Дім\".")
    } else {
        print("Назва будинку Ореста не починається з \"Дім\".")
    }
}
// Надрукує "Назва будинку Ореста починається з "Дім"."
```

> **Примітка**
>
> У прикладі вище, знак питання треба ставити _після_ круглих дужок, оскільки опціональним значенням, з якого будується ланцюжок, є значення, що повертає метод `buildingIdentifier()`, а не сам метод `buildingIdentifier()`.

