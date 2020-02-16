## Розширені оператори

Окрім операторів, які описані у розділі [Базові оператори](1_base_operators.md), Swift має певну кількість розширених операторів, для виконання складніших маніпуляції зі значеннями. Вони включають у себе побітові оператори та бітові зсуви, з якими ви можете бути знайомими з мови C та Objective-C.

Навідміну від арифметичних операторів у мові C, арифметичні оператори у Swift за замовчанням не дозволяють переповнення змінних. Щоб увімкнути поведінку переповнення, у Swift використовується другий набір арифметичних операторів, що за замовчанням дозволяють переповнення, як, наприклад, оператор додавання з переповненням (`&+`). Усі ці оператори з переповненням починаються із амперсанду (`&`).

При створення власних структур, класів та перечислень, буває корисною можливість створювати власні реалізації стандартних операторів Swift для цих власних типів. Swift дозволяє легко надавати спеціалізовані реалізації цих операторів та точно визначати поведінку для кожного з типів, що ви створюєте.

Ви не обмежені попередньо визначеними операторами. Swift дає свободу визначати ваші власні інфіксні, префіксні та постфіксні оператори, оператори присвоєння, із власною черговістю та асоціативністю. Ці оператори можнуть використовуватись у вашому коді, як і будь-які інші попередньо визначені оператори, і ви навіль можете розширювати існуючі типи для підтримки ваших власних операторів. 

### Побітові оператори

*Побітові оператори* дозволяють маніпулювати окремими бітами даних усередині структури даних. Вона часто використовуються для низькорівневого програмування, зокрема у програмуванні графіки та створенні драйверів. Побітові оператори можуть також стати у нагоді при роботі із сирими даними із зовнішніх джерел, наприклад при кодуванні та декодуванні даних для комунікації по власному протоколу.

Swift підтримує усі побітові оператори, що є у мові C, котрі описані нижче.

#### Оператор побітового НЕ

*Оператор побітового НЕ* (`~`) інвертує усі біти у числі:

![](images/bitwiseNOT_2x.png)

Оператор побітового НЕ є префіксним оператором, і пишеться безпосередньо перед значенням, над яким він оперує, без пробілу:

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // дорівнює 11110000
```

Цілі числа типу `UInt8` мають вісім біт та можуть зберігати значення від `0` до `255`. У цьому прикладі ініціалізовано ціле типу `UInt8` з бінарним значенням `00001111`, в якому перші чотири біти дорівнюють нулю, а другі чотири біти дорівнюють одиниці. Це є еквівалентом десяткового значенням `15`.

Оператор побітового НЕ далі використовується для створення нової константи на ім'я `invertedBits`, котра дорівнює `initialBits`, однак з інвертованими бітами. Нулі стають одиницями, а одиниці стають нулями. Значення `invertedBits` дорівнює `11110000`, що є еквівалентом беззнакового десяткового значення `240`.

#### Оператор побітового І

*Оператор побітового І*  (`&`) поєднує біти двох чисел. Він повертає нове число, чиї біти дорівнюють `1` лише тоді, коли відповідні біти дорівнюють `1` в *обох* вхідних числах.

![](images/bitwiseAND_2x.png)

У прикладі нижче, значення `firstSixBits` та `lastSixBits` мають чотири біти посередині, що дорівнюють `1`. Оператор побітового І поєднує їх для створення числа `00111100`, що є еквівалентом беззнакового десяткового значення `60`:

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits  // дорівнює 00111100
```

#### Оператор побітового АБО

*Оператор побітового АБО* (`|`) порівнює біти двох чисел. Оператор повертає нове число, чиї біти дорівнюють `1`  у позиціях, де біти *хоча б одного* із двох вхідних чисел дорівнюють `1`:

![](images/bitwiseOR_2x.png)

У прикладі нижче, значення `someBits` та `moreBits` мають різні біти, що дорівнюють `1`. Оператор побітового АБО поєднує їх для створення числа `11111110`, що є еквівалентом беззнакового десяткового числа `254`:

