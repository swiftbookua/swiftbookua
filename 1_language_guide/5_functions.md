## Функції

*Функція* - це самодостаній шматок коду, що виконує конкретну задачу. Функціям дають імена, що вказують на призначення функції, і ці імена використовуються для “виклику” функції для виконання її задачі, коли це потрібно. 

Уніфікований синтаксис функцій у мові Swift є достатньо гнучким для вираження будь-якої функції, від простої функції в стилі С без імен параметрів, до складного метода в стилі Objective-C із іменами та мітками аргументів для кожного параметра. Параметри можуть мати значення за замовчанням для спрощення виклику функції. Також, параметри можуть бути двонаправленими, тобто вони можуть не тільки передавати значення всередину функції, а й модифікувати значення змінної, що передається у функцію в якості параметра.

Кожна функція у Swift має тип, що складається із типів параметрів функції та типу, що повертається. Ці типи можна використовувати як і будь-які інші типи у Swift, що полегшує передачу функцій у якості параметрів до інших функцій, та повернення функції з іншої функції. Функції також можна записувати всередині інших функцій, щоб інкапсулювати корисну функціональність всередині контексту певної функції.

### Оголошення та виклик функційПід час оголошення функції, можна (але необов'язково) оголосити одне чи більше іменоване й типізоване значення, котре функція прийматиме на вхід, яке називається *параметром функції*. Також можна (але необов'язково) оголосити тип значення, що буде повертатись із функції після її виконання. Цей тип ми будемо називати *тип, що повертається*.

Кожна функція має *ім'я функції*, котре описує задачу, що виконує функція. Щоб скористатись функцією, слід “викликати” функцію за допомогою її імені, та передавши їй вхідні значення (відомі як *аргументи*), що співпадають за типами із параметрами функції. Агрументи функції повинні завжди йти у тому ж порядку, що й список параметрів функції. 

У наступному прикладі функція має ім'я `greet(person:)` (дослівно, `привітати(особу:)`), бо це власне те, що вона робить: приймає ім'я особи на вхід та повертає привітання для цієї особи. Щоб досягнути цього, оголошується один вхідний параметр – значення типу `String`, що називається `person` – та тип, що повертається, `String`, котрий буде містити привітання для даної особи.

```swiftfunc greet(person: String) -> String {    let greeting = "Вітаємо вас, " + person + "!"    return greeting}
```

Всю цю інформацію згорнуто в оголошення функції, котре починається із ключового слова `func`. Назва типу, що повертає функція, записується після *стрілки повернення* `->` (дефіс та права кутова дужка).

Оголошення описує, що робить дана функція, що вона очікує на вході, і що вона поверне після завершення. Оголошення дозволяє легко однозначно викликати функцію з інших місць у коді: 

```swiftprint(greet(person: "Муся"))// Надрукує "Вітаємо вас, Муся!"print(greet(person: "Гриша"))// Надрукує "Вітаємо вас, Гриша!"
```

Функція `greet(person:)` викликається з передачею в неї значення типу `String`, що вказується після мітки аргумента `person`, наприклад `greet(person: "Муся")`. Оскільки дана функція повертає значення типу `String`, виклик цієї функції можна обгорнути у виклик функції `print(_:separator:terminator:)`, щоб одразу надрукувати і переглянути значення, котре поверне `greet(person:)`.
> **Примітка**
> 
> Функція `print(_:separator:terminator:)` не має мітки для свого першого аргумента, а решта її аргументів необов'язкові, бо мають значення за замовчанням. Ці варіації синтаксису функцій описані нижче у підрозділах [Мітки аргументів функцій та імена їх параметрів](5_functions.md#Мітки-аргументів-функцій-та-імена-їх-параметрів) та [Значення параметрів за замовчуванням](5_functions.md#значення-параметрів-за-замовчуванням).

Тіло функції `greet(person:)` починається із оголошення нової константи типу `String` на ім'я `greeting` та присвоєння їй простого вітального повідомлення. Це повідомлення передається назад із функції за допомогою ключового слова `return`. У рядку коду `return greeting`, функція завершує своє виконання та передає поточне значення константи `greeting`. 

