## Основи

Мова Swift - це нова мова програмування для розробки додатків для iOS, macOS, watchOS, та tvOS. Проте багато частин Swift будуть знайомі з вашого досвіду розробки на мовах C та Objective-C.

Мова Swift надає свої власні версії всіх фундаментальних типів C та Objective-C, включаючи `Int` для цілих чисел, `Double` та `Float` для значень з плаваючою комою, `Bool` для Булевих значень, та `String` для текстової інформації. Мова Swift також надає потужні версії трьох основних типів колекцій: `Array`, `Set`, та `Dictionary`, як описано в [Колекціях](3_collection_types.md).

Як і мова C, мова Swift використовує зміння щоб зберігати значення та щоб посилатись на них по ідентифікатору. У мові Swift також широко вживаються змінні, чиє значення не може бути змінене. Вони відомі як константи, але вони набагато потужніші ніж константи в мові C. Константи вживаються всюди у Swift, щоб зробити код безпечнішим та більш ясним у намірах, коли ви працюєте зі значеннями, котрим не потрібно змінюватись. 

Окрім знайомих типів, у мові Swift з'являються більш розвинені типи, котрих нема в Objective-C, наприклад, кортежі. Кортежі дозволяють створювати та передавати групи значень. За допомогою кортежа можна повернути одразу декілька значень з функції як єдине об'єднане значення.

У мові Swift також вводяться опціональні типи - опціонали - які дозволяють обробляти відсутність значення. Опціонали виражають або "є деяке значення, і воно дорівнює x” або “немає взагалі жодного значення”. Користування опціоналами схоже на використання `nil` із вказівниками в Objective-C, але опціонали працюють з усіма типами, а не тільки з класами. Опціонали є не просто більш безпечніші та виразніші аніж вказівники на `nil` в Objective-C, вони лежать у серці найбільш потужних можливостей Swift.

Мова Swift типобезпечна, тобто мова допомагає вам виражатись ясно щодо типів значень, з якими  працює ваш код. Якщо частина вашого коду очікує `String`, типобезпечність не дасть вам помилково передати в нього `Int`. Подібним чином типобезпечність не дасть вам помилково передати опціональний `String` в частину коду, котра очікує неопціональний `String`. Типобезпечність дозволяє в процесі розробки відловлювати і виправляти помилки як можна раніше.

### Константи і змінні

Константи і змінні асоціюють ім'я (як, наприклад, `maximumNumberOfLoginAttempts` чи `welcomeMessage`) із значенням певного типу (такі як число `10` чи рядок `"Hello"`). Як тільки значення константи задано, воно не може бути змінене, тоді як значення змінної може бути змінене на інше в майбутньому.

#### Оголошення констант та змінних

Константи і змінні повинні бути оголошені до того, як вони будуть вжиті. Константи оголошуються за допомогою ключового слова `let`, а змінні - за допомогою ключового слова `var`. Ось приклад того, як константи і змінні можуть бути використані для того, щоб відслідковувати кількість спроб користувача увійти в систему.  

```swift
let maximumNumberOfLoginAttempts = 10var currentLoginAttempt = 0
```

This code can be read as:
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

```swift
// This is a comment.
```

Multiline comments start with a forward-slash followed by an asterisk (`/*`) and end with an asterisk followed by a forward-slash (`*/`):

```swift
/* This is also a comment
 but is written over multiple lines. */
```
Unlike multiline comments in C, multiline comments in Swift can be nested inside other multiline comments. You write nested comments by starting a multiline comment block and then starting a second multiline comment within the first block. The second block is then closed, followed by the first block:

```swift/* This is the start of the first multiline comment. /* This is the second, nested multiline comment. */ This is the end of the first multiline comment. */
```Nested multiline comments enable you to comment out large blocks of code quickly and easily, even if the code already contains multiline comments.

#### SemicolonsUnlike many other languages, Swift does not require you to write a semicolon (`;`) after each statement in your code, although you can do so if you wish. However, semicolons *are* required if you want to write multiple separate statements on a single line:

```swift
let cat = "🐱"; print(cat)// Prints "🐱"
```

#### Integers*Integers* are whole numbers with no fractional component, such as `42` and `-23`. Integers are either *signed* (positive, zero, or negative) or *unsigned* (positive or zero).
Swift provides signed and unsigned integers in 8, 16, 32, and 64 bit forms. These integers follow a naming convention similar to C, in that an 8-bit unsigned integer is of type `UInt8`, and a 32-bit signed integer is of type `Int32`. Like all types in Swift, these integer types have capitalized names.

