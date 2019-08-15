## Колекції

Мова Swift надає три основті *типи колекцій*, відомих як масиви (`Array`), множини (`Set`) та словники (`Dictionary`), для зберігання колекцій значень. Масиви - це впорядковані колекції значень. Множини - це невпорядковані колекції унікальних значень. Словники - це невпорядковані колекції асоціацій ключ-значення. 

![](images/CollectionTypes_intro_2x.png)￼
При роботі із масивами, множинами та словниками у Swift завжди зрозуміло, які типи значень та ключів вони можуть зберігати. Це означає, що ви не можете помилково вставити значення невідповідного типу в колецію. Це також означає, що ви можете бути впевнені щодо типів значень, котрі ви отримуєти з колекції. 
> **Примітка**> 
> У Swift масиви, множини та словники реалізовано як *узагальнені колекції*. Щоб дізнатись більше про узагальнені типи й колекції, дивіться розділ [Узагальнення](22_generics.md).### Змінюваність колекцій

Якщо створити масив, множину чи словник, та присвоїти їх змінній, створена колекція буде *змінюваною*. Це значить, що після її створення її вміст можна змінити додаючи, видаляючи чи міняючи елементи колекції. Якщо присвоїти масив, множину чи словник константі, колекція буде *незмінюваною*, і її розмір чи вміст не можна буде змінити. 
> **Примітка**> 
> Гарною практикою є створення незмінюваних колекцій в усіх випадках, де колекція не потребує змін. Цей підхід полегшує вам міркування про ваш код та дозволяє компілятору Swift оптимізувати швидкодію колекцій, що ви створюєте.### Масиви

*Масив* зберігає значення однакового типу у упорядкованому списку. Одне й те ж значення може з'являтись у масиві кілька разів у різних позиціях.> **Примітка**>
> Існує міст між типом `Array` у  Swift та класом `NSArray` у Foundation.
> 
> За детальнішою інформацією про використання типу `Array` із Foundation та Cocoa, дивіться розділ "Working with Cocoa Data Types" в книзі *Using Swift with Cocoa and Objective-C (Swift 3.0.1)*

#### Скорочений синтаксис масивів

Тип масиву у Swift у повній формі записується як `Array<Element>`, де `Element` - це тип значень, що можуть зберігатись в масиві. Також тип масиву можна записати у скороченій формі як `[Element]`. Хоч обидві форми є ідентичними, бажано вживати коротку форму, і саме вона переважно використовується в цій книзі для опису типу масивів.

#### Створення порожнього масиву

Щоб створити порожній масив певного типу, можна використати синтаксис ініціалізації:

```swiftvar someInts = [Int]()print("someInts має тип [Int] та містить \(someInts.count) елементів.")// Надрукує "someInts має тип [Int] та містить 0 елементів."
```

Слід зазничити, що тип змінної `someInts` визачено як `[Int]` із типу ініціалізатора.Іншим способом, за умови що контекст вже надає інформацію про тип, наприклад як аргумент функції чи заздалегіть типізована змінна чи константа, є створення порожнього масиву за допомогою літерала порожнього масиву, що записується як `[]` (порожня пара квадратних дужок).

```swiftsomeInts.append(3)// someInts тепер містить 1 значення типу IntsomeInts = []// someInts тепер порожній масив, проте досі має тип [Int]
```

#### Створення масиву зі значенням за замовчанням

Тип `Array` у Swift також надає ініціалізатор для створення масиву певного розміру зі всіма його значеннями рівними одному значенню за замовчанням. В цей ініціалізатор передається значення за замованням відповідного типу (називається `repeating:`) та кількість повторів цього значення в новому масиві (називається `count:`):

```swiftvar threeDoubles = Array(repeating: 0.0, count: 3)// threeDoubles тепер має тип [Double], і дорівнює [0.0, 0.0, 0.0]
```
#### Creating an Array by Adding Two Arrays Together
You can create a new array by adding together two existing arrays with compatible types with the addition operator (`+`). The new array’s type is inferred from the type of the two arrays you add together:

