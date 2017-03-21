## Базові оператори


Оператор - це спеціальний символ або фраза, що вживається для перевірки, зміни чи об'єднання значень. Наприклад, оператор додавання (`+`) додає два числа, як   `let i = 1 + 2`, і оператор логічного "і" (`&&`) об'єднує два булевих значення, як `if enteredDoorCode && passedRetinaScan`.

Мова Swift підтримує більшість стандартних операторів мови C та покращує можливості деяких із них, щоб позбутись типових помилок програмування. Оператор присвоєння (`=`) не повертає значення, щоб недопустити його випадкове вживання замість оператора порівняння (`==`). Арифметичні оператори (`+`, `-`, `*`, `/`, `%` і так далі) виявляють і недопускають переповнення значення, щоб уникнути 
неочікуваних результатів під час роботи з числами, що стають більшими чи меншими, ніж найбільше чи найменше значення з діапазону значень типу, що їх зберігають. Переповнення змінних можна увімкнути за допомогою операторів переповнення мови Swift, як описано в розділі [Оператори переповнення](24_advanced_operators.md#оператори-переповнення).
Мова Swift також надає два оператори діапазонів (`a..<b` та `a...b`), котрих нема у мові C, як скорочення для оголошення діапазону значень. 

Даний розділ описує типові оператори у мові Swift. Розділ [Розширені оператори](24_advanced_operators.md) покриває розширені оператори мови Swift, а також описує способи створення власних операторів та реалізації стандартних операторів для власних типів. 

### Термінологія

Оператори бувають унарні, бінарні та тернарні: 

 + *Унарні* оператори оперують єдиним операндом (наприклад, `-a`). Унарні префіксні оператори ставляться безпосередньо перед їх операндами (наприклад, `!b`), а унарні постфіксні оператори ставляться безпосередньо після їх операндів (наприклад, `c!`).
	 + *Бінарні* оператори оперують двома операндами (наприклад, `2 + 3`), і є інфіксними, бо вони ставляться між їх операндами. + *Тернарні* оператори оперують трьома операндами. Як і мова C, Swift має лише один тернарний оператор, тернарний умовний оператор (`a ? b : c`).

Значення, на які діє оператор, називаються *операндами*. У виразі `1 + 2`, символ `+` є бінарним оператором, а його операндами є значення `1` і `2`.### Оператор присвоєння

Оператор *присвоєння* (`a = b`) ініціалізує чи оновлює значення `a` значенням  `b`:```swiftlet b = 10var a = 5a = b// a тепер дорівнює 10```

Якщо права сторона присвоєння є кортежем з кількома значеннями, його елементи 
розкладаються у кілька констант чи змінних одночасно: 
```swiftlet (x, y) = (1, 2)// x тепер дорівнює 1, а y тепер дорівнює 2```На відміну від оператору присвоєння у мовах C та Objective-C, оператор присвоєння у мові Swift сам по собі не повертає значення. Наступні інструкції некоректні:

```swiftif x = y {    // Це не коректно, бо x = y не повертає значення.}
```

Ця особливість унеможливлює випадкове використання оператору присвоєння (`=`) у випадках, коли мається на увазі оператор порівняння (`==`). Роблячи `if x = y` некоректним, мова Swift допомагає уникати помилок такого роду у коді.

### Арифметичні оператори

Мова Swift підтримує чотири стандартні *арифметичні оператори* для всіх числових типів: + Додавання (`+`) + Віднімання (`-`) + Множення (`*`) + Ділення (`/`)

```swift1 + 2       // дорівнює 35 - 3       // дорівнює 22 * 3       // дорівнює 610.0 / 2.5  // дорівнює 4.0
```

На відміну від арифметичних операторів у мовах C та Objective-C, арифметичні оператори мови Swift не дозволяють переповлення змінних за замовчуванням. Можна увімкнути переповнення за допомогою операторів переповнення мови Swift (як, наприклад, `a &+ b`). Детальніше з ними можна ознайомитись у розділі [Оператори переповнення](24_advanced_operators.md#оператори-переповнення).

Оператор додавання також підтримується для конкатенації рядків (`String`):```swift
"hello, " + "world"  // дорівнює "hello, world"
```
#### Оператор остачі

*Оператор остачі* (`a % b`) обчислює, скільки разів `b` поміститься в `a` і повертає значення, що залишиться (відоме також як остача).
> **Примітка**
> 
> Оператор остачі (`%`) також відомий як оператор модулю у інших мовах. Однак його поведінка у мові Swift для від'ємних значень значить більше відповідає саме оператору остачі, а не оператору модуля, якщо говорити строго.

Ось як працює оператор остачі. Щоб обчислити `9 % 4`, слід спочатку обчислити скільки разів `4` вміститься всередині `9`:￼
![](images/remainderInteger_2x.png)
Всередині `9` можна вмістити `4` два рази, а залишок буде `1` (показано оранжевим).У мові Swift, це можна описати як:```swift
9 % 4    // дорівнює 1
```

Щоб визначити відповідь для `a % b`, оператор `%` розв'язує наступне рівняння і повертає остачу як вихідне значення:`a` = (`b` x `якийсь множник`) + `остача`
де `якийсь множник ` - це найбільше число, що помножене на `b`не перевищує `a`.

Підставивши `9` та `4` у дане рівняння, отримаємо:
`9` = (`4` x `2`) + `1`Точно такий же метод застосовується для обчислення остачі для від'ємного значення `a`:

```swift-9 % 4   // дорівнює -1
```
Підставивши `-9` та `4` у рівняння, отримаємо:
`-9` = (`4` x `-2`) + `-1`звідки значення остачі дорівнює `-1`.

Знак числа `b` ігнорується для від'ємних значень `b`. Тобто `a % b` та `a % -b` завжди дають одну і ту ж відповідь.
#### Унарний мінус

Знак числового значення можна змінити за допомогою профіксу `-`, відомого як *оператор унарного мінусу*.
```swiftlet three = 3let minusThree = -three       // minusThree дорівнює -3let plusThree = -minusThree   // plusThree дорівнює 3, або "мінус мінус три"
```

Оператор унарного мінусу (`-`) додається прямо перед значенням, до якого він застосовується, без жодних пробілів. #### Унарний плюс
*Оператор унарного плюсу* (`+`) просто повертає значення, до якого він застосовується, без жодних змін:

```swiftlet minusSix = -6let alsoMinusSix = +minusSix  // alsoMinusSix дорівнює -6
```

Хоча унарний плюс фактично нічого не змінює, його доречно вживати, щоб надати симетрії коду для додатніх чисел, коли поруч вживається унарний мінус для від'ємних чисел. 
### Складені оператори присвоєння

Як і в мові C, у мові Swift є *складені оператори присвоєння*, що поєднують присвоєння (`=`) із іншою операцією. Одним із прикладів таких операторів є  *оператор додавання з присвоєнням* (`+=`):

```swiftvar a = 1a += 2// a тепер дорівнює 3
```

Вираз `a += 2` є скороченим записом `a = a + 2`. Фактично, додавання і присвоєння поєднано в один оператор, що виконує обидві задачі одночасно. 
> **Примітка**
> 
> Складені оператори присвоєння не повертають значення. Наприклад, не можна написати `let b = a += 2`. Повний список складених операторів присвоєння стандартної бібліотеки Swift можна знайти у [Standard Library Operators Reference](https://developer.apple.com/reference/swift/swift_standard_library_operators)
### Оператори порівняння

Мова Swift підтримує всі стандартні *оператори порівняння* мови C:
+ Дорівнює (`a == b`)+ Не дорівнює (`a != b`)+ Більше (`a > b`)+ Менше (`a < b`)+ Більше або дорівнює (`a >= b`)+ Менше або дорівнює (`a <= b`)> **Примітка**>
> Мова Swift також підтримує два *оператори тотожності* (`===` та `!==`), котрий слід вживати, що перевірити, що два об'єктних посилання посилаються на один і той же об'єкт. Детальніше це описано у розділі [Класи і структури](8_classes_and_structures.md).

Кожен оператор порівняння повертає значення типу `Bool` щоб вказати, чи істинний вираз:

```swift1 == 1   // true, істина, бо 1 дорівнює 12 != 1   // true, істина, бо 2 не дорівнює 12 > 1    // true, істина, бо 2 більше ніж 11 < 2    // true, істина, бо 1 менше ніж 21 >= 1   // true, істина, бо 1 більше або дорівнює 12 <= 1   // false, хиба, бо 2 не менше або дорівнює 1
```

Оператори порівняння часто використовуються в умовних інструкціях, таких як інструкція `if`:

```swiftlet name = "world"if name == "world" {    print("hello, world")} else {    print("I'm sorry \(name), but I don't recognize you")}// Друкує "hello, world", бо name звісно дорівнює "world".
```

Детальніше із інструкцією `if` можна ознайомитись у розділі [Потік керування](4_control_flow.md).

Можна також порівнювати кортежі, що мають однакову кількість значень, якщо кожну пару із значень можна порівняти. Наприклад, значення типу `Int` можуть порівнюватись, і значення типу `String` теж можуть порівнюватись, а тому і кортежі типу `(Int, String)` можуть порівнюватись. На відміну від них, значення типу `Bool` не можна порівнювати (тобто `true` < `false` не має сенсу), тому не можна порівнювати кортежі, що містять значення типу `Bool`.

Кортежі порівнюються зліва направо, по одній парі значеннь за раз, допоки не зустрінеться пара значень, що не дорівнюють одне одному. Ці два значення порівнюються, і результат цього порівняння визначає весь результат порівняння кортежів. Якщо всі елементи рівні, тоді і кортежі в свою чергу теж рівні. Наприклад: 

```swift(1, "zebra") < (2, "apple")   // істина, бо 1 менше ніж 2; "zebra" та "apple" не порівнюються(3, "apple") < (3, "bird")    // істина, бо 3 дорівнює 3, а "apple" менше ніж "bird"(4, "dog") == (4, "dog")      // істина, бо 4 дорівнює 4, а "dog" дорівнює "dog"
```

У прикладі вище, можна бачити як працює порівняння зліва направо у першому рядку. Оскільки `1` менше ніж `2`, кортеж `(1, "zebra")` вважається меншим, ніж `(2, "apple")`, незважаючи на інші значення у кортежі. Не має значення, що  `"zebra"` не менше ніж `"apple"`, тому що результат порівняння кортежів вже визначено по парі перших елементів кортежів. Однак, коли перші елементи кортежів дорівнюють одне одному, друга пара елементів *порівнюється* — що й відбувається у другому і третьому рядках.
> **Примітка**
> 
> Стандартна бібліотека мови Swift включає оператори порівняння кортежів із кількістю елементів до семи. Щоб порівнювати кортежі із сімома чи більше елементами, слід самому реалізувати оператори порівняння для них. 
### Тернарний умовний оператор
*Тернарний умовний оператор* є особливим оператором із трьома частинами, що має форму `питання ? відповідь1 : відповідь2`. Це скорочення від виконання одного із двох виразів в залежності від того, чи питання є `true` або `false`. Якщо питання є `true`, буде виконано `відповідь1` і повернуто відповідне значення; у іншому випадку, буде виконано `відповідь2` і повернуто її значення.Тернарний умовний оператор є скороченим записом наступного коду:

```swiftif питання {    відповідь1} else {    відповідь2}
```

Ось приклад, де підраховується висота рядку в таблиці. Висота рядку має бути на 50 точок вище ніж висота контенту, якщо у рядку є заголовок, і на 20 точок вище, якщо рядок не має заголовку: 

```swiftlet contentHeight = 40				// висота контентуlet hasHeader = true				// чи має рядок заголовокlet rowHeight = contentHeight + (hasHeader ? 50 : 20)// rowHeight дорівнює 90
```

Попередній приклад є скороченням від наступного коду:

```swiftlet contentHeight = 40let hasHeader = truelet rowHeight: Intif hasHeader {    rowHeight = contentHeight + 50} else {    rowHeight = contentHeight + 20}// rowHeight дорівнює 90
```

Використання тернарного умовного оператору в першому прикладі дає можливість задати правильне значення `rowHeight` одним рядком коду, що більше лаконічно, ніж код у другому прикладі. 

Тернарний умовний оператор надає ефективне скорочення, щоб вибрати, який із виразів обчислювати. Однак тернарних умовний оператор слід вживати обережно. При надмірному вживанні його стислість може призвезти до коду, який важко читати. Слід уникати поєднання кількох тернарних умовних операторів у одну складену інструкцію.  
### Оператор поглинання nil

*Оператор поглинання nil* (`a ?? b`) розгортає опціонал `a` якщо він містить значення, або повертає значення за замовчуванням `b`, якщо `a` є `nil`. Вираз `a` завжди має опціональний тип. Вираз `b` має мати тип, що співпадає з типом, що зберігається всередині `a`.
Оператор поглинання nil є скороченням наступного коду: 

```swifta != nil ? a! : b
```

Код вище використовує тернарний умовний оператор і примусове розгортання (`a!`) 
щоб отримати значення, загорнуте всередині коли `a` не `nil`, і повернути `b` у іншому випадку. Оператор поглинання nil надає більш елегантний спосіб інкупсулювати цю умовну перевірку та розгортання у лаконічну і легку для читання форму.> **Примітка**
> 
> Якщо значення `a` не `nil`, значення `b` не обчислюється. Це відомо як [обчислення за коротким обходом](https://en.wikipedia.org/wiki/Short-circuit_evaluation).

У приклад нижче оператор поглинання nil використовується, щоб вибрати між назвою кольору за замовчанням і назвою кольору, опціонально заданою користувачем:

```swiftlet defaultColorName = "red"var userDefinedColorName: String?   // nil за замовчуванням var colorNameToUse = userDefinedColorName ?? defaultColorName// userDefinedColorName дорівнює nil, тому colorNameToUse присвоєно колір за замовчуванням "red"
```

Змінну `userDefinedColorName` визначено як опціональний `String`, із значенням `nil` за замовчуванням. Оскільки змінна `userDefinedColorName` має опціональний тип, до неї можна застосувати оператор поглинання nil. У прикладі вище цей оператор вживається, щоб визначити початкове значення змінної `colorNameToUse` типу `String`. Оскільки `userDefinedColorName` містить `nil`, вираз `userDefinedColorName ?? defaultColorName` повертає значення `defaultColorName`, або `"red"`.

Якщо присвоїти певне значення, що не є `nil`, змінній `userDefinedColorName`, і провести операцію поглинання nil знову, значення загорнуте у опціонал `userDefinedColorName` буде використано замість значення за замовчуванням:

```swiftuserDefinedColorName = "green"colorNameToUse = userDefinedColorName ?? defaultColorName// userDefinedColorName не дорівнює nil, тому colorNameToUse присвоєно значення "green"
```
### Range Operators
Swift includes two *range operators*, which are shortcuts for expressing a range of values.
#### Closed Range Operator
The *closed range operator* (`a...b`) defines a range that runs from `a` to `b`, and includes the values `a` and `b`. The value of `a` must not be greater than `b`.The closed range operator is useful when iterating over a range in which you want all of the values to be used, such as with a `for`-`in` loop:

```swiftfor index in 1...5 {    print("\(index) times 5 is \(index * 5)")}// 1 times 5 is 5// 2 times 5 is 10// 3 times 5 is 15// 4 times 5 is 20// 5 times 5 is 25
```
For more on `for`-`in` loops, see [Потік керування](4_control_flow.md).#### Half-Open Range Operator
The *half-open range operator* (`a..<b`) defines a range that runs from `a` to `b`, but does not include `b`. It is said to be *half-open* because it contains its first value, but not its final value. As with the closed range operator, the value of `a` must not be greater than `b`. If the value of `a` is equal to `b`, then the resulting range will be empty.
Half-open ranges are particularly useful when you work with zero-based lists such as arrays, where it is useful to count up to (but not including) the length of the list:

```swiftlet names = ["Anna", "Alex", "Brian", "Jack"]let count = names.countfor i in 0..<count {    print("Person \(i + 1) is called \(names[i])")}// Person 1 is called Anna// Person 2 is called Alex// Person 3 is called Brian// Person 4 is called Jack
```
Note that the array contains four items, but `0..<count` only counts as far as `3` (the index of the last item in the array), because it is a half-open range. For more on arrays, see [Arrays](3_collection_types.md#масиви).

### Logical Operators
*Logical operators* modify or combine the Boolean logic values `true` and `false`. Swift supports the three standard logical operators found in C-based languages:
 + Logical NOT (`!a`) + Logical AND (`a && b`) + Logical OR (`a || b`) 
#### Logical NOT Operator
 The *logical NOT operator* (`!a`) inverts a Boolean value so that `true` becomes `false`, and `false` becomes true.
The logical NOT operator is a prefix operator, and appears immediately before the value it operates on, without any white space. It can be read as “not `a`”, as seen in the following example:

```swiftlet allowedEntry = falseif !allowedEntry {    print("ACCESS DENIED")}// Prints "ACCESS DENIED"
```
The phrase `if !allowedEntry` can be read as “if not allowed entry.” The subsequent line is only executed if “not allowed entry” is `true`; that is, if `allowedEntry` is `false`.As in this example, careful choice of Boolean constant and variable names can help to keep code readable and concise, while avoiding double negatives or confusing logic statements.
#### Logical AND Operator
The *logical AND operator* (`a && b`) creates logical expressions where both values must be `true` for the overall expression to also be `true`.
If either value is `афдыу`, the overall expression will also be `false`. In fact, if the first value is `false`, the second value won’t even be evaluated, because it can’t possibly make the overall expression equate to `true`. This is known as *short-circuit evaluation*.This example considers two `Bool` values and only allows access if both values are `true`:

```swiftlet enteredDoorCode = truelet passedRetinaScan = falseif enteredDoorCode && passedRetinaScan {    print("Welcome!")} else {    print("ACCESS DENIED")}// Prints "ACCESS DENIED"```
#### Logical OR Operator
The *logical OR operator* (`a || b`) is an infix operator made from two adjacent pipe characters. You use it to create logical expressions in which only *one* of the two values has to be `true` for the overall expression to be `true`.
Like the Logical AND operator above, the Logical OR operator uses short-circuit evaluation to consider its expressions. If the left side of a Logical OR expression is `true`, the right side is not evaluated, because it cannot change the outcome of the overall expression.
In the example below, the first `Bool` value (`hasDoorKey`) is `false`, but the second value (`knowsOverridePassword`) is `true`. Because one value is `true`, the overall expression also evaluates to `true`, and access is allowed:

```swiftlet hasDoorKey = falselet knowsOverridePassword = trueif hasDoorKey || knowsOverridePassword {    print("Welcome!")} else {    print("ACCESS DENIED")}// Prints "Welcome!"
```
#### Combining Logical OperatorsYou can combine multiple logical operators to create longer compound expressions:

```swiftif enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {    print("Welcome!")} else {    print("ACCESS DENIED")}// Prints "Welcome!"
```
This example uses multiple `&&` and `||` operators to create a longer compound expression. However, the `&&` and `||` operators still operate on only two values, so this is actually three smaller expressions chained together. The example can be read as:
If we’ve entered the correct door code and passed the retina scan, or if we have a valid door key, or if we know the emergency override password, then allow access.
Based on the values of `enteredDoorCode`, `passedRetinaScan`, and `hasDoorKey`, the first two subexpressions are `false`. However, the emergency override password is known, so the overall compound expression still evaluates to `true`.
> NoteThe Swift logical operators `&&` and `||` are left-associative, meaning that compound expressions with multiple logical operators evaluate the leftmost subexpression first.
#### Explicit Parentheses
It is sometimes useful to include parentheses when they are not strictly needed, to make the intention of a complex expression easier to read. In the door access example above, it is useful to add parentheses around the first part of the compound expression to make its intent explicit:

```swiftif (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {    print("Welcome!")} else {    print("ACCESS DENIED")}// Prints "Welcome!"
```
The parentheses make it clear that the first two values are considered as part of a separate possible state in the overall logic. The output of the compound expression doesn’t change, but the overall intention is clearer to the reader. Readability is always preferred over brevity; use parentheses where they help to make your intentions clear.