#### Integer BoundsYou can access the minimum and maximum values of each integer type with its `min` and `max` properties:

```swift
let minValue = UInt8.min  // minValue is equal to 0, and is of type UInt8let maxValue = UInt8.max  // maxValue is equal to 255, and is of type UInt8
```

The values of these properties are of the appropriate-sized number type (such as `UInt8` in the example above) and can therefore be used in expressions alongside other values of the same type.
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
> `Double` has a precision of at least 15 decimal digits, whereas the precision of `Float` can be as little as 6 decimal digits. The appropriate floating-point type to use depends on the nature and range of values you need to work with in your code. In situations where either type would be appropriate, `Double` is preferred.

#### Type Safety and Type Inference
Swift is a type-safe language. A type safe language encourages you to be clear about the types of values your code can work with. If part of your code expects a String, you can’t pass it an Int by mistake.

Because Swift is type safe, it performs type checks when compiling your code and flags any mismatched types as errors. This enables you to catch and fix errors as early as possible in the development process.
Type-checking helps you avoid errors when you’re working with different types of values. However, this doesn’t mean that you have to specify the type of every constant and variable that you declare. If you don’t specify the type of value you need, Swift uses *type inference* to work out the appropriate type. Type inference enables a compiler to deduce the type of a particular expression automatically when it compiles your code, simply by examining the values you provide.
Because of type inference, Swift requires far fewer type declarations than languages such as C or Objective-C. Constants and variables are still explicitly typed, but much of the work of specifying their type is done for you.Type inference is particularly useful when you declare a constant or variable with an initial value. This is often done by assigning a *literal value* (or *literal*) to the constant or variable at the point that you declare it. (A literal value is a value that appears directly in your source code, such as `42` and `3.14159` in the examples below.)
For example, if you assign a literal value of `42` to a new constant without saying what type it is, Swift infers that you want the constant to be an `Int`, because you have initialized it with a number that looks like an integer:

```swiftlet meaningOfLife = 42// meaningOfLife is inferred to be of type Int
```
Likewise, if you don’t specify a type for a floating-point literal, Swift infers that you want to create a `Double`:

```swiftlet pi = 3.14159// pi is inferred to be of type Double
```
Swift always chooses `Double` (rather than `Float`) when inferring the type of floating-point numbers.
If you combine integer and floating-point literals in an expression, a type of `Double` will be inferred from the context:```swiftlet anotherPi = 3 + 0.14159// anotherPi is also inferred to be of type Double
```
The literal value of `3` has no explicit type in and of itself, and so an appropriate output type of `Double` is inferred from the presence of a floating-point literal as part of the addition.
####Numeric LiteralsInteger literals can be written as:
 + A decimal number, with no prefix + A binary number, with a 0b prefix + An octal number, with a 0o prefix + A hexadecimal number, with a 0x prefix
	All of these integer literals have a decimal value of `17`:

```swiftlet decimalInteger = 17let binaryInteger = 0b10001       // 17 in binary notationlet octalInteger = 0o21           // 17 in octal notationlet hexadecimalInteger = 0x11     // 17 in hexadecimal notation
```Floating-point literals can be decimal (with no prefix), or hexadecimal (with a `0x` prefix). They must always have a number (or hexadecimal number) on both sides of the decimal point. Decimal floats can also have an optional exponent, indicated by an uppercase or lowercase `e`; hexadecimal floats must have an exponent, indicated by an uppercase or lowercase `p`.
For decimal numbers with an exponent of `exp`, the base number is multiplied by 10<sup>exp</sup>:
 `1.25e2` means 1.25 x 10<sup>2</sup>, or `125.0`.
 
 `1.25e-2` means 1.25 x 10<sup>-2</sup>, or `0.0125`.
 For hexadecimal numbers with an exponent of exp, the base number is multiplied by 2<sup>exp</sup>:
 `0xFp2` means 15 x 2<sup>2</sup>, or 60.0.
  `0xFp-2` means 15 x 2<sup>-2</sup>, or 3.75.
 All of these floating-point literals have a decimal value of `12.1875`:
```swift
let decimalDouble = 12.1875let exponentDouble = 1.21875e1let hexadecimalDouble = 0xC.3p0
```
Numeric literals can contain extra formatting to make them easier to read. Both integers and floats can be padded with extra zeros and can contain underscores to help with readability. Neither type of formatting affects the underlying value of the literal:

```swiftlet paddedDouble = 000123.456let oneMillion = 1_000_000let justOverOneMillion = 1_000_000.000_000_1
```

#### Numeric Type Conversion
Use the `Int` type for all general-purpose integer constants and variables in your code, even if they are known to be non-negative. Using the default integer type in everyday situations means that integer constants and variables are immediately interoperable in your code and will match the inferred type for integer literal values.
Use other integer types only when they are specifically needed for the task at hand, because of explicitly-sized data from an external source, or for performance, memory usage, or other necessary optimization. Using explicitly-sized types in these situations helps to catch any accidental value overflows and implicitly documents the nature of the data being used.
##### Integer Conversion
The range of numbers that can be stored in an integer constant or variable is different for each numeric type. An `Int8` constant or variable can store numbers between `-128` and `127`, whereas a `UInt8` constant or variable can store numbers between `0` and `255`. A number that will not fit into a constant or variable of a sized integer type is reported as an error when your code is compiled:

```swiftlet cannotBeNegative: UInt8 = -1// UInt8 cannot store negative numbers, and so this will report an errorlet tooBig: Int8 = Int8.max + 1// Int8 cannot store a number larger than its maximum value,// and so this will also report an error
```
Because each numeric type can store a different range of values, you must opt in to numeric type conversion on a case-by-case basis. This opt-in approach prevents hidden conversion errors and helps make type conversion intentions explicit in your code.
To convert one specific number type to another, you initialize a new number of the desired type with the existing value. In the example below, the constant twoThousand is of type `UInt16`, whereas the constant one is of type `UInt8`. They cannot be added together directly, because they are not of the same type. Instead, this example calls `UInt16(one)` to create a new `UInt16` initialized with the value of one, and uses this value in place of the original:

```swiftlet twoThousand: UInt16 = 2_000let one: UInt8 = 1let twoThousandAndOne = twoThousand + UInt16(one)
```
Because both sides of the addition are now of type `UInt16`, the addition is allowed. The output constant (`twoThousandAndOne`) is inferred to be of type `UInt16`, because it is the sum of two `UInt16` values.
`SomeType(ofInitialValue)` is the default way to call the initializer of a Swift type and pass in an initial value. Behind the scenes, `UInt16` has an initializer that accepts a `UInt8` value, and so this initializer is used to make a new `UInt16` from an existing `UInt8`. You can’t pass in any type here, however—it has to be a type for which `UInt16` provides an initializer. Extending existing types to provide initializers that accept new types (including your own type definitions) is covered in [Extensions]().

##### Integer and Floating-Point Conversion
Conversions between integer and floating-point numeric types must be made explicit:

```swiftlet three = 3let pointOneFourOneFiveNine = 0.14159let pi = Double(three) + pointOneFourOneFiveNine// pi equals 3.14159, and is inferred to be of type Double```

Here, the value of the constant `three` is used to create a new value of type `Double`, so that both sides of the addition are of the same type. Without this conversion in place, the addition would not be allowed.
Floating-point to integer conversion must also be made explicit. An integer type can be initialized with a `Double` or `Float` value:

```swiftlet integerPi = Int(pi)// integerPi equals 3, and is inferred to be of type Int
```
Floating-point values are always truncated when used to initialize a new integer value in this way. This means that `4.75` becomes `4`, and `-3.9` becomes `-3`.> Note>
> The rules for combining numeric constants and variables are different from the rules for numeric literals. The literal value 3 can be added directly to the literal value `0.14159`, because number literals do not have an explicit type in and of themselves. Their type is inferred only at the point that they are evaluated by the compiler.
#### Type Aliases
*Type aliases* define an alternative name for an existing type. You define type aliases with the typealias `keyword`.
Type aliases are useful when you want to refer to an existing type by a name that is contextually more appropriate, such as when working with data of a specific size from an external source:

```swifttypealias AudioSample = UInt16
```
Once you define a type alias, you can use the alias anywhere you might use the original name:

```swiftvar maxAmplitudeFound = AudioSample.min// maxAmplitudeFound is now 0
```

