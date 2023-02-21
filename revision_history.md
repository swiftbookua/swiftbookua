# Історія змін - Document Revision History

Останні [зміни у оригіналі книги](https://docs.swift.org/swift-book/RevisionHistory/RevisionHistory.html). Перекладені оновлення є перекладеними у списку нижче.

**2022-06-06**

- Updated for Swift 5.7.
- Added the <doc:Concurrency#Sendable-Types> section,
  with information about sending data between actors and tasks,
  and added information about the `@Sendable` and `@unchecked` attributes
  to the <doc:Attributes#Sendable> and <doc:Attributes#unchecked> sections.
- Added the <doc:LexicalStructure#Regular-Expression-Literals> section
  with information about creating a regular expression.
- Added information about the short form of `if`-`let`
  to the <doc:TheBasics#Optional-Binding> section.
- Added information about `#unavailable`
  to the <doc:ControlFlow#Checking-API-Availability> section.

**2022-03-14**

- Updated for Swift 5.6.
- Updated the <doc:Expressions#Explicit-Member-Expression> section
  with information about using `#if`
  around chained method calls and other postfix expressions.
- Updated the visual styling of figures throughout.

**2021-09-20**

- Updated for Swift 5.5.
- Added information about asynchronous functions, tasks, and actors
  to the <doc:Concurrency> chapter,
  and to the <doc:Declarations#Actor-Declaration>,
  <doc:Declarations#Asynchronous-Functions-and-Methods>,
  and <doc:Expressions#Await-Operator> sections.
- Updated the <doc:LexicalStructure#Identifiers> section
  with information about identifiers that start with an underscore.

**2021-04-26**

- Updated for Swift 5.4.
- Added the <doc:AdvancedOperators#Result-Builders> and <doc:Attributes#resultBuilder> sections with information about result builders.
- Added the <doc:Expressions#Implicit-Conversion-to-a-Pointer-Type> section with information about how in-out parameters can be implicitly converted to unsafe pointers in a function call.
- Updated the <doc:Functions#Variadic-Parameters> and <doc:Declarations#Function-Declaration> sections, now that a function can have multiple variadic parameters.
- Updated the <doc:Expressions#Implicit-Member-Expression> section, now that implicit member expressions can be chained together.

**2020-09-16**

- Updated for Swift 5.3.
- Додано інформацію про функції з декількома прикінцевими замиканнями до розділу [Синтаксис прикінцевих замикань]({% link _book/1_language_guide/6_closures.md %}#Синтаксис-прикінцевих-замикань), and added information about how trailing closures are matched to parameters to the <doc:Expressions#Function-Call-Expression> section.
- До розділу [Підпорядкування протоколу за допомогою синтезованої реалізації]({% link _book/1_language_guide/20_protocols.md %}#Підпорядкування-протоколу-за-допомогою-синтезованої-реалізації) додано інформацію про синтезовані реалізації протоколу `Comparable` для перечислень.
- До розділу [Контекстуальна інструкція узагальнення Where]({% link _book/1_language_guide/21_generics.md %}#Контекстуальна-інструкція-узагальнення-Where) додано інформацію про нові місця, де можна застосувати інстукцію узагальнення `where`.
- Додано розділ [Безхазяйні опціональні посилання]({% link _book/1_language_guide/23_automatic_reference_counting.md %}#Безхазяйні-опціональні-посилання) з інформацією про використання безхазяйних посилань із опціональними значеннями.
- Added information about the `@main` attribute
  to the <doc:Attributes#main> section.
- Added `#filePath` to the <doc:Expressions#Literal-Expression> section,
  and updated the discussion of `#file`.
- Оновлено розділ [Замикання-емігранти]({% link _book/1_language_guide/6_closures.md %}#Замикання-емігранти), оскільки тепер замикання можуть неявно посилатись на `self` у нових сценаріях.
- Оновлено розділи [Обробка помилок за допомогою інструкції Do-Catch]({% link _book/1_language_guide/16_error_handling.md %}#Обробка-помилок-за-допомогою-інструкції-Do-Catch) та [Do-Statement]({% link _book/2_language_reference/05_statements.md %}#Do-Statement), оскільки тепер пункт `catch` може співпадати із кількома помилками.
- Added more information about `Any` and moved it into the new <doc:Types#Any-Type> section.
- Оновлено розділ [Спостерігачі за властивостями]({% link _book/1_language_guide/9_properties.md %}#Спостерігачі-за-властивостями), оскільки тепер ліниві властивості можуть мати спостерігачі.
- Updated the <doc:Declarations#Protocol-Declaration> section, now that members of an enumeration can satisfy protocol requirements.
- Updated the <doc:Declarations#Stored-Variable-Observers-and-Property-Observers> section to describe when the getter is called before the observer.
- Оновлено розділ [Безпека пам'яті]({% link _book/1_language_guide/24_memory_safety.md %}) згадками про атомарні операції.

**2020-03-24**

- Updated for Swift 5.2.
- Added information about passing a key path instead of a closure
  to the <doc:Expressions#Key-Path-Expression> section.
- Added the <doc:Declarations#Methods-with-Special-Names> section
  with information about syntactic sugar the lets instances of
  classes, structures, and enumerations be used with function call syntax.
- Оновлено розділ [Опції індексів]({% link _book/1_language_guide/11_subscripts.md %}#Опції-індексів): тепер індекси підтримують параметри зі значеннями за замовчанням.
- Updated the <doc:Types#Self-Type> section, now that the `Self` can be used in more contexts.
- Оновлено розділ [Опціонали, що розгортаються неявно]({% link _book/1_language_guide/0_the_basics.md %}#Опціонали-що-розгортаються-неявно), щоб зрозуміліше пояснити, що опціонали, що розгортаються неявно, можуть використовуватися або як опціональні, або як неопціональні значення. 

**2019-09-10**

- Updated for Swift 5.1.
- До розділу [Непрозорі типи]({% link _book/1_language_guide/22_opaque_types.md %}#Непрозорі-типи) додано інформацію про функції, котрі замість явного зазначення типу, що повертається, визначають лише протокол, до якого підпорядковане значення, що повертається.
- До розділу [Обгортки властивостей]({% link _book/1_language_guide/9_properties.md %}#Обгортки-властивостей) додано інформацію про обгортки властивостей.
- Added information about enumerations and structures that are frozen for library evolution to the <doc:Attributes#frozen> section.
- Додано розділи [Функції з неявним поверненням]({% link _book/1_language_guide/5_functions.md %}#Функції-з-неявним-поверненням) та [Скорочене оголошення гетерів]({% link _book/1_language_guide/9_properties.md %}#Скорочене-оголошення-гетерів) з інформацією про функції та гетери, в яких пропущено `return`.
- Додано інформацію про використання індексів у типах у розділ [Індекси типів]({% link _book/1_language_guide/11_subscripts.md %}#Індекси-типів).
- Updated the <doc:Patterns#Enumeration-Case-Pattern> section,
  now that an enumeration case pattern can match an optional value.
- Оновлено розділ [Почленні ініціалізатори для структур]({% link _book/1_language_guide/13_initialization.md %}#Почленні-ініціалізатори-для-структур): додано інформацію про опускання параметрів для властивостей зі значенням за замовчанням.
- Added information about dynamic members that are looked up by key path at runtime to the <doc:Attributes#dynamicMemberLookup> section.
- Added `macCatalyst` to the list of target environments in <doc:Statements#Conditional-Compilation-Block>.
- Updated the <doc:Types#Self-Type> section, now that `Self` can be used to refer to the type introduced by the current class, structure, or enumeration declaration.

**2019-03-25**

- Updated for Swift 5.0.
- Додано розділ [Розширені розділювачі рядків]({% link _book/1_language_guide/2_strings_and_characters.md %}#Розширені-розділювачі-рядків) та додано інформацію про розширені розділювачі рядків у розділ [Рядкові літерали]({% link _book/1_language_guide/2_strings_and_characters.md %}#рядкові-літерали).
- Added the <doc:Attributes#dynamicCallable> section
  with information about dynamically calling instances as functions
  using the `dynamicCallable` attribute.
- Added the <doc:Attributes#unknown> and <doc:Statements#Switching-Over-Future-Enumeration-Cases> sections
  with information about handling future enumeration cases
  in switch statements using
  the `unknown` switch case attribute.
- Added information about the identity key path (`\.self`)
  to the <doc:Expressions#Key-Path-Expression> section.
- Added information about using the less than (`<`) operator
  in platform conditions to the <doc:Statements#Conditional-Compilation-Block> section.

**2018-09-17**

- Updated for Swift 4.2.
- Додано інформацію про доступ до всіх елементів перечислення у розділ [Ітерування по елементах перечислення]({% link _book/1_language_guide/7_enumerations.md %}#Ітерування-по-елементах-перечислення).
- Added information about `#error` and `#warning`
  to the <doc:Statements#Compile-Time-Diagnostic-Statement> section.
- Added information about inlining
  to the <doc:Attributes#Declaration-Attributes> section
  under the `inlinable` and  `usableFromInline` attributes.
- Added information about members that are looked up by name at runtime
  to the <doc:Attributes#Declaration-Attributes> section
  under the `dynamicMemberLookup` attribute.
- Added information about the `requires_stored_property_inits` and `warn_unqualified_access` attributes
  to the <doc:Attributes#Declaration-Attributes> section.
- Added information about how to conditionally compile code
  depending on the Swift compiler version being used
  to the <doc:Statements#Conditional-Compilation-Block> section.
- Added information about `#dsohandle`
  to the <doc:Expressions#Literal-Expression> section.

**2018-03-29**

- Оновлено до Swift 4.1.
- Додано інформацію про синтезовані реалізації операторів рівності до розділу [Оператори рівності]({% link _book/1_language_guide/26_advanced_operators.md %}#Оператори-рівності).
- Added information about conditional protocol conformance
  to the <doc:Declarations#Extension-Declaration> section
  of the <doc:Declarations> chapter,
  та до розділу [Умовне підпорядкування до протоколу]({% link _book/1_language_guide/20_protocols.md %}#Умовне підпорядкування до протоколу) глави [Протоколи]({% link _book/1_language_guide/20_protocols.md %}).
- Додано інформацію про рекурсивні обмеження протоколів до розділу [Використання протоколів з обмеженнями асоційованих типів]({% link _book/1_language_guide/21_generics.md %}#Використання-протоколів-з-обмеженнями-асоційованих-типів).
- Added information about
  the `canImport()` and `targetEnvironment()` platform conditions
  to <doc:Statements#Conditional-Compilation-Block>.

**2017-12-04**

- Updated for Swift 4.0.3.
- Updated the <doc:Expressions#Key-Path-Expression> section,
  now that key paths support subscript components.

**2017-09-19**

- Оновлено до Swift 4.0.
- Додано інформацію про виключний доступ до пам'яті [Безпека доступу до пам'яті](language_guide/24_memory_safety.md).
- Додано розділ [Associated Types with a Generic Where Clause](https://docs.swift.org/swift-book/LanguageGuide/Generics.html#ID557) [Асоційовані типи з інструкцією узагальнення Where]({% link _book/1_language_guide/21_generics.md %}#Асоційовані-типи-з-інструкцією-узагальнення-Where). Тепер можна використовувати інструкцію узагальнення `where` для накладення обмежень на асоційовані типи.
- Додано інформацію про багаторядкові літерали до розділу [Рядкові літерали]({% link _book/1_language_guide/2_strings_and_characters.md %}#рядкові-літерали) глави [Рядки та символи]({% link _book/1_language_guide/2_strings_and_characters.md %}) 
  and to the <doc:LexicalStructure#String-Literals> section
  of the <doc:LexicalStructure> chapter.
- Updated the discussion of the `objc` attribute
  in <doc:Attributes#Declaration-Attributes>,
  now that this attribute is inferred in fewer places.
- Додано розділ [Узагальнені індекси]({% link _book/1_language_guide/21_generics.md %}#Узагальнені-індекси), оскільки індекси тепер можуть бути узагальненими.
- Updated the discussion
  in the <doc:Protocols#Protocol-Composition> section
  of the <doc:Protocols> chapter,
  and in the <doc:Types#Protocol-Composition-Type> section
  of the <doc:Types> chapter,
  now that protocol composition types can contain a superclass requirement.
- Updated the discussion of protocol extensions
  in <doc:Declarations#Extension-Declaration>
  now that `final` isn't allowed in them.
- Added information about preconditions and fatal errors
  to the <doc:TheBasics#Assertions-and-Preconditions> section.

**2017-03-27**

- Оновлено до Swift 3.1.
- Додано секцію [Extensions with a Generic Where Clause](Extensions-with-a-Generic-Where-Clause) - [Розширення з інструкцією узагальнення Where]({% link _book/1_language_guide/21_generics.md %}#розширення-з-інструкцією-узагальнення-where).
- Added examples of iterating over a range
  to the <doc:ControlFlow#For-In-Loops> section.
- Додано приклад перетворення числових типів, що можуть провалюватись, до розділу [Ненадійні ініціалізатори]({% link _book/1_language_guide/13_initialization.md %}#ненадійні-ініціалізатори).
- Added information to the <doc:Attributes#Declaration-Attributes> section
  about using the `available` attribute with a Swift language version.
- Updated the discussion in the <doc:Types#Function-Type> section
  to note that argument labels aren't allowed when writing a function type.
- Updated the discussion of Swift language version numbers
  in the <doc:Statements#Conditional-Compilation-Block> section,
  now that an optional patch number is allowed.
- Updated the discussion
  in the <doc:Types#Function-Type> section,
  now that Swift distinguishes between functions that take multiple parameters
  and functions that take a single parameter of a tuple type.
- Removed the Dynamic Type Expression section
  from the <doc:Expressions> chapter,
  now that `type(of:)` is a Swift standard library function.

