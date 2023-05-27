# Конкурентність

🚧 Наразі триває переклад даного розділу. 🚧 

Swift має вбудовану підтримку написання асинхронного та паралельного коду структурованим чином. *Асинхронний код* може бути призупинений і продовжений пізніше, хоч лише одна частина програми виконується в одиницю часу. Призупинення і продовження виконання коду у вашій програмі дозволяє їй продовжувати прогрес короткострокових операцій, на кшталт оновлення інтерфейсу користувача, при цьому продовжуючи працювати над довгостроковими операціями, наприклад над викачуванням даних із мережі чи над парсингом файлів. *Паралельний код* – означає, що декілька частин коду можуть виконуватись одночасно. Наприклад, комп'ютер з чотириядерним процесором може виконувати чотири шматки коду в один і той же час, виділяючи на кожну задачу по ядру. Програми, що використовують паралельний та асинхронний код, виконують декілька операцій одночасно; вони призупиняють операції, котрі очікують на відповідь від зовнішніх систем, і це робить написання такого коду з урахуванням безпеки пам'яті легшим. 

Втім, додаткова гнучкість планування паралельного та асинхронного коду має свою ціну, що полягає у збільшеній складності. Swift дозволяє вам виражати свої наміри у спосіб, що має певні перевірки часу компіляції. Наприклад, ви можете послуговуватись акторами для безпечного звернення до змінюваного стану. Однак, додавання конкурентності до повільного чи забагованого коду не гарантує, що він стане швидшим чи коректнішим. Ймовірніше за все додавання конкурентності може навіть нашкодити, зробивши ваш код складним для зневадження. Однак, використання підтримки конкурентності на рівні мови Swift у тому вашому коді, який повинен бути конкурентним, допоможе вам знаходити проблеми під час компіляції.

Решта цього розділу буде використовувати термін *конкурентність* для позначення комбінації асинхронного і паралельного коду.

> **Примітка** 
>
> Якщо ви писали конкурентний код раніше, ви, певне, звикнути працювати з потоками. Модель конкурентності у Swift побудована поверх потоків, але ви не взаємодієте з ними напряму. Асинхронна функція у Swift може поступитись потоком, на якому вона виконується, що дозволяє іншій асинхронній функції працювати на цьому потоці допоки перша функція заблокована. Коли асинхронний код продовжиться, Swift не дає жодних гарантій щодо того, на якому потоці функція працюватиме далі.

Хоч і можливо писати конкурентний код без підтримки на рівні мови Swift, цей код як правило складніше читати. Наприклад, код нижче завантажує список назв фотографій, потім завантажує перше фото зі списку, та показує це фото користувачу:

```swift
listPhotos(inGallery: "Літня відпустка") { photoNames in
    let sortedNames = photoNames.sorted()
    let name = sortedNames[0]
    downloadPhoto(named: name) { photo in
        show(photo)
    }
}
```

Навіть у цьому простому випадку, код потрібно писати як послідовність обробників завершень, призводячи до написання вкладених замикань. У цьому стилі, складніший код із глибшим рівнем вкладення може швидко стати непідйомним. 

## Визначення та виклик асинхронних функцій

*Асинхронна функція* чи *асинхронний метод* – це особливий вид функцій чи методів, які можна призупиняти під час виконання. Це відрізняється від звичайних, тобто синхронних функцій чи методів, котрі або виконуються до завершення, або викидують помилку, або ніколи не повертають результат. Асинхронні функції чи методи також роблять одну з цих трьох речей, але вони також можуть стати на паузу посеред виконання, коли вони чогось очікують. Кожне з таких місць у тілі методу чи функції, де він або вона можуть бути призупинені, слід позначати явно. 

Щоб вказати, що функція або метод є асинхронним, використовується ключове слово `async`. Його слід вказувати в оголошенні після параметрів, аналогічно до того, як ключове слово `throws` позначає функцію, що може викидати помилку. Якщо функція чи метод повертає значення, слід писати `async` до стрілочки повернення (`->`). Наприклад, ось як можна оголосити функцію для отримання назв фотографій у галереї:

```swift
func listPhotos(inGallery name: String) async -> [String] {
    let result = // ... якийсь асинхронний код, що працює з мережею ...
    return result
}
```

