---
title: Макроси
layout: default
parent: Посібник з мови
nav_order: 19
has_children: false
has_toc: false
---

# Макроси

⚠️🛠️ Увага! Цей підрозділ наразі перекладається, тому наразі пропонуємо вашій увазі неповторний оригінал 🛠️⚠️

Макроси генерують код під час компіляції.

Макроси трансформують ваш вихідний код при його компіляції, що дозволяє уникнути написання повторюваного коду вручну. Під час компіляції, Swift розгортає кожен макрос у вашому коді перед звичайною компіляцією вашого коду.

![Діаграма, що зображає розгортання макросу. Зліва – стилізована репрезентація коду Swift. Справа – цей же код з кількома рядками, які додались за допомогою макросу.](../../assets/macro-expansion@2x.png)

Розгортання макросу є завжди адитивною операцією: макроси додають новий код, але вони ніколи не видаляють і не змінюють вже наявний код.

Як вхідні дані макросу, так і результат розгортання макросу перевіряються, щоб гарантувати синтаксичну правильність цього коду на Swift. Аналогічно перевіряється коректність типів значень, що ви передаєте до макросу, та значення у коді, згенерованому макросом. На додачу, якщо реалізація макросу під час розгортання макросу стикається з помилкою, компілятор вважає це помилкою компіляції. Ці гарантії дозволяють легше розмірковувати про код, що використовує макроси, а також дозволяють легше ідентифікувати проблеми на кшталт некоректного використання макросу, чи реалізації макросу, що містить баг. 

У Swift є два види макросів:
 - *Окремо стоячий макрос* з'являється в коді самостійно, без прив'язки до оголошення.
 - *Прив'язаний макрос* змінює оголошення, до якого він прив'язаний.

Прив'язані та окремо стоячі макроси викликаються дещо по-різному, але обидва ці види макросів слідують одній і тій же моделі розгортання макросів, і їх реалізовують за допомогою одного і того ж підходу. Наступні розділи детально описують кожен вид макросів.

## Окремо стоячі макроси

Щоб викликати окремо стоячий макрос, слід написати знак решітки (`#`) перед його назвою, і слід також вказати всі його аргументи, якщо вони є, у дужках після його назви. Наприклад:

```swift
func myFunction() {
    print("Наразі виконується \(#function)")
    #warning("Щось не так")
}
```

У першому рядку, запис `#function` викликає макрос [`function`][] зі стандартної бібліотеки Swift. Під час компіляції цього коду, Swift викликає реалізацію цього макросу, котра замінює вираз `#function` назвою поточної функції. Якщо запустити цей код та викликати функцію `myFunction()`, вона надрукує "Наразі виконується myFunction()". На другому рядку, вираз `#warning` викликає макрос [`warning(_:)`][] зі стандартної бібліотеки Swift, що створює ваше власне попередження компілятора.

[`function`]: https://developer.apple.com/documentation/swift/function
[`warning(_:)`]: https://developer.apple.com/documentation/swift/warning(_:)

Окремо стоячі макроси можуть створювати значення, як робить макрос `#function`, або вони можуть виконувати дію під час компіляції, як робить макрос `#warning`.

## Прив'язані макроси

Щоб викликати прив'язаний макрос, слід написати знак (`@`) перед його назвою, та вказати аргументи макроса, якщо вони є, у дужках після його назви. 

Прив'язані макроси змінюють оголошення, до якого вони прив'язані. Вони додають код до цього оголошення, на кшталт додавання нового методу або додавання підпорядкування до протоколу. 

Наприклад, розглянемо наступний код, що не використовує макросів:

```swift
struct SundaeToppings: OptionSet {
    let rawValue: Int
    static let nuts = SundaeToppings(rawValue: 1 << 0)
    static let cherry = SundaeToppings(rawValue: 1 << 1)
    static let fudge = SundaeToppings(rawValue: 1 << 2)
}
```

У цьому коді, оголошення кожної опції із множини опцій `SundaeToppings` містить виклик ініціалізатора, котрий є повторюваним кодом, написаним вручну. У такому коді легко зробити помилку при додаванні нової опції, набравши неправильне число наприкінці рядка. 

