## Основи

Мова Swift - це нова мова програмування для розробки додатків для iOS, macOS, watchOS, та tvOS. Проте багато частин Swift будуть знайомі з вашого досвіду розробки на мовах C та Objective-C.

Мова Swift надає свої власні версії всіх фундаментальних типів C та Objective-C, включаючи `Int` для цілих чисел, `Double` та `Float` для значень з плаваючою комою, `Bool` для Булевих значень, та `String` для текстової інформації. Мова Swift також надає потужні версії трьох основних типів колекцій: `Array`, `Set`, та `Dictionary`, як описано в [Колекціях](3_collection_types.md).

Як і мова C, мова Swift використовує зміння щоб зберігати значення та щоб посилатись на них по ідентифікатору. У мові Swift також широко вживаються змінні, чиє значення не може бути змінене. Вони відомі як константи, але вони набагато потужніші ніж константи в мові C. Константи вживаються всюди у Swift, щоб зробити код безпечнішим та більш ясним у намірах, коли ви працюєте зі значеннями, котрим не потрібно змінюватись. 

Окрім знайомих типів, у мові Swift з'являються більш розвинені типи, котрих нема в Objective-C, наприклад, кортежі. Кортежі дозволяють створювати та передавати групи значень. За допомогою кортежа можна повернути одразу декілька значень з функції як єдине об'єднане значення.

У мові Swift також вводяться опціональні типи - опціонали - які дозволяють обробляти відсутність значення. Опціонали виражають або "є деяке значення, і воно дорівнює x” або “немає взагалі жодного значення”. Користування опціоналами схоже на використання `nil` із вказівниками в Objective-C, але опціонали працюють з усіма типами, а не тільки з класами. Опціонали є не просто більш безпечніші та виразніші аніж вказівники на `nil` в Objective-C, вони лежать у серці найбільш потужних можливостей Swift.

Мова Swift типобезпечна, тобто мова допомагає вам виражатись ясно щодо типів значень, з якими  працює ваш код. Якщо частина вашого коду очікує `String`, типобезпечність не дасть вам помилково передати в нього `Int`. Подібним чином типобезпечність не дасть вам помилково передати опціональний `String` в частину коду, котра очікує неопціональний `String`. Типобезпечність дозволяє в процесі розробки відловлювати і виправляти помилки як можна раніше.

### Constants and VariablesConstants and variables associate a name (such as `maximumNumberOfLoginAttempts` or `welcomeMessage`) with a value of a particular type (such as the number `10` or the string `"Hello"`). The value of a constant cannot be changed once it is set, whereas a variable can be set to a different value in the future.####Declaring Constants and VariablesConstants and variables must be declared before they are used. You declare constants with the `let` keyword and variables with the `var` keyword. Here’s an example of how constants and variables can be used to track the number of login attempts a user has made:

```swiftlet maximumNumberOfLoginAttempts = 10var currentLoginAttempt = 0```This code can be read as:
“Declare a new constant called `maximumNumberOfLoginAttempts`, and give it a value of `10`. Then, declare a new variable called `currentLoginAttempt`, and give it an initial value of `0`.”In this example, the maximum number of allowed login attempts is declared as a constant, because the maximum value never changes. The current login attempt counter is declared as a variable, because this value must be incremented after each failed login attempt.You can declare multiple constants or multiple variables on a single line, separated by commas:```swiftvar x = 0.0, y = 0.0, z = 0.0```> **Note**
> > If a stored value in your code is not going to change, always declare it as a constant with the `let` keyword. Use variables only for storing values that need to be able to change.####Type AnnotationsYou can provide a type annotation when you declare a constant or variable, to be clear about the kind of values the constant or variable can store. Write a type annotation by placing a colon after the constant or variable name, followed by a space, followed by the name of the type to use.This example provides a type annotation for a variable called `welcomeMessage`, to indicate that the variable can store `String` values:```swiftvar welcomeMessage: String
```
The colon in the declaration means “…of type…,” so the code above can be read as:
“Declare a variable called `welcomeMessage` that is of type `String`.”
The phrase “of type `String`” means “can store any `String` value.” Think of it as meaning “the type of thing” (or “the kind of thing”) that can be stored.
The `welcomeMessage` variable can now be set to any string value without error:

```swiftwelcomeMessage = "Hello"
```
You can define multiple related variables of the same type on a single line, separated by commas, with a single type annotation after the final variable name:

```swiftvar red, green, blue: Double
```> **Note**
> > It is rare that you need to write type annotations in practice. If you provide an initial value for a constant or variable at the point that it is defined, Swift can almost always infer the type to be used for that constant or variable, as described in [Type Safety and Type Inference](). In the `welcomeMessage` example above, no initial value is provided, and so the type of the `welcomeMessage` variable is specified with a type annotation rather than being inferred from an initial value.
 

####Naming Constants and VariablesConstant and variable names can contain almost any character, including Unicode characters:

```swiftlet π = 3.14159let 你好 = "你好世界"let 🐶🐮 = "dogcow"
```Constant and variable names cannot contain whitespace characters, mathematical symbols, arrows, private-use (or invalid) Unicode code points, or line- and box-drawing characters. Nor can they begin with a number, although numbers may be included elsewhere within the name.
Once you’ve declared a constant or variable of a certain type, you can’t redeclare it again with the same name, or change it to store values of a different type. Nor can you change a constant into a variable or a variable into a constant.> **Note**>
> If you need to give a constant or variable the same name as a reserved Swift keyword, surround the keyword with backticks (`` ` ``) when using it as a name. However, avoid using keywords as names unless you have absolutely no choice.You can change the value of an existing variable to another value of a compatible type. In this example, the value of friendlyWelcome is changed from "Hello!" to "Bonjour!":