```swift
let someBits: UInt8 = 0b10110010
let moreBits: UInt8 = 0b01011110
let combinedbits = someBits | moreBits  // дорівнює 11111110
```

#### Оператор побітового виключного АБО (XOR)

*Оператор побітового виключного АБО*, або XOR (від “exclusive OR”) (^), поєднує біти двох чисел. Оператор повертає нове число, чиї біти дорівнюють `1` там, де біти вхідних чисел відрізняються, та `0` там, де біти вхідних чисел співпадають:

![](images/bitwiseXOR_2x.png)

У прикладі нижче, значення `firstBits` та `otherBits` мають одиничні біти у різних позиціях. Оператор побітового виключного АБО присвоює результату 1 у двох позиціях, котрі відрізняється. Решта біт `firstBits` та `otherBits` співпададють, і тому в цих позиціях результат має значення `0`:

```swift
let firstBits: UInt8 = 0b00010100
let otherBits: UInt8 = 0b00000101
let outputBits = firstBits ^ otherBits  // дорівнює 00010001
```

#### Оператори побітового зсуву вліво та вправо

*Оператор побітового зсуву вліво* (`<<`) та *оператор побітового зсуву вправо* (`>>`) пересувають усі біти числа вліво чи вправо на певну кількість позицій, відповідно до правил нижче.

Побітовий зсув уліво та вправо має той же ефект, що й множення та ділення цілого на степінь двійки. Зсув бітів цілого числа вліво на одиницю подвоює це число, в той час як зсув вправо ділить це число на два. 

##### Поведінка зсуву беззнакових цілих

Поведінка зсуву для беззнакових цілих є наступною:

1. Існуючі біти пересуваються вліво чи вправо на задану кількість позицій.
2. Усі біти, що зсуваються за межі сховища цілого числа відкидаються.
3. Вивільнене місце зліва чи справа після оригінальних бітів заповнюється нулями. 

Даний підхід є відомий як *логічний зсув*.

Ілюстрація нижче демонструє результати зсуву `11111111 << 1` (тобто число `11111111` зсунуте на 1 позицію вліво), та зсуву `11111111 >> 1` (тобто `11111111` зсунуте на 1 позицію вправо). Сині біти зсовуються, сірі – відкидаються, а помаранчеві – вставляються:

![](images/bitshiftUnsigned_2x.png)

Ось як побітовий зсув виглядає у коді мовою Swift:

```swift
let shiftBits: UInt8 = 4   // 00000100 у двійковій системі числення
shiftBits << 1             // 00001000
shiftBits << 2             // 00010000
shiftBits << 5             // 10000000
shiftBits << 6             // 00000000
shiftBits >> 2             // 00000001
```

Побітовий зсув можна використовувати для кодування та декодування значень інших типів даних:

```swift
let pink: UInt32 = 0xCC6699                   // рожевий колір у RGB
let redComponent = (pink & 0xFF0000) >> 16    // червона компонента дорівнює 0xCC, або 204
let greenComponent = (pink & 0x00FF00) >> 8   // зелена компонента дорівнює 0x66, або 102
let blueComponent = pink & 0x0000FF           // синя компонента дорівнює 0x99, або 153
```

У даному прикладі константа типу `UInt32` на ім'я `pink` використовується для зберігання рожевого кольору у форматі [CSS](https://uk.wikipedia.org/wiki/CSS). Значення кольору CSS `#CC6699` записується у Swift як шістнадцяткове число `0xCC6699`. Цей колір потім декомпонується на його червону (`СС`), зелену (`66`) та синю (`99`) компоненти за допомогою оператору побітового І (`&`) та оператору побітового зсуву вправо (`>>`). 

Червона компонента отримується за допомогою виконання побітового І між числами `0xCC6699` та `0xFF0000`. Нулі у `0xFF0000` фактично “маскують” другий та третій байти у `0xCC6699`, змушуючи частину `6699` бути проігнорованою та залишаючи результатом `0xCC0000`.