Ось версія цього коду, що натомість використовує макрос:

```swift
@OptionSet<Int>
struct SundaeToppings {
    private enum Options: Int {
        case nuts
        case cherry
        case fudge
    }
}
```

Ця версія `SundaeToppings` викликає макрос [`@OptionSet`][] зі стандартної бібліотеки Swift. Цей макрос вичитує список елементів приватного перечислення, генерує список констант для кожної опції, та додає підпорядкування до протоколу [`OptionSet`][].

[`@OptionSet`]: https://developer.apple.com/documentation/swift/optionset-swift.macro
[`OptionSet`]: https://developer.apple.com/documentation/swift/optionset-swift.protocol

Для порівняння, ось як виглядає цей код після розгортання макросу `@OptionSet`. Вам не доведеться писати такий код, і ви його побачите лише якщо ви спеціально попросите Swift показати розгортання макросу. 

```swift
struct SundaeToppings {
    private enum Options: Int {
        case nuts
        case cherry
        case fudge
    }

    typealias RawValue = Int
    var rawValue: RawValue
    init() { self.rawValue = 0 }
    init(rawValue: RawValue) { self.rawValue = rawValue }
    static let nuts: Self = Self(rawValue: 1 << Options.nuts.rawValue)
    static let cherry: Self = Self(rawValue: 1 << Options.cherry.rawValue)
    static let fudge: Self = Self(rawValue: 1 << Options.fudge.rawValue)
}
extension SundaeToppings: OptionSet { }
```

Весь код після приватного перечислення з'являється з макросу `@OptionSet`. Версія `SundaeToppings`, що використовує макрос для генерації всіх статичних змінних, є простішою для читання та підтримки, аніж попередня версія, оголошена повністю вручну. 

## Оголошення макросів

У більшості коду Swift, якщо вам треба оголосити символ, такий як функцію чи тип, вам не потрібне окреме його оголошення. Однак, для макросів, оголошення та реалізація є розділеними. Оголошення макросу містить його назву, параметри, що він приймає, місце, де його можна застосувати, та якого виду код він генерує. Реалізація макросу містить код, що розгортає макрос, генеруючи код мовою Swift.

Оголошення макросу вводиться за допомогою ключового слова `macro`. Наприклад, ось частина оголошення макросу `@OptionSet`, що використовувався у попередньому прикладі: 

```swift
public macro OptionSet<RawType>() =
        #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
```

Перший рядок визначає назву макросу та його аргументи: назвою є `OptionSet`, і він не приймає жодних аргументів. Другий рядок використовує макрос [`externalMacro(module:type:)`][] зі стандартної бібліотеки Swift, і за допомогою його вказує Swift, де буде знаходитись реалізація цього макросу. У цьому випадку, модуль `SwiftMacros` містить тип на ім'я `OptionSetMacro`, що реалізовує макрос `@OptionSet`.

[`externalMacro(module:type:)`]: https://developer.apple.com/documentation/swift/externalmacro(module:type:)

Оскільки макрос `OptionSet` є прив'язаним, його назва використовує [ВерхнійВерблюдячийРегістр][], так само як назви структур та класів. Окремо стоячі макроси мають нижнійВерблюдячийРегістр, як назви змінних та функцій.

[ВерхнійВерблюдячийРегістр]: https://uk.wikipedia.org/wiki/Верблюдячий_регістр

> **Примітка**:
>
> Макроси завжди оголошуються публічними (`public`). Оскільки код, що оголошує макрос, та код, що його використовує, знаходяться у різних модулях, непублічні макроси неможливо застосувати будь-де.

Оголошення макросу визначає його *роль* – місця у вихідному коді, в яких можна викликати цей макрос, і різновиди коду, що може згенерувати цей макрос. Кожен макрос має одну або декілька ролей, які записуються як частина атрибутів на початку оголошення макросу. Ось ще трошки оголошення макросу `@OptionSet`, включно з атрибутами, що визначають його ролі:

```swift
@attached(member)
@attached(conformance)
public macro OptionSet<RawType>() =
        #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
```

