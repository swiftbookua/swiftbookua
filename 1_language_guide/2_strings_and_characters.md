## Рядки та cимволи

Рядком ми будемо називати послідовність символів, таку як  `"hello, world"` чи `"albatross"`. У Swift рядки представлені типом `String`. Вміст рядка `String` можна отримати у кілька різних способів, у тому числі через колекцію значень типу `Character`.

У мові Swift, типи `String` та `Character` надають швидкий та сумісний з кодуванням [Unicode](https://uk.wikipedia.org/wiki/Юнікод) спосіб працювати із текстами у вашому коді. Синтаксис створення рядків та маніпуляцій з ними є легковісним, легким для читання, а синтаксис оголошення рядкових літералів є подібним до мови C. Для конкатенації, тобто поєднання рядків, достатньо просто поєднати дві рядкові змінні оператором `+`, а щоб керувати константністю/змінністю рядка, досить просто обрати між констаною чи змінною, як, власне, і з будь-яким іншим значенням в Swift. Ви можете також вставляти в рядки константи, змінні, літерали чи вирази за допомогою механізму відомого як інтерполяція рядків. Це дозволяє легко створювати власні рядкові значення для відображення, зберігання та виводу на екран. 

Незважаючи на простоту синтаксису, тип `String` у Swift є швидкою та сучасною реалізацією рядка. Кожен рядок складається із незалежних від кодування символів Unicode, і надає підтримку доступу до цих символів у різних представленнях Unicode.> **Примітка**
> 
> Тип `String` у Swift має міст із класом `NSString` в модулі Foundation. Модуль Foundation розширює тип `String` методами, визначеними в `NSString`. Це означає, що включивши модуль Foundation, ми можете користупитись методами `NSString` на значеннях типу `String` без приведення типів.
> 
> За додатковою інформацією щодо використання типу `String` із модулями Foundation та Cocoa, дивіться розділ "Working with Cocoa Data Types" в книзі *Using Swift with Cocoa and Objective-C (Swift 3.0.1)*### Рядкові літерали

*Рядкові літерали* потрібні для того, щоб включати заздалегіть визначені значення `String` у ваш код. Рядковим літералом є фіксована послідовність тестових символів, оточена парою подвійних лапок (`""`).
You can include predefined `String` values within your code as *string literals*. A string literal is a fixed sequence of textual characters surrounded by a pair of double quotes (`""`).

Рядкових літерал можна використовувати як початкове значення для константи чи змінної:```swift
let someString = "Some string literal value"
```

Слід помітити, що Swift визначає тип константи `someString` як `String`, бо вона ініціалізується за допомогою рядкового літерала.
> **Примітка**
> 
> Щоб дізнатися більше про використання спеціальних символів в рядкових літералах, дивіться підрозділ [Спеціальні символи в рядкових літералах](Спеціальні-символи-в-рядкових-літералах).

### Ініціалізація порожнього рядка

Щоб створити порожній рядок, як початковий крок в побудові довшого рядка, можна або присвоїти порожній рядковий літерал змінній, або створити новий екземпляр `String` за допомогою синтаксису ініціалізації:

```swiftvar emptyString = ""               // порожній рядковий літералvar anotherEmptyString = String()  // синтаксис ініціалізації// обидва рядки порожні, і дорівнюють одне одному
```

Щоб визначити, чи порожній рядок, слід перевірити Булеве значення властивості `isEmpty`:

```swiftif emptyString.isEmpty {    print("Nothing to see here")}// Друкує "Nothing to see here"
```

### Змінюваність рядків

Щоб вказати, чи можна змінити (або *мутувати*) певне значення `String`, слід або присвоїти це значення змінній (тоді його можна буде змінити), або присвоїти це значення константі (тоді його не можна буде змінити):
You indicate whether a particular `String` can be modified (or *mutated*) by assigning it to a variable (in which case it can be modified), or to a constant (in which case it cannot be modified):

```swiftvar variableString = "Horse"variableString += " and carriage"// змінна variableString тепер має значення "Horse and carriage" let constantString = "Highlander"constantString += " and another Highlander"
// це призведе до помилки часу компіляції - константний рядок не можна змінювати
```
> **Примітка**>
> Даний підхід відрізняється від мутацій рядків в Objective-C та Cocoa, де ви обираєте між двома класами,  (`NSString` та `NSMutableString`), щоб вказати, чи можна змінювати рядок.

### Рядки є Типами-значенняТип `String` у Swift є типом-значення. Якщо створити нове значення `String`, це значення `String` буде *копіюватись* при передаванні до функції чи методу, чи при присвоєнні змінній чи константі. В кожному випадку буде створюватись нова копія існуючого значення `String`, і ця нова копія буде передаватись чи присвоюватись, не оригінальна версія. Типи-значення детальніше описані в розділі [Структури і Перечислення як Типи-значення](8_classes_and_structures.md#структури_і_перечислення_як_типи-значення).

Поведінка копіювання за замовчанням рядків у Swift гарантує, що коли функція чи метод передає вам значення `String`, саме ви володієти цим точним значенням `String`, незалежно від того, звідки воно прийшло. Ви можете бути впервнені, що рядок, переданий вами, не буде змінено жодним чином, окрім як вами самими. 

За лаштунками, компілятор Swift оптимізовує використання рядків таким чином, що фактичне копіювання відбувається тільки тоді, коли це дійсно необхідно. Це означає, що ви завжди отримаєте чудову швидкодію при роботі з рядками як із типами-значення. ### Робота із символами

Отримати доступ до окремих символів `Character` у рядку `String` можна ітеруючи його властивість `characters` за допомогою циклу `for`-`in`:

```swiftfor character in "Собака!🐶".characters {    print(character)}// С// о// б
// а
// к
// а// !// 🐶
```

Цикл `for`-`in` детально описаний в підрозділі [Цикл For-In](4_control_flow.md#Цикл-For-In).

Як варіант, можна створити окремий символ `Character` у вигляді константи чи змінної із односимвольного рядкового літералу, вказавши анотацію типу `Character`:

```swiftlet exclamationMark: Character = "!"
```

Значення `String` можна сконструювати, передавши масив символів `Character` як аргумент його ініціалізатора:

```swiftlet catCharacters: [Character] = ["К", "и", "ц", "я", "!", "🐱"]let catString = String(catCharacters)print(catString)// Друкує "Киця!🐱"
```

### Конкатенація рядків та символівЗначення `String` можна з'єднувати разом (або *конкатенувати*) за допомогою оператора додавання (`+`), при цьому буде створено новий рядок:

```swiftlet string1 = "hello"let string2 = " there"var welcome = string1 + string2// змінна welcome тепер дорівнює "hello there"
```

Ви можете також додавати значення `String` в кінець існуючої змінної `String` за допомогою оператора додавання з присвоєнням (`+=`):

```swiftvar instruction = "look over"instruction += string2// змінна instruction тепер дорівнює "look over there"
```

Ви можете додати символ `Character` до змінної `String` за допомогою методу `append()` типу `String`:

```swiftlet exclamationMark: Character = "!"welcome.append(exclamationMark)// змінна welcome тепер дорівнює "hello there!"
```
> **Примітка**> 
> Ви не можете додати рядок `String` чи символ `Character` до існуючої змінної `Character`, бо значення  `Character` можуть містити лише один символ.### Інтерполяція рядків

*Інтерполяція рядків* є способом створити новий рядок `String` із комбінації констант, змінних, літералів та виразів, включаючи їх значення всередині рядкового літералу. Кожен елемент, що вставляється в рядковий літерал, має бути оточений парою дужок, перед якими йде зворотній слеш (`\`):

```swiftlet multiplier = 3let message = "\(multiplier) рази по 2.5 дорівнює \(Double(multiplier) * 2.5)"// константа message дорівнює "3 рази по 2.5 дорівнюєи 7.5"
```

У прикладі вище, значення `multiplier` вставляється в рядковий літерал як `\(multiplier)`. Цей заповнювач буде замінено фактичним значенням константи `multiplier` під час виконання інтерполяції рядку у ході створення даного рядка. 

Значення `multiplier` є також частиною більшого виразу далі в рядку. Цей вираз обчислює значення `Double(multiplier) * 2.5` і вставляє результат (`7.5`) в рядок. В цьому випадку, вираз записується як `\(Double(multiplier) * 2.5)` для включення в рядковий літерал. 
> **Примітка**> 
> Вирази, котрі ви пишете всередині дужок при інтерполяції рядків, не можуть містити неекранованих зворотніх слешів (`\`), символів зміни рядка (`\n`) чи символів повернення каретки (`\r`). Однак вони можуть містити інші рядкові літерали. 
 ### Юнікод

*Юнікод* - це міжнародний стандарт для кодування, представлення і обробки тексту в різних системах писемності. Він дозволяє вам представити практично будь-який символ з будь-якої мови у стандартизованій формі, зчитувати та виводити ці символи із будь-якого зовнішнього джерела в будь-яке інше зовнішнє джерело, такі як текстовий файл чи веб-сторінка. Типи `String` та `Character` у мові Swift є повністю сумісними із Юнікодом, про що й ітиме мова у даному підрозділі. 
#### Юнікодові скаляри

За лаштунками, вбудований у Swift тип `String` будується із юнікодових скалярних значень. Юнікодовим скаляром є унікальне 21-бітне число для позначення символа чи модифікатора, як, наприклад, `U+0061` для `LATIN SMALL LETTER A` (`"a"`), або `U+1F425` для `FRONT-FACING BABY CHICK` (`"🐥"`).
> **Примітка**
> 
> Юнікодовим скаляром є будь-яка *кодова позиція* у проміжку від `U+0000` до `U+D7FF` включно, або від `U+E000` до `U+10FFFF` включно. Юнікодові скаляри не включають кодові позиції *сурогатних пар*, котрі є кодовими позиціями у проміжку від `U+D800` до `U+DFFF` включно.

Слід відмітити, що не всі 21-бітні юнікодові скаляри присвоєні символам: деякі скаляри зарезервовано для майбунього використання. Скаляри, котрі були присвоєні символам, як правило також мають ім'я, як наприклад `LATIN SMALL LETTER A` та `FRONT-FACING BABY CHICK` у прикладах вище.
#### Спеціальні символи в рядкових літералах

Рядкові літерали можуть включати наступні спеціальні символи:

+ Екрановані спеціальні символи: `\0` (нульовий символ), `\\` (зворотній слеш), `\t` (горизонтальний таб), `\n` (перенесення рядка), `\r` (повернення каретки), `\"` (подвійні лапки) та `\'` (оданарні лапки)
+ Довільний юнікодовий скаляр, записаний у формі `\u{`n`}`, де n - це 1–8-цифове число в шістнадцятковому записі зі значенням, що відповідає дійсній кодовій позиції Юнікоду.

Код нижче показує чотири приклади спеціальних символів. Константа `wiseWords` містить пару екранованих подвійних лапок. Константи `dollarSign`, `blackHeart`, та `sparklingHeart` демонструють формат юнікодових скалярів:

```swiftlet wiseWords = "\"Уява важливіша за знання\" - Ейнштейн"// "Уява важливіша за знанн" - Ейнштейнlet dollarSign = "\u{24}"        // $,  Unicode scalar U+0024let blackHeart = "\u{2665}"      // ♥,  Unicode scalar U+2665let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496```

#### Розширені кластери графем

Кожен екземпляр типу `Character` у Swift представляє єдиний *розширений кластер графем*. Розширений кластер графем - це послідовність одного чи більше юнікодових скалярів, котрі при об'єднанні дають єдиний графічний символ.

Ось напридклад. Літера `é` може бути представлена як єдиний юнікодовий скаляр  `é` (`LATIN SMALL LETTER E WITH ACUTE`, або `U+00E9`). Однак, ця ж літера також може бути представлена як пара скалярів: стандарна літера `e` (`LATIN SMALL LETTER E`, або `U+0065`), і слідом за нею скаляр `COMBINING ACUTE ACCENT` (`U+0301`). Скаляр `COMBINING ACUTE ACCENT` графічно приміняється до скаляру, що йому передує, перетворюючи `e` в `é` при рендерингу в системі рендерингу тексту, сумісної з Юнікодом. 

В обох випадках, літера  `é` представляється єдиним значенням `Character`, що представляє розширений кластер графем. В першому випадку, кластер містить єдиний скаляр, в другому випадку - це кластер із двох скалярів:


```swiftlet eAcute: Character = "\u{E9}"                         // élet combinedEAcute: Character = "\u{65}\u{301}"          // e та слідом за нею  ́// eAcute дорівнює é, combinedEAcute дорівнює é
```

Розширені кластери графем є гручким способом представити багато складних символів письма як єдине значення `Character`. Наприклад, склади Ханґиль із корейської абетки можуть бути представлені як у вигляді попередньо складеного єдиного юнікодового скаляру, так і у вигляді послідовності окремих юнікодових скалярів. Обидва представлення визначають одне й те ж саме значення `Character` у Swift:

```swiftlet precomposed: Character = "\u{D55C}"                  // 한let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ᅡ, ᆫ// precomposed дорівнює 한, decomposed дорівнює 한
```

Розширені кластери графем дозволяють знакам обведення (таким як `COMBINING ENCLOSING CIRCLE`, або `U+20DD`) обводити інші юнікодові скаляри всередині єдиного значення `Character`:

```swiftlet enclosedEAcute: Character = "\u{E9}\u{20DD}"// enclosedEAcute дорівнює é⃝
```

Юнікодові скаляри для символів індикації регіонів можуть поєднуватись у пари, щоб зібрати одне значення `Character`, як наприклад ця комбінація `REGIONAL INDICATOR SYMBOL LETTER U` (`U+1F1FA`) та `REGIONAL INDICATOR SYMBOL LETTER A` (`U+1F1E6`):

```swiftlet regionalIndicatorForUA: Character = "\u{1F1FA}\u{1F1E6}"// regionalIndicatorForUA дорівнює 🇺🇦
```### Counting Characters

Щоб отримати кількість символів `Character` у рядку, слід використовувати властивість `count` властивості `characters` рядка. 


```swiftlet unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"print("unusualMenagerie містить \(unusualMenagerie.characters.count) символів")// Друкує "unusualMenagerie містить 40 символів"
```

Варто зазначити, що використання розширених графемних кластерів для значень `Character` у Swift означає, що конкатенація та модифікація рядків не завжди впливає на кількість символів у рядку. 

Наприклад, якщо ініціалізувати новий рядок чотирьохсимвольним словом `cafe`, а потім приєднати до його кінця символ `COMBINING ACUTE ACCENT` (`U+0301`), результуючий рядок буде все ще мати кількість символів `4`, із четвертим символом `é`, замість `e`:

```swiftvar word = "cafe"print("кількість символів у \(word) дорівнює \(word.characters.count)")// Друкує "кількість символів у cafe дорівнює 4" word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301 print("кількість символів у \(word) дорівнює \(word.characters.count)")// Друкує "кількість символів у café дорівнює 4"
```
> **Примітка**
> 
> Розширені графемні кластери можуть складатись із одного чи більше юнікодових скалярів. Це означає що різні символи - і різні представлення одного й того ж самого символа - можуть потребувати різної кількості пам'яті для зберігання. Через це, різні символи у Swift займають різну кількість пам'яті всередині представлення рядка. Як результат, кількість символів у рядку не може бути обчисленою без ітерування самого рядка для визначення меж графемних кластерів. Якщо ви працюєти із досить довгими рядками, майте на увазі, що властивість `characters` повинна проітерувати всі юнікодові скаляри у всьому рядку, щоб визначити символи для даного рядка. 
> 
> Кількість символів, якуи поверне властивість `characters` не завжди дорівнює властивості `length` в екземплярі класу `NSString`, що містить ті ж само символи. Довжина `NSString` базується на кількості 16-бітних кодових одиниць в представленні рядка UTF-16, а не кількістю юнікодових розширених графемних кластерів всередині рядка. ### Доступ до елеменів рядка і його модифікація

Маніпулювати рядком можна за допомогою його методів та властивостей, або за допомогою синтаксису індексації.#### Індекси рядка

Кожен рядок `String` має асоційований *тип індекса*, `String.Index`, котрий відповідає позиції кожного символа `Character` в рядку. 

Як зазначено вище, різні символи можуть вимагати різної кількості пам'яті для зберігання, тому для того, щоб визначити, який саме символ знаходиться за даною позицією, слід проітерувати кожен юнікодовий скаляр з початку чи з кінця цього рядка. З цієї причини, рядку у Swift не можуть мати цілочисельні індекси. 

За допомогою властивості `startIndex` можна отримати позицію першого символа в рядку. Властивість `endIndex` є позицією після останнього символа в рядку. Як результат, властивість `endIndex` не є коректним агрументом для індексу рядка. Якщо рядок порожній, властивості `startIndex` та `endIndex` дорівнюють одна одній.

Щоб отримати індекси перед та після даного індексу, слід використовувати методи `String` `index(before:)` та `index(after:)` відповідно. Щоб отримати індекс подалі від даного, слід використовувати метод `index(_:offsetBy:)` замість того, щоб викликати один з попередніх методів кілька разів. Щоб отримати символ рядка за даним індексом, слід використовувати синтаксис індексації (`[]`).```swiftlet greeting = "Guten Tag!"greeting[greeting.startIndex]// Ggreeting[greeting.index(before: greeting.endIndex)]// !greeting[greeting.index(after: greeting.startIndex)]// ulet index = greeting.index(greeting.startIndex, offsetBy: 7)greeting[index]// a
```

Спроби отримати індекс за межами діапазону символів рядка, чи звернутись до символу за межами рядка, призведе до помилки на етапі виконання.

```swiftgreeting[greeting.endIndex] // Помилкаgreeting.index(after: greeting.endIndex) // ПомилкаUse the indices property of the characters property to access all of the indices of individual characters in a string.for index in greeting.characters.indices {    print("\(greeting[index]) ", terminator: "")}// Надрукує "G u t e n   T a g ! "
```
> **Примітка**> 
> Можна користуватись властивостями `startIndex` та `endIndex`, а також методами `index(before:)`, `index(after:)`, та `index(_:offsetBy:)` на будь-якому типі, що реалізовує протокол `Collection`. Таким типом є `String`, як показано тут, так само такими є типи колекцій, такі як  `Array`, `Dictionary`, та `Set`.#### Вставка та видалення

Щоб вставити один символ в рядок за визначеним індексом, можна користуватись методом `insert(_:at:)`, а щоб вставити вміст іншого рядка за визначеним індексом, можна скористатись методом  `insert(contentsOf:at:)`.

```swiftvar welcome = "hello"welcome.insert("!", at: welcome.endIndex)// welcome тепер дорівнює "hello!"welcome.insert(contentsOf:" there".characters, at: welcome.index(before: welcome.endIndex))// welcome тепер дорівнює "hello there!"
```

Щоб видалити один символ із рядка за визначеним індексом, можна використати метод `remove(at:)`, а щоб видалити підрядок за визначеним діапазоном, можна скористатись методом `removeSubrange(_:)`:


```swiftwelcome.remove(at: welcome.index(before: welcome.endIndex))// welcome тепер дорівнює "hello there" let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndexwelcome.removeSubrange(range)// welcome тепер дорівнює "hello"
```
> **Примітка**> 
> Методи `insert(_:at:)`, `insert(contentsOf:at:)`, `remove(at:)`, та `removeSubrange(_:)` можна використовувати на будь-якому типі, що реалізовує протокол `RangeReplaceableCollection`. Це включає `String`, як показано тут, так же як і типи колекцій, такі як `Array`, `Dictionary`, та `Set`.и### Comparing Strings
Swift provides three ways to compare textual values: string and character equality, prefix equality, and suffix equality.#### Рівність рядків та символів
String and Character Equality
String and character equality is checked with the “equal to” operator (`==`) and the “not equal to” operator (`!=`), as described in Comparison Operators:

```swiftlet quotation = "We're a lot alike, you and I."let sameQuotation = "We're a lot alike, you and I."if quotation == sameQuotation {    print("These two strings are considered equal")}// Prints "These two strings are considered equal"
```
Two `String` values (or two `Character` values) are considered equal if their extended grapheme clusters are *canonically equivalent*. Extended grapheme clusters are canonically equivalent if they have the same linguistic meaning and appearance, even if they are composed from different Unicode scalars behind the scenes.
For example, `LATIN SMALL LETTER E WITH ACUTE` (`U+00E9`) is canonically equivalent to `LATIN SMALL LETTER E` (`U+0065`) followed by `COMBINING ACUTE ACCENT` (`U+0301`). Both of these extended grapheme clusters are valid ways to represent the character `é`, and so they are considered to be canonically equivalent:

```swift// "Voulez-vous un café?" using LATIN SMALL LETTER E WITH ACUTElet eAcuteQuestion = "Voulez-vous un caf\u{E9}?" // "Voulez-vous un café?" using LATIN SMALL LETTER E and COMBINING ACUTE ACCENTlet combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?" if eAcuteQuestion == combinedEAcuteQuestion {    print("These two strings are considered equal")}// Prints "These two strings are considered equal"
```
Conversely, `LATIN CAPITAL LETTER A` (`U+0041`, or `"A"`), as used in English, is not equivalent to `CYRILLIC CAPITAL LETTER A` (`U+0410`, or `"А"`), as used in Russian. The characters are visually similar, but do not have the same linguistic meaning:

```swiftlet latinCapitalLetterA: Character = "\u{41}" let cyrillicCapitalLetterA: Character = "\u{0410}" if latinCapitalLetterA != cyrillicCapitalLetterA {    print("These two characters are not equivalent.")}// Prints "These two characters are not equivalent."
```
> **Примітка**>
> String and character comparisons in Swift are not locale-sensitive.#### Prefix and Suffix Equality
To check whether a string has a particular string prefix or suffix, call the string’s `hasPrefix(_:)` and `hasSuffix(_:)` methods, both of which take a single argument of type `String` and return a Boolean value.
The examples below consider an array of strings representing the scene locations from the first two acts of Shakespeare’s *Romeo and Juliet*:

```swiftlet romeoAndJuliet = [    "Act 1 Scene 1: Verona, A public place",    "Act 1 Scene 2: Capulet's mansion",    "Act 1 Scene 3: A room in Capulet's mansion",    "Act 1 Scene 4: A street outside Capulet's mansion",    "Act 1 Scene 5: The Great Hall in Capulet's mansion",    "Act 2 Scene 1: Outside Capulet's mansion",    "Act 2 Scene 2: Capulet's orchard",    "Act 2 Scene 3: Outside Friar Lawrence's cell",    "Act 2 Scene 4: A street in Verona",    "Act 2 Scene 5: Capulet's mansion",    "Act 2 Scene 6: Friar Lawrence's cell"]
```
You can use the `hasPrefix(_:)` method with the `romeoAndJuliet` array to count the number of scenes in Act 1 of the play:

```swiftvar act1SceneCount = 0for scene in romeoAndJuliet {    if scene.hasPrefix("Act 1 ") {        act1SceneCount += 1    }}print("There are \(act1SceneCount) scenes in Act 1")// Prints "There are 5 scenes in Act 1"
```
Similarly, use the `hasSuffix(_:)` method to count the number of scenes that take place in or around Capulet’s mansion and Friar Lawrence’s cell:

```swiftvar mansionCount = 0var cellCount = 0for scene in romeoAndJuliet {    if scene.hasSuffix("Capulet's mansion") {        mansionCount += 1    } else if scene.hasSuffix("Friar Lawrence's cell") {        cellCount += 1    }}print("\(mansionCount) mansion scenes; \(cellCount) cell scenes")// Prints "6 mansion scenes; 2 cell scenes"
```
> **Примітка**> 
> The `hasPrefix(_:)` and `hasSuffix(_:)` methods perform a character-by-character canonical equivalence comparison between the extended grapheme clusters in each string, as described in [Рівність рядків та символів](Рівність-рядків-та-символів).### Unicode Representations of StringsWhen a Unicode string is written to a text file or some other storage, the Unicode scalars in that string are encoded in one of several Unicode-defined *encoding forms*. Each form encodes the string in small chunks known as *code units*. These include the UTF-8 encoding form (which encodes a string as 8-bit code units), the UTF-16 encoding form (which encodes a string as 16-bit code units), and the UTF-32 encoding form (which encodes a string as 32-bit code units).
Swift provides several different ways to access Unicode representations of strings. You can iterate over the string with a `for`-`in` statement, to access its individual `Character` values as Unicode extended grapheme clusters. This process is described in [Working with Characters](робота-із-символами).Alternatively, access a `String` value in one of three other Unicode-compliant representations:
 + A collection of UTF-8 code units (accessed with the string’s `utf8` property) + A collection of UTF-16 code units (accessed with the string’s `utf16` property) + A collection of 21-bit Unicode scalar values, equivalent to the string’s UTF-32 encoding form (accessed with the string’s `unicodeScalars` property)
 Each example below shows a different representation of the following string, which is made up of the characters `D`, `o`, `g`, `‼` (`DOUBLE EXCLAMATION MARK`, or Unicode scalar `U+203C`), and the `🐶`character (`DOG FACE`, or Unicode scalar `U+1F436`):

```swiftlet dogString = "Dog‼🐶"
```#### UTF-8 RepresentationYou can access a UTF-8 representation of a `String` by iterating over its utf8 property. This property is of type `String.UTF8View`, which is a collection of unsigned 8-bit (`UInt8`) values, one for each byte in the string’s UTF-8 representation:

![UTF-8 Representation example](images/UTF8_2x.png)

￼```swiftfor codeUnit in dogString.utf8 {    print("\(codeUnit) ", terminator: "")}print("")// 68 111 103 226 128 188 240 159 144 182
```
In the example above, the first three decimal `codeUnit` values (`68`, `111`, `103`) represent the characters `D`, `o`, and `g`, whose UTF-8 representation is the same as their ASCII representation. The next three decimal `codeUnit` values (`226`, `128`, `188`) are a three-byte UTF-8 representation of the `DOUBLE EXCLAMATION MARK` character. The last four codeUnit values (`240`, `159`, `144`, `182`) are a four-byte UTF-8 representation of the `DOG FACE` character.#### UTF-16 RepresentationYou can access a UTF-16 representation of a `String` by iterating over its `utf16` property. This property is of type `String.UTF16View`, which is a collection of unsigned 16-bit (`UInt16`) values, one for each 16-bit code unit in the string’s UTF-16 representation:￼
![UTF-8 Representation example](images/UTF16_2x.png)

```swiftfor codeUnit in dogString.utf16 {    print("\(codeUnit) ", terminator: "")}print("")// Prints "68 111 103 8252 55357 56374 "
```
Again, the first three codeUnit values (`68`, `111`, `103`) represent the characters `D`, `o`, and `g`, whose UTF-16 code units have the same values as in the string’s UTF-8 representation (because these Unicode scalars represent ASCII characters).
The fourth `codeUnit` value (`8252`) is a decimal equivalent of the hexadecimal value `203C`, which represents the Unicode scalar `U+203C` for the `DOUBLE EXCLAMATION MARK` character. This character can be represented as a single code unit in UTF-16.
The fifth and sixth `codeUnit` values (`55357` and `56374`) are a UTF-16 surrogate pair representation of the `DOG FACE` character. These values are a high-surrogate value of `U+D83D` (decimal value `55357`) and a low-surrogate value of `U+DC36` (decimal value `56374`).#### Unicode Scalar RepresentationYou can access a Unicode scalar representation of a `String` value by iterating over its `unicodeScalars` property. This property is of type `UnicodeScalarView`, which is a collection of values of type `UnicodeScalar`.Each `UnicodeScalar` has a `value` property that returns the scalar’s 21-bit value, represented within a `UInt32` value:

![UTF-8 Representation example](images/UnicodeScalar_2x.png)￼
```swiftfor scalar in dogString.unicodeScalars {    print("\(scalar.value) ", terminator: "")}иprint("")// Prints "68 111 103 8252 128054 "
```
The value properties for the first three `UnicodeScalar` values (`68`, `111`, `103`) once again represent the characters `D`, `o`, and `g`.
The fourth `codeUnit` value (`8252`) is again a decimal equivalent of the hexadecimal value `203C`, which represents the Unicode scalar `U+203C` for the `DOUBLE EXCLAMATION MARK` character.
The `value` property of the fifth and final `UnicodeScalar`, `128054`, is a decimal equivalent of the hexadecimal value `1F436`, which represents the Unicode scalar `U+1F436` for the `DOG FACE` character.
As an alternative to querying their value properties, each `UnicodeScalar` value can also be used to construct a new `String` value, such as with string interpolation:

```swiftfor scalar in dogString.unicodeScalars {    print("\(scalar) ")}// D// o// g// ‼// 🐶
```