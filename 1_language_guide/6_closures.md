## Замикання

*Замикання* – це самодостатні блоки функціональності, що можуть передаватись та використовуватись у коді. Замикання у Swift є аналогічними блокам в C та Objective-C, і лямбдам в інших мовах програмування.

Замикання можуть захоплювати і зберігати посилання на константи та змінні з контексту, в якому вони оголошені. Цю поведінку називають *замиканням на* константи та змінні. Swift бере на себе все управління пам'яттю при захопленні змінних.

> **Note**
> 
> Не слід переживати, якщо ви не знайомі з концепцією захоплення. Вона детально описана нижче у підрозділі [Захоплення значень](#Захоплення-значень).

Глобальні та вкладені функції, які представлені в попередньому розділі [Функції](5_functions.md), є особливими випадками замикань. Загалом, замикання можуть приймати одну із трьох форм:

 + Глобальні функції є замиканнями, що мають ім'я та не захоплюють значень.
 + Вкладені функції є замиканнями, що мають ім'я та захоплюють значення із функції, де їх оголошено.
 + Вирази замикань є безіменними замиканнями, що записуються за допомогою легковісного синтаксису та можуть захоплювати значення із довколишнього контексту.

У Swift вирази замикань мають чистий, зрозумілий стиль, з оптимізаціями, котрі заохочують которкий, незахаращений синтаксис у найчастіших сценаріях. Ці оптимізації включають:

 + Виведення типів параметрів та значення, що повертається, з контексту
 + Неявний `return` у замиканнях, що складаються з одного рядка
 + Скорочений запис імен аргументів
 + Синтаксис прикінцевих замикань

### Вирази замикань

Вкладені функції, як показано у підрозділі [Вкладені функції](5_functions.md#Вкладені-функції), є зручним засобом визначення та іменування самодостатніх блоків коду в якості частини більшої функції. Однак, іноді корисно писати коротші версії функцієподібних конструктів без повного оголошення та імені. Це найбільш доречно при роботі із функціями чи методами, що приймають інші функції в якості одного чи кілької аргументів.

Вирази замикань є способом запису замикання по місцю використання за допомогою короткого, зосередженого синтаксису. Синтаксис виразів замикань має кілька оптимізацій, потрібних для запису замикань у найкоротшій формі без втрати ясності чи наміру. Приклади виразів замикань нижче ілюструють ці оптимізації шляхом удосконалення одного прикладу із методом `sorted(by:)` протягом кількох ітерацій, у кожній з яких одна й та ж функціональність буде записана у більш лаконічний спосіб.

#### Метод Sorted

Стандартна бібліотека Swift надає метод на ім'я `sorted(by:)`, котрий сортує масив значень відомого типу, в залежності від результату сортуючого замикання, що ви надаєте. Після завершення процесу сортування, метод `sorted(by:)` повертає новий масив також ж типу і розміру, як і старий, із елементами, котрі відсортовані у відповідному порядку. Початковий масив методом `sorted(by:)` не змінюється.

У прикладі нижче використовується метод `sorted(by:)` для сортування масиву значень `String` у зворотньому алфавітному порядку. Ось початковий масив для сортування:

The closure expression examples below use the `sorted(by:)` method to sort an array of `String` values in reverse alphabetical order. Here’s the initial array to be sorted:

```swiftlet names = ["В'ячеслав", "Сергій", "Анна", "Ярослав", "Святослав"]
```

Метод `sorted(by:)` приймає замикання, що бере два аргументи того ж типу, що й вміст масиву, та повертає булеве значення, котре вказує, чи є повинно йти перше значення перед другим після сортування масиву. Замикання, що сортує, повинно поветрати `true`, якщо перше значення повинно йти *перед* другим значенням, і `false` в інакшому випадку.

У цьому прикладі відсортовуєтсья масив значень типу `String`, і тому типом замикання, що сортує, має бути функція типу `(String, String) -> Bool`.

Одним із способів надати замикання, що сортує, є написання звичайної функції коректного типу, і передача її як аргумент в метод `sorted(by:)`:

```swiftfunc backward(_ s1: String, _ s2: String) -> Bool {    return s1 > s2}var reversedNames = names.sorted(by: backward)// reversedNames дорівнює ["Ярослав", "Сергій", "Святослав", "В'ячеслав", "Анна"]
```

Якщо перший рядок (`s1`) більший за другий рядок (`s2`), функція `backward(_:_:)` поверне `true`, вказуючи, що у відсортованому масиві рядок `s1` повинен іти перед рядком `s2`. Для символів у рядку, “більше ніж” означає “з'являється у алфафілі після”. Це означає, що літера `"Б"` є “більшою ніж” літера `"А"`, а рядок `"віл"` є більшим за рядок `"вал"`. Це й призводить до зворотнього алфавітного сортування, де `"Богдан"` знаходиться перед `"Анна"`, і так далі.

Однак, це досить довготривалий шлях для запису того, що є по суті функцією з одного виразу (`a > b`). В цьому прикладі було би доречніше записати замикання, що сортує, одразу в місці використання, використовуючи синтаксис виразів замикань.
#### Синтаксис виразів замикань

Синтаксис виразів замикань має наступний загальний вигляд:

```swift{ (<параметри>) -> <тип, що повертається> in    <інструкції>}
```

*Параметри* у виразах замикань можуть бути двонаправленими, але не можуть мати значень за замовчанням. Варіативні параметри також можуть використовуватись у виразах замикань. Кортежі можуть використовуватись і як параметри, і як значення, що повертається.

У наступному прикладі продемонстровано вираз замикання замість функції `backward(_:_:)` з попереднього прикладу:

```swiftreversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in    return s1 > s2})
```

Слід помітити, що оголошення параметрів та типу, що повертається, для цього замикання є ідентичним до такого ж оголошення в функції `backward(_:_:)`. В обох випадках, це записується як `(s1: String, s2: String) -> Bool`. Однак, для виразів замикань, оголошених по місцю використання, параметри та значення, що повертається, записуються *всередині* фігурних дужок, а не поза ними.

Початок тіла замикання вводиться ключовим словом `in`. Це ключове слово розмежовує кінець оголошенню параметрів замикання та типу, що воно повертає, та початок тіла замикання.

Оскільки тіло цього замикання є досить коротке, його можна навіть записати в один рядок:

```swiftreversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

Приклад вище ілюструє, що весь виклик методу `sorted(by:)` залишився без змін. Круглі дужки досі огортають весь аргумент цього методу. Однак, цей аргумент тепер є замиканням, оголошеним по місцю використання.
#### Визначення типу з контексту

Оскільки замикання, що сортує, передається як аргумент в метод, Swift може визначити типи його параметрів та тип, що повертається. Метод `sorted(by:)` викликається на масиві рядків, тому його аргумент повинен бути функцією типу `(String, String) -> Bool`. Це означає, що типи `(String, String)` та `Bool` не обов'язково треба записувати в оголошенні виразу замикання. Оскільки всі ці типи можуть бути визначеними, стрілка повертання та дужки довкола імен параметрів теж можна пропустити:

```swiftreversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```

При передачі замикання до функції чи методу у виглиді виразу замикання, оголошеного по місцю використання, завжди можна визначити типи його параметрів та тип, що ним повертається. В результаті цього, при передачі замикання до функції чи методу в якості аргумента, ніколи намає необхідності записувати замикання, оголошене по місця використання, у найповнішій його формі.

Тим не менше, якщо ви бажаєте вказувати типи явно, ви завжди можете це зробити, і у вападках, коли це допоможе читачу вашого коду уникнути неоднозначності – це дійсно варто робити. У випадку методу `sorted(by:)`, мета замикання випливає з факту, що відбувається сортування, і тому читач може спокійно припустити, що дане замикання буде працювати зі значеннями типу `String`, оскільки це замикання допомагає сортувати масив рядків. 

#### Неявний `return` у замиканнях, що складаються з одного рядка

Однорядкові замикання можуть повертати результат свого єдиного виразу неявно, пропускаючи ключове слово `return` у їх оголошенні, як видно у наступній версії попереднього прикладу:

```swiftreversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
```

Тут, функціональний тип аргумента метода `sorted(by:)` дає зрозуміти, що замикання повинно повернути значення типу `Bool`. Оскільки тіло замикання складається з єдиного виразу (`s1 > s2`), що повертає значення типу `Bool`, тут відсутня неоднозначність і ключове слово `return` можна пропустити.

#### Скорочений запис імен аргументів

Swift автоматично надає імена аргументів замиканням, оголошеним по місцю використання. Ці імена виглядають як `$0`, `$1`, `$2`, і так далі, і їх можна використовувати для посилання на значення аргументів замикання. 

При використанні скороченого запису імен агрументів у виразах замикань, можна пропускати список аргументів у їх оголошеннях. Кількість аргументів та їх типи будуть виведені компілятором з відповідного функціонального типу. Ключове слово `in` в таких випадках теж можна пропустити, оскільки вираз замикання складатиметься лише з його тіла:

```swiftreversedNames = names.sorted(by: { $0 > $1 } )
```

В даному випадку, `$0` та `$1` посилаються на перший та другий аргументи типу `String`.
#### Методи-оператори

Насправді, є навіть *коротший* спосіб записати вираз замикання з прикладу вище. У Swift, тип `String` визначає спеціфичну для рядків реалізацію оператору більше (`>`) як метод, що має два параметри типу `String`, та повертає значення типу `Bool`. Це точно співпадає з типом, котрий потребує метод `sorted(by:)`. Таким чином, можна просто передати оператор більше, і Swift автоматично визначить, що потрібно використати саме специфічну для рядків його реалізацію:

```swiftreversedNames = names.sorted(by: >)
```

Детальніше з методами-операторами можна ознайомитись у розділі [Методи-оператори](24_advanced_operators.md#Методи-оператори).

### Синтаксис прикінцевих замикань

Якщо потрібно передати вираз замикання до функції її останнім аргументом, а вираз замикання досить довгий, зручно використовувати синтаксис прикінцевих замикань. Прикінцеве замикання записується після дужок виклику функції, хоч воно і є фактично аргументом функції. При використанні синтаксису прикінцевих замикань, не потрібно вказувати мітку аргумента для цього замикання у виклику функції. 

```swiftfunc someFunctionThatTakesAClosure(closure: () -> Void) {    // Тут йде тіло функції} 
// Ось як виглядає виклик функції без використання синтаксису прикінцевого замикання: someFunctionThatTakesAClosure(closure: {    // Тут йде тіло замикання}) // Ось як виглядає виклик функції із використанням синтаксису прикінцевого замикання: someFunctionThatTakesAClosure() {    // Тут йде тіло прикінцевого замикання}
```

Замикання, що сортує, з прикладу із підрозділу [Синтаксис виразів замикань](6_closures.md#Синтаксис-виразів-замикань), можна записати зовні дужок методу `sorted(by:)` як прикінцеве замикання:

```swiftreversedNames = names.sorted() { $0 > $1 }
```

Якщо вираз замикання є єдиним аргументом, що передається до функції чи методу, і воно записується у вигляді прикінцевого замикання, то й навіть дужки `()` після імені функції чи методу вказувати необов'язково:

```swiftreversedNames = names.sorted { $0 > $1 }
```

Прикінцеві замикання є найбільш доречними, коли тіло замикання є досить довгим, і його не можна записати одним рядком. Наприклад, у масивах в Swift є метод `map(_:)`, що приймає вираз замикання як єдиний аргумент. Замикання викликається по одному разу для кожного елементу масиву, і повертає відображене значення (можливо, іншого типу) для цього елемента. Природа цього відображення та тип, що повертається має визначатись самим замиканням. 

Після застосування замикання до кожного елемента масиву, метод `map(_:)` повертає новий масив, що містить всі нові відображені значення, в тому ж порядку, що й відповідні значення в початковому масиві.

Ось приклад того, як використовуючи метод `map(_:)` з прикінцевими замиканнями, можна перетворити масив цілих чисел у масив рядків таким чином: масив `[16, 58, 510]` використовується для створення нового масиву `["ОдинШість", "П'ятьВісім", "П'ятьОдинНуль"]`:

```swiftlet digitNames = [    0: "Нуль",  1: "Один",  2: "Два", 3: "Три",   4: "Чотири",    5: "П'ять", 6: "Шість", 7: "Сім", 8: "Вісім", 9: "Дев'ять"]let numbers = [16, 58, 510]
```

У коді вище створено словник відображень між цифрами та україномовними версіями їх назв. Також створено масив чисел, готовий для конвертації в рядки.

Тепер з масиву `numbers` можна створити масив значень типу `String`, передаючи вираз замикання до методу масива `map(_:)` як прикінцеве замикання:

```swiftlet strings = numbers.map { 
    (number) -> String in    var number = number    var output = ""    repeat {        output = digitNames[number % 10]! + output        number /= 10    } while number > 0    return output}// тип масиву strings визначено як [String]// його значення - ["ОдинШість", "П'ятьВісім", "П'ятьОдинНуль"]
```

Метод `map(_:)` викликає замикання по одному разу для кожного елементу масиву. Немає необхідності вказувати тип вхідного параметра замикання (`number`), бо його можна визначити з типу значень масиву, елементи якого відображаються. 

В даному прикладі, змінна `number` ініціалізується значенням параметру замикання `number`, для того, щоб це значення можна було змінити в тілі замикання. (Параметри функції та замикать є завжди константами). В оголошенні виразу замикання також вказується тип, що повертається – `String` – по ньому визначається тип значень, що зберігатимуться в новому масиві. 

Вираз замикання будує рядок на ім'я `output` при кожному виклику. Він обчислює останню цифру числа `number` за допомогою оператора остачі від ділення (`number % 10`), і використовує цю цифру для знаходження у словнику `digitNames` рядка, що їй відповідає. Дане замикання можна використовувати для створення текстового представлення будь-якого додатнього цілого числа.
> **Note**
> 
> Після виклику індексу словника `digitNames` вказано знак оклику (`!`), бо індекси словників повертають опціональні значення, щоб вказати, що пошук у словнику може не вдатись, коли у ньому не існує даний ключ. У прикладі вище, значення `number % 10` буде гарантовано коректним індексом для словника `digitNames`, тому знак оклику використовується для примусового розгортання значення `String`, що зберігається в опціоналі, котрий повертає індекс. 

Рядок, що дістається зі словника `digitNames`, вставляється в *початок* змінної `output`, таким чином рядкова версія числа будується задом наперед. (Вираз `number % 10` дає значення `6` для `16`, `8` для `58`, та `0` для `510`).

Далі, змінна `number` ділиться на `10`. Оскільки це ціле число, воно округлюється вниз після ділення, тому `16` стає `1`, `58` стає `5`, і `510` стає `51`.

Процес повторюється допоки значення `number` не досягне `0`, після чого рядок `output` повертається із замикання, і додається до нового масиву всередині методу `map(_:)`.

Використання синтаксису прикінцевих замикань у прикладі вище акуратно інкапсулює функціональність замикання одразу після функції, що використовує це замикання, без необхідності обгортати все замикання дужками виклику методу `map(_:)`.### Захоплення значеньЗамикання можуть *захоплювати* константи та змінні із довколишнього контексту. Замикання можуть посилатись на значення цих констант та змінних, змінювати їх, навіть якщо більше не існує початкового контексту, де було оголошено ці константи чи змінні.
In Swift, the simplest form of a closure that can capture values is a nested function, written within the body of another function. A nested function can capture any of its outer function’s arguments and can also capture any constants and variables defined within the outer function.
Here’s an example of a function called `makeIncrementer`, which contains a nested function called `incrementer`. The nested `incrementer()` function captures two values, `runningTotal` and `amount`, from its surrounding context. After capturing these values, `incrementer` is returned by `makeIncrementer` as a closure that increments `runningTotal` by `amount` each time it is called.

```swiftfunc makeIncrementer(forIncrement amount: Int) -> () -> Int {    var runningTotal = 0    func incrementer() -> Int {        runningTotal += amount        return runningTotal    }    return incrementer}
```
The return type of `makeIncrementer` is `() -> Int`. This means that it returns a *function*, rather than a simple value. The function it returns has no parameters, and returns an `Int` value each time it is called. To learn how functions can return other functions, see [Function Types as Return Types](5_functions.md#Function-Types-as-Return-Types).
The `makeIncrementer(forIncrement:)` function defines an integer variable called `runningTotal`, to store the current running total of the incrementer that will be returned. This variable is initialized with a value of `0`.
The `makeIncrementer(forIncrement:)` function has a single `Int` parameter with an argument label of `forIncrement`, and a parameter name of `amount`. The argument value passed to this parameter specifies how much `runningTotal` should be incremented by each time the returned incrementer function is called. The `makeIncrementer` function defines a nested function called `incrementer`, which performs the actual incrementing. This function simply adds `amount` to `runningTotal`, and returns the result.
When considered in isolation, the nested `incrementer()` function might seem unusual:

```swiftfunc incrementer() -> Int {    runningTotal += amount    return runningTotal}
```
The `incrementer()` function doesn’t have any parameters, and yet it refers to `runningTotal` and `amount` from within its function body. It does this by capturing a *reference* to `runningTotal` and `amount` from the surrounding function and using them within its own function body. Capturing by reference ensures that `runningTotal` and `amount` do not disappear when the call to `makeIncrementer` ends, and also ensures that `runningTotal` is available the next time the `incrementer` function is called.
> **Note**
> > As an optimization, Swift may instead capture and store a *copy* of a value if that value is not mutated by a closure, and if the value is not mutated after the closure is created.
> > Swift also handles all memory management involved in disposing of variables when they are no longer needed.
Here’s an example of `makeIncrementer` in action:

```swiftlet incrementByTen = makeIncrementer(forIncrement: 10)
```
This example sets a constant called `incrementByTen` to refer to an incrementer function that adds `10` to its `runningTotal` variable each time it is called. Calling the function multiple times shows this behavior in action:

```swiftincrementByTen()// returns a value of 10incrementByTen()// returns a value of 20incrementByTen()// returns a value of 30
```
If you create a second incrementer, it will have its own stored reference to a new, separate `runningTotal` variable:

```swiftlet incrementBySeven = makeIncrementer(forIncrement: 7)incrementBySeven()// returns a value of 7
```
Calling the original incrementer (`incrementByTen`) again continues to increment its own `runningTotal` variable, and does not affect the variable captured by `incrementBySeven`:

```swiftincrementByTen()// returns a value of 40
```
> **Note**
> > If you assign a closure to a property of a class instance, and the closure captures that instance by referring to the instance or its members, you will create a strong reference cycle between the closure and the instance. Swift uses capture lists to break these strong reference cycles. For more information, see [Strong Reference Cycles for Closures](15_automatic_reference_counting.md#Strong-Reference-Cycles-for-Closures).
 ### Closures Are Reference Types
In the example above, `incrementBySeven` and `incrementByTen` are constants, but the closures these constants refer to are still able to increment the `runningTotal` variables that they have captured. This is because functions and closures are *reference types*.
Whenever you assign a function or a closure to a constant or a variable, you are actually setting that constant or variable to be a *reference* to the function or closure. In the example above, it is the choice of closure that `incrementByTen` *refers* to that is constant, and not the contents of the closure itself.
This also means that if you assign a closure to two different constants or variables, both of those constants or variables will refer to the same closure:

```swiftlet alsoIncrementByTen = incrementByTenalsoIncrementByTen()// returns a value of 50
```
### Escaping Closures
A closure is said to *escape* a function when the closure is passed as an argument to the function, but is called after the function returns. When you declare a function that takes a closure as one of its parameters, you can write `@escaping` before the parameter’s type to indicate that the closure is allowed to escape.
One way that a closure can escape is by being stored in a variable that is defined outside the function. As an example, many functions that start an asynchronous operation take a closure argument as a completion handler. The function returns after it starts the operation, but the closure isn’t called until the operation is completed — the closure needs to escape, to be called later. For example:

```swiftvar completionHandlers: [() -> Void] = []func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {    completionHandlers.append(completionHandler)}
```
The `someFunctionWithEscapingClosure(_:)` function takes a closure as its argument and adds it to an array that’s declared outside the function. If you didn’t mark the parameter of this function with `@escaping`, you would get a compiler error.
Marking a closure with `@escaping` means you have to refer to `self` explicitly within the closure. For example, in the code below, the closure passed to `someFunctionWithEscapingClosure(_:)` is an escaping closure, which means it needs to refer to `self` explicitly. In contrast, the closure passed to `someFunctionWithNonescapingClosure(_:)` is a nonescaping closure, which means it can refer to `self` implicitly.

```swiftfunc someFunctionWithNonescapingClosure(closure: () -> Void) {    closure()} class SomeClass {    var x = 10    func doSomething() {        someFunctionWithEscapingClosure { self.x = 100 }        someFunctionWithNonescapingClosure { x = 200 }    }} let instance = SomeClass()instance.doSomething()print(instance.x)// Prints "200" completionHandlers.first?()print(instance.x)// Prints "100"
```
### Autoclosures
An *autoclosure* is a closure that is automatically created to wrap an expression that’s being passed as an argument to a function. It doesn’t take any arguments, and when it’s called, it returns the value of the expression that’s wrapped inside of it. This syntactic convenience lets you omit braces around a function’s parameter by writing a normal expression instead of an explicit closure.
It’s common to *call* functions that take autoclosures, but it’s not common to *implement* that kind of function. For example, the `assert(condition:message:file:line:)` function takes an autoclosure for its `condition` and `message` parameters; its condition parameter is evaluated only in debug builds and its `message` parameter is evaluated only if `condition` is `false`.
An autoclosure lets you delay evaluation, because the code inside isn’t run until you call the closure. Delaying evaluation is useful for code that has side effects or is computationally expensive, because it lets you control when that code is evaluated. The code below shows how a closure delays evaluation.

```swiftvar customersInLine = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]print(customersInLine.count)// Prints "5" let customerProvider = { customersInLine.remove(at: 0) }print(customersInLine.count)// Prints "5" print("Now serving \(customerProvider())!")// Prints "Now serving Chris!"print(customersInLine.count)// Prints "4"
```
Even though the first element of the `customersInLine` array is removed by the code inside the closure, the array element isn’t removed until the closure is actually called. If the closure is never called, the expression inside the closure is never evaluated, which means the array element is never removed. Note that the type of `customerProvider` is not `String` but `() -> String` — a function with no parameters that returns a string.
You get the same behavior of delayed evaluation when you pass a closure as an argument to a function.

```swift// customersInLine is ["Alex", "Ewa", "Barry", "Daniella"]func serve(customer customerProvider: () -> String) {    print("Now serving \(customerProvider())!")}serve(customer: { customersInLine.remove(at: 0) } )// Prints "Now serving Alex!"
```
The `serve(customer:)` function in the listing above takes an explicit closure that returns a customer’s name. The version of `serve(customer:)` below performs the same operation but, instead of taking an explicit closure, it takes an autoclosure by marking its parameter’s type with the `@autoclosure` attribute. Now you can call the function as if it took a `String` argument instead of a closure. The argument is automatically converted to a closure, because the `customerProvider` parameter’s type is marked with the `@autoclosure` attribute.

```swift// customersInLine is ["Ewa", "Barry", "Daniella"]func serve(customer customerProvider: @autoclosure () -> String) {    print("Now serving \(customerProvider())!")}serve(customer: customersInLine.remove(at: 0))// Prints "Now serving Ewa!"
```
> **Note**
> > Overusing autoclosures can make your code hard to understand. The context and function name should make it clear that evaluation is being deferred.If you want an autoclosure that is allowed to escape, use both the `@autoclosure` and `@escaping` attributes. The `@escaping` attribute is described above in [Escaping Closures](6_closures.md#escaping-closures).

```swift// customersInLine is ["Barry", "Daniella"]var customerProviders: [() -> String] = []func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {    customerProviders.append(customerProvider)}collectCustomerProviders(customersInLine.remove(at: 0))collectCustomerProviders(customersInLine.remove(at: 0)) print("Collected \(customerProviders.count) closures.")// Prints "Collected 2 closures."for customerProvider in customerProviders {    print("Now serving \(customerProvider())!")}// Prints "Now serving Barry!"// Prints "Now serving Daniella!"
```
In the code above, instead of calling the closure passed to it as its `customerProvider` argument, the `collectCustomerProviders(_:)` function appends the closure to the `customerProviders` array. The array is declared outside the scope of the function, which means the closures in the array can be executed after the function returns. As a result, the value of the `customerProvider` argument must be allowed to escape the function’s scope.