Функцію `greet(person:)` можна викликати багато разів із різними вхідними значеннями. У прикладі вище показано, що відбувається, коли цю функцію викликають із вхідним значенням `"Муся"`, та із вхідним значенням `"Гриша"`. Функція повертає зшите привітання у кожному з випадків. 

Щоб зробити тіло цієї функції кототшим, можна об'єднати створення повідомлення та інструкцію повернення `return` в одному рядку:

```swiftfunc greetAgain(person: String) -> String {    return "Знову вітаємо вас, " + person + "!"}print(greetAgain(person: "Муся"))// Надрукує "Знову вітаємо вас, Муся!"
```

### Параметри функцій та значення, які вони повертають

Параметри функцій та значення, які вони повертають, є надзвичайно гнучкими. Можна оголосити все, що завгодно, від простої допоміжної функції з єдиним неіменованим параметром до складної функції із виразними іменами параметрів та різними опціями параметрів.

#### Функції без параметрів

Функції не обов'язково повинні мати вхідні параметри. Ось приклад функції без вхідних параметрів, котра повертає завжди один і той же рядок при виклику:

```swiftfunc sayHelloWorld() -> String {    return "привіт, світ"}print(sayHelloWorld())// Надрукує "привіт, світ"
```

В оголошенні функції все ще треба вказувати дужки після імені функції, хоч вона і не має ніяких параметрів. При виклику функції також слід вказувати порожні дужки після імені функції. 

#### Функції із кількома параметрами

Функції можуть мати декілька вхідних параметрів, котрі записуються всередині дужок і розділяються комами.

Наступна функція приймає на вхід ім'я особи та булеве значення, яке вказує, чи привітали вже цю особу; функція найбільш доречне привітання для цієї особи:

```swiftfunc greet(person: String, alreadyGreeted: Bool) -> String {    if alreadyGreeted {        return greetAgain(person: person)    } else {        return greet(person: person)    }}print(greet(person: "Сергій", alreadyGreeted: true))// Надрукує "Знову вітаємо вас, Сергій!"
```

При виклику функції `greet(person:alreadyGreeted:)` до неї передаються агрумент типу `String` з міткою `person`, та аргумент типу `Bool` з міткою `alreadyGreeted`, при цьому ці аргументи записуються в дужках та розділяються комою. Слід помітити, що дана функція не співпадає із функцією `greet(person:)` з попереднього підрозділу. Хоч обидві функції і мають імена, що починаються із `greet`, функція `greet(person:alreadyGreeted:)` приймає два аргументи, тоді як функція `greet(person:)` приймає лише один аргумент. 

#### Функції, що не повертають значення

Функції не обов'язково повинні повертати значення. Ось приклад функції `greet(person:)`, котра друкує рядок із привітанням замість того, щоб повертати його:

```swiftfunc greet(person: String) {    print("Вітаємо вас, \(person)!")}greet(person: "Людмила")// Надрукує "Вітаємо вас, Людмила!"
```

Оскільки функція не повертає жодного значення, в оголошенні функції пропущено стрілку повертання та тип, що повертається. 
> **Примітка**
> 
> Строго кажучи, дана версія функції `greet(person:)` повертає значення, хоч воно і не визначене явно. Функції, що не оголошують тип, що повертається, мають особливий тип, що повертається: `Void`. Цей тип у Swift є просто порожнім кортежем, що записується як `()`.

Значення, що повертається функцією, можна проігнорувати при її виклику:
```swiftfunc printAndCount(string: String) -> Int {    print(string)    return string.characters.count}func printWithoutCounting(string: String) {    let _ = printAndCount(string: string)}printAndCount(string: "привіт, світ")// Надрукує "привіт, світ" та поверне значення 12printWithoutCounting(string: "привіт, світ")// Надрукує "привіт, світ" та не поверне жодного значення
```