```swiftvar friendlyWelcome = "Hello!"friendlyWelcome = "Bonjour!"// friendlyWelcome is now "Bonjour!"
```Unlike a variable, the value of a constant cannot be changed once it is set. Attempting to do so is reported as an error when your code is compiled:

```swiftlet languageName = "Swift"languageName = "Swift++"// This is a compile-time error: languageName cannot be changed.```

####Printing Constants and VariablesYou can print the current value of a constant or variable with the `print(_:separator:terminator:)` function:```swiftprint(friendlyWelcome)// Prints "Bonjour!"
``` 
The `print(_:separator:terminator:)` function is a global function that prints one or more values to an appropriate output. In Xcode, for example, the `print(_:separator:terminator:)` function prints its output in Xcode’s “console” pane. The separator and terminator parameter have default values, so you can omit them when you call this function. By default, the function terminates the line it prints by adding a line break. To print a value without a line break after it, pass an empty string as the terminator—for example, `print(someValue, terminator: "")`. For information about parameters with default values, see [Default Parameter Values]().Swift uses *string interpolation* to include the name of a constant or variable as a placeholder in a longer string, and to prompt Swift to replace it with the current value of that constant or variable. Wrap the name in parentheses and escape it with a backslash before the opening parenthesis:

```swiftprint("The current value of friendlyWelcome is \(friendlyWelcome)")// Prints "The current value of friendlyWelcome is Bonjour!"
```
> **Note**> 
> All options you can use with string interpolation are described in String Interpolation.#### CommentsUse comments to include nonexecutable text in your code, as a note or reminder to yourself. Comments are ignored by the Swift compiler when your code is compiled.Comments in Swift are very similar to comments in C. Single-line comments begin with two forward-slashes (`//`):

```swift// This is a comment.
```
Multiline comments start with a forward-slash followed by an asterisk (`/*`) and end with an asterisk followed by a forward-slash (`*/`):

```swift/* This is also a comment but is written over multiple lines. */
```
Unlike multiline comments in C, multiline comments in Swift can be nested inside other multiline comments. You write nested comments by starting a multiline comment block and then starting a second multiline comment within the first block. The second block is then closed, followed by the first block:

```swift/* This is the start of the first multiline comment. /* This is the second, nested multiline comment. */ This is the end of the first multiline comment. */
```Nested multiline comments enable you to comment out large blocks of code quickly and easily, even if the code already contains multiline comments.

#### SemicolonsUnlike many other languages, Swift does not require you to write a semicolon (`;`) after each statement in your code, although you can do so if you wish. However, semicolons *are* required if you want to write multiple separate statements on a single line:

```swiftlet cat = "🐱"; print(cat)// Prints "🐱"```

#### Integers*Integers* are whole numbers with no fractional component, such as `42` and `-23`. Integers are either *signed* (positive, zero, or negative) or *unsigned* (positive or zero).
Swift provides signed and unsigned integers in 8, 16, 32, and 64 bit forms. These integers follow a naming convention similar to C, in that an 8-bit unsigned integer is of type `UInt8`, and a 32-bit signed integer is of type `Int32`. Like all types in Swift, these integer types have capitalized names.

#### Integer BoundsYou can access the minimum and maximum values of each integer type with its `min` and `max` properties:

```swiftlet minValue = UInt8.min  // minValue is equal to 0, and is of type UInt8let maxValue = UInt8.max  // maxValue is equal to 255, and is of type UInt8
```The values of these properties are of the appropriate-sized number type (such as `UInt8` in the example above) and can therefore be used in expressions alongside other values of the same type.
#### IntIn most cases, you don’t need to pick a specific size of integer to use in your code. Swift provides an additional integer type, `Int`, which has the same size as the current platform’s native word size:
 + On a 32-bit platform, `Int` is the same size as `Int32`. 
 + On a 64-bit platform, `Int` is the same size as `Int64`.
	Unless you need to work with a specific size of integer, always use `Int` for integer values in your code. This aids code consistency and interoperability. Even on 32-bit platforms, `Int` can store any value between `-2,147,483,648` and `2,147,483,647`, and is large enough for many integer ranges.#### UIntSwift also provides an unsigned integer type, `UInt`, which has the same size as the current platform’s native word size:
 + On a 32-bit platform, `UInt` is the same size as `UInt32`.
 + On a 64-bit platform, `UInt` is the same size as `UInt64`.
 > **Note**
> 
> Use `UInt` only when you specifically need an unsigned integer type with the same size as the platform’s native word size. If this is not the case, `Int` is preferred, even when the values to be stored are known to be non-negative. A consistent use of `Int` for integer values aids code interoperability, avoids the need to convert between different number types, and matches integer type inference, as described in [Type Safety and Type Inference]().
 
#### Floating-Point NumbersFloating-point numbers are numbers with a fractional component, such as `3.14159`, `0.1`, and `-273.15`.Floating-point types can represent a much wider range of values than integer types, and can store numbers that are much larger or smaller than can be stored in an `Int`. Swift provides two signed floating-point number types:
 
 + Double represents a 64-bit floating-point number. + Float represents a 32-bit floating-point number.
 > **Note**> 
> `Double` has a precision of at least 15 decimal digits, whereas the precision of `Float` can be as little as 6 decimal digits. The appropriate floating-point type to use depends on the nature and range of values you need to work with in your code. In situations where either type would be appropriate, `Double` is preferred.#### Type Safety and Type InferenceSwift is a type-safe language. A type safe language encourages you to be clear about the types of values your code can work with. If part of your code expects a String, you can’t pass it an Int by mistake.