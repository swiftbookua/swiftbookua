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

Виставлена на гора детальна інформація про створення фігури дозволяє типам, котрі не призначені бути частиною публічного інтерфейсу модуля ASCII-графіки, витікати назовні, оскільки потрібно зазначати повністю тип, що повертається. Код всередині модуля 

Exposing detailed information about the creation of a shape allows types that aren’t meant to be part of the ASCII art module’s public interface to leak out because of the need to state the full return type. The code inside the module could build up the same shape in a variety of ways, and other code outside the module that uses the shape shouldn’t have to account for the implementation details about the list of transformations. Wrapper types like `JoinedShape` and `FlippedShape` don’t matter to the module’s users, and they shouldn’t be visible. The module’s public interface consists of operations like joining and flipping a shape, and those operations return another `Shape` value.