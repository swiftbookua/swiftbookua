## Основи

Мова Swift - це нова мова програмування для розробки додатків для iOS, macOS, watchOS, та tvOS. Проте багато частин Swift будуть знайомі з вашого досвіду розробки на мовах C та Objective-C.

Мова Swift надає свої власні версії всіх фундаментальних типів C та Objective-C, включаючи `Int` для цілих чисел, `Double` та `Float` для значень з плаваючою комою, `Bool` для Булевих значень, та `String` для текстової інформації. Мова Swift також надає потужні версії трьох основних типів колекцій: `Array`, `Set`, та `Dictionary`, як описано в [Колекціях](3_collection_types.md).

Як і мова C, мова Swift використовує зміння щоб зберігати значення та щоб посилатись на них по ідентифікатору. У мові Swift також широко вживаються змінні, чиє значення не може бути змінене. Вони відомі як константи, але вони набагато потужніші ніж константи в мові C. Константи вживаються всюди у Swift, щоб зробити код безпечнішим та більш ясним у намірах, коли ви працюєте зі значеннями, котрим не потрібно змінюватись. 

Окрім знайомих типів, у мові Swift з'являються більш розвинені типи, котрих нема в Objective-C, наприклад, кортежі. Кортежі дозволяють створювати та передавати групи значень. За допомогою кортежа можна повернути одразу декілька значень з функції як єдине об'єднане значення.

У мові Swift також вводяться опціональні типи - опціонали - які дозволяють обробляти відсутність значення. Опціонали виражають або "є деяке значення, і воно дорівнює x” або “немає взагалі жодного значення”. Користування опціоналами схоже на використання `nil` із вказівниками в Objective-C, але опціонали працюють з усіма типами, а не тільки з класами. Опціонали є не просто більш безпечніші та виразніші аніж вказівники на `nil` в Objective-C, вони лежать у серці найбільш потужних можливостей Swift.

Мова Swift типобезпечна, тобто мова допомагає вам виражатись ясно щодо типів значень, з якими  працює ваш код. If part of your code expects a String, type safety prevents you from passing it an Int by mistake. Likewise, type safety prevents you from accidentally passing an optional String to a piece of code that expects a nonoptional String. Type safety helps you catch and fix errors as early as possible in the development process.


Swift is a type-safe language, which means the language helps you to be clear about the types of values your code can work with. If part of your code expects a String, type safety prevents you from passing it an Int by mistake. Likewise, type safety prevents you from accidentally passing an optional String to a piece of code that expects a nonoptional String. Type safety helps you catch and fix errors as early as possible in the development process.