Перша функція, `printAndCount(string:)`, надрукує рядок, і потім поверне кількість символів у ньому у вигляді числа типу `Int`. Друга функція, `printWithoutCounting(string:)`, викличе першу функцію, але проігнорує значення, котре поверне перша функція. Коли викликається друга функція, першою фукнкцією буде надруковано повідомлення, проте повернуте нею значення не буде використано. 
> **Примітка**
> 
> Значення, що повертається функцією, може бути проігноровано, але якщо оголошення функції каже, що повинно повертатись значення, то функція повинна завжди повертати значення. Функція із оголошеним типом значення, що повертається, не може дозволити виконанню коду дійти до кінця тіла функції та не повернути жодного значення. Спроба написати таку функцію виллється у помилку часу компіляції.### Функції, що повертають кілька значеньЩоб повернути одразу декілька значень із одної функції, можна об'єднати ці значення в кортеж і повертати його як об'єднане значення.

Наступний приклад демонструє оголошення функції на ім'я `minMax(array:)`, котра знаходить найменше та найбільше число в масиві цілочисельних значень типу `Int`:

```swiftfunc minMax(array: [Int]) -> (min: Int, max: Int) {    var currentMin = array[0]    var currentMax = array[0]    for value in array[1..<array.count] {        if value < currentMin {            currentMin = value        } else if value > currentMax {            currentMax = value        }    }    return (currentMin, currentMax)}
```

Функція `minMax(array:)` повертає кортеж, що містить два цілих значення `Int`. Ці значення іменуються `min` та `max`, тому їх можна дістати з кортежу, що повертає дана функція, за допомогою цих імен.

Тіло функції `minMax(array:)` починає роботу із присвоєння двом робочим змінним `currentMin` та `currentMax` значення першого елементу в масиві. Далі функція ітерує решту значень у масиві, та перевіряє кожне значення, чи воно менше за `currentMin` або більше за `currentMax`. Таким чином обчислюються загальні мінімум та максимум даного масиву, які повертаються як кортеж значень `Int`.

Оскільки значення елементів кортежу було проіменовано в оголошенні типу, що повертає функція, їм можна отримати за допомогою наступного синтаксису: крапа + ім'я елементу. Ось приклад звертання до іменованих елементів кортежу:

```swiftlet bounds = minMax(array: [8, -6, 2, 109, 3, 71])print("min = \(bounds.min); max = \(bounds.max)")// Надрукує "min = -6; max = 109"
```

Слід зазначити, що елементи кортежу не обов'язково іменувати в момент повернення із функції, бо їх імена вже вказано в оголошенні типу, що повертає дана функція. 

#### Опціональні кортежі у якості значеннь, що повертають функції

Нехай ми маємо функцію, що повертає кортеж. Якщо існує ймовірність, що ця функція може не повернути жодного значення, слід використовувати *опціональний* кортеж як тип значення, що повертається, і таким чином відобразити факт, що весь кортеж може мати значення `nil`. Щоб записати опціональний кортеж як значення, що повертає функція, слід додати знак питання після дужки, що закриває кортеж. Наприклад: `(Int, Int)?` або `(String, Int, Bool)?`.> **Примітка**
> 
> Опціональний кортеж, як наприклад `(Int, Int)?`, відрізняється від кортежу, що містить опціональний тип,  як наприклад `(Int?, Int?)`. У першому випадку весь кортеж є опціональним, тоді як у другому випадку опціональним є лише окреме значення в кортежі.

Функція `minMax(array:)` з прикладу вище повертає кортеж із двох значень `Int`. Однак, дана функція не робить жодних перевірок безпеки на масиві, що їй передається. Якщо аргумент `array` буде містити порожній масив, функція `minMax(array:)` в оголошенні вище спровокує помилку часу виконання при спробі доступу до `array[0]`.

Щоб обробити ситуацію з порожнім масивом, напишемо нову функцію `minMax(array:)`, котра буде повертати опціональний кортеж, або `nil`, коли масив є порожнім:

```swiftfunc minMax(array: [Int]) -> (min: Int, max: Int)? {    if array.isEmpty { return nil }    var currentMin = array[0]    var currentMax = array[0]    for value in array[1..<array.count] {        if value < currentMin {            currentMin = value        } else if value > currentMax {            currentMax = value        }    }    return (currentMin, currentMax)}
```