```swiftvar anotherThreeDoubles = Array(repeating: 2.5, count: 3)// anotherThreeDoubles is of type [Double], and equals [2.5, 2.5, 2.5] var sixDoubles = threeDoubles + anotherThreeDoubles// sixDoubles is inferred as [Double], and equals [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```

#### Створення масиву за допомогою літералу масиву

Можна також створити масив за допомогою *літералу масиву*, що є скороченим способом записати одне чи більше значень як масив. Літерал масиву записується як список значень, розділений комами, оточений парою квадратних дужок:

<span style="text-align: center;">[<span style="background-color:#E9EFFA">значення 1</span>, <span style="background-color:#E9EFFA">значення 2</span>, <span style="background-color:#E9EFFA">значення 3</span>]</span>

У прикладі нижче створено масив з назвою `shoppingList`, котрий зберігає значення типу `String`:

```swiftvar shoppingList: [String] = ["Яйця", "Молоко"]// shoppingList було проініціалізовано двома початковими елементами
```

Змінну `shoppingList` оголошено як "масив рядкових значень", що записано як `[String]`. Оскільки цей конкретний масив визначає тип своїх значень як `String`, у ньому можуть міститисть тільки значення типу `String`. Тут, масив `shoppingList` ініціалізовано двома значеннями типу `String` (`"Eggs"` та `"Milk"`), що записані всередині літералу масиву.
> **Примітка**
> 
> Масив `shoppingList` оголошено як змінну (за допомогою інструкції `var`), а не константою (за допомогою інструкції `let`), бо у наступних прикладах у нього будуть додаватись нові елементи.

У цьому випадку, літерал масиву містить два рядки і більше нічого. Це відповідає типу змінної `shoppingList` (масив, що можна містити тільки значення `String`), і тому присвоєння літералу масиву дозволяється як спосіб проініціалізувати `shoppingList` двома початковими елементами. 

Дякуючи Богу та визначенню типів у Swift, не вам потрібно завжди писати тип масиву, якщо ви ініціалізуєте його літералом масиву, що містить значення одного й того ж типу. Ініціалізацію масиву `shoppingList` можна було записати у більш короткій формі: 

```swiftvar shoppingList = ["Яйця", "Молоко"]
```

Оскільки всі значення літералу масиву мають однаковий тип, Swift може визначити, що `[String]` є коректним типом для змінної `shoppingList`.

#### Доступ до елементів масиву та його модифікація

Щоб отримати доступ до елементів масиву чи змінити їх, слід користуватись методами і властивостями масиву, або синтаксисом його індексації.

Щоб дізнатись кількість елементів у масиві, слід перевірити його властивість `count`:

```swiftprint("Список покупок містить \(shoppingList.count) елементи.")// Надрукує "Список покупок містить 2 елементи."
```

Булева властивість `isEmpty` є скороченим записом перевірки, чи дорівнює властивість `count` нулю:

```swiftif shoppingList.isEmpty {    print("Список покупок порожній.")} else {    print("Список покупок не порожній.")}// Надрукує "Список покупок не порожній."
```

Можна додавати нові елементи в кінець масиву, викликаючи метод `append(_:)`:

```shoppingList.append("Борошно")// shoppingList тепер містить 3 елементи, і, схоже, хтось готує млинці
```

Також можна додавати в кінець масив із одного чи кілької сумісних елементів за допомогою оператора додавання з присвоєнням (`+=`):

```swiftshoppingList += ["Пекарний порошок"]// shoppingList тепер містить 4 елементівshoppingList += ["Шоколадна паста", "Сир", "Вершкове масло"]// shoppingList тепер містить 7 елементів
```

Отримати значення із масиву можна за допомогою *синтаксису індексації*, передаючи індекс значення, котре слід отримати, в квадратних дужках одразу після імені масиву:

```swiftvar firstItem = shoppingList[0]// firstItem тепер дорівнює "Яйця"
```> **Примітка**> 
> Перший елемент у масиві має індекс `0`, а не `1`. Масиви у Swift індексуються, починаючи з нуля.

Синтаксис індексації також можна використовувати для зміни існуючого значення за заданим індексом:и

```swiftиshoppingList[0] = "Шість яєць"// перший елемент у масиві тепер дорівнює "Шість яєць", а не "Яйця"
```

