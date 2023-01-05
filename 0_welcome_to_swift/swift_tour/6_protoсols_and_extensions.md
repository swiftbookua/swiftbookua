---
title: Протоколи та Розширення
layout: default
parent: Тур по Swift
grand_parent: Ласкаво просимо до Swift
nav_order: 6
has_children: false
has_toc: false
---

# Протоколи та Розширення

Вживаємо `protocol` щоб оголосити протокол.

```swift
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}
```

Класи, перечислення, і структури можуть реалізовувати протокол.

```swift
class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += "  Now 100% adjusted."
    }
}
var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStructure: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}
var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription
```

> **Експеримент**
>
> Створіть перечислення, що реалізовує цей протокол.

Помітимо ключове слово `mutating` в оголошенні `SimpleStructure`: воно вказує, що даний метод мутуючий, тобто він змінює дану структуру. В оголошенні `SimpleClass` не потрібно помічати жодних методів мутуючими, бо методи класу завжди можуть його змінювати.

Щоб додати функціональності вже наявним типам, використовуємо розширення, котрі оголошуються за допомогою ключового слова`extension`. Розширення можуть додавати нові методи та властивості, що розраховуються. Також за допомогою розширення можна додати відповідність до протоколу типу, що оголошений десь в іншому місці, або навіть до типу, що було імпортовано з бібліотеки чи фреймворку.

```swift
extension Int: ExampleProtocol {
    var simpleDescription: String {
        return "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
}
print(7.simpleDescription)
```

> **Експеримент**
>
> Створіть розширення типу `Double` що додає властвість `absoluteValue` - математичний модуль числа.

Можна використовувати ім'я протоколу так само, як і будь-яке інше ім'я типу. Наприклад, щоб створити колекцію об'єктів різних типів, кожен з яких відповідає єдиному протоколу. Коли ви працюєте зі значеннями, чий тип є протоколом, його методи та властивості, котрі оголошені зовні протоколу, є недоступними.

```swift
let protocolValue: ExampleProtocol = a
print(protocolValue.simpleDescription)
// print(protocolValue.anotherProperty)  // Розкоментуйте, щоб побачити помилку
```

Хоч змінна `protocolValue` під час виконання має тип `SimpleClass`, компілятор відноситься до неї як до екземпляра типу `ExampleProtocol`. Це означає, що ви не можете випадково звертатись до методів чи властивостей класу, що не є частиною реалізації протоколу.

