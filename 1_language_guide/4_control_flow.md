## Потік керуванняМова Swift надає багато різноманітних інструкцій потоку керування. Вони включають у себе цикли `while`, щоб виконувати задачу кілька разів; інструкції `if`, `guard`, та `switch` для розгалуження коду в залежності від певних умов; інструкції на кшталт `break` та `continue` для передачі керування до іншої точки у вашому коді. 

Мова Swift також надає цикл `for`-`in`, що спрощує ітерування масивів, словників, діапазонів, рядків та інших послідовностей. 

Інструкція `switch` у Swift значно потужніша за її аналоги у багатьох C-подібних мовах. Випадки в інструкції `switch` не переходять автоматично до наступного випадку в Swift, що унеможливлює типові помилки мови C спричинені пропущеною інструкцією `break`. Випадки можуть співпадати із багатьма різними шаблонами, включаючи потрапляння до інтервалу, кортежі, чи приведення до певних типів. При співпаданні значення у випадках `switch` може бути прив'язане до тимчасової константи чи змінної для використання у тілі випадку, а складні умови співпадання можуть виражатись за допомогою пункту `where` для кожного випадку.

### Цикл For-InЦикл `for`-`in` використовується для ітерування послідовності, такої як діапазон чисел, елементи масиву, чи символи в рядку.

У наступному прикладі друкуються перші кілька рядків таблиці множення на п'ять:

```swiftfor index in 1...5 {    print("\(index) × 5 = \(index * 5)")}// 1 × 5 = 5// 2 × 5 = 10// 3 × 5 = 15// 4 × 5 = 20// 5 × 5 = 25
```

Послідовність, що ітерується, є діапазоном чисел від `1` до `5` включно, що вказано за допомогою використання оператору закритого діапазону (`...`). Значення `index` встановлюється у перше число в діапазоні (`1`), і після цього виконуються інструкції всередині циклу. В цьому випадку, цикл містить одну інструкцію, котра друкує рядок із таблиці множення на п'ять для поточного значення `index`. Після виконання інструкції, значення `index` встановлюється у друге значення в діапазоні (`2`), і функція `print(_:separator:terminator:)` викликається знову. Цей процес триває допоки не буде досягнуто кінець діапазону.

У прикладі вище, `index` є константою, чиє значення автоматично встановлюється на початку кожної ітерації циклу. Константа `index` як така не зобов'язана бути оголошеною до використання. Вона неявно оголошена просто за допомогою входження її в оголошення циклу, без необхідності використання ключового слова `let`.

Якщо вам не потрібно кожне значення в послідовності, їх можна проігнорувати записавши символ підкреслення замість імені змінної.

```swiftlet base = 3let power = 10var answer = 1for _ in 1...power {    answer *= base}print("\(base) в степені \(power) дорівнює \(answer)")// Надрукує "3 в степені 10 дорівнює 59049"
```

У прикладі вище обчислюється значення числа `base` в степені `power` (в даному випадку, `3` в степені `10`). Початкове значення `1` (тобто `3` в степені `0`) перемножується на `3` десять разів, що вказано за допомогою закритого діапазону, від `1` до `10`. Для даного обчислення, поточне значення лічильника всередині циклу не потрібне, код просто виконує цикл потрібну кількість разів. Використання символу підкреслення (`_`) замість змінної циклу дозволяє ігнорувати окремі значення і не надавати доступ до поточного значення всередині кожної ітерації циклу.

Цикл `for`-`in` зручно використовувати для ітерування елементів масиву.

```swiftlet names = ["Карпо", "Мотря", "Лаврін", "Мелашка"]for name in names {    print("Привіт, \(name)!")}// Привіт, Карпо!// Привіт, Мотря!// Привіт, Лаврін!// Привіт, Мелашка!
```

Також можна ітерувати елементи словнику для доступу до пар ключ-значення. Під час ітерації словнику, кожен елемент із словника повертається як кортеж `(key, value)`, і ви можете робити декомпозицію елементів кортежу `(key, value)` у явно іменовані константи для використання їх у тілі циклу `for`-`in`. Тут, ключі словника декомпонуються у константу на ім'я `animalName`, а значення словника декомпонуються у константу на ім'я `legCount`.

```swiftlet numberOfLegs = ["павуки": 8, "мурахи": 6, "коти": 4]for (animalName, legCount) in numberOfLegs {    print("\(animalName) мають \(legCount) ніг")}// мурахи мають 6 ніг// павуки мають 8 ніг// коти мають 4 ніг
```

Елементи у словнику `Dictionary` не обов'язково ітеруватимуться в тому ж порядку, в якому їх було додано в словник. Вміст словнику по суті невпорядкований, тому їх ітерування не гарантує порядку, в якому з'являтимуться елементи словнику. Детальнішу інформацію про масиви і словники можна знайти в розділі [Колекції](3_collection_types.md).