Here, `AudioSample` is defined as an alias for `UInt16`. Because it is an alias, the call to `AudioSample.min` actually calls `UInt16.min`, which provides an initial value of `0` for the `maxAmplitudeFound` variable.#### Booleans
Swift has a basic *Boolean* type, called `Bool`. Boolean values are referred to as *logical*, because they can only ever be true or false. Swift provides two Boolean constant values, `true` and `false`:

```swiftlet orangesAreOrange = truelet turnipsAreDelicious = false
```
The types of `orangesAreOrange` and `turnipsAreDelicious` have been inferred as `Bool` from the fact that they were initialized with Boolean literal values. As with `Int` and `Double` above, you don’t need to declare constants or variables as `Bool` if you set them to `true` or `false` as soon as you create them. Type inference helps make Swift code more concise and readable when it initializes constants or variables with other values whose type is already known.
Boolean values are particularly useful when you work with conditional statements such as the `if` statement:

```swiftif turnipsAreDelicious {    print("Mmm, tasty turnips!")} else {    print("Eww, turnips are horrible.")}// Prints "Eww, turnips are horrible."
```
Conditional statements such as the if statement are covered in more detail in [Control Flow]().Swift’s type safety prevents non-Boolean values from being substituted for `Bool`. The following example reports a compile-time error:

```swiftlet i = 1if i {    // this example will not compile, and will report an error}
```
However, the alternative example below is valid:

```swiftlet i = 1if i == 1 {    // this example will compile successfully}
```
The result of the `i == 1` comparison is of type `Bool`, and so this second example passes the type-check. Comparisons like `i == 1` are discussed in [Basic Operators]().As with other examples of type safety in Swift, this approach avoids accidental errors and ensures that the intention of a particular section of code is always clear.

####Tuples
*Tuples* group multiple values into a single compound value. The values within a tuple can be of any type and do not have to be of the same type as each other.In this example, `(404, "Not Found")` is a tuple that describes an HTTP *status code*. An HTTP status code is a special value returned by a web server whenever you request a web page. A status code of `404 Not Found` is returned if you request a webpage that doesn’t exist.

```swiftlet http404Error = (404, "Not Found")// http404Error is of type (Int, String), and equals (404, "Not Found")
```
The `(404, "Not Found")` tuple groups together an `Int` and a `String` to give the HTTP status code two separate values: a number and a human-readable description. It can be described as “a tuple of type `(Int, String)`”.You can create tuples from any permutation of types, and they can contain as many different types as you like. There’s nothing stopping you from having a tuple of type `(Int, Int, Int)`, or `(String, Bool)`, or indeed any other permutation you require.
You can *decompose* a tuple’s contents into separate constants or variables, which you then access as usual:```swiftlet (statusCode, statusMessage) = http404Errorprint("The status code is \(statusCode)")// Prints "The status code is 404"print("The status message is \(statusMessage)")// Prints "The status message is Not Found"
```
If you only need some of the tuple’s values, ignore parts of the tuple with an underscore (`_`) when you decompose the tuple:

```swiftlet (justTheStatusCode, _) = http404Errorprint("The status code is \(justTheStatusCode)")// Prints "The status code is 404"
```
Alternatively, access the individual element values in a tuple using index numbers starting at zero:

```swiftprint("The status code is \(http404Error.0)")// Prints "The status code is 404"print("The status message is \(http404Error.1)")// Prints "The status message is Not Found"
```
You can name the individual elements in a tuple when the tuple is defined:

```swiftlet http200Status = (statusCode: 200, description: "OK")If you name the elements in a tuple, you can use the element names to access the values of those elements:print("The status code is \(http200Status.statusCode)")// Prints "The status code is 200"print("The status message is \(http200Status.description)")// Prints "The status message is OK"
```
Tuples are particularly useful as the return values of functions. A function that tries to retrieve a web page might return the `(Int, String)` tuple type to describe the success or failure of the page retrieval. By returning a tuple with two distinct values, each of a different type, the function provides more useful information about its outcome than if it could only return a single value of a single type. For more information, see Functions with Multiple Return Values.

> **Note**
> 
> Tuples are useful for temporary groups of related values. They are not suited to the creation of complex data structures. If your data structure is likely to persist beyond a temporary scope, model it as a class or structure, rather than as a tuple. For more information, see [Classes and Structures]().