Також можна використовувати синтаксис індексації для зміни одразу кількох значень у заданому діапазоні, навіть якщо довжина нового діапазону значень відрізняється. Наступний приклад замінює `"Шоколадна паста"`, `"Сир"`, та `"Вершкове масло"` на `"Банани"` та `"Яблука"`: 

```swiftshoppingList[4...6] = ["Банани", "Яблука"]// shoppingList тепер містить 6 елементів
```
> **Примітка**
> 
> Синтаксис індексації не може використовуватись для додавання нових елементів у кінець масиву.

Щоб вставити елемент до масиву за вказаним індексом, слід викликати метод `insert(_:at:)`:

```swiftshoppingList.insert("Кленовий сироп", at: 0)// shoppingList тепер містить 7 елементів// "Кленовий сироп" тепер є першим елементом у масиві
```

Цей виклик методу `insert(_:at:)` вставляє новий елемент зі значенням `"Кленовий сироп"` в самий початок списку покупок, що вказується за допомогою індексу `0`.

Аналогічно, щоб видалити елемент із масиву, слід викликати метод `remove(at:)`. Цей метод видаляє елемент за вказаним індексом і повертає видалений елемент (хоча ви можете проігнорувати повернене значення, якщо воно вам не потрібне):

```swiftlet mapleSyrup = shoppingList.remove(at: 0)// елемент, що був за індексом 0, щойно було видалено.// shoppingList тепер містить 6 елементів, і не містить "Кленовий сироп"// константа mapleSyrup тепер дорівнює видаленому рядку "Кленовий сироп"
```
> **Примітка**
> 
> Якщо ви спробуєте отримати чи змінити елемент за індексом, що не входить в існуючі межі масиву, ви спровокуєте помилку часу виконання. Ви можете перевірити, чи є коректним індекс перед його використанням, порівнявши його із властивістю масиву `count`. Окрім випадку, коли `count` дорівнює `0` (що означає, що масив порожній), найбільший коректний індекс у масиві завжди буде дорівнювати `count - 1`, оскільки масиви індексуються, починаючи із нуля.

Будь-які прогалини у масиві закриваються при видалені елементу, тому значення за індексом `0` знову дорівнює `"Шість яєць"`:

```swiftfirstItem = shoppingList[0]// firstItem тепер дорівнює "Шість яєць"
```

Якщо потрібно видалити останній елемент у масиві, слід використовувати метод `removeLast()` замість методу `remove(at:)`, щоб уникнути зайвого звертання до властивості масиву `count`. Як і метод `remove(at:)`, метод `removeLast()` повертає видалений елемент:

```swiftlet apples = shoppingList.removeLast()// останній елемент у масиві було щойно видалено// shoppingList тепер містить 5 елементів, і жодного яблука// константа apples тепер дорівнює видаленому рядку "Яблука"
```
#### Ітерування масиву

Щоб проітерувати весь набір елементів масиву, слід використовувати цикл `for`-`in`:

```swiftfor item in shoppingList {    print(item)}// Шість яєць// Молоко// Борошно// Пекарний порошок// Банани
```

Якщо вам потрібен цілочисельний індекс кожного елемента разом із його значенням, слід використовувати метод `enumerated()` та ітерувати його результат. Для кожного елементу масиву, метод `enumerated()` повертає кортеж, що складається із цілого індексу та елемента. Індекси починаються із нуля і збільшуються на одиницю із кожним елементом. Під час ітерації, ви можете декомпонувати кортеж на тимчасові константи чи змінні:

```swiftfor (index, value) in shoppingList.enumerated() {    print("Елемент \(index + 1): \(value)")}// Елемент 1: Шість яєць// Елемент 2: Молоко// Елемент 3: Борошно// Елемент 4: Пекарний порошок// Елемент 5: Банани
```