### Цикли While

Цикли `while` виконують набір інструкцій, допоки умова на стане хибною (`false`). Ці типи циклів найкраще використовувати тоді, коли кількість ітерацій невідома до початку першої ітерації. У Swift є два види циклів `while`:

+ цикл `while` перевіряє умову на початку кожного проходження циклу.
+ цикл `repeat`-`while` перевіряє умову в кінці кожного проходження циклу.
#### While

Цикл `while` починається із перевірки єдиної умови. Якщо умова виконується (`true`), то виконується набір інструкцій до тих пір, поки умова не перестане виконуватись (`false`).

Ось загальна форма циклу `while`:

```swiftwhile <умова> {    <інструкції>}
```

Наступний приклад демонструє просту гру *[Ліла](https://uk.wikipedia.org/wiki/Ліла_(гра))* (також відому як *Змії і сходи*):

![](images/snakesAndLadders_2x.png)￼
Правила гри наступні:

 + Дошка має 25 клітин, і метою є потрапити на клітину 25 або за неї.
 + Під час кожного ходу, ви кидаєте гральні кості, і рухаєтесь на відповідне число клітин вперед, слідуючи горизонтальним шляхом відповідно до штрихпунктирної стрілки вище. 
 + Якщо хід закінчується на клітинці із нижньою частиною сходів, ви піднімаєтесь угору по цим сходам
 + Якщо хід закінчується на голові змії, ви спускаєтесь униз по цій змії

Дошку представлено масивом цілочисельних значень (`Int`). Її розмір визначається константою на ім'я `finalSquare`, котра використовується для ініціалізації масиву і для перевірки виграшу в умові далі в цьому прикладі. Дошка ініціалізується 26-ма нульовими значеннями `Int`, не 25-ма (по одному індексу від `0` до `25`). 

```swiftlet finalSquare = 25var board = [Int](repeating: 0, count: finalSquare + 1)
```

Деяким клітинкам після присвоюються конкретніші значення для позначення змій та сходів. Клітинки із початком сходів зберігають додатнє число: кількість клітинок, на яку дані сходи переносять гравця вперед; тоді як клітинки зі зміїними головами зберігають від'ємне число: кількість клітинок, на яку змії переносять гравця назад.

```swiftboard[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02.   // сходиboard[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08    // змії
```

Клітинка 3 містить початок сходів, що піднімає гравця на клітинку 11. Щоб представити це, третьому елементу `board[03]` присвоєно значення `8` (різниця між 3 та 11). Оператор унарного плюса (`+i`) використано для балансування оператору унарного мінуса (`-i`), а числа, менші за `10`, префіксуються незначущим нулем, щоб вирівняти всі інструкції по тексту. (Жоден із цих стилістичних прийомів не є строго обов'язковим, але вони роблять код значно акуратнішим).

Гравці починають рухатись із “нульової клітинки”, котра знаходиться поруч із нижній лівим кутом дошки. Перший кидок костей вводить гравця на дошку. 

```swiftvar square = 0		// поточна клітинкаvar diceRoll = 0		// поточне значення на гральних костяхwhile square < finalSquare {    // кидаємо кості:    diceRoll += 1    if diceRoll == 7 { diceRoll = 1 }    // рухаємось на кількість клітинок, що випала під час кидання костей:    square += diceRoll    if square < board.count {
        // якщо ми досі знаходимось на дошці, рухаємось вверх чи вниз у випадку сходів чи змії:        square += board[square]    }}print("Гру завершено!")
```

У прикладі вище використовується дуже простий підхід до кидання костей. Замість генерування випадкового числа, ми починаємо із нульового значення `diceRoll`. Кожної ітерації циклу `while` змінна `diceRoll` збільшується на `1`, та перевіряється, чи не стало це значення завеликим. Щоразу коли змінна `diceRoll` дорівнює `7`, значення костей стає завеликим і тому воно скидається назад до `1`. Таким чином послідовність значень `diceRoll` буде завжди `1`, `2`, `3`, `4`, `5`, `6`, `1`, `2`, і так далі.

Після кидання костей, гравець рухається вперед на `diceRoll` клітинок. Можливо, що в цей момент гравець прийшов за клітинку 25, що означає закінчення гри. Щоб покрити цей сценарій, код перевіряє, що змінна `square` менша за властивість `count` масиву `board` перед тим як додати значення, котре зберігається в `board[square]`, до поточного значення `square`, і таким чином пересунути гравця вверх чи вниз відповідно до поточних сходів чи змій.
> **Примітка**
> 
> Якби дана перевірка не виконувалась, у `board[square]` ми могли б звертатись до значення за межами масиву `board`, що призвело би до помилки. Якщо би змінна `square` дорівнювала б `26`, код намагався би перевірити значення `board[26]`, що більше за розмір цього масиву.

Поточна ітерація циклу `while` закінчується, і далі перевіряється умова, щоб дізнатись, що потрібна ще одна ітерація циклу. Якщо гравець прийшов на клітинку `25` або за неї, умова циклу обчислюється як `false`, а гра закінчується.

Цикл `while` є доречним у даному випадку, бо тривалість гри є невідомою на початок циклу `while`. Цикл триває до моменту виконання умови закінчення гри.
#### Цикли Repeat-While

Інша варіація циклу `while`, відома як цикл `repeat`-`while`, виконує одну ітерацію циклу *перед* тим, як перевіряє умову циклу. Після цього вона повторює ітерації циклу допоки умова не стане хибною (`false`).
> **Примітка**
> 
> Цикл `repeat`-`while` у Swift є аналогом циклу `do`-`while` у інших мовах.

Ось загальна форма циклу `repeat`-`while`:```swiftrepeat {    <інструкції>} while <умова>
```

Переглянемо ще раз приклад зі грою *Ліла*, переписавши його за допомгою циклу `repeat`-`while` замість `while`. Значення змінних `finalSquare`, `board`, `square`, та `diceRoll` ініціалізовано точно так само як і з циклом `while`.

```swiftlet finalSquare = 25var board = [Int](repeating: 0, count: finalSquare + 1)board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08var square = 0var diceRoll = 0
```

В цій версії гри, *першою* дією в циклі є перевірка на сходи чи змій. Жодні сходи у грі не ведуть гравця до клітинки 25, і тому неможливо виграти у грі, тільки рухаючись вверх по драбині. Таким чином, допустимо перевіряти на сходи чи змій першою дією в циклі.

На початку гри, гравець знаходиться у “нульовій клітинці. `board[0]` завжди дорівнює `0` і тому ніяк не впливає.

```swiftrepeat {    // рухаємось вверх чи вниз у випадку сходів чи змії:    square += board[square]    // кидаємо кості    diceRoll += 1    if diceRoll == 7 { diceRoll = 1 }    // рухаємось на кількість клітинок, що випала під час кидання костей:    square += diceRoll} while square < finalSquareprint("Гру завершено!")
```

Після того, як код перевіряє клітинку на сходи чи змії, кидаються кості, і гравець рухається вперед на `diceRoll` клітинок. Поточна ітерація циклу закінчується.

Умова циклу (`while square < finalSquare`) така ж як і в попередньому прикладі, але цього разу вона не перевіряється *допоки не закінчиться* перша ітерація циклу. Структура циклу `repeat`-`while` в прикладі вище краще підходить для даної гри, ніж структура циклу `while` в попередньому прикладі. В циклі `repeat`-`while` вище, `square += board[square]` завжди виконується *одразу після* того, як виконання умови циклу підтвердить, що `square` все ще знаходиться в межах ігрової дошки. Дана поведінка позбавляє необхідності перевіряти потрапляння в межі масиву, котра виконувалась в попередній версії цієї гри. 

### Інструкції умови

Дуже часто потрібно виконувати різні частини коду в залежності від певних умов. Вам може бути необхідно виконати додаткову частину коду у випадку помилки, або для виведення повідомлення, коли якесь значення стає занадто великим або малим. Для цього частини коду роблять *умовними*.

У мові Swift є два способи додавати умовні гілки коду: інструкція `if` та інструкція `switch`. Як правило, інструкція `if` використовується для перевірки простих умов із невеликою кількістю можливих результатів. Інструкція `switch` краще підходить для більш складних умов із багатьма можливими варіантами, і є корисною в ситуаціях, коли співпадання шаблону може допомогти обрати потрібну гілку коду для виконання. 
#### If

У найпростішій формі, інструкція `if` має єдину умову. Вона виконує набір інструкцій тільки тоді, коли значення умови обчислюється як `true`.

```swiftvar temperatureInCelsius = -1if temperatureInCelsius <= 0 {    print("Дуже холодно. Варто вдягнути шарф.")}// Надрукує "Дуже холодно. Варто вдягнути шарф."
```

Наступний приклад перевіряє, чи є значення температури менше або рівне 0°C (точці замерзання води). Якщо це так, буде надруковане повідомлення. В іншому випадку не буде надруковано жодного повідомлення, і виконання коду буде продовжено від закриваючої фігурної дужки. 

В інструкції `if` можна вписати альтернативний набір інструкцій, відомий як *пункт else*, для ситуацій, коли умова `if` обчислюється як `false`. Ці інструкції позначаються ключовим словом `else`.

```swifttemperatureInCelsius = 5if temperatureInCelsius <= 0 {    print("Дуже холодно. Варто вдягнути шарф.")} else {    print("Не так вже й холодно, вдягайте футболку.")}// Надрукує "Не так вже й холодно, вдягайте футболку."
```

Одна з цих гілок коду завжди буде виконана. Оскільки температура піднялась до 5°C, вже не достатньо холодно, щоб радити вдягнути шарф, і тому виконується гілка коду `else`.

Можна об'єднувати кілька інструкцій `if` у ланцюжок для розгляду додаткових умов.

```swift	temperatureInCelsius = 32if temperatureInCelsius <= 0 {    print("Дуже холодно. Варто вдягнути шарф.")} else if temperatureInCelsius >= 30 {    print("Спекотно. Не забудьте скористуватись сонцезахисним кремом.")} else {    print("Не так вже й холодно, вдягайте футболку.")}// Надрукує "Спекотно. Не забудьте скористуватись сонцезахисним кремом."
```

У прикладі вище, було додано додаткову інструкцію `if`, щоб реагувати на особливо високі температури. Фінальна гілка `else` залишається незмінною, але тепер вона друкує повідомлення для температур, що не є занадто високими чи занадто низькими.

Однак, фітальна гілка `else` не є обов'язковою, і може бути пропущеною, якщо набір умов не повинен бути повним. 

```swifttemperatureInCelsius = 22if temperatureInCelsius <= 32 {    print("Дуже холодно. Варто вдягнути шарф.")} else if temperatureInCelsius >= 86 {    print("Спекотно. Не забудьте скористуватись сонцезахисним кремом.")}
```

Оскільки температура не є ні занадто низькою, ні занадто високою, щоб спрацювала умова `if` чи `else if`, не буде надруковано жодного повідомення.
#### Switch
A `switch` statement considers a value and compares it against several possible matching patterns. It then executes an appropriate block of code, based on the first pattern that matches successfully. A `switch` statement provides an alternative to the `if` statement for responding to multiple potential states.In its simplest form, a `switch` statement compares a value against one or more values of the same type.

```swiftswitch <some value to consider> {case <value 1>:    <respond to value 1>case <value 2>,     <value 3>:    <respond to value 2 or 3>default:    <otherwise, do something else>}
```
Every `switch` statement consists of multiple possible *cases*, each of which begins with the `case` keyword. In addition to comparing against specific values, Swift provides several ways for each case to specify more complex matching patterns. These options are described later in this chapter.
Like the body of an `if` statement, each `case` is a separate branch of code execution. The `switch` statement determines which branch should be selected. This procedure is known as *switching* on the value that is being considered.
Every `switch` statement must be *exhaustive*. That is, every possible value of the type being considered must be matched by one of the `switch` cases. If it’s not appropriate to provide a case for every possible value, you can define a default case to cover any values that are not addressed explicitly. This default case is indicated by the `default` keyword, and must always appear last.This example uses a `switch` statement to consider a single lowercase character called `someCharacter`:

```swiftlet someCharacter: Character = "z"switch someCharacter {case "a":    print("The first letter of the alphabet")case "z":    print("The last letter of the alphabet")default:    print("Some other character")}// Надрукує "The last letter of the alphabet"
```
The `switch` statement’s first case matches the first letter of the English alphabet, `a`, and its second case matches the last letter, `z`. Because the `switch` must have a case for every possible character, not just every alphabetic character, this `switch` statement uses a `default` case to match all characters other than `a` and `z`. This provision ensures that the `switch` statement is exhaustive.
#### No Implicit Fallthrough
In contrast with `switch` statements in C and Objective-C, `switch` statements in Swift do not fall through the bottom of each case and into the next one by default. Instead, the entire `switch` statement finishes its execution as soon as the first matching `switch` case is completed, without requiring an explicit `break` statement. This makes the `switch` statement safer and easier to use than the one in C and avoids executing more than one `switch` case by mistake.> **Примітка**
> > Although `break` is not required in Swift, you can use a `break` statement to match and ignore a particular case or to break out of a matched case before that case has completed its execution. For details, see [Break in a Switch Statement]().
 The body of each case *must* contain at least one executable statement. It is not valid to write the following code, because the first case is empty:

```swiftlet anotherCharacter: Character = "a"switch anotherCharacter {case "a": // Invalid, the case has an empty bodycase "A":    print("The letter A")default:    print("Not the letter A")}// This will report a compile-time error.
```
Unlike a `switch` statement in C, this `switch` statement does not match both `"a"` and `"A"`. Rather, it reports a compile-time error that `case "a":` does not contain any executable statements. This approach avoids accidental fallthrough from one case to another and makes for safer code that is clearer in its intent.
To make a `switch` with a single case that matches both `"a"` and `"A"`, combine the two values into a compound case, separating the values with commas.

```swiftlet anotherCharacter: Character = "a"switch anotherCharacter {case "a", "A":    print("The letter A")default:    print("Not the letter A")}// Надрукує "The letter A"
```
For readability, a compound case can also be written over multiple lines. For more information about compound cases, see [Compound Cases]().
> **Примітка**
> > To explicitly fall through at the end of a particular switch case, use the `fallthrough` keyword, as described in [Fallthrough]().
 ##### Interval Matching
Values in `switch` cases can be checked for their inclusion in an interval. This example uses number intervals to provide a natural-language count for numbers of any size:

```swiftlet approximateCount = 62let countedThings = "moons orbiting Saturn"var naturalCount: Stringswitch approximateCount {case 0:    naturalCount = "no"case 1..<5:    naturalCount = "a few"case 5..<12:    naturalCount = "several"case 12..<100:    naturalCount = "dozens of"case 100..<1000:    naturalCount = "hundreds of"default:    naturalCount = "many"}print("There are \(naturalCount) \(countedThings).")// Надрукує "There are dozens of moons orbiting Saturn."
```
In the above example, `approximateCount` is evaluated in a `switch` statement. Each `case` compares that value to a number or interval. Because the value of `approximateCount` falls between 12 and 100, `naturalCount` is assigned the value `"dozens of"`, and execution is transferred out of the `switch` statement.##### Tuples
You can use tuples to test multiple values in the same `switch` statement. Each element of the tuple can be tested against a different value or interval of values. Alternatively, use the underscore character (`_`), also known as the wildcard pattern, to match any possible value.
The example below takes an (x, y) point, expressed as a simple tuple of type `(Int, Int)`, and categorizes it on the graph that follows the example.

```swiftlet somePoint = (1, 1)switch somePoint {case (0, 0):    print("(0, 0) is at the origin")case (_, 0):    print("(\(somePoint.0), 0) is on the x-axis")case (0, _):    print("(0, \(somePoint.1)) is on the y-axis")case (-2...2, -2...2):    print("(\(somePoint.0), \(somePoint.1)) is inside the box")default:    print("(\(somePoint.0), \(somePoint.1)) is outside of the box")}// Надрукує "(1, 1) is inside the box"```

![](images/coordinateGraphSimple_2x.png)
The `switch` statement determines whether the point is at the origin (0, 0), on the red x-axis, on the orange y-axis, inside the blue 4-by-4 box centered on the origin, or outside of the box.
Unlike C, Swift allows multiple `switch` cases to consider the same value or values. In fact, the point (0, 0) could match all *four* of the cases in this example. However, if multiple matches are possible, the first matching case is always used. The point (0, 0) would match `case (0, 0)` first, and so all other matching cases would be ignored.
##### Value Bindings
A `switch` case can bind the value or values it matches to temporary constants or variables, for use in the body of the case. This behavior is known as *value binding*, because the values are bound to temporary constants or variables within the case’s body.
The example below takes an (x, y) point, expressed as a tuple of type *(Int, Int)*, and categorizes it on the graph that follows:

```swiftlet anotherPoint = (2, 0)switch anotherPoint {case (let x, 0):    print("on the x-axis with an x value of \(x)")case (0, let y):    print("on the y-axis with a y value of \(y)")case let (x, y):    print("somewhere else at (\(x), \(y))")}// Надрукує "on the x-axis with an x value of 2"
```

![](images/coordinateGraphMedium_2x.png)
The `switch` statement determines whether the point is on the red x-axis, on the orange y-axis, or elsewhere (on neither axis).
The three `switch` cases declare placeholder constants `x` and `y`, which temporarily take on one or both tuple values from `anotherPoint`. The first case, `case (let x, 0)`, matches any point with a `y` value of `0` and assigns the point’s `x` value to the temporary constant `x`. Similarly, the second case, `case (0, let y)`, matches any point with an `x` value of `0` and assigns the point’s `y` value to the temporary constant `y`.
After the temporary constants are declared, they can be used within the case’s code block. Here, they are used to print the categorization of the point.
This `switch` statement does not have a `default` case. The final case, `case let (x, y)`, declares a tuple of two placeholder constants that can match any value. Because `anotherPoint` is always a tuple of two values, this case matches all possible remaining values, and a `default` case is not needed to make the switch statement exhaustive.
##### Where
A `switch` case can use a `where` clause to check for additional conditions.
The example below categorizes an (x, y) point on the following graph:

```swiftlet yetAnotherPoint = (1, -1)switch yetAnotherPoint {case let (x, y) where x == y:    print("(\(x), \(y)) is on the line x == y")case let (x, y) where x == -y:    print("(\(x), \(y)) is on the line x == -y")case let (x, y):    print("(\(x), \(y)) is just some arbitrary point")}// Надрукує "(1, -1) is on the line x == -y"
```

![](images/coordinateGraphComplex_2x.png)
The `switch` statement determines whether the point is on the green diagonal line where `x == y`, on the purple diagonal line where `x == -y`, or neither.
The three `switch` cases declare placeholder constants `x` and `y`, which temporarily take on the two tuple values from `yetAnotherPoint`. These constants are used as part of a `where` clause, to create a dynamic filter. The `switch` case matches the current value of `point` only if the where clause’s condition evaluates to `true` for that value.
As in the previous example, the final case matches all possible remaining values, and so a `default` case is not needed to make the `switch` statement exhaustive.
##### Compound CasesMultiple switch cases that share the same body can be combined by writing several patterns after `case`, with a comma between each of the patterns. If any of the patterns match, then the case is considered to match. The patterns can be written over multiple lines if the list is long. For example:

```swiftlet someCharacter: Character = "e"switch someCharacter {case "a", "e", "i", "o", "u":    print("\(someCharacter) is a vowel")case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",     "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":    print("\(someCharacter) is a consonant")default:    print("\(someCharacter) is not a vowel or a consonant")}// Надрукує "e is a vowel"
```
The `switch` statement’s first case matches all five lowercase vowels in the English language. Similarly, its second case matches all lowercase English consonants. Finally, the `default` case matches any other character.
Compound cases can also include value bindings. All of the patterns of a compound case have to include the same set of value bindings, and each binding has to get a value of the same type from all of the patterns in the compound case. This ensures that, no matter which part of the compound case matched, the code in the body of the case can always access a value for the bindings and that the value always has the same type.

```swiftlet stillAnotherPoint = (9, 0)switch stillAnotherPoint {case (let distance, 0), (0, let distance):    print("On an axis, \(distance) from the origin")default:    print("Not on an axis")}// Надрукує "On an axis, 9 from the origin"
```
The `case` above has two patterns: `(let distance, 0)` matches points on the x-axis and `(0, let distance)` matches points on the y-axis. Both patterns include a binding for `distance` and `distance` is an integer in both patterns — which means that the code in the body of the `case` can always access a value for `distance`.### Control Transfer Statements*Control transfer statements* change the order in which your code is executed, by transferring control from one piece of code to another. Swift has five control transfer statements:
 + continue + break + fallthrough + return + throw
 The `continue`, `break`, and `fallthrough` statements are described below. The `return` statement is described in [Функції](5_functions.md), and the `throw` statement is described in [Propagating Errors Using Throwing Functions](17_error_handling.md#Propagating-Errors-Using-Throwing-Functions).
#### Continue
The `continue` statement tells a loop to stop what it is doing and start again at the beginning of the next iteration through the loop. It says “I am done with the current loop iteration” without leaving the loop altogether.
The following example removes all vowels and spaces from a lowercase string to create a cryptic puzzle phrase:

```swiftlet puzzleInput = "great minds think alike"var puzzleOutput = ""let charactersToRemove: [Character] = ["a", "e", "i", "o", "u", " "]for character in puzzleInput.characters {    if charactersToRemove.contains(character) {        continue    } else {        puzzleOutput.append(character)    }}print(puzzleOutput)// Надрукує "grtmndsthnklk"
```
The code above calls the `continue` keyword whenever it matches a vowel or a space, causing the current iteration of the loop to end immediately and to jump straight to the start of the next iteration.
#### Break
The `break` statement ends execution of an entire control flow statement immediately. The `break` statement can be used inside a `switch` statement or loop statement when you want to terminate the execution of the `switch` or loop statement earlier than would otherwise be the case.
##### Break in a Loop StatementWhen used inside a loop statement, break ends the loop’s execution immediately and transfers control to the code after the loop’s closing brace (}). No further code from the current iteration of the loop is executed, and no further iterations of the loop are started.##### Break in a Switch Statement
When used inside a `switch` statement, `break` causes the `switch` statement to end its execution immediately and to transfer control to the code after the `switch` statement’s closing brace (`}`).
This behavior can be used to match and ignore one or more cases in a `switch` statement. Because Swift’s switch statement is exhaustive and does not allow empty cases, it is sometimes necessary to deliberately match and ignore a case in order to make your intentions explicit. You do this by writing the `break` statement as the entire body of the case you want to ignore. When that case is matched by the `switch` statement, the `break` statement inside the case ends the `switch` statement’s execution immediately.
> **Примітка**
> > A switch case that contains only a comment is reported as a compile-time error. Comments are not statements and do not cause a switch case to be ignored. Always use a `break` statement to ignore a `switch` case.
The following example switches on a `Character` value and determines whether it represents a number symbol in one of four languages. For brevity, multiple values are covered in a single `switch` case.

```swiftlet numberSymbol: Character = "三"  // Chinese symbol for the number 3var possibleIntegerValue: Int?switch numberSymbol {case "1", "١", "一", "๑":    possibleIntegerValue = 1case "2", "٢", "二", "๒":    possibleIntegerValue = 2case "3", "٣", "三", "๓":    possibleIntegerValue = 3case "4", "٤", "四", "๔":    possibleIntegerValue = 4default:    break}if let integerValue = possibleIntegerValue {    print("The integer value of \(numberSymbol) is \(integerValue).")} else {    print("An integer value could not be found for \(numberSymbol).")}// Надрукує "The integer value of 三 is 3."
```
This example checks `numberSymbol` to determine whether it is a Latin, Arabic, Chinese, or Thai symbol for the numbers `1` to `4`. If a match is found, one of the `switch` statement’s cases sets an optional `Int?` variable called `possibleIntegerValue` to an appropriate integer value.
After the switch statement completes its execution, the example uses optional binding to determine whether a value was found. The `possibleIntegerValue` variable has an implicit initial value of `nil` by virtue of being an optional type, and so the optional binding will succeed only if `possibleIntegerValue` was set to an actual value by one of the `switch` statement’s first four cases.
Because it’s not practical to list every possible `Character` value in the example above, a `default` case handles any characters that are not matched. This `default` case does not need to perform any action, and so it is written with a single `break` statement as its body. As soon as the default case is matched, the `break` statement ends the `switch` statement’s execution, and code execution continues from the `if let` statement.
##### FallthroughSwitch statements in Swift don’t fall through the bottom of each case and into the next one. Instead, the entire switch statement completes its execution as soon as the first matching case is completed. By contrast, C requires you to insert an explicit `break` statement at the end of every `switch` case to prevent fallthrough. Avoiding default fallthrough means that Swift `switch` statements are much more concise and predictable than their counterparts in C, and thus they avoid executing `multiple` switch cases by mistake.
If you need C-style fallthrough behavior, you can opt in to this behavior on a case-by-case basis with the `fallthrough` keyword. The example below uses `fallthrough` to create a textual description of a number.

```swiftlet integerToDescribe = 5var description = "The number \(integerToDescribe) is"switch integerToDescribe {case 2, 3, 5, 7, 11, 13, 17, 19:    description += " a prime number, and also"    fallthroughdefault:    description += " an integer."}print(description)// Надрукує "The number 5 is a prime number, and also an integer."
```
This example declares a new `String` variable called `description` and assigns it an initial value. The function then considers the value of `integerToDescribe` using a `switch` statement. If the value of `integerToDescribe` is one of the prime numbers in the list, the function appends text to the end of `description`, to note that the number is prime. It then uses the `fallthrough` keyword to “fall into” the `default` case as well. The `default` case adds some extra text to the end of the description, and the `switch` statement is complete.
Unless the value of `integerToDescribe` is in the list of known prime numbers, it is not matched by the first `switch` case at all. Because there are no other specific cases, `integerToDescribe` is matched by the `default` case.
After the `switch` statement has finished executing, the number’s description is printed using the `print(_:separator:terminator:)` function. In this example, the number `5` is correctly identified as a prime number.> **Примітка**
> > The `fallthrough` keyword does not check the case conditions for the switch case that it causes execution to fall into. The `fallthrough` keyword simply causes code execution to move directly to the statements inside the next case (or `default` case) block, as in C’s standard `switch` statement behavior.
 ### Labeled Statements
In Swift, you can nest loops and conditional statements inside other loops and conditional statements to create complex control flow structures. However, loops and conditional statements can both use the `break` statement to end their execution prematurely. Therefore, it is sometimes useful to be explicit about which loop or conditional statement you want a `break` statement to terminate. Similarly, if you have multiple nested loops, it can be useful to be explicit about which loop the `continue` statement should affect.
To achieve these aims, you can mark a loop statement or conditional statement with a *statement label*. With a conditional statement, you can use a statement label with the `break` statement to end the execution of the labeled statement. With a loop statement, you can use a statement label with the `break` or `continue` statement to end or continue the execution of the labeled statement.A labeled statement is indicated by placing a label on the same line as the statement’s introducer keyword, followed by a colon. Here’s an example of this syntax for a `while` loop, although the principle is the same for all loops and `switch` statements:

```swift<label name>: while <condition> {    <statements>}
```
The following example uses the `break` and `continue` statements with a labeled `while` loop for an adapted version of the *Snakes and Ladders* game that you saw earlier in this chapter. This time around, the game has an extra rule:
 + To win, you must land *exactly* on square 25.
If a particular dice roll would take you beyond square 25, you must roll again until you roll the exact number needed to land on square 25.
The game board is the same as before.

![](images/snakesAndLadders_2x.png)￼The values of `finalSquare`, `board`, `square`, and `diceRoll` are initialized in the same way as before:

```swiftlet finalSquare = 25var board = [Int](repeating: 0, count: finalSquare + 1)board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08var square = 0var diceRoll = 0
```
This version of the game uses a `while` loop and a `switch` statement to implement the game’s logic. The `while` loop has a statement label called `gameLoop` to indicate that it is the main game loop for the Snakes and Ladders game.
The `while` loop’s condition is `while square != finalSquare`, to reflect that you must land exactly on square 25.

```swiftgameLoop: while square != finalSquare {    diceRoll += 1    if diceRoll == 7 { diceRoll = 1 }    switch square + diceRoll {    case finalSquare:        // diceRoll will move us to the final square, so the game is over        break gameLoop    case let newSquare where newSquare > finalSquare:        // diceRoll will move us beyond the final square, so roll again        continue gameLoop    default:        // this is a valid move, so find out its effect        square += diceRoll        square += board[square]    }}print("Game over!")
```
The dice is rolled at the start of each loop. Rather than moving the player immediately, the loop uses a `switch` statement to consider the result of the move and to determine whether the move is allowed:

 + If the dice roll will move the player onto the final square, the game is over. The `break gameLoop` statement transfers control to the first line of code outside of the `while` loop, which ends the game.  + If the dice roll will move the player beyond the final square, the move is invalid and the player needs to roll again. The `continue gameLoop` statement ends the current `while` loop iteration and begins the next iteration of the loop.
 + In all other cases, the dice roll is a valid move. The player moves forward by `diceRoll` squares, and the game logic checks for any snakes and ladders. The loop then ends, and control returns to the `while` condition to decide whether another turn is required.
  > **Примітка**
> 
> If the `break` statement above did not use the `gameLoop` label, it would break out of the `switch` statement, not the `while` statement. Using the `gameLoop` label makes it clear which control statement should be terminated.
> 
> It is not strictly necessary to use the `gameLoop` label when calling `continue gameLoop` to jump to the next iteration of the loop. There is only one loop in the game, and therefore no ambiguity as to which loop the `continue` statement will affect. However, there is no harm in using the `gameLoop` label with the `continue` statement. Doing so is consistent with the label’s use alongside the `break` statement and helps make the game’s logic clearer to read and understand.### Ранній вихідEarly Exit
A `guard` statement, like an if statement, executes statements depending on the Boolean value of an expression. You use a `guard` statement to require that a condition must be true in order for the code after the guard statement to be executed. Unlike an `if` statement, a guard statement always has an `else` clause — the code inside the `else` clause is executed if the condition is not true.

```swiftfunc greet(person: [String: String]) {    guard let name = person["name"] else {        return    }        print("Hello \(name)!")        guard let location = person["location"] else {        print("I hope the weather is nice near you.")        return    }        print("I hope the weather is nice in \(location).")} greet(person: ["name": "John"])// Надрукує "Hello John!"// Надрукує "I hope the weather is nice near you."greet(person: ["name": "Jane", "location": "Cupertino"])// Надрукує "Hello Jane!"// Надрукує "I hope the weather is nice in Cupertino."
```
If the `guard` statement’s condition is met, code execution continues after the `guard` statement’s closing brace. Any variables or constants that were assigned values using an optional binding as part of the condition are available for the rest of the code block that the `guard` statement appears in.
If that condition is not met, the code inside the `else` branch is executed. That branch must transfer control to exit the code block in which the `guard` statement appears. It can do this with a control transfer statement such as `return`, `break`, `continue`, or `throw`, or it can call a function or method that doesn’t return, such as `fatalError(_:file:line:)`.
Using a `guard` statement for requirements improves the readability of your code, compared to doing the same check with an `if` statement. It lets you write the code that’s typically executed without wrapping it in an `else` block, and it lets you keep the code that handles a violated requirement next to the requirement.
### Checking API Availability
Swift has built-in support for checking API availability, which ensures that you don’t accidentally use APIs that are unavailable on a given deployment target.
The compiler uses availability information in the SDK to verify that all of the APIs used in your code are available on the deployment target specified by your project. Swift reports an error at compile time if you try to use an API that isn’t available.
You use an *availability condition* in an `if` or `guard` statement to conditionally execute a block of code, depending on whether the APIs you want to use are available at runtime. The compiler uses the information from the availability condition when it verifies that the APIs in that block of code are available.

```swiftif #available(iOS 10, macOS 10.12, *) {    // Use iOS 10 APIs on iOS, and use macOS 10.12 APIs on macOS} else {    // Fall back to earlier iOS and macOS APIs}
```
The availability condition above specifies that on iOS, the body of the `if` executes only on iOS 10 and later; on macOS, only on macOS 10.12 and later. The last argument, `*`, is required and specifies that on any other platform, the body of the `if` executes on the minimum deployment target specified by your target.
    In its general form, the availability condition takes a list of platform names and versions. You use platform names such as `iOS`, `macOS`, `watchOS`, and `tvOS` — for the full list, see [Declaration Attributes](). In addition to specifying major version numbers like iOS 8, you can specify minor versions numbers like iOS 8.3 and macOS 10.10.3.

```swiftif #available(platform name version, ..., *) {    statements to execute if the APIs are available} else {    fallback statements to execute if the APIs are unavailable}
```
