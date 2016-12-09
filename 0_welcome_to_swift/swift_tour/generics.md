### Шаблони

Щоб оголосити шаблонну функцію або тип, напишіть ім'я в кутових дужках.
```swift
func makeArray<Item>(repeating item: Item, numberOfTimes: Int) -> [Item] {
    var result = [Item]()
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    return result
}
makeArray(repeating: "knock", numberOfTimes:4)
```
Можна створювати шаблонні функції та методи, так само як і класи, перечислення і структури.
```swift
// Відтворення опціонального типу із стандартної бібліотеки Swift
enum OptionalValue<Wrapped> {
    case none
    case some(Wrapped)
}
var possibleInteger: OptionalValue<Int> = .none
possibleInteger = .some(100)
```
Щоб вказати список вимог, пишемо `where` прямо перед тілом. Наприклад, можна вимагати, щоб тип реалізовував певний протокол, або щоб два типи були однаковими, або щоб клас був нащадком певного класу.
```swift
func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool
    where T.Iterator.Element: Equatable, T.Iterator.Element == U.Iterator.Element {
        for lhsItem in lhs {
            for rhsItem in rhs {
                if lhsItem == rhsItem {
                    return true
                }
            }
        }
        return false
}
anyCommonElements([1, 2, 3], [3])
```
> **Ексеримент**
>
> Змініть функцію `anyCommonElements(_:_:)` так, щоб вона повертала масив спільних для обох колекцій елементів.

Написання `<T: Equatable>` еквівалентне написанню `<T> ... where T: Equatable>`.
