### Функції та Замикання

Використовуйте `func` щоб оголосити функцію. Щоб викликати функцію, вкажіть її ім’я та список аргументів у круглих дужках. Вживайте `->` щоб відділити імена і типи параметрів від типу, котрий повертає функція.
```swift
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet(person: "Bob", day: "Tuesday")
```

> **Ексеримент**
>
> Приберіть параметр `day`. Додайте параметр, щоб включити пору дня до вітання.

За замовчуванням, функції використовують імена параметрів як мітки для своїх аргументів. Щоб вказати іншу мітку, напишіть її ім’я перед іменем параметра, або напишіть `_` щоб не використовувати мітку аргумента.
```swift
func greet(_ person: String, on day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet("John", on: "Wednesday")
```

За допомогою кортежів можна створити поєднане значення. Наприклад, щоб повернути кілька значень із функції. До елементів кортежа можна звертатись по імені або по індексу.
```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0
    
    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }
    
    return (min, max, sum)
}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
print(statistics.sum)
print(statistics.2)
```
Функції також можуть мати змінну кількість аргументів, збираючи їх в масив.
```swift
func sumOf(numbers: Int...) -> Int {
    var sum = 0
    for number in numbers {
        sum += number
    }
    return sum
}
sumOf()
sumOf(numbers: 42, 597, 12)
```
> **Експеримент**
>
> Напишіть функцію, що підраховує середнє значення її аргументів.

Функції можуть бути вкладеними. Вкладені функції мають доступ до змінних, що були оголошені у зовнішній функції. Вкладені функції допомагають організовувати код у складних чи довгих функціях.
```swift
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()
```
Функції є об'єктами першого класу. Це означає, що функція може повертати іншу функцію як значення.
```swift
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

Функція може пряймати іншу функцію у якості одного із своїх аргументів.
```swift
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```
Насправді фунції являють собою особливий випадок замикань: блоків коду, що можуть бути викликані пізніше. Код в замиканні має доступ до таких речей як змінні та функції, що були доступні в контексті, де було створено саме замикання, навіть якщо замикання знаходиться в іншому контексті на момент виклику. Ми вже бачили приклад цього з вкладеними функціями. Ми можемо оголосити замикання без імені огорнувши його код в фігурні дужки (`{}`). За допомогою `in` відділяємо аргументи і тип, котрий повертає функція, від її тіла.
```swift
numbers.map({
    (number: Int) -> Int in
    let result = 3 * number
    return result
})
```
> **Експеримент**
>
> Перепишіть замикання так, щоб воно повертало нуль для всіх непарних чисел.

Є кілька способів написання замикань більш коротко. Коли тип замикання вже відомо, як, наприклад, колбек для делегату, можемо опустити тип його параметрів, тип, що повертається, або і те й інше. Замикання, що складаються з єдиного виразу, неявно повертають значення цього виразу.
```swift
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
```
Можемо звертатись до параметрів по номеру замість імені - це дуже зручно в дуже коротких замиканнях. Замикання, що передається в функцію останнім аргументом, може йти одразу за круглими дужками. Коли єдиним аргументом функції є замикання, то не обов'язкові навіть і круглі дужки.
```swift
let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbers)

```