Атрибут `@attached` використовується у цьому оголошенні двічі, по одному разу на кожну роль макросу. У першому використанні, `@attached(member)` вказує, що макрос додає нові члени до типу, на якому його застосовують. Макрос `@OptionSet` додає ініціалізатор `init(rawValue:)`, наявність якого вимагається протоколом `OptionSet`, так само як і додаткові члени. Друге використання – `@attached(conformance)` – вказує, що `@OptionSet` додає одне або декілька підпорядкувань протоколу. Макрос `@OptionSet` розширює тип, до якого він застосовується, додаючи до нього підпорядкування протоколу `OptionSet`.

Для окремо стоячого макросу, слід писати атрибут `@freestanding`, щоб вказати його роль:

```swift
@freestanding(expression)
public macro line<T: ExpressibleByIntegerLiteral>() -> T =
        /* ... місцезнаходження реалізації макроса... */
```

Макрос `#line` вище має роль `expression`, тобто він є виразом. Макроси-вирази продукують значення, або виконують дії часу компіляції, на кшталт генерації попередження. 

На додачу до ролі макросу, оголошення макросу надає інформацію про назви символів, котрі генерує макрос. Коли оголошення макросу має список імен, гарантується, що він створить лише оголошення, що використовують ці імена. Це допомагає зрозуміти та зневадити згенерований код. Ось повне оголошення макросу `@OptionSet`:

```swift
@attached(member, names: named(RawValue), named(rawValue), 
        named(`init`), arbitrary)
@attached(conformance)
public macro OptionSet<RawType>() =
        #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
```

У оголошенні вище, атрибут `@attached(member)` включає аргументи після мітки `named:` для кожного символу, що генерує макрос `@OptionSet`. Макрос додає оголошення для символів з назвами `RawValue`, `rawValue`, та `init`: оскільки ці імена є відомими наперед, оголошення макросу перечислює їх явно.

Оголошення макросу також включає `arbitrary` після списку назв, що дозволяє макросу генерувати оголошення, чиї назви є невідомими до використання макросу. Наприклад, коли макрос `@OptionSet` застосовується до структури `SundaeToppings` вище, він генерує властивості типу, що відповідають елементам перечислення `nuts`, `cherry`, та `fudge`.