Якщо функція або метод є одночасно асинхронними та викидають помилку, ключове слово `async` слід писати перед `throws`.

При виклику асинхронного методу, виконання призупиняється до того моменту, поки цей метод не поверне значення. Слід писати ключове слово `await` перед таким викликом, щоб явно відмітити можливу точку призупинення. Це схоже на написання `try` при виклику функції, що викидає помилку, для позначення можливої зміни у ході виконання програми при помилці. Всередині асинхронного методу потік виконання призупиняється *лише* при виклику іншого асинхронного методу чи функції: призупинення ніколи не є неявним або превентивним. Це означає, що кожне можливе місце призупинення є завжди відмічене ключовим словом `await`.

Наприклад, у коді нижче отримуються назви усіх фотографій у галереї й після цього показується перша фотографія:

```swift
let photoNames = await listPhotos(inGallery: "Літня відпустка")
let sortedNames = photoNames.sorted()
let name = sortedNames[0]
let photo = await downloadPhoto(named: name)
show(photo)
```

Because the `listPhotos(inGallery:)` and `downloadPhoto(named:)` functions both need to make network requests, they could take a relatively long time to complete. Making them both asynchronous by writing `async` before the return arrow lets the rest of the app's code keep running while this code waits for the picture to be ready.

To understand the concurrent nature of the example above, here's one possible order of execution:

- The code starts running from the first line and runs up to the first `await`. It calls the `listPhotos(inGallery:)` function and suspends execution while it waits for that function to return.
- While this code's execution is suspended, some other concurrent code in the same program runs. For example, maybe a long-running background task continues updating a list of new photo galleries. That code also runs until the next suspension point, marked by `await`, or until it completes.
- After `listPhotos(inGallery:)` returns, this code continues execution starting at that point. It assigns the value that was returned to `photoNames`.
- The lines that define `sortedNames` and `name` are regular, synchronous code. Because nothing is marked `await` on these lines,
  there aren't any possible suspension points.
- The next `await` marks the call to the `downloadPhoto(named:)` function. This code pauses execution again until that function returns, giving other concurrent code an opportunity to run.
- After `downloadPhoto(named:)` returns, its return value is assigned to `photo` and then passed as an argument when calling `show(_:)`.

The possible suspension points in your code marked with `await` indicate that the current piece of code might pause execution while waiting for the asynchronous function or method to return. This is also called *yielding the thread* because, behind the scenes, Swift suspends the execution of your code on the current thread and runs some other code on that thread instead. Because code with `await` needs to be able to suspend execution, only certain places in your program can call asynchronous functions or methods:

- Code in the body of an asynchronous function, method, or property.
- Code in the static `main()` method of
  a structure, class, or enumeration that's marked with `@main`.
- Code in an unstructured child task, as shown in <doc:Concurrency#Unstructured-Concurrency> below.

<!--
  SE-0296 specifically calls out that top-level code is *not* an async context,
  contrary to what you might expect.
  If that gets changed, add this bullet to the list above:

  - Code at the top level that forms an implicit main function.
    -->

Code in between possible suspension points runs sequentially, without the possibility of interruption from other concurrent code. For example, the code below moves a picture from one gallery to another.

```swift
let firstPhoto = await listPhotos(inGallery: "Літня відпустка")[0]
add(firstPhoto, toGallery: "Road Trip")
// At this point, firstPhoto is temporarily in both galleries.
remove(firstPhoto, fromGallery: "Літня відпустка")
```

There's no way for other code to run in between the call to `add(_:toGallery:)` and `remove(_:fromGallery:)`. During that time, the first photo appears in both galleries, temporarily breaking one of the app's invariants. To make it even clearer that this chunk of code must not have `await` added to it in the future, you can refactor that code into a synchronous function:

```swift
func move(_ photoName: String, from source: String, to destination: String) {
    add(photoName, toGallery: destination)
    remove(photoName, fromGallery: source)
}
// ...
let firstPhoto = await listPhotos(inGallery: "Літня відпустка")[0]
move(firstPhoto, from: "Літня відпустка", to: "Road Trip")
```