Далі це число зсовується на 16 позицій вправо (`>> 16`). Кожна пара символів у шістнадцятковому числі використовує 8 біт, тому зсув на 16 позицій вправо перетворить `0xCC0000` на `0x0000CC`. Це те ж саме, що число `0xCC`, котре в десятковому записі має вигляд “`204`”.

Аналогічно, зелена компонента отримується за допомого виконання побітового І між числами `0xCC6699` та `0x00FF00`, котрі дають вихідне значення `0x006600`. Це значення потім зсувається на 8 позицій вправо, що дає значення `0x66`, або `102` в десятковому записі.

Нарешті, синя компонента отримується за допомогою виконання побітового І між числами `0xCC6699` та `0x0000FF`, що дає вихідне значення `0x000099`. Немає потреби зсувати це число вправо, оскільки `0x000099` вже дорівнює `0x99`, або `153` в десятковому записі.

##### Поведінка зсуву знакових цілих

Поведінка зсуву для знакових цілих є складнішою ніж для беззнакових цілих, через спосіб представлення знакових цілих у бінарному вигляді. (Приклади нижче базуються на 8-бітних знакових цілих для простоти, однак ті ж само принципи стосуються знакових цілих будь-якого розміру).

У знакових цілих перший біт використовується для зберігання знаку числа, тому він носить назву “[знаковий біт](https://uk.wikipedia.org/wiki/Знаковий_біт)”  та визначає, чи є число додатнім або від'ємним. Якщо знаковий біт дорівнює `0` – число вважається додатнім, якщо `1` – від'ємним. 

Решта біт позначають величину числа (або абсолютне значення), тому їх називають *бітами значення*. Додатні числа зберігаються у той же спосіб, що й беззнакові цілі, рахуючи вгору від нуля. Ось як виглядають біти всередині числа `4` типу `Int8`:

![bitshiftSignedFour_2x](images/bitshiftSignedFour_2x.png)

Знаковий біт дорівнює `0` (означаючи “додатнє”), а сім бітів значення формують число 4, записане у бінарній формі.

Від'ємні числа зберігаються в інший спосіб. Вони зберігаються у вигляді їх абсолютного значення, віднятого від `2` в степені `n`, де `n` – це кількість бітів значення. Восьмибітне знакове число має сім бітів значення, тому використовується `2` в степені `7`, тобто `128`.

Ось як виглядають біти всередині числа `-4` типу `Int8`:

![bitshiftSignedMinusFour_2x](images/bitshiftSignedMinusFour_2x.png)

Цього разу, знаковий біт дорівнює `1` (означаючи “від'ємне”), а сім біт значення містять бінарне значення `124` (тобто `128 - 4`):

![bitshiftSignedMinusFourValue_2x](images/bitshiftSignedMinusFourValue_2x.png)

Це кодування ві'дємних чисел також відоме як [*доповняльний код*](https://uk.wikipedia.org/wiki/Доповняльний_код). Це може здаватись незвичниим способом представляти від'ємні числа, однак він має кілька переваг.

По-перше, щоб додати `-1` до `-4`, достатньо виконати стандартне бінарне додавання на всіх восьми бітах (включно із знаковим бітом), та відкинути все, що не вміщується у `8` біт по завершенню операції:

![bitshiftSignedAddition_2x](images/bitshiftSignedAddition_2x.png)

По-друге, у доповляльний код дозволяє зсовувати біти від'ємних чисел вліво та вправо аналогічно до додатніх чисел, все ще подвоюючи їх при кожному зсуві вліво, та розділюючи їх на два при кожному зсуві вправо. Щоб досягнути цього, вводиться додаткове правило, що використовується при зсуві знакових цілих вправо: при зсуві знакових цілих вправо, виконують ті ж правила, що й при зсуві беззнакових цілих, однак порожні біти зліва заповнюються значенням знакового біту замість нуля.

![bitshiftSigned_2x](images/bitshiftSigned_2x.png)

Ця дія гарантує, що знакові цілі мають той же знак після зсуву вправо, а цей різновид зсуву називають *арифметичним зсувом*.

Внаслідок особливого способу зберігання додатніх та від'ємних чисел, зсув як додатніх, так і від'ємних чисел вправо наближує їх до нуля. Зберігання знакового біту однаковим при такому зсуві дозволяє від'ємним цілим числам зберігати свій знак, рухаючись ближче до нуля.

### Оператори переповнення

Якщо спробувати вставити число у цілочисельну константу чи змінну, що не може втримати це значення, за замовчанням Swift повідомить про помилку, не дозволяючи створити це некоректне значення. Ця поведінка дає додаткову безпеку при роботі із нанадто великими чи занадто малими числами. 

Наприклад, цілочисельний тип `Int16` може зберігати знакові цілочисельні значення між `-32768` та `32767`. Спроба присвоїти константі чи змінній типу `Int16` значення за межами цього діапазону призведе до помилки:

```swift
var potentialOverflow = Int16.max
// potentialOverflow дорівнює 32767, що є максимальним значенням, яке може втримати Int16
potentialOverflow += 1
// це призводить до помилки
```

Забезпечення обробки помилок, коли значення стають занадто великими або занадто малими, дає вам набагато більше гнучкості при кодуванні для граничних умов.

Однак, коли вам спеціально потрібна поведінка переповнення, щоб обрізати число до доступних бітів, можна увімкнути її замість провокування помилок. Swift має три арифметичних оператори переповнення, що дозволяють поведінку переповнення для цілочисельних розрахунків. Ці оператори починаються із символу (`&`):

+ Додавання з переповненням (&+)
+ Віднімання з переповненням (&-)
+ Множення з переповненням (&*)

#### Переповнення значень

Числа можуть переповнюватись як у додатньому, так і у від'ємному напрямках.

Ось приклад того, що трапляється, коли беззнакове ціле переповнюється у додатньому напрямку, за допомогою оператору додавання з переповненням: (`&+`):

```swift
var unsignedOverflow = UInt8.max
// unsignedOverflow дорівнює 255, що є максимальним значенням типу UInt8
unsignedOverflow = unsignedOverflow &+ 1
// unsignedOverflow тепер дорівнює 0
```

Змінну `unsignedOverflow` ініціалізовано із максимальним значенням типу `UInt8` (`255`, або `11111111` у бінарному представленні). До нього додається число `1` за допомогою оператору додавання з переповненням (`&+`). Це збільшує бінарне представлення до розміру, що на 1 більше за розмір числа, що вміщається у типі `UInt8`, призводячи до переповнення його меж, як показано на діаграмі нижче. Значення, що вміщається в межах `UInt8` після переповнення є `00000000`, або нуль.

![overflowAddition_2x](images/overflowAddition_2x.png)

Щось аналогічне відбувається, коли беззнакове ціло переповнюється у від'ємному напрямку. Ось приклад використання оператору віднімання із переповненням (`&-`):

```swift
var unsignedOverflow = UInt8.min
// unsignedOverflow дорівнює 0, що є мінімальним значенням типу UInt8
unsignedOverflow = unsignedOverflow &- 1
// unsignedOverflow тепер дорівнює 255
```

Мінімальним значенням, що лежить в межах `UInt8` є нуль, або `00000000` у двійковому представленні. Якщо відняти `1` від `00000000` використовуючи оператор віднімання із переповненням (`&-`), число переповниться та обріжеться до `11111111`, або `255` у десятковому записі.

Переповнення також відбувається знаковими цілими. Додавання та віднімання знакових цілих відбувається у побітовий спосіб, зі знаковим бітом як частиною числа, що додається чи віднімається, як описано вище у підрозділі [Оператори побітового зсуву вліво та вправо](#Оператори-побітового-зсуву-вліво-та-вправо). 

```swift
var signedOverflow = Int8.min
// signedOverflow дорівнює -128, що є мінімальним значенням типу Int8
signedOverflow = signedOverflow &- 1
// signedOverflow тепер дорівнює 127
```

Мінімальним значенням типу `Int8` є `-128`, або `10000000` у бінарній формі. Віднімаючи `1` від цього бінарного числа за допомогою оператору з переповненням дає бінарне значення `01111111`, що переключає знаковий біт та дає додатнє `127`, максимальне додатнє значення, що може міститись у `Int8`.

![overflowSignedSubtraction_2x](images/overflowSignedSubtraction_2x.png)

Як знакові, так і беззнакові цілі при переповненні у додатньому напрямку переходять із максимального коректного цілого до мінімального, а при переповненні у від'ємному напрямку переходять із мінімального значення до максимального.

### Черговість та асоціативність

Черговість операторів дає одним операторам пріорітет над іншими; ці оператори застосовуються першими.

Осоціативність операторів визначає, як оператори однієї черговості групуються разом: зліва направо чи справа наліво. Слід думати про це як “вони асоційовані з виразом зліва від них,” або “вони асоційовані з виразом справа від них”.

Важливо пам'ятати про черговість кожного оператора та асоціатисність при визначенні порядку, у якому буде обчислено складний вираз. Наприклад, черговість операторів пояснює, чому наступний вираз дорівнює 17.

```swift
2 + 3 % 4 * 5
// це дорівнює 17
```

Якщо читати цей вираз строго зліва направо, можна очікувати, що вираз буде обчислено наступним чином:

+ 2 плюс 3 дорівнює 5
+ остача від ділення 5 на 4 дорівнює 1
+ 1 помножити на 5 дорівнює 5

Однак, фактична відповідь є 17, не 5. Оператори з вищою черговістю опрацьовуються перед операторами з нижчою черговістю. У Swift, як і у C, оператор остачі від ділення (`%`) та оператор множення мають вищу черговість, ніж оператор додавання (`+`). У результаті вони обидва опрацьовуються перед додаванням.

Однак, оператори остачі та множення мають однакову черговість по відношенню один до одного. Щоб зрозуміти порядок виконання обчислення, слід також узяти до уваги їх асоціативність. Як остача, так і множення є асоційованими із виразами зліва від них. Про це слід думати як про додавання неявних дужок довкола цих частин виразу, починаючи зліва:

```swift
2 + ((3 % 4) * 5)
```

`(3 % 4)` дорівнює `3`, тому це є еквівалентом:

```swift
2 + (3 * 5)
```

`(3 * 5)` дорівнює `15`, тому це є еквівалентом:

```swift
2 + 15
```

Дане обчислення видає остаточну відповідь 17.

Детальніше із операторами, що є у стандартній бібліотеці Swift, включно зі списком груп черговості операторів, та налаштувань асоціативності, можна ознайомитись за посиланням [Operator Declarations](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations).

> **Note**
>
> Правила черговості та асоціативності операторів у Swift’s є простішими та передбачуванішими, аніж аналогічні у мовах C та Objective-C. Однак, це означає, що вони не точно такі ж як у C-подібних мовах. При портуванні існуючого коду на Swift слід бути обережним та пересвідчуватись, що взаємодія між операторами зберігає бажану поведінку.

### Operator Methods

### Методи-оператори

Classes and structures can provide their own implementations of existing operators. This is known as *overloading* the existing operators.

The example below shows how to implement the arithmetic addition operator (+) for a custom structure. The arithmetic addition operator is a *binary operator* because it operates on two targets and is said to be *infix* because it appears in between those two targets.

The example defines a Vector2D structure for a two-dimensional position vector (x, y), followed by a definition of an *operator method* to add together instances of the Vector2D structure:

```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}

extension Vector2D {
    static func + (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y + right.y)
    }
}
```

The operator method is defined as a type method on Vector2D, with a method name that matches the operator to be overloaded (+). Because addition isn’t part of the essential behavior for a vector, the type method is defined in an extension of Vector2D rather than in the main structure declaration of Vector2D. Because the arithmetic addition operator is a binary operator, this operator method takes two input parameters of type Vector2D and returns a single output value, also of type Vector2D.

In this implementation, the input parameters are named left and right to represent the Vector2D instances that will be on the left side and right side of the + operator. The method returns a new Vector2D instance, whose x and y properties are initialized with the sum of the x and y properties from the two Vector2D instances that are added together.

The type method can be used as an infix operator between existing Vector2D instances:

```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector is a Vector2D instance with values of (5.0, 5.0)
```

This example adds together the vectors (3.0, 1.0) and (2.0, 4.0) to make the vector (5.0, 5.0), as illustrated below.

![vectorAddition_2x](images/vectorAddition_2x.png)

#### Prefix and Postfix Operators

The example shown above demonstrates a custom implementation of a binary infix operator. Classes and structures can also provide implementations of the standard unary operators. Unary operators operate on a single target. They are prefix if they precede their target (such as -a) and *postfix operators* if they follow their target (such as b!).

You implement a prefix or postfix unary operator by writing the prefix or postfix modifier before the func keyword when declaring the operator method:

```swift
extension Vector2D {
    static prefix func - (vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y)
    }
}
```

The example above implements the unary minus operator (-a) for Vector2D instances. The unary minus operator is a prefix operator, and so this method has to be qualified with the prefix modifier.

For simple numeric values, the unary minus operator converts positive numbers into their negative equivalent and vice versa. The corresponding implementation for Vector2D instances performs this operation on both the x and y properties:

```swift
let positive = Vector2D(x: 3.0, y: 4.0)
let negative = -positive
// negative is a Vector2D instance with values of (-3.0, -4.0)
let alsoPositive = -negative
// alsoPositive is a Vector2D instance with values of (3.0, 4.0)
```

#### Compound Assignment Operators

Compound assignment operators combine assignment (=) with another operation. For example, the addition assignment operator (+=) combines addition and assignment into a single operation. You mark a compound assignment operator’s left input parameter type as inout, because the parameter’s value will be modified directly from within the operator method.

The example below implements an addition assignment operator method for Vector2D instances:

```swift
extension Vector2D {
    static func += (left: inout Vector2D, right: Vector2D) {
        left = left + right
    }
}
```

Because an addition operator was defined earlier, you don’t need to reimplement the addition process here. Instead, the addition assignment operator method takes advantage of the existing addition operator method, and uses it to set the left value to be the left value plus the right value:

```swift
var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// original now has values of (4.0, 6.0)
```

> **Note**
>
> It isn’t possible to overload the default assignment operator (=). Only the compound assignment operators can be overloaded. Similarly, the ternary conditional operator (a ? b : c) can’t be overloaded.

### Оператори рівності

### Equivalence Operators

By default, custom classes and structures don’t have an implementation of the equivalence operators, known as the equal to operator (==) and not equal to operator (!=). You usually implement the == operator, and use the standard library’s default implementation of the != operator that negates the result of the == operator. There are two ways to implement the == operator: You can implement it yourself, or for many types, you can ask Swift to synthesize an implementation for you. In both cases, you add conformance to the standard library’s Equatable protocol.

You provide an implementation of the == operator in the same way as you implement other infix operators:

```swift
extension Vector2D: Equatable {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
}
```

The example above implements an == operator to check whether two Vector2D instances have equivalent values. In the context of Vector2D, it makes sense to consider “equal” as meaning “both instances have the same x values and y values”, and so this is the logic used by the operator implementation.

You can now use this operator to check whether two Vector2D instances are equivalent:

```swift
let twoThree = Vector2D(x: 2.0, y: 3.0)
let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
if twoThree == anotherTwoThree {
    print("These two vectors are equivalent.")
}
// Prints "These two vectors are equivalent."
```

In many simple cases, you can ask Swift to provide synthesized implementations of the equivalence operators for you. Swift provides synthesized implementations for the following kinds of custom types:

+ Structures that have only stored properties that conform to the Equatable protocol
+ Enumerations that have only associated types that conform to the Equatable protocol
+ Enumerations that have no associated types

To receive a synthesized implementation of ==, declare Equatable conformance in the file that contains the original declaration, without implementing an == operator yourself.

The example below defines a Vector3D structure for a three-dimensional position vector (x, y, z), similar to the Vector2D structure. Because the x, y, and z properties are all of an Equatable type, Vector3D receives synthesized implementations of the equivalence operators.

```swift
struct Vector3D: Equatable {
    var x = 0.0, y = 0.0, z = 0.0
}

let twoThreeFour = Vector3D(x: 2.0, y: 3.0, z: 4.0)
let anotherTwoThreeFour = Vector3D(x: 2.0, y: 3.0, z: 4.0)
if twoThreeFour == anotherTwoThreeFour {
    print("These two vectors are also equivalent.")
}
// Prints "These two vectors are also equivalent."
```

### Custom Operators

You can declare and implement your own custom operators in addition to the standard operators provided by Swift. For a list of characters that can be used to define custom operators, see [Operators]().

New operators are declared at a global level using the operator keyword, and are marked with the prefix, infix or postfix modifiers:

```swift
prefix operator +++
```

The example above defines a new prefix operator called +++. This operator does not have an existing meaning in Swift, and so it is given its own custom meaning below in the specific context of working with Vector2D instances. For the purposes of this example, +++ is treated as a new “prefix doubling” operator. It doubles the x and y values of a Vector2D instance, by adding the vector to itself with the addition assignment operator defined earlier. To implement the +++ operator, you add a type method called +++ to Vector2D as follows:

```swift
extension Vector2D {
    static prefix func +++ (vector: inout Vector2D) -> Vector2D {
        vector += vector
        return vector
    }
}

var toBeDoubled = Vector2D(x: 1.0, y: 4.0)
let afterDoubling = +++toBeDoubled
// toBeDoubled now has values of (2.0, 8.0)
// afterDoubling also has values of (2.0, 8.0)
```

#### Precedence for Custom Infix Operators

Custom infix operators each belong to a precedence group. A precedence group specifies an operator’s precedence relative to other infix operators, as well as the operator’s associativity. See [Precedence and Associativity](#Precedence-and-Associativity) for an explanation of how these characteristics affect an infix operator’s interaction with other infix operators.

A custom infix operator that is not explicitly placed into a precedence group is given a default precedence group with a precedence immediately higher than the precedence of the ternary conditional operator.

The following example defines a new custom infix operator called +-, which belongs to the precedence group AdditionPrecedence:

```swift
infix operator +-: AdditionPrecedence
extension Vector2D {
    static func +- (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y - right.y)
    }
}
let firstVector = Vector2D(x: 1.0, y: 2.0)
let secondVector = Vector2D(x: 3.0, y: 4.0)
let plusMinusVector = firstVector +- secondVector
// plusMinusVector is a Vector2D instance with values of (4.0, -2.0)
```

This operator adds together the x values of two vectors, and subtracts the y value of the second vector from the first. Because it is in essence an “additive” operator, it has been given the same precedence group as additive infix operators such as + and -. For information about the operators provided by the Swift standard library, including a complete list of the operator precedence groups and associativity settings, see [Operator Declarations](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations). For more information about precedence groups and to see the syntax for defining your own operators and precedence groups, see [Operator Declaration](../2_language_reference/06_declarations.md#Operator-Declarations).

> **Note**
>
> You do not specify a precedence when defining a prefix or postfix operator. However, if you apply both a prefix and a postfix operator to the same operand, the postfix operator is applied first.