Детальніше зі списком ролей макросу можна ознайомитись у секціях [attached]({% link _book/2_language_reference/07_attributes.md %}#attached) та [freestanding]({% link _book/2_language_reference/07_attributes.md %}#freestanding) розділу [Атрибути]({% link _book/2_language_reference/07_attributes.md %}).

## Macro Expansion

When building Swift code that uses macros, the compiler calls the macros' implementation to expand them.

![Diagram showing the four steps of expanding macros.  The input is Swift source code.  This becomes a tree, representing the code's structure.  The macro implementation adds branches to the tree.  The result is Swift source with additional code.](../../assets/macro-expansion-full@2x.png)

Specifically, Swift expands macros in the following way:

1. The compiler reads the code,
   creating an in-memory representation of the syntax.

1. The compiler sends part of the in-memory representation
   to the macro implementation,
   which expands the macro.

1. The compiler replaces the macro call with its expanded form.

1. The compiler continues with compilation,
   using the expanded source code.

To go through the specific steps, consider the following:

```swift
let magicNumber = #fourCharacterCode("ABCD")
```

The `#fourCharacterCode` macro takes a string that's four characters long
and returns an unsigned 32-bit integer
that corresponds to the ASCII values in the string joined together.
Some file formats use integers like this to identify data
because they're compact but still readable in a debugger.
The <doc:Macros#Implementing-a-Macro> section below
shows how to implement this macro.

To expand the macros in the code above,
the compiler reads the Swift file
and creates an in-memory representation of that code
known an as *abstract syntax tree*, or AST.
The AST makes the code's structure explicit,
which makes it easier to write code that interacts with that structure ---
like a compiler or a macro implementation.
Here's a representation of the AST for the code above,
slightly simplified by omitting some extra detail:

![A tree diagram, with a constant as the root element.  The constant has a name, magic number, and a value.  The constant's value is a macro call.  The macro call has a name, fourCharacterCode, and arguments.  The argument is a string literal, ABCD.](../../assets/macro-ast-original@2x.png)

The diagram above shows how the structure of this code
is represented in memory.
Each element in the AST
corresponds to a part of the source code.
The "Constant declaration" AST element
has two child elements under it,
which represent the two parts of a constant declaration:
its name and its value.
The "Macro call" element has child elements
that represent the macro's name
and the list of arguments being passed to the macro.

As part of constructing this AST,
the compiler checks that the source code is valid Swift.
For example, `#fourCharacterCode` takes a single argument,
which must be a string.
If you tried to pass an integer argument,
or forgot the quotation mark (`"`) at the end of the string literal,
you'd get an error at this point in the process.

The compiler finds the places in the code where you call a macro,
and loads the external binary that implements those macros.
For each macro call,
the compiler passes part of the AST to that macro's implementation.
Here's a representation of that partial AST:

![A tree diagram, with a macro call as the root element.  The macro call has a name, fourCharacterCode, and arguments.  The argument is a string literal, ABCD.](../../assets/macro-ast-input@2x.png)

The implementation of the `#fourCharacterCode` macro
reads this partial AST as its input when expanding the macro.
A macro's implementation
operates only on the partial AST that it receives as its input,
meaning a macro always expands the same way
regardless of what code comes before and after it.
This limitation helps make macro expansion easier to understand,
and helps your code build faster
because Swift can avoid expanding macros that haven't changed.
<!-- TODO TR: Confirm -->
Swift helps macro authors avoid accidentally reading other input
by restricting the code that implements macros:

- The AST passed to a macro implementation
  contains only the AST elements that represent the macro,
  not any of the code that comes before or after it.

- The macro implementation runs in a sandboxed environment
  that prevents it from accessing the file system or the network.

In addition to these safeguards,
the macro's author is responsible for not reading or modifying anything
outside of the macro's inputs.
For example, a macro's expansion must not depend on the current time of day.

The implementation of `#fourCharacterCode`
generates a new AST containing the expanded code.
Here's what that code returns to the compiler:

![A tree diagram with a sigle node, the integer literal 1145258561.](../../assets/macro-ast-output@2x.png)

When the compiler receives this expansion,
it replaces the AST element that contains the macro call
with the element that contains the macro's expansion.
After macro expansion,
the compiler checks again to ensure
the program is still syntactically valid Swift
and all the types are correct.
That produces a final AST that can be compiled as usual:

![A tree diagram, with a constant as the root element.  The constant has a name, magic number, and a value.  The constant's value is the integer literal 1145258561](../../assets/macro-ast-result@2x.png)

This AST corresponds to Swift code like this:

```
let magicNumber = 1145258561
```

In this example, the input source code has only one macro,
but a real program could have several instances of the same macro
and several calls to different macros.
The compiler expands macros one at a time.

If one macro appears inside another,
the outer macro is expanded first ---
this lets the outer macro modify the inner macro before it's expanded.

<!-- OUTLINE

- TR: Is there any limit to nesting?
  TR: Is it valid to nest like this -- if so, anything to note about it?

  ```
  let something = #someMacro {
      struct A { }
      @someMacro struct B { }
  }
  ```

- Macro recursion is limited.
  One macro can call another,
  but a given macro can't directly or indirectly call itself.
  The result of macro expansion can include other macros,
  but it can't include a macro that uses this macro in its expansion
  or declare a new macro.
  (TR: Likely need to iterate on details here)
-->

## Implementing a Macro

To implement a macro, you make two components:
A type that performs the macro expansion,
and a library that declares the macro to expose it as API.
These parts are built separately from code that uses the macro,
even if you're developing the macro and its clients together,
because the macro implementation runs
as part of building the macro's clients.

To create a new macro using Swift Package Manager,
run `swift package init --type macro` ---
this creates several files,
including a template for a macro implementation and declaration.

To add macros to an existing project,
add a target for the macro implementation
and a target for the macro library.
For example,
you can add something like the following to your `Package.swift` file,
changing the names to match your project:

```swift
targets: [
    // Macro implementation that performs the source transformations.
    .macro(
        name: "MyProjectMacros",
        dependencies: [
            .product(name: "SwiftSyntaxMacros", package: "swift-syntax"),
            .product(name: "SwiftCompilerPlugin", package: "swift-syntax")
        ]
    ),

    // Library that exposes a macro as part of its API.
    .target(name: "MyProject", dependencies: ["MyProjectMacros"]),
]
```

The code above defines two targets:
`MyProjectMacros` contains the implementation of the macros,
and `MyProject` makes those macros available.

The implementation of a macro
uses the [SwiftSyntax][] module to interact with Swift code
in a structured way, using an AST.
If you created a new macro package with Swift Package Manager,
the generated `Package.swift` file
automatically includes a dependency on SwiftSyntax.
If you're adding macros to an existing project,
add a dependency on SwiftSyntax in your `Package.swift` file:

[SwiftSyntax]: http://github.com/apple/swift-syntax/

```swift
dependencies: [
    .package(url: "https://github.com/apple/swift-syntax.git", from: "some-tag"),
],
```

Replace the `some-tag` placeholder in the code above
with the Git tag for the version of SwiftSyntax you want to use.

Depending on your macro's role,
there's a corresponding protocol from SwiftSystem
that the macro implementation conforms to.
For example,
consider `#fourCharacterCode` from the previous section.
Here's a structure that implements that macro:

```swift
public struct FourCharacterCode: ExpressionMacro {
    public static func expansion(
        of node: some FreestandingMacroExpansionSyntax,
        in context: some MacroExpansionContext
    ) throws -> ExprSyntax {
        guard let argument = node.argumentList.first?.expression,
              let segments = argument.as(StringLiteralExprSyntax.self)?.segments,
              segments.count == 1,
              case .stringSegment(let literalSegment)? = segments.first
        else {
            throw CustomError.message("Need a static string")
        }

        let string = literalSegment.content.text
        guard let result = fourCharacterCode(for: string) else {
            throw CustomError.message("Invalid four-character code")
        }

        return "\(raw: result)"
    }
}

private func fourCharacterCode(for characters: String) -> UInt32? {
    guard characters.count == 4 else { return nil }

    var result: UInt32 = 0
    for character in characters {
        result = result << 8
        guard let asciiValue = character.asciiValue else { return nil }
        result += UInt32(asciiValue)
    }
    return result.bigEndian
}
enum CustomError: Error { case message(String) }
```

The `#fourCharacterCode` macro
is a freestanding macro that produces an expression,
so the `FourCharacterCode` type that implements it
conforms to the `ExpressionMacro` protocol.
The `ExpressionMacro` protocol has one requirement,
a `expansion(of:in:)` method that expands the AST.
For the list of macro roles and their corresponding SwiftSystem protocols,
see <doc:Attributes#attached> and <doc:Attributes#freestanding>
in <doc:Attributes>

To expand the `#fourCharacterCode` macro,
Swift sends the AST for the code that uses this macro
to the library that contains the macro implementation.
Inside the library, Swift calls `FourCharacterCode.expansion(of:in:)`,
passing in the AST and the context as arguments to the method.
The implementation of `expansion(of:in:)`
finds the string that was passed as an argument to `#fourCharacterCode`
and calculates the corresponding integer literal value.

In the example above,
the first `guard` block extracts the string literal from the AST,
assigning that AST element to `literalSegment`.
The second `guard` block
calls the private `FourCharacterCode(for:)` function.
Both of these blocks throw an error if the macro is used incorrectly ---
the error message becomes a compiler error
at the malformed call site.
For example,
if you try to call the macro as `#fourCharacterCode("AB" + "CD")`
the compiler shows the error "Need a static string".

The `expansion(of:in:)` method returns an instance of `ExprSyntax`,
a type from SwiftSyntax that represents an expression in an AST.
Because this type conforms to the `StringLiteralConvertible` protocol,
the macro implementation uses a string literal
as a lightweight syntax to create its result.
All of the SwiftSyntax types that you return from a macro implementation
conform to `StringLiteralConvertible`,
so you can use this approach when implementing any kind of macro.

<!-- TODO contrast the `\(raw:)` and non-raw version.  -->

<!--
The return-a-string APIs come from here

https://github.com/apple/swift-syntax/blob/main/Sources/SwiftSyntaxBuilder/Syntax%2BStringInterpolation.swift
-->


<!-- OUTLINE:

- Note:
  Behind the scenes, Swift serializes and deserializes the AST,
  to pass the data across process boundaries,
  but your macro implementation doesn't need to deal with any of that.

- This method is also passed a macro-expansion context, which you use to:

    + Generate unique symbol names
    + Produce diagnostics (`Diagnostic` and `SimpleDiagnosticMessage`)
    + Find a node's location in source

- Macro expansion happens in their surrounding context.
  A macro can affect that environment if it needs to —
  and a macro that has bugs can interfere with that environment.
  (Give guidance on when you'd do this.  It should be rare.)

- Generated symbol names let a macro
  avoid accidentally interacting with symbols in that environment.
  To generate a unique symbol name,
  call the `MacroExpansionContext.makeUniqueName()` method.

- Ways to create a syntax node include
  Making an instance of the `Syntax` struct,
  or `SyntaxToken`
  or `ExprSyntax`.
  (Need to give folks some general ideas,
  and enough guidance so they can sort through
  all the various `SwiftSyntax` node types and find the right one.)

- Attached macros follow the same general model as expression macros,
  but with more moving parts.

- Pick the subprotocol of `AttachedMacro` to conform to,
  depending on which kind of attached macro you're making.
  [This is probably a table]

  + `AccessorMacro` goes with `@attached(accessor)`
  + `ConformanceMacro` goes with `@attached(conformance)`
    [missing from the list under Declaring a Macro]
  + `MemberMacro` goes with `@attached(member)`
  + `PeerMacro` goes with `@attached(peer)`
  + `MemberAttributeMacro` goes with `@member(memberAttribute)`

- Code example of conforming to `MemberMacro`.

  ```
  static func expansion<
    Declaration: DeclGroupSyntax,
    Context: MacroExpansionContext
  >(
    of node: AttributeSyntax,
    providingMembersOf declaration: Declaration,
    in context: Context
  ) throws -> [DeclSyntax]
  ```

- Adding a new member by making an instance of `Declaration`,
  and returning it as part of the `[DeclSyntax]` list.

-->

## Developing and Debugging Macros

Macros are well suited to development using tests:
They transform one AST into another AST
without depending on any external state,
and without making changes to any external state.
In addition, you can create syntax nodes from a string literal,
which simplifies setting up the input for a test.
You can also read the `description` property of an AST
to get a string to compare against an expected value.
For example,
here's a test of the `#fourCharacterCode` macro from previous sections:

```swift
let source: SourceFileSyntax =
    """
    let abcd = #fourCharacterCode("ABCD")
    """

let file = BasicMacroExpansionContext.KnownSourceFile(
    moduleName: "MyModule",
    fullFilePath: "test.swift"
)

let context = BasicMacroExpansionContext(sourceFiles: [source: file])

let transformedSF = source.expand(
    macros:["fourCharacterCode": FourCC.self],
    in: context
)

let expectedDescription =
    """
    let abcd = 1145258561
    """

precondition(transformedSF.description == expectedDescription)
```

The example above tests the macro using a precondition,
but you could use a testing framework instead.

<!-- OUTLINE:

- Ways to view the macro expansion while debugging.
  The SE prototype provides `-Xfrontend -dump-macro-expansions` for this.
  [TR: Is this flag what we should suggest folks use,
  or will there be better command-line options coming?]

- Use diagnostics for macros that have constraints/requirements
  so your code can give a meaningful error to users when those aren't met,
  instead of letting the compiler try & fail to build the generated code.

Additional APIs and concepts to introduce in the future,
in no particular order:

- Using `SyntaxRewriter` and the visitor pattern for modifying the AST

- Adding a suggested correction using `FixIt`

- concept of trivia

- `TokenSyntax`
-->

<!--
This source file is part of the Swift.org open source project

Copyright (c) 2014 - 2023 Apple Inc. and the Swift project authors
Licensed under Apache License v2.0 with Runtime Library Exception

See https://swift.org/LICENSE.txt for license information
See https://swift.org/CONTRIBUTORS.txt for the list of Swift project authors
-->
