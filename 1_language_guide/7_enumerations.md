## Перечислення

*Перечислення* визначають спільний тип для групи пов'язаних значень, та дозволяють працювати з цими значеннями в коді у типобезпечний спосіб. 

Якщо ви знайомі з мовою C, вам могло бути відомо, що перечислення в C присвоюють пов'язані імена набору цілочисельних значень. Перечислення у Swift є набагато більш гнучкі, а їх елементи не обов'язково повинні мати конкретні значення. Однак елементи пересислення можуть мати значення (відомі, як “сирі” значення), і ці значення не обов'язково повинні бути цілочисельного типу: це можуть бути рядки, символи, цілі чи числа з палавючою комою. 

Елементи перечислень також можуть визначати асоційовині значення будь-якого типу, що будуть зберігатись разом із кожним елементом, подібно до юніонів чи варіантів в інших мовах програмування. Можна визначити спільний набір елементів всередині одного перечислення, кожен з яких матиме різний набір асоційованих із ним значень певних типів.

Перечислення у Swift є самі по собі першокласними типами. Вони підтримують багато можливостей, які раніше традиційно підтримувилась лише класами, такі як властивсоті, що обчислються (щоб надавати додаткову інформацію про поточне значення перечислення), та методи екземплярів (щоб надавати функціональність, пов'язану зі значенням, котре представляє перечислення). Перечислення можуть також визначати ініціалізатори для задавання початкового значення елемента; для перечислень можна створювати розширення, щоб виводити їх функціональність за межі початкової реалізації; перечислення можуть підпорядковуватись до протоколів, щоб надавати стандартну функціональність.

Детальніше з цими можливостями можна ознайомитись у розділах [Властивості](9_properties.md), [Методи](10_methods.md), [Ініціалізація](13_initialization.md), [Розширення](20_extensions.md), та [Протоколи](21_protocols.md).

### Синтаксис перечислень

Щоб створити перечислення, слід використати ключове слово `enum` та помістити все оголошення в пару фігурних дужок:

```swiftenum <ЯкесьПеречислення> {    // тут йде визначення перечислення}
```

Ось приклад перечислення для чотирьох основних точок компасу:

```swiftenum CompassPoint {    case north    case south    case east    case west}
```

Значення, оголошені в перечисленні (такі як `north`, `south`, `east`, та `west`) є його *елементами перечислення*. Для оголошення нового елементу перечислення використовують ключове слово `case`.
> **Примітка**
> 
> Навідміну від C та Objective-C, елементам перечислень Swift при створенні не присвоюється цілочисельне значення за замовчанням. У прикладі вище, елементи перечислення `CompassPoint` – `north`, `south`, `east`, та `west` – не дорівнюватимуть неявно `0`, `1`, `2` та `3`, як це було б в C. Замість цього, елементи перечислення є повноцінними значеннями на власних правах, із явно визначеним типом – `CompassPoint`.

Кілька різних елементів перечислення можуть оголошуватись в одному рядку, розділені комою:
 
```swiftenum Planet {    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune}
```

Кожне оголошення перечислення визначає новий тип. Як і інші типи у Swift, їх назви (такі як `CompassPoint` та `Planet`) прийнято писати з великої літери. Бажано давати перечисленням назви в однині, а не у множині, щоб вони читались самоочевидним чином:

```swiftvar directionToHead = CompassPoint.west
// дослівдно: var напрямокРуху = ТочкаКомпасу.захід
```

Тип змінної `directionToHead` буде визначено з того, що вона ініціалізується одним із можливих значень типу `CompassPoint`. Після того, як змінну `directionToHead` оголошено як змінну типу `CompassPoint`, їй можна присвоїти нове значення типу `CompassPoint` за допомогою кототшого синтаксису, через точку:

```swiftdirectionToHead = .east
```

Тип змінної `directionToHead` вже відомий, і тому  можна пропустити ім'я типу при присвоєнні нового значення. Це робить код набагато легшим для читання при роботі з явно типізованими значеннями перечислень.

### Визначення елементу пречислення за допомогою інструкції Switch

Визначити елементи перечислення можна за допомогою інструкції `switch` наступним чином:

```swiftdirectionToHead = .southswitch directionToHead {case .north:    print("На багатьох планетах є північ")case .south:    print("Стережіться пінгвінів")case .east:    print("Тут встає сонце")case .west:    print("Тут небо синє")}// Надрукує "Стережіться пінгвінів"
```

Цей код можна прочитати так:

“Розглядаємо значення змінної `directionToHead`. Якщо вона дорівнює `.north`, друкуємо `"На багатьох планетах є північ"`. Якщо вона дорівнює `.south`, друкуємо `"Стережіться пінгвінів"`.”…і так далі.

Як описано в розділі [Потік керування](4_control_flow.md), інструкція `switch` повинна бути вичерпною при розгляді елементів перечислення. Якщо пропистити випадок для елементу `.west`, код не скомпілюється, бо він не розглядатиме повного списку елементів `CompassPoint`. Вимога вичерпності гарантує, що елементи перечислення не будуть пропучені випадково.

Якщо в даному випадку недоречно створювати окремий випадок для кожного можливого елементу перечислення, можна створити випадок за замовчанням `default` для покриття всіх елементів, які не оброблено явно:

```swiftlet somePlanet = Planet.earth		// Планета.земляswitch somePlanet {case .earth:    print("Здебільшого, нешкідлива")default:    print("Небезпечне місце для людей")}// Надрукує "Здебільшого, нешкідлива"и
```### Асоційовані значення

У прикладах в попередньому підрозділі показано, як елементи перечислення визначено (і типізовано) як повноправне значення. Константі чи змінній можна присвоїти значення `Planet.earth`, і звірити це значення пізніше. Однак, іноді буває корисно мати можливсіть зберігати *асоційовані значення* інших типів разом із елементами перечислень. Це дозволяє зберігати власну додаткову інформацію разом із елементом перечислення, і дозволяє цій інформації змінюватись при кожному використанні цього елементу перечислення у коді. У Swift можна оголошувани перечислення для зберігання асоційованих значень будь-якого заданого типу, і типи цих значень можуть за потреби бути різними для кожного елементу перечислення. Перечислення тикого виду відомі як *розмічені об'єднання*, *тип-сума* або *варіант* в інших мовах програмування.

Наприклад, припустимо, що система інвентаризації магазину потребує, щоб товари відслідковувались за допомогою одного з двох видів штрих-кодів. Одні товари маруються одномірним штрих-кодом в форматі UPC, що використовує цифри від `0` до `9`. Кожен штрих-код містить цифру, що кодує “систему числення”, після цього йде п'ять цифр “коду виробника” та п'ять цифр “коду товарум. Останньою є “перевірочна” цифра, котра потрібна для підтвердження правильності зчитування коду:

![](images/barcode_UPC_2x.png)
￼
Інші товари маркуються двовимірним штрих-кодом у форматі QR-коду, що може використовувати будь-які символи в кодуванні ISO 8859-1, та може кодувати рядок довжиною до 2953 символів.

![](images/barcode_QR_2x.png)


Для системи інвентаризації було б зручно мати можливість зберігати штрих-коди UPC як кортеж із чотирьох цілих чисел, а QR-коди - як рядки довільної довжини.￼
У Swift, можна створити перечислення, котре може зберігати штрих-коди обох типів:

```swiftenum Barcode {    case upc(Int, Int, Int, Int)    case qrCode(String)}
```

Це можна прочитати так:

“Оголошуємо тип-перечислення із назвою `Barcode`, котрий може приймати або значення `upc` з асоційованим значенням типу `(Int, Int, Int, Int)`, або значення `qrCode` з асоційованим значенням типу `String`.”

Це оголошення не надає дожного фактиного значення `Int` чи `String` – воно лише визначає *тип* асоційованих значень, що константи чи змінні типу `Barcode` можуть зберігати, коли вони дорівнюють `Barcode.upc` чи `Barcode.qrCode`.

Нові значення штрих-кодів можна створювати, шляхом використання одного із елементів перечислення:

```swiftvar productBarcode = Barcode.upc(8, 85909, 51226, 3)
```

У цьому прикладу створено нову змінну на ім'я `productBarcode`, і їй присвоєно значення `Barcode.upc` з асоційованим значенням кортежу `(8, 85909, 51226, 3)`.

Той же товар може мати штрих-код іншого типу:

```swiftproductBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

На даному етапі, початкове значення `Barcode.upc` та його цілочисельні значення було замінено новим значенням `Barcode.qrCode` та рядком. Константи та змінні типу `Barcode` можуть зберігати або значення `.upc`, або значення `.qrCode` (разом із їх асоційованими значеннями), але вони не можуть зберігати обидва значення одночасно. 

Різні види штрих-кодів модуть визначатись за допомогою інструкції `switch`, як і раніше. Цього разу, однак, в інструкції `switch` можна витягувати асоційовані значення. Кожне з асоційованих значень можна витягнути у вигляді константи (за допомогою префіксу `let`) чи змінної (за допомогою префіксу `var`) для використання в тілі випадку інструкції `switch`:

```swiftswitch productBarcode {case .upc(let numberSystem, let manufacturer, let product, let check):    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")case .qrCode(let productCode):    print("QR: \(productCode).")}// Надрукує "QR: ABCDEFGHIJKLMNOP."
```

Якщо всі асоційовані значення елементу пересислення витягуються як константи, або якщо всі вони витягуються як змінні, для лаконічності можна вставляти лише одине ключове слово `let` чи `var` перед іменем цього елементу:

```swiftswitch productBarcode {case let .upc(numberSystem, manufacturer, product, check):    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")case let .qrCode(productCode):    print("QR: \(productCode).")}// Надрукує "QR: ABCDEFGHIJKLMNOP."
```

### Сирі значення

Приклад зі штрих-кодами в підрозділі [Асоційовані значення](7_enumerations.md#Асоційовані-значення) вище демонструє, як випадки перечислення можуть оголошувати, що вони зберігають асоційовані значення різних типів. Альтернативою асоційованим значенням є *сирі значення* - це значення, які відповідають елементам перечислення. При цьому для всіх елемнтів сирі значення повинні мати однаковий тип. 

Ось приклад перечислення, у якому разом з елементами перечислення зберігаються сирі тестові символи з кодування ASCII:

```swiftenum ASCIIControlCharacter: Character {    case tab = "\t"    case lineFeed = "\n"    case carriageReturn = "\r"}
```

Тут визначено, що перечислення `ASCIIControlCharacter` має сирі значення типу `Character`, а його елементам відповідають деякі з досить уживаних контрольних символів з кодування ASCII. Детальніше текстові символи `Character` були описані в розділі [Рядки та символи](2_strings_and_characters.md).

Сирим значенням може бути рядок, символи, чи будь-який з цілочисельних типів або типів чисел з плаваючою комою. Сирі значення в оголошенні перечислення не можуть повторюватись.
> **Примітка**
> 
> Сирі значення *не є* тип же само, що асоційоване значення. Сирі значення заповнюються одразу при їх оголошенні в перечисленні, як три коди ASCII вище. Сирі значення для певного елементу перечислення є завжди однаковими. Асоційовані значення задаються при створенні нової константи чи змінної з одного з елементів перечислення, і може бути щоразу різним.

#### Неявно присвоєні сирі значення

При роботі з перечисленнями, що зберігають цілочисельні чи рядкові сирі значення, не обов'язково завжди явно вказувати сире значення для кожного елементу. Якщо не вказувати їх, Swift автоматично прив'яже ці значення за вас. 

Наприклад, для цілочисельних сирих значень, неявне сире значення для кожного наступного елемента на одиницю більше за сире значення попереднього елемента. Якщо для першого елемента не вказано сирого значення, Swift прийме його значення за `0`.

Перечислення нижче є вдосконаленням попереднього перечислення `Planet`, з цілочисельними сирими значеннями, що преставляють порядок планети від сонця:

```swiftenum Planet: Int {    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune}
```

У прикладі вище, елемент `Planet.mercury` має явне сире значення `1`, елемент `Planet.venus` має неявне сире значення `2`, і так далі.

Коли сирим значенням є рядок, неявним значенням для кожного елементу є текст назви самого елемента.

Перечислення нижче є вдосконаленням попереднього перечислення `CompassPoint`, із сирим значенням типу `String`, що представляє назву кожного напрямку:

```swiftenum CompassPoint: String {    case north, south, east, west}
```

У прикладі вище, елемент `CompassPoint.south` має сире значення `"south"`, і так далі.

Отримати сире значення елементу перечислення можна за допомогою його властивості `rawValue`:

```swiftlet earthsOrder = Planet.earth.rawValue// earthsOrder дорівнює 3 let sunsetDirection = CompassPoint.west.rawValue// sunsetDirection дорівнює "west"
```

#### Ініціалізація за допомогою сирого значення

Для перечислень із сирими значеннями автоматично створюються ініціалізатори, що приймають значення типу сирого значення, і повертають або елемент перечислення, або `nil`. Цей ініціалізатор можна використовувати для створення нового екземпляру перечислення.

Наступний приклад демонструє створення елементу, що відповідає планеті Уран, з числа `7`:

```swiftlet possiblePlanet = Planet(rawValue: 7)// possiblePlanet має тип Planet? та дорівнює Planet.uranus
```

Однак, не кожному можливому значенню `Int` відповідає планета з перечислення `Planet`. Через це, ініціалізатор сирого значення завжди повертає *опціональний* елемент перечислення. У прикладі вище, `possiblePlanet` має тип `Planet?`, або “опціональна `Planet`.”
> **Примітка**
> 
> The raw value initializer is a failable initializer, because not every raw value will return an enumeration case. For more information, see [Failable Initializers](2_language_reference/06_declarations.md/#Failable-Initializers).
 If you try to find a planet with a position of `11`, the optional `Planet` value returned by the raw value initializer will be `nil`:

```swiftlet positionToFind = 11if let somePlanet = Planet(rawValue: positionToFind) {    switch somePlanet {    case .earth:        print("Mostly harmless")    default:        print("Not a safe place for humans")    }} else {    print("There isn't a planet at position \(positionToFind)")}// Надрукує "There isn't a planet at position 11"
```
This example uses optional binding to try to access a planet with a raw value of `11`. The statement `if let somePlanet = Planet(rawValue: 11)` creates an optional `Planet`, and sets `somePlanet` to the value of that optional `Planet` if it can be retrieved. In this case, it is not possible to retrieve a planet with a position of `11`, and so the `else` branch is executed instead.### Recursive Enumerations
A *recursive enumeration* is an enumeration that has another instance of the enumeration as the associated value for one or more of the enumeration cases. You indicate that an enumeration case is recursive by writing `indirect` before it, which tells the compiler to insert the necessary layer of indirection.
For example, here is an enumeration that stores simple arithmetic expressions:

```swiftenum ArithmeticExpression {    case number(Int)    indirect case addition(ArithmeticExpression, ArithmeticExpression)    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)}
```
You can also write `indirect` before the beginning of the enumeration, to enable indirection for all of the enumeration’s cases that need it:

```swiftindirect enum ArithmeticExpression {    case number(Int)    case addition(ArithmeticExpression, ArithmeticExpression)    case multiplication(ArithmeticExpression, ArithmeticExpression)}
```
This enumeration can store three kinds of arithmetic expressions: a plain number, the addition of two expressions, and the multiplication of two expressions. The `addition` and `multiplication` cases have associated values that are also arithmetic expressions — these associated values make it possible to nest expressions. For example, the expression `(5 + 4) * 2` has a number on the right hand side of the multiplication and another expression on the left hand side of the multiplication. Because the data is nested, the enumeration used to store the data also needs to support nesting — this means the enumeration needs to be recursive. The code below shows the `ArithmeticExpression` recursive enumeration being created for `(5 + 4) * 2`:

```swiftlet five = ArithmeticExpression.number(5)let four = ArithmeticExpression.number(4)let sum = ArithmeticExpression.addition(five, four)let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))
```
A recursive function is a straightforward way to work with data that has a recursive structure. For example, here’s a function that evaluates an arithmetic expression:

```swiftfunc evaluate(_ expression: ArithmeticExpression) -> Int {    switch expression {    case let .number(value):        return value    case let .addition(left, right):        return evaluate(left) + evaluate(right)    case let .multiplication(left, right):        return evaluate(left) * evaluate(right)    }} print(evaluate(product))// Надрукує "18"
```
This function evaluates a plain number by simply returning the associated value. It evaluates an addition or multiplication by evaluating the expression on the left hand side, evaluating the expression on the right hand side, and then adding them or multiplying them.