In the example above, because the `move(_:from:to:)` function is synchronous, you guarantee that it can never contain possible suspension points. In the future, if you try to add concurrent code to this function, introducing a possible suspension point, you'll get compile-time error instead of introducing a bug.

> **Note** 
>
> The [`Task.sleep(until:tolerance:clock:)`](https://developer.apple.com/documentation/swift/task/sleep(until:tolerance:clock:)) method is useful when writing simple code to learn how concurrency works. This method does nothing, but waits at least the given number of nanoseconds before it returns. Here's a version of the `listPhotos(inGallery:)` function that uses `sleep(until:tolerance:clock:)` to simulate waiting for a network operation:
>
> ```swift
> func listPhotos(inGallery name: String) async throws -> [String] {
>     try await Task.sleep(until: .now + .seconds(2), clock: .continuous)
>     return ["IMG001", "IMG99", "IMG0404"]
> }
> ```

## Asynchronous Sequences

The `listPhotos(inGallery:)` function in the previous section asynchronously returns the whole array at once, after all of the array's elements are ready. Another approach is to wait for one element of the collection at a time using an *asynchronous sequence*. Here's what iterating over an asynchronous sequence looks like:

```swift
import Foundation

let handle = FileHandle.standardInput
for try await line in handle.bytes.lines {
    print(line)
}
```

Instead of using an ordinary `for`-`in` loop, the example above writes `for` with `await` after it. Like when you call an asynchronous function or method, writing `await` indicates a possible suspension point. A `for`-`await`-`in` loop potentially suspends execution at the beginning of each iteration, when it's waiting for the next element to be available.

In the same way that you can use your own types in a `for`-`in` loop by adding conformance to the [`Sequence`](https://developer.apple.com/documentation/swift/sequence) protocol, you can use your own types in a `for`-`await`-`in` loop by adding conformance to the [`AsyncSequence`](https://developer.apple.com/documentation/swift/asyncsequence) protocol.

## Calling Asynchronous Functions in Parallel

Calling an asynchronous function with `await` runs only one piece of code at a time. While the asynchronous code is running, the caller waits for that code to finish before moving on to run the next line of code. For example, to fetch the first three photos from a gallery, you could await three calls to the `downloadPhoto(named:)` function as follows:

```swift
let firstPhoto = await downloadPhoto(named: photoNames[0])
let secondPhoto = await downloadPhoto(named: photoNames[1])
let thirdPhoto = await downloadPhoto(named: photoNames[2])

let photos = [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

This approach has an important drawback: Although the download is asynchronous and lets other work happen while it progresses, only one call to `downloadPhoto(named:)` runs at a time. Each photo downloads completely before the next one starts downloading. However, there's no need for these operations to wait --- each photo can download independently, or even at the same time.

To call an asynchronous function and let it run in parallel with code around it, write `async` in front of `let` when you define a constant, and then write `await` each time you use the constant.

```swift
async let firstPhoto = downloadPhoto(named: photoNames[0])
async let secondPhoto = downloadPhoto(named: photoNames[1])
async let thirdPhoto = downloadPhoto(named: photoNames[2])

let photos = await [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

In this example, all three calls to `downloadPhoto(named:)` start without waiting for the previous one to complete. If there are enough system resources available, they can run at the same time. None of these function calls are marked with `await` because the code doesn't suspend to wait for the function's result. Instead, execution continues until the line where `photos` is defined --- at that point, the program needs the results from these asynchronous calls, so you write `await` to pause execution until all three photos finish downloading.

Here's how you can think about the differences between these two approaches:

- Call asynchronous functions with `await` when the code on the following lines depends on that function's result. This creates work that is carried out sequentially.
- Call asynchronous functions with `async`-`let` when you don't need the result until later in your code. This creates work that can be carried out in parallel.
- Both `await` and `async`-`let` allow other code to run while they're suspended.
- In both cases, you mark the possible suspension point with `await` to indicate that execution will pause, if needed, until an asynchronous function has returned.

You can also mix both of these approaches in the same code.

## Tasks and Task Groups

A *task* is a unit of work that can be run asynchronously as part of your program. All asynchronous code runs as part of some task. The `async`-`let` syntax described in the previous section creates a child task for you. You can also create a task group and add child tasks to that group, which gives you more control over priority and cancellation, and lets you create a dynamic number of tasks.

Tasks are arranged in a hierarchy. Each task in a task group has the same parent task, and each task can have child tasks. Because of the explicit relationship between tasks and task groups, this approach is called *structured concurrency*. Although you take on some of the responsibility for correctness, the explicit parent-child relationships between tasks let Swift handle some behaviors like propagating cancellation for you, and lets Swift detect some errors at compile time.

```swift
await withTaskGroup(of: Data.self) { taskGroup in
    let photoNames = await listPhotos(inGallery: "Літня відпустка")
    for name in photoNames {
        taskGroup.addTask { await downloadPhoto(named: name) }
    }
}
```

For more information about task groups, see [`TaskGroup`](https://developer.apple.com/documentation/swift/taskgroup).

### Unstructured Concurrency

In addition to the structured approaches to concurrency described in the previous sections, Swift also supports unstructured concurrency. Unlike tasks that are part of a task group, an *unstructured task* doesn't have a parent task. You have complete flexibility to manage unstructured tasks in whatever way your program needs, but you're also completely responsible for their correctness. To create an unstructured task that runs on the current actor, call the [`Task.init(priority:operation:)`](https://developer.apple.com/documentation/swift/task/3856790-init) initializer. To create an unstructured task that's not part of the current actor, known more specifically as a *detached task*, call the [`Task.detached(priority:operation:)`](https://developer.apple.com/documentation/swift/task/3856786-detached) class method. Both of these operations return a task that you can interact with --- for example, to wait for its result or to cancel it.

```swift
let newPhoto = // ... some photo data ...
let handle = Task {
    return await add(newPhoto, toGalleryNamed: "Spring Adventures")
}
let result = await handle.value
```

For more information about managing detached tasks, see [`Task`](https://developer.apple.com/documentation/swift/task).

### Task Cancellation

Swift concurrency uses a cooperative cancellation model. Each task checks whether it has been canceled at the appropriate points in its execution, and responds to cancellation in whatever way is appropriate. Depending on the work you're doing, that usually means one of the following:

- Throwing an error like `CancellationError`
- Returning `nil` or an empty collection
- Returning the partially completed work

To check for cancellation, either call [`Task.checkCancellation()`](https://developer.apple.com/documentation/swift/task/3814826-checkcancellation), which throws `CancellationError` if the task has been canceled, or check the value of [`Task.isCancelled`](https://developer.apple.com/documentation/swift/task/3814832-iscancelled) and handle the cancellation in your own code. For example, a task that's downloading photos from a gallery might need to delete partial downloads and close network connections.

To propagate cancellation manually, call [`Task.cancel()`](https://developer.apple.com/documentation/swift/task/3851218-cancel).

## Actors

You can use tasks to break up your program into isolated, concurrent pieces. Tasks are isolated from each other, which is what makes it safe for them to run at the same time, but sometimes you need to share some information between tasks. Actors let you safely share information between concurrent code.

Like classes, actors are reference types, so the comparison of value types and reference types in <doc:ClassesAndStructures#Classes-Are-Reference-Types> applies to actors as well as classes. Unlike classes, actors allow only one task to access their mutable state at a time, which makes it safe for code in multiple tasks to interact with the same instance of an actor. For example, here's an actor that records temperatures:

```swift
actor TemperatureLogger {
    let label: String
    var measurements: [Int]
    private(set) var max: Int

    init(label: String, measurement: Int) {
        self.label = label
        self.measurements = [measurement]
        self.max = measurement
    }
}
```

You introduce an actor with the `actor` keyword, followed by its definition in a pair of braces. The `TemperatureLogger` actor has properties that other code outside the actor can access, and restricts the `max` property so only code inside the actor can update the maximum value.

You create an instance of an actor using the same initializer syntax as structures and classes. When you access a property or method of an actor, you use `await` to mark the potential suspension point. For example:

```swift
let logger = TemperatureLogger(label: "Outdoors", measurement: 25)
print(await logger.max)
// Prints "25"
```

In this example, accessing `logger.max` is a possible suspension point. Because the actor allows only one task at a time to access its mutable state, if code from another task is already interacting with the logger, this code suspends while it waits to access the property.

In contrast, code that's part of the actor doesn't write `await` when accessing the actor's properties. For example, here's a method that updates a `TemperatureLogger` with a new temperature:

```swift
extension TemperatureLogger {
    func update(with measurement: Int) {
        measurements.append(measurement)
        if measurement > max {
            max = measurement
        }
    }
}
```

The `update(with:)` method is already running on the actor, so it doesn't mark its access to properties like `max` with `await`. This method also shows one of the reasons why actors allow only one task at a time to interact with their mutable state: Some updates to an actor's state temporarily break invariants. The `TemperatureLogger` actor keeps track of a list of temperatures and a maximum temperature, and it updates the maximum temperature when you record a new measurement. In the middle of an update, after appending the new measurement but before updating `max`, the temperature logger is in a temporary inconsistent state. Preventing multiple tasks from interacting with the same instance simultaneously prevents problems like the following sequence of events:

1. Your code calls the `update(with:)` method. It updates the `measurements` array first.
2. Before your code can update `max`, code elsewhere reads the maximum value and the array of temperatures.
3. Your code finishes its update by changing `max`.

In this case, the code running elsewhere would read incorrect information because its access to the actor was interleaved in the middle of the call to `update(with:)` while the data was temporarily invalid. You can prevent this problem when using Swift actors because they only allow one operation on their state at a time, and because that code can be interrupted only in places where `await` marks a suspension point. Because `update(with:)` doesn't contain any suspension points, no other code can access the data in the middle of an update.

If you try to access those properties from outside the actor, like you would with an instance of a class, you'll get a compile-time error. For example:

```swift
print(logger.max)  // Error
```

Accessing `logger.max` without writing `await` fails because the properties of an actor are part of that actor's isolated local state. Swift guarantees that only code inside an actor can access the actor's local state. This guarantee is known as *actor isolation*.

## Sendable Types

Tasks and actors let you divide a program into pieces that can safely run concurrently. Inside of a task or an instance of an actor, the part of a program that contains mutable state, like variables and properties, is called a *concurrency domain*. Some kinds of data can't be shared between concurrency domains, because that data contains mutable state, but it doesn't protect against overlapping access.

A type that can be shared from one concurrency domain to another is known as a *sendable* type. For example, it can be passed as an argument when calling an actor method or be returned as the result of a task. The examples earlier in this chapter didn't discuss sendability because those examples use simple value types that are always safe to share for the data being passed between concurrency domains. In contrast, some types aren't safe to pass across concurrency domains. For example, a class that contains mutable properties and doesn't serialize access to those properties can produce unpredictable and incorrect results when you pass instances of that class between different tasks.

You mark a type as being sendable by declaring conformance to the `Sendable` protocol. That protocol doesn't have any code requirements, but it does have semantic requirements that Swift enforces. In general, there are three ways for a type to be sendable:

- The type is a value type, and its mutable state is made up of other sendable data --- for example, a structure with stored properties that are sendable or an enumeration with associated values that are sendable.
- The type doesn't have any mutable state, and its immutable state is made up of other sendable data --- for example, a structure or class that has only read-only properties.
- The type has code that ensures the safety of its mutable state, like a class that's marked `@MainActor` or a class that serializes access to its properties on a particular thread or queue.

For a detailed list of the semantic requirements, see the [`Sendable`](https://developer.apple.com/documentation/swift/sendable) protocol reference.

Some types are always sendable, like structures that have only sendable properties and enumerations that have only sendable associated values. For example:

```swift
struct TemperatureReading: Sendable {
    var measurement: Int
}

extension TemperatureLogger {
    func addReading(from reading: TemperatureReading) {
        measurements.append(reading.measurement)
    }
}

let logger = TemperatureLogger(label: "Tea kettle", measurement: 85)
let reading = TemperatureReading(measurement: 45)
await logger.addReading(from: reading)
```

Because `TemperatureReading` is a structure that has only sendable properties, and the structure isn't marked `public` or `@usableFromInline`, it's implicitly sendable. Here's a version of the structure where conformance to the `Sendable` protocol is implied:

```swift
struct TemperatureReading {
    var measurement: Int
}
```