Більше інформаціє про цикл `for`-`in` можна отримати у підрозділі [Цикл For-In](4_control_flow.md#Цикл-For-In).### Sets
A *set* stores distinct values of the same type in a collection with no defined ordering. You can use a set instead of an array when the order of items is not important, or when you need to ensure that an item only appears once.
> **Примітка**
> 
> Swift’s `Set` type is bridged to Foundation’s `NSSet` class.
> 
> For more information about using `Set` with Foundation and Cocoa, see Working with Cocoa Data Types in *Using Swift with Cocoa and Objective-C (Swift 3.0.1)*.
#### Hash Values for Set Types
A type must be *hashable* in order to be stored in a set—that is, the type must provide a way to compute a *hash value* for itself. A hash value is an *Int* value that is the same for all objects that compare equally, such that if `a == b`, it follows that `a.hashValue == b.hashValue`.
All of Swift’s basic types (such as `String`, `Int`, `Double`, and `Bool`) are hashable by default, and can be used as set value types or dictionary key types.   Enumerations case values without associated values (as described in [Перечислення](7_enumerations.md)) are also hashable by default.
> **Примітка**
> 
> You can use your own custom types as set value types or dictionary key types by making them conform to the Hashable protocol from Swift’s standard library. Types that conform to the Hashable protocol must provide a gettable Int property called hashValue. The value returned by a type’s hashValue property is not required to be the same across different executions of the same program, or in different programs.
> 
> Because the Hashable protocol conforms to Equatable, conforming types must also provide an implementation of the equals operator (==). The Equatable protocol requires any conforming implementation of == to be an equivalence relation. That is, an implementation of == must satisfy the following three conditions, for all values a, b, and c:
> > + a == a (Reflexivity)
> + a == b implies b == a (Symmetry)
> + a == b && b == c implies a == c (Transitivity)
> 
> For more information about conforming to protocols, see [Протоколи](21_protocols.md).
 #### Set Type Syntax
The type of a Swift set is written as `Set<Element>`, where `Element` is the type that the set is allowed to store. Unlike arrays, sets do not have an equivalent shorthand form.
#### Creating and Initializing an Empty Set
You can create an empty set of a certain type using initializer syntax:
```swiftvar letters = Set<Character>()print("letters is of type Set<Character> with \(letters.count) items.")// Надрукує "letters is of type Set<Character> with 0 items."
```> **Примітка**
> > The type of the `letters` variable is inferred to be `Set<Character>`, from the type of the initializer.
 Alternatively, if the context already provides type information, such as a function argument or an already typed variable or constant, you can create an empty set with an empty array literal:

```swiftletters.insert("a")// letters now contains 1 value of type Characterletters = []// letters is now an empty set, but is still of type Set<Character>
```
#### Creating a Set with an Array Literal
You can also initialize a set with an array literal, as a shorthand way to write one or more values as a set collection.
The example below creates a set called `favoriteGenres` to store `String` values:

```swiftvar favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]// favoriteGenres has been initialized with three initial items
```
The `favoriteGenres` variable is declared as “a set of `String` values”, written as `Set<String>`. Because this particular set has specified a value type of `String`, it is *only* allowed to store `String` values. Here, the `favoriteGenres` set is initialized with three `String` values (`"Rock"`, `"Classical"`, and `"Hip hop"`), written within an array literal.
> **Примітка**
> > The `favoriteGenres` set is declared as a variable (with the `var` introducer) and not a constant (with the `let` introducer) because items are added and removed in the examples below.
A set type cannot be inferred from an array literal alone, so the type `Set` must be explicitly declared. However, because of Swift’s type inference, you don’t have to write the type of the set if you’re initializing it with an array literal containing values of the same type. The initialization of `favoriteGenres `could have been written in a shorter form instead:

```swiftvar favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```
Because all values in the array literal are of the same type, Swift can infer that `Set<String>` is the correct type to use for the `favoriteGenres` variable.#### Accessing and Modifying a Set
You access and modify a set through its methods and properties.
To find out the number of items in a set, check its read-only `count` property:

```swiftprint("I have \(favoriteGenres.count) favorite music genres.")// Надрукує "I have 3 favorite music genres."
```
Use the Boolean `isEmpty` property as a shortcut for checking whether the `count` property is equal to `0`:```swiftif favoriteGenres.isEmpty {    print("As far as music goes, I'm not picky.")} else {    print("I have particular music preferences.")}// Надрукує "I have particular music preferences."
```
You can add a new item into a set by calling the set’s `insert(_:)` method:

```swiftfavoriteGenres.insert("Jazz")// favoriteGenres now contains 4 items
```
You can remove an item from a set by calling the set’s `remove(_:)` method, which removes the item if it’s a member of the set, and returns the removed value, or returns `nil` if the set did not contain it. Alternatively, all items in a set can be removed with its `removeAll()` method.

```swiftif let removedGenre = favoriteGenres.remove("Rock") {    print("\(removedGenre)? I'm over it.")} else {    print("I never much cared for that.")}// Надрукує "Rock? I'm over it."
```
To check whether a set contains a particular item, use the `contains(_:)` method.

```swiftif favoriteGenres.contains("Funk") {    print("I get up on the good foot.")} else {    print("It's too funky in here.")}// Надрукує "It's too funky in here."
```
#### Iterating Over a SetYou can iterate over the values in a set with a `for`-`in` loop.

```swiftfor genre in favoriteGenres {    print("\(genre)")}// Jazz// Hip hop// Classical
```
For more about the for-in loop, see [Цикл For-In](4_control_flow.md#Цикл-For-In).
Swift’s `Set` type does not have a defined ordering. To iterate over the values of a set in a specific order, use the `sorted()` method, which returns the set’s elements as an array sorted using the `<` operator.

```swiftfor genre in favoriteGenres.sorted() {    print("\(genre)")}// Classical// Hip hop// Jazz
```
### Performing Set Operations
You can efficiently perform fundamental set operations, such as combining two sets together, determining which values two sets have in common, or determining whether two sets contain all, some, or none of the same values.
#### Fundamental Set OperationsThe illustration below depicts two sets — `a` and `b` — with the results of various set operations represented by the shaded regions.

![](images/setVennDiagram_2x.png)￼ 
+ Use the `intersection(_:)` method to create a new set with only the values common to both sets.
+ Use the `symmetricDifference(_:)` method to create a new set with values in either set, but not both.
+ Use the `union(_:)` method to create a new set with all of the values in both sets.
+ Use the `subtracting(_:)` method to create a new set with values not in the specified set.

```swiftlet oddDigits: Set = [1, 3, 5, 7, 9]let evenDigits: Set = [0, 2, 4, 6, 8]let singleDigitPrimeNumbers: Set = [2, 3, 5, 7] oddDigits.union(evenDigits).sorted()// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]oddDigits.intersection(evenDigits).sorted()// []oddDigits.subtracting(singleDigitPrimeNumbers).sorted()// [1, 9]oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()// [1, 2, 9]
```
#### Set Membership and Equality
The illustration below depicts three sets — `a`, `b` and `c` — with overlapping regions representing elements shared among sets. Set `a` is a *superset* of set `b`, because a contains all elements in `b`. Conversely, set `b` is a *subset* of set `a`, because all elements in `b` are also contained by `a`. Set `b` and set `c` are *disjoint* with one another, because they share no elements in common.

![](images/setEulerDiagram_2x.png)￼

 + Use the “is equal” operator (`==`) to determine whether two sets contain all of the same values.
 + Use the `isSubset(of:)` method to determine whether all of the values of a set are contained in the specified set. + Use the `isSuperset(of:)` method to determine whether a set contains all of the values in a specified set. + Use the `isStrictSubset(of:)` or `isStrictSuperset(of:)` methods to determine whether a set is a subset or superset, but not equal to, a specified set. + Use the `isDisjoint(with:)` method to determine whether two sets have any values in common.

```swiftlet houseAnimals: Set = ["🐶", "🐱"]let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]let cityAnimals: Set = ["🐦", "🐭"] houseAnimals.isSubset(of: farmAnimals)// truefarmAnimals.isSuperset(of: houseAnimals)// truefarmAnimals.isDisjoint(with: cityAnimals)// true
```
### Dictionaries
A *dictionary* stores associations between keys of the same type and values of the same type in a collection with no defined ordering. Each value is associated with a unique `key`, which acts as an identifier for that value within the dictionary. Unlike items in an array, items in a dictionary do not have a specified order. You use a dictionary when you need to look up values based on their identifier, in much the same way that a real-world dictionary is used to look up the definition for a particular word.
> **Примітка**
> 
> Swift’s `Dictionary` type is bridged to Foundation’s `NSDictionary` class.For more information about using `Dictionary` with Foundation and Cocoa, see Working with Cocoa Data Types in *Using Swift with Cocoa and Objective-C (Swift 3.0.1)*.#### Dictionary Type Shorthand Syntax
The type of a Swift dictionary is written in full as `Dictionary<Key, Value>`, where `Key` is the type of value that can be used as a dictionary key, and `Value` is the type of value that the dictionary stores for those keys.
> **Примітка**
> > A dictionary `Key` type must conform to the `Hashable` protocol, like a set’s value type.
 You can also write the type of a dictionary in shorthand form as `[Key: Value]`. Although the two forms are functionally identical, the shorthand form is preferred and is used throughout this guide when referring to the type of a dictionary.
#### Creating an Empty Dictionary
As with arrays, you can create an empty `Dictionary` of a certain type by using initializer syntax:

```swiftvar namesOfIntegers = [Int: String]()// namesOfIntegers is an empty [Int: String] dictionary
```
This example creates an empty dictionary of type `[Int: String]` to store human-readable names of integer values. Its keys are of type `Int`, and its values are of type `String`.
If the context already provides type information, you can create an empty dictionary with an empty dictionary literal, which is written as `[:]` (a colon inside a pair of square brackets):

```swiftnamesOfIntegers[16] = "sixteen"// namesOfIntegers now contains 1 key-value pairnamesOfIntegers = [:]// namesOfIntegers is once again an empty dictionary of type [Int: String]
```
#### Creating a Dictionary with a Dictionary Literal
You can also initialize a dictionary with a *dictionary literal*, which has a similar syntax to the array literal seen earlier. A dictionary literal is a shorthand way to write one or more key-value pairs as a `Dictionary` collection.
A *key-value pair* is a combination of a key and a value. In a dictionary literal, the key and value in each key-value pair are separated by a colon. The key-value pairs are written as a list, separated by commas, surrounded by a pair of square brackets:

<span style="text-align: center;">[<span style="background-color:#E9EFFA">key 1</span>: <span style="background-color:#E9EFFA">value 1</span>, <span style="background-color:#E9EFFA">key 2</span>: <span style="background-color:#E9EFFA">value 2</span>, <span style="background-color:#E9EFFA">key 2</span>: <span style="background-color:#E9EFFA">value 3</span>]</span>The example below creates a dictionary to store the names of international airports. In this dictionary, the keys are three-letter International Air Transport Association codes, and the values are airport names:

```swiftvar airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]```
The `airports` dictionary is declared as having a type of `[String: String]`, which means “a `Dictionary` whose keys are of type `String`, and whose values are also of type `String`”.
> **Примітка**
> > The `airports` dictionary is declared as a variable (with the `var` introducer), and not a constant (with the `let` introducer), because more airports are added to the dictionary in the examples below.
The `airports` dictionary is initialized with a dictionary literal containing two key-value pairs. The first pair has a key of `"YYZ"` and a value of `"Toronto Pearson"`. The second pair has a key of `"DUB"` and a value of `"Dublin"`.
This dictionary literal contains two `String: String` pairs. This key-value type matches the type of the `airports` variable declaration (a dictionary with only `String` keys, and only `String` values), and so the assignment of the dictionary literal is permitted as a way to initialize the `airports` dictionary with two initial items.
As with arrays, you don’t have to write the type of the dictionary if you’re initializing it with a dictionary literal whose keys and values have consistent types. The initialization of `airports` could have been written in a shorter form instead:

```swiftvar airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```
Because all keys in the literal are of the same type as each other, and likewise all values are of the same type as each other, Swift can infer that `[String: String]` is the correct type to use for the `airports` dictionary.
#### Accessing and Modifying a Dictionary
You access and modify a dictionary through its methods and properties, or by using subscript syntax.
As with an array, you find out the number of items in a `Dictionary` by checking its read-only `count` property:

```swiftprint("The airports dictionary contains \(airports.count) items.")// Надрукує "The airports dictionary contains 2 items."
```
Use the Boolean `isEmpty` property as a shortcut for checking whether the `count` property is equal to `0`:

```swiftif airports.isEmpty {    print("The airports dictionary is empty.")} else {    print("The airports dictionary is not empty.")}// Надрукує "The airports dictionary is not empty."
```
You can add a new item to a dictionary with subscript syntax. Use a new key of the appropriate type as the subscript index, and assign a new value of the appropriate type:

```swiftairports["LHR"] = "London"// the airports dictionary now contains 3 items
```
You can also use subscript syntax to change the value associated with a particular key:

```swiftairports["LHR"] = "London Heathrow"// the value for "LHR" has been changed to "London Heathrow"
```
As an alternative to subscripting, use a dictionary’s `updateValue(_:forKey:)` method to set or update the value for a particular key. Like the subscript examples above, the `updateValue(_:forKey:)` method sets a value for a key if none exists, or updates the value if that key already exists. Unlike a subscript, however, the `updateValue(_:forKey:)` method returns the old value after performing an update. This enables you to check whether or not an update took place.
The `updateValue(_:forKey:)` method returns an optional value of the dictionary’s value type. For a dictionary that stores `String` values, for example, the method returns a value of type `String?`, or “optional `String`”. This optional value contains the old value for that key if one existed before the update, or `nil` if no value existed:

```swiftif let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {    print("The old value for DUB was \(oldValue).")}// Надрукує "The old value for DUB was Dublin."
```
You can also use subscript syntax to retrieve a value from the dictionary for a particular key. Because it is possible to request a key for which no value exists, a dictionary’s subscript returns an optional value of the dictionary’s value type. If the dictionary contains a value for the requested key, the subscript returns an optional value containing the existing value for that key. Otherwise, the subscript returns `nil`:

```swiftif let airportName = airports["DUB"] {    print("The name of the airport is \(airportName).")} else {    print("That airport is not in the airports dictionary.")}// Надрукує "The name of the airport is Dublin Airport."
```
You can use subscript syntax to remove a key-value pair from a dictionary by assigning a value of `nil` for that key:

```swiftairports["APL"] = "Apple International"// "Apple International" is not the real airport for APL, so delete itairports["APL"] = nil// APL has now been removed from the dictionary
```
Alternatively, remove a key-value pair from a dictionary with the `removeValue(forKey:)` method. This method removes the key-value pair if it exists and returns the removed value, or returns `nil` if no value existed:

```swiftif let removedValue = airports.removeValue(forKey: "DUB") {    print("The removed airport's name is \(removedValue).")} else {    print("The airports dictionary does not contain a value for DUB.")}// Надрукує "The removed airport's name is Dublin Airport."
```
#### Iterating Over a Dictionary
You can iterate over the key-value pairs in a dictionary with a `for`-`in` loop. Each item in the dictionary is returned as a `(key, value)` tuple, and you can decompose the tuple’s members into temporary constants or variables as part of the iteration:

```swiftfor (airportCode, airportName) in airports {    print("\(airportCode): \(airportName)")}// YYZ: Toronto Pearson// LHR: London Heathrow
```
For more about the for-in loop, see [Цикл For-In](4_control_flow.md#Цикл-For-In).
You can also retrieve an iterable collection of a dictionary’s keys or values by accessing its `keys` and `values` properties:

```swiftfor airportCode in airports.keys {    print("Airport code: \(airportCode)")}// Airport code: YYZ// Airport code: LHR for airportName in airports.values {    print("Airport name: \(airportName)")}// Airport name: Toronto Pearson// Airport name: London Heathrow
```
If you need to use a dictionary’s keys or values with an API that takes an `Array` instance, initialize a new array with the `keys` or `values` property:

```swiftlet airportCodes = [String](airports.keys)// airportCodes is ["YYZ", "LHR"] let airportNames = [String](airports.values)// airportNames is ["Toronto Pearson", "London Heathrow"]
```
Swift’s `Dictionary` type does not have a defined ordering. To iterate over the keys or values of a dictionary in a specific order, use the `sorted()` method on its `keys` or `values` property.