Для перевірки, чи повернула функція `minMax(array:)` власне значення чи `nil`, можна використати прив'язування опціоналів:

```swiftif let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {    print("min = \(bounds.min); max = \(bounds.max)")}// Надрукує "min = -6; max = 109"
```### Мітки аргументів функцій та імена їх параметрів### Function Argument Labels and Parameter Names
Each function parameter has both an *argument label* and a *parameter name*. The argument label is used when calling the function; each argument is written in the function call with its argument label before it. The parameter name is used in the implementation of the function. By default, parameters use their parameter name as their argument label.

```swiftfunc someFunction(firstParameterName: Int, secondParameterName: Int) {    // In the function body, firstParameterName and secondParameterName    // refer to the argument values for the first and second parameters.}someFunction(firstParameterName: 1, secondParameterName: 2)
```
All parameters must have unique names. Although it’s possible for multiple parameters to have the same argument label, unique argument labels help make your code more readable.
#### Specifying Argument Labels
You write an argument label before the parameter name, separated by a space:

```swiftfunc someFunction(argumentLabel parameterName: Int) {    // In the function body, parameterName refers to the argument value    // for that parameter.}
```
Here’s a variation of the `greet(person:)` function that takes a person’s name and hometown and returns a greeting:

```swiftfunc greet(person: String, from hometown: String) -> String {    return "Hello \(person)!  Glad you could visit from \(hometown)."}print(greet(person: "Bill", from: "Cupertino"))// Надрукує "Hello Bill!  Glad you could visit from Cupertino."
```
The use of argument labels can allow a function to be called in an expressive, sentence-like manner, while still providing a function body that is readable and clear in intent.
#### Omitting Argument Labels
If you don’t want an argument label for a parameter, write an underscore (`_`) instead of an explicit argument label for that parameter.

```swiftfunc someFunction(_ firstParameterName: Int, secondParameterName: Int) {    // In the function body, firstParameterName and secondParameterName    // refer to the argument values for the first and second parameters.}someFunction(1, secondParameterName: 2)
```
If a parameter has an argument label, the argument `must` be labeled when you call the function.

### Значення параметрів за замовчуваннямDefault Parameter Values
You can define a *default value* for any parameter in a function by assigning a value to the parameter after that parameter’s type. If a default value is defined, you can omit that parameter when calling the function.

```swiftfunc someFunction(parameterWithoutDefault: Int, parameterWithDefault: Int = 12) {    // If you omit the second argument when calling this function, then    // the value of parameterWithDefault is 12 inside the function body.}someFunction(parameterWithoutDefault: 3, parameterWithDefault: 6) // parameterWithDefault is 6someFunction(parameterWithoutDefault: 4) // parameterWithDefault is 12
```
Place parameters that don’t have default values at the beginning of a function’s parameter list, before the parameters that have default values. Parameters that don’t have default values are usually more important to the function’s meaning—writing them first makes it easier to recognize that the same function is being called, regardless of whether any default parameters are omitted.
#### Variadic Parameters
A *variadic parameter* accepts zero or more values of a specified type. You use a variadic parameter to specify that the parameter can be passed a varying number of input values when the function is called. Write variadic parameters by inserting three period characters (`...`) after the parameter’s type name.
The values passed to a variadic parameter are made available within the function’s body as an array of the appropriate type. For example, a variadic parameter with a name of `numbers` and a type of `Double...` is made available within the function’s body as a constant array called `numbers` of type `[Double]`.
The example below calculates the *arithmetic mean* (also known as the *average*) for a list of numbers of any length:

```swiftfunc arithmeticMean(_ numbers: Double...) -> Double {    var total: Double = 0    for number in numbers {        total += number    }    return total / Double(numbers.count)}arithmeticMean(1, 2, 3, 4, 5)// returns 3.0, which is the arithmetic mean of these five numbersarithmeticMean(3, 8.25, 18.75)// returns 10.0, which is the arithmetic mean of these three numbers
```
> **Примітка**
> > A function may have at most one variadic parameter.
 
#### Двонаправлені параметри#### In-Out Parameters
Function parameters are constants by default. Trying to change the value of a function parameter from within the body of that function results in a compile-time error. This means that you can’t change the value of a parameter by mistake. If you want a function to modify a parameter’s value, and you want those changes to persist after the function call has ended, define that parameter as an *in-out parameter* instead.
You write an in-out parameter by placing the `inout` keyword right before a parameter’s type. An in-out parameter has a value that is passed *in* to the function, is modified by the function, and is passed back *out* of the function to replace the original value. For a detailed discussion of the behavior of in-out parameters and associated compiler optimizations, see [In-Out Parameters]().
You can only pass a variable as the argument for an in-out parameter. You cannot pass a constant or a literal value as the argument, because constants and literals cannot be modified. You place an ampersand (&) directly before a variable’s name when you pass it as an argument to an in-out parameter, to indicate that it can be modified by the function.
> **Примітка**
> > In-out parameters cannot have default values, and variadic parameters cannot be marked as `inout`.
 Here’s an example of a function called `swapTwoInts(_:_:)`, which has two in-out integer parameters called `a` and `b`:

```swiftfunc swapTwoInts(_ a: inout Int, _ b: inout Int) {    let temporaryA = a    a = b    b = temporaryA}
```
The `swapTwoInts(_:_:)` function simply swaps the value of b into a, and the value of a into b. The function performs this swap by storing the value of a in a temporary constant called `temporaryA`, assigning the value of `b` to `a`, and then assigning `temporaryA` to `b`.
You can call the `swapTwoInts(_:_:)` function with two variables of type `Int` to swap their values. Note that the names of `someInt` and `anotherInt` are prefixed with an ampersand when they are passed to the `swapTwoInts(_:_:)` function:

```swiftvar someInt = 3var anotherInt = 107swapTwoInts(&someInt, &anotherInt)print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")// Надрукує "someInt is now 107, and anotherInt is now 3"
```
The example above shows that the original values of `someInt` and `anotherInt` are modified by the `swapTwoInts(_:_:)` function, even though they were originally defined outside of the function.> **Примітка**
> > In-out parameters are not the same as returning a value from a function. The `swapTwoInts` example above does not define a return type or return a value, but it still modifies the values of `someInt` and `anotherInt`. In-out parameters are an alternative way for a function to have an effect outside of the scope of its function body.
### Function Types
Every function has a specific *function type*, made up of the parameter types and the return type of the function.For example:

```swiftfunc addTwoInts(_ a: Int, _ b: Int) -> Int {    return a + b}
func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {    return a * b}
```
This example defines two simple mathematical functions called `addTwoInts` and `multiplyTwoInts`. These functions each take two `Int` values, and return an `Int` value, which is the result of performing an appropriate mathematical operation.The type of both of these functions is `(Int, Int) -> Int`. This can be read as:“A function type that has two parameters, both of type `Int`, and that returns a value of type `Int`.”
Here’s another example, for a function with no parameters or return value:

```swiftfunc printHelloWorld() {    print("hello, world")}
```
The type of this function is `() -> Void`, or “a function that has no parameters, and returns `Void`.”
#### Using Function Types
You use function types just like any other types in Swift. For example, you can define a constant or variable to be of a function type and assign an appropriate function to that variable:

```swiftvar mathFunction: (Int, Int) -> Int = addTwoInts
```
This can be read as:
“Define a variable called `mathFunction`, which has a type of ‘a function that takes two `Int` values, and returns an `Int` value.’ Set this new variable to refer to the function called `addTwoInts`.”
The `addTwoInts(_:_:)` function has the same type as the `mathFunction` variable, and so this assignment is allowed by Swift’s type-checker.
You can now call the assigned function with the name `mathFunction`:

```swiftprint("Result: \(mathFunction(2, 3))")// Надрукує "Result: 5"
```
A different function with the same matching type can be assigned to the same variable, in the same way as for non-function types:

```swiftmathFunction = multiplyTwoIntsprint("Result: \(mathFunction(2, 3))")// Надрукує "Result: 6"
```
As with any other type, you can leave it to Swift to infer the function type when you assign a function to a constant or variable:

```swiftlet anotherMathFunction = addTwoInts// anotherMathFunction is inferred to be of type (Int, Int) -> Int
```
#### Function Types as Parameter Types
You can use a function type such as `(Int, Int) -> Int` as a parameter type for another function. This enables you to leave some aspects of a function’s implementation for the function’s caller to provide when the function is called.
Here’s an example to print the results of the math functions from above:

```swiftfunc printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {    print("Result: \(mathFunction(a, b))")}printMathResult(addTwoInts, 3, 5)// Надрукує "Result: 8"
```
This example defines a function called `printMathResult(_:_:_:)`, which has three parameters. The first parameter is called `mathFunction`, and is of type `(Int, Int) -> Int`. You can pass any function of that type as the argument for this first parameter. The second and third parameters are called `a` and `b`, and are both of type `Int`. These are used as the two input values for the provided math function.
When `printMathResult(_:_:_:)` is called, it is passed the `addTwoInts(_:_:)` function, and the integer values `3` and `5`. It calls the provided function with the values `3` and `5`, and prints the result of `8`.
The role of `printMathResult(_:_:_:)` is to print the result of a call to a math function of an appropriate type. It doesn’t matter what that function’s implementation actually does—it matters only that the function is of the correct type. This enables `printMathResult(_:_:_:)` to hand off some of its functionality to the caller of the function in a type-safe way.
#### Function Types as Return Types
You can use a function type as the return type of another function. You do this by writing a complete function type immediately after the return arrow (`->`) of the returning function.
The next example defines two simple functions called `stepForward(_:)` and `stepBackward(_:)`. The `stepForward(_:)` function returns a value one more than its input value, and the `stepBackward(_:)` function returns a value one less than its input value. Both functions have a type of `(Int) -> Int`:

```swiftfunc stepForward(_ input: Int) -> Int {    return input + 1}
func stepBackward(_ input: Int) -> Int {    return input - 1}
```Here’s a function called `chooseStepFunction(backward:)`, whose return type is `(Int) -> Int`. The `chooseStepFunction(backward:)` function returns the `stepForward(_:)` function or the `stepBackward(_:)` function based on a `Boolean` parameter called backward:

```swiftfunc chooseStepFunction(backward: Bool) -> (Int) -> Int {    return backward ? stepBackward : stepForward}
```
You can now use `chooseStepFunction(backward:)` to obtain a function that will step in one direction or the other:

```swiftvar currentValue = 3let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)// moveNearerToZero now refers to the stepBackward() function
```
The preceding example determines whether a positive or negative step is needed to move a variable called `currentValue` progressively closer to zero. `currentValue` has an initial value of `3`, which means that `currentValue > 0` returns `true`, causing `chooseStepFunction(backward:)` to return the `stepBackward(_:)` function. A reference to the returned function is stored in a constant called `moveNearerToZero`.
Now that `moveNearerToZero` refers to the correct function, it can be used to count to zero:

```swiftprint("Counting to zero:")// Counting to zero:while currentValue != 0 {    print("\(currentValue)... ")    currentValue = moveNearerToZero(currentValue)}print("zero!")// 3...// 2...// 1...// zero!
```
### Nested Functions
All of the functions you have encountered so far in this chapter have been examples of *global functions*, which are defined at a global scope. You can also define functions inside the bodies of other functions, known as *nested functions*.
Nested functions are hidden from the outside world by default, but can still be called and used by their enclosing function. An enclosing function can also return one of its nested functions to allow the nested function to be used in another scope.
You can rewrite the `chooseStepFunction(backward:)` example above to use and return nested functions:

```swiftfunc chooseStepFunction(backward: Bool) -> (Int) -> Int {    func stepForward(input: Int) -> Int { return input + 1 }    func stepBackward(input: Int) -> Int { return input - 1 }    return backward ? stepBackward : stepForward}var currentValue = -4let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)// moveNearerToZero now refers to the nested stepForward() functionwhile currentValue != 0 {    print("\(currentValue)... ")    currentValue = moveNearerToZero(currentValue)}print("zero!")// -4...// -3...// -2...// -1...// zero!
```