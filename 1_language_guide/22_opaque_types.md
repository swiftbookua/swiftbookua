## Непрозорі типи

Функція чи метод із непрозорим типом, що повертається, приховують інформацію про тип значення, що вони повертають. Замість того, щоб вказати конкретний тип, котрий повертає функція, значення, що повертається, описується у термінах протоколів, котрим воно підпорядковано. Приховування інформації про тип буває корисним на межі між модулем та кодом, що робить виклики методів чи функцій з цього модуля, оскільки інформація про фактичний тип значення, що повертається, залишається приватною. Навідміну від повернення значення, чий тип є протоколом, непрозорі типи зберігають повну інформацію про тип: компілятор має доступ до інформації про тип, однак клієнти цьгого модуля – ні. 

### Проблема, яку вирішують непрозорі типи

Наприклад, припустимо, що ми пишемо модуль, котрий малює фігури з [ASCII-графіки](https://uk.wikipedia.org/wiki/ASCII-графіка). Базовою характеристикою будь-якої фігури є функція `draw()`, котра повертає представлення фігури у вигляді рядка. Цей метод можна використати як вимогу до протоколу `Shape`:

```swift
protocol Shape {
    func draw() -> String
}

struct Triangle: Shape {
    var size: Int

    func draw() -> String {
        var result = [String]()
        for length in 1...size {
            result.append(String(repeating: "*", count: length))
        }
        return result.joined(separator: "\n")
    }
}
let smallTriangle = Triangle(size: 3)
print(smallTriangle.draw())
// *
// **
// ***
```

Для реалізації операцій на кшталт перевороту фігури по вертикалі, можна скористатись узагальненнями, як  показано у коді нижче. Однак, даний підхід має певні обмеження: перевернутий результат виставляє напоказ точний узагальнений тип, що було використано для створення перевернутої фігури. 

```swift
struct FlippedShape<T: Shape>: Shape {
    var shape: T
    func draw() -> String {
        let lines = shape.draw().split(separator: "\n")
        return lines.reversed().joined(separator: "\n")
    }
}
let flippedTriangle = FlippedShape(shape: smallTriangle)
print(flippedTriangle.draw())
// ***
// **
// *
```

Цей же підхід можна застосувати для визначення структури `JoinedShape<T: Shape, U: Shape>`, за домогою якої можна поєднувати дві фігури вертикально. Однак, якщо, наприклад, потрібно поєднати перевернутий трикутник із іншим трикутником, результат матиме тип на кшталт `JoinedShape<FlippedShape<Triangle>, Triangle>`.

```swift
struct JoinedShape<T: Shape, U: Shape>: Shape {
    var top: T
    var bottom: U
    func draw() -> String {
        return top.draw() + "\n" + bottom.draw()
    }
}
let joinedTriangles = JoinedShape(top: smallTriangle, bottom: flippedTriangle)
print(joinedTriangles.draw())
// *
// **
// ***
// ***
// **
// *
```

Виставлена на гора детальна інформація про створення фігури дозволяє типам, котрі не призначені бути частиною публічного інтерфейсу модуля ASCII-графіки, витікати назовні, оскільки потрібно зазначати повністю тип, що повертається. Код всередині модуля може створити одну й ту ж фігуру у кілька різних способів, а код зовні модуля, що використовує фігури, не повинен брати до уваги деталі реалізації, зокрема інформацію про список перетворень фігур. Обгортки на кшталт `JoinedShape` та `FlippedShape` не мають значення для користувача модулі, і тому вони не повинні бути видимими. Публічний інтерфейс модуля повинен складається із операцій, на кшталт об'єднання та перевертання фігури, і ці операції повинні повертати інше значення `Shape`.

### Повернення непрозорих типів

Непрозорі типи можна уявляти узагальненими типами навпаки. Узагальнені типи дозволяють коду, що викликає функцію, обирати тип паметрів функції та тип, що нею повертається, у абстрагований від реалізації функції спосіб. Наприклад, функція у наступному коді повертає тип, що залижить від коду, що викликає дану функцію:

```swift
func max<T>(_ x: T, _ y: T) -> T where T: Comparable { ... }
```

Код, що викликає функцію `max(_:_:)` обирає значення `x` та `y`, і тип цих значень визначеє конкретний тип `T`. Код, що викликає цю функцію, може використовувати будь-який підпорядкований протоколу `Comparable` тип. Код усередині функції записаний в узагальнений спосіб, тому він може опрацювати будь-який наданий тип. Реалізація функції `max(_:_:)` використовує лише ту функціональність, котру мають усі підпорядковані протоколу `Comparable` типи.

Ці ролі міняються місцями для функції, що повертає непрозорий тип. Непрозорий тип дозволяє реалізації функції обирати тип значення, що вона повертає, у спосіб, абстрагований від коду, що викликає дану функцію. Наприклад, у наступному фрагменті коду функція повертає трапецію, без викривання фактичного типу даної фігури. 

```swift
struct Square: Shape {
    var size: Int
    func draw() -> String {
        let line = String(repeating: "*", count: size)
        let result = Array<String>(repeating: line, count: size)
        return result.joined(separator: "\n")
    }
}

func makeTrapezoid() -> some Shape {
    let top = Triangle(size: 2)
    let middle = Square(size: 2)
    let bottom = FlippedShape(shape: top)
    let trapezoid = JoinedShape(
        top: top,
        bottom: JoinedShape(top: middle, bottom: bottom)
    )
    return trapezoid
}
let trapezoid = makeTrapezoid()
print(trapezoid.draw())
// *
// **
// **
// **
// **
// *
```

Функція `makeTrapezoid()` у цьому прикладі оголошує тип, що повертається, як `some Shape`. Як результат, функція повертає значення якогось заданого типу, що підпорядковується до протоколу `Shape`, не вказуючи будь-якого конкретного типу. Оголошення функції `makeTrapezoid()` у такий спосіб виражає фундаментальний аспект її публічного інтерфейсу – значення, що повертається, є фігурою – без зазначення фактичних типів з яких складається фігура у публічному інтерфейсі. Дана реалізація використовує два трикутники та квадрат, однак цю функцію можна переписати, генеруючи трапецію одним із багатьох різних способів, не змінюючи тип, що повертається. 

У даному прикладі підкреслюється той факт, що непрозорий тип, що повертається, є подібним до узагальненого типу навпаки. Код всередині `makeTrapezoid()` може повертати будь-який тип, який потрібно, аби лише цей тип був підпорядкований протоколу `Shape`, аналогічно до коду, що викликає узагальнену функцію. Код, що викликає функцію `makeTrapezoid()`, повинен бути написаним в узагальненій манері, аналогічно до реалізації узагальненої функції, так, щоб він міг працювати із будь-яким значенням `Shape`, що повертається функцією `makeTrapezoid()`.

Можна також комбінувати непрозорі типи, що повертаються, із узагальненнями. Обидві функції у коді нижче повертають значення якогось типу, що підпорядковується протоколу `Shape`:

```swift
func flip<T: Shape>(_ shape: T) -> some Shape {
    return FlippedShape(shape: shape)
}
func join<T: Shape, U: Shape>(_ top: T, _ bottom: U) -> some Shape {
    JoinedShape(top: top, bottom: bottom)
}

let opaqueJoinedTriangles = join(smallTriangle, flip(smallTriangle))
print(opaqueJoinedTriangles.draw())
// *
// **
// ***
// ***
// **
// *
```

Значення `opaqueJoinedTriangles` у цьому прикладі є таким же, як і `joinedTriangles` у прикладі з узагальненнями у підрозділі [Проблема, яку вирішують непрозорі типи](#Проблема,-яку-вирішують-непрозорі-типи) вище. Однак, на відміну від значення у тому прикладі, функції `flip(_:)` та `join(_:_:)` обгортають тип за лаштунками таким чином, що узагальнені операції з фігурами стають прихованими за непрозорим типом, що запобігає видимості цих операцій. Обидві функції є узагальненими, оскільки типи, на які вони покладаються, є узагальненими, а параметри типів, що передаються до цих функції, передають інформацію про тип, потрібну для структур `FlippedShape` та `JoinedShape`.

У функції, котра повертає непрозорий тип із декілької різних місць, усі можливі значення, що повертаються, повинні мати однаковий тип. В тому числі й для узагальненої функції, у якої тип, що повертається, може визначатись одним із параметрів типу, це має бути одним типом. Наприклад, ось некоректна версія функції, котра перевертає фігуру, та обробляє квадрати окремим чином:

```swift
func invalidFlip<T: Shape>(_ shape: T) -> some Shape {
    if shape is Square {
        return shape // Помилка: типи, що повертаються, не співпадають
    }
    return FlippedShape(shape: shape) // Помилка: типи, що повертаються, не співпадають
}
```

Якщо викликати функцію із аргументом типу `Square`, вона повертає `Square`, в інших випадках вона повертає `FlippedShape`. Це суперечить вимозі співпадіння типів, що повертаються, та робить реалізацію `invalidFlip(_:)` некоректним кодом. Одним зі способів виправити функцію `invalidFlip(_:)` є перенесення спеціального випадку для квадратів до реалізації структури `FlippedShape`, що дозволяє функції завжди повертати значення типу `FlippedShape`:

```swift
struct FlippedShape<T: Shape>: Shape {
    var shape: T
    func draw() -> String {
        if shape is Square {
            return shape.draw()
        }
        let lines = shape.draw().split(separator: "\n")
        return lines.reversed().joined(separator: "\n")
    }
}
```

Вимога завжди повертати значення одного типу не заважає використовувати узагальнення у не прозорих типах, що повертаються. Ось приклад функції, що вбудовує параметр типу у внутрішній тип значення, що повертається: 

```swift
func `repeat`<T: Shape>(shape: T, count: Int) -> some Collection {
    return Array<T>(repeating: shape, count: count)
}
```

В даному прикладі, тип значення, що повертається, залежить від `T`: яка б фігура не передавалась, функція `repeat(shape:count:)` створює та повертає масив типу цієї фігури. Тим не менш, значення, що повертається, завжди має один і той же тип `[T]`, тому дана функція відповідає вимозі, що функції що повертають непрозорий тип, повинні повертати значення одного типу. 

### Різниця між непрозорими типами та протоколами

### Differences Between Opaque Types and Protocol Types

Returning an opaque type looks very similar to using a protocol type as the return type of a function, but these two kinds of return type differ in whether they preserve type identity. An opaque type refers to one specific type, although the caller of the function isn’t able to see which type; a protocol type can refer to any type that conforms to the protocol. Generally speaking, protocol types give you more flexibility about the underlying types of the values they store, and opaque types let you make stronger guarantees about those underlying types.

For example, here’s a version of flip(_:) that returns a value of protocol type instead of using an opaque return type:

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
    return FlippedShape(shape: shape)
}
```

This version of protoFlip(_:) has the same body as flip(_:), and it always returns a value of the same type. Unlike flip(_:), the value that protoFlip(_:) returns isn’t required to always have the same type—it just has to conform to the Shape protocol. Put another way, protoFlip(_:) makes a much looser API contract with its caller than flip(_:) makes. It reserves the flexibility to return values of multiple types:

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
    if shape is Square {
        return shape
    }

    return FlippedShape(shape: shape)
}
```

The revised version of the code returns an instance of Square or an instance of FlippedShape, depending on what shape is passed in. Two flipped shapes returned by this function might have completely different types. Other valid versions of this function could return values of different types when flipping multiple instances of the same shape. The less specific return type information from protoFlip(_:) means that many operations that depend on type information aren’t available on the returned value. For example, it’s not possible to write an == operator comparing results returned by this function.

```swift
let protoFlippedTriangle = protoFlip(smallTriangle)
let sameThing = protoFlip(smallTriangle)
protoFlippedTriangle == sameThing  // Error
```

The error on the last line of the example occurs for several reasons. The immediate issue is that the Shape doesn’t include an == operator as part of its protocol requirements. If you try adding one, the next issue you’ll encounter is that the == operator needs to know the types of its left-hand and right-hand arguments. This sort of operator usually takes arguments of type Self, matching whatever concrete type adopts the protocol, but adding a Self requirement to the protocol doesn’t allow for the type erasure that happens when you use the protocol as a type.

Using a protocol type as the return type for a function gives you the flexibility to return any type that conforms to the protocol. However, the cost of that flexibility is that some operations aren’t possible on the returned values. The example shows how the == operator isn’t available—it depends on specific type information that isn’t preserved by using a protocol type.

Another problem with this approach is that the shape transformations don’t nest. The result of flipping a triangle is a value of type Shape, and the protoFlip(_:) function takes an argument of some type that conforms to the Shape protocol. However, a value of a protocol type doesn’t conform to that protocol; the value returned by protoFlip(_:) doesn’t conform to Shape. This means code like protoFlip(protoFlip(smallTriange)) that applies multiple transformations is invalid because the flipped shape isn’t a valid argument to protoFlip(_:).

In contrast, opaque types preserve the identity of the underlying type. Swift can infer associated types, which lets you use an opaque return value in places where a protocol type can’t be used as a return value. For example, here’s a version of the Container protocol from [Generics](21_generics.md):

```swift
protocol Container {
    associatedtype Item
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
extension Array: Container { }
```

You can’t use Container as the return type of a function because that protocol has an associated type. You also can’t use it as constraint a generic return type because there isn’t enough information outside the function body to infer what the generic type needs to be.

```swift
// Error: Protocol with associated types can't be used as a return type.
func makeProtocolContainer<T>(item: T) -> Container {
    return [item]
}

// Error: Not enough information to infer C.
func makeProtocolContainer<T, C: Container>(item: T) -> C {
    return [item]
}
```

Using the opaque type some Container as a return type expresses the desired API contract—the function returns a container, but declines to specify the container’s type:

```swift
func makeOpaqueContainer<T>(item: T) -> some Container {
    return [item]
}
let opaqueContainer = makeOpaqueContainer(item: 12)
let twelve = opaqueContainer[0]
print(type(of: twelve))
// Prints "Int"
```

The type of twelve is inferred to be Int, which illustrates the fact that type inference works with opaque types. In the implementation of makeOpaqueContainer(item:), the underlying type of the opaque container is [T]. In this case, T is Int, so the return value is an array of integers and the Item associated type is inferred to be Int. The subscript on Container returns Item, which means that the type of twelve is also inferred to be Int.

