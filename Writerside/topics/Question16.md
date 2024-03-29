# Вопрос 16

Принцип можно описать так:
<tip>
Модуль должен отвечать за одного и только за одного пользователя или заинтересованное лицо.
</tip>

Или же:
<tip>
Модуль должен отвечать за одного и только за одного актора.
</tip>
Программное обеспечение изменяется для удовлетворения нужд пользователей и заинтересованных лиц. Пользователи и заинтересованные лица как
раз и есть та самая «причина для изменения», о которой говорит принцип.
    К сожалению, слова «пользователь» и «заинтересованное лицо» не совсем правильно использовать здесь, потому что одного и того же изменения системы могут желать несколько пользователей или заинтересованных лиц.
Более правильным выглядит понятие группы, состоящей из одного или нескольких лиц, желающих данного изменения. Мы будем называть такие
группы акторами (actor).

Теперь определим, что означает слово «модуль». Самое простое определение — файл с исходным кодом. В большинстве случаев это определение
можно принять. Однако некоторые языки среды разработки не используют
исходные файлы для хранения кода. В таких случаях модуль — это просто
связный набор функций и структур данных.

Слово «связный» подразумевает принцип единственной ответственности.
Связность — это сила, которая связывает код, ответственный за единственного актора.

## Пример {collapsible="true"}
Класс Employee из приложения платежной ведомости. Он имеет три метода: calculatePay(), reportHours() и save()

![image.png](image.png)

Этот класс нарушает принцип единственной ответственности, потому что
три его метода отвечают за три разных актора.
* Реализация метода calculatePay() определяется бухгалтерией.
* Реализация метода reportHours() определяется и используется отделом по работе с персоналом.
* Реализация метода save() определяется администраторами баз данных.

Поместив исходный код этих трех методов в общий класс Employee, разработчики объединили перечисленных акторов. В результате такого объединения
действия сотрудников бухгалтерии могут затронуть что-то, что требуется
сотрудникам отдела по работе с персоналом.

Теперь вообразите, что сотрудники бухгалтерии решили немного изменить
алгоритм расчета не сверхурочных часов. Сотрудники отдела по работе
с персоналом были бы против такого изменения, потому что вычисленное
время они используют для других целей.

Разработчик, которому было поручено внести изменение, заметил, что функция regularHours() вызывается методом calculatePay(), но, к сожалению, не
заметил, что она также вызывается методом reportHours().

Разработчик внес требуемые изменения и тщательно протестировал результат. Сотрудники бухгалтерии проверили и подтвердили, что обновленная
функция действует в соответствии с их пожеланиями, после чего измененная версия системы была развернута.

Разумеется, сотрудники отдела по работе с персоналом не знали о произошедшем и продолжали использовать отчеты, генерируемые функцией
reportHours(), но теперь содержащие неправильные цифры. В какой-то
момент проблема вскрылась, и сотрудники отдела по работе с персоналом
разом побледнели от ужаса, потому что ошибочные данные обошлись их
бюджету в несколько миллионов долларов.

Все мы видели нечто подобное. Эти проблемы возникают из-за того, что
мы вводим в работу код, от которого зависят разные акторы. Принцип
единственной ответственности требует разделять код, от которого зависят
разные акторы.

## Решение примера {collapsible="true"}

Существует много решений этой проблемы. Но каждое связано с перемещением функций в разные классы.

Наиболее очевидным, пожалуй, является решение, связанное с отделением
данных от функций. Три класса, как показано на рис. 7.3, используют общие
данные EmployeeData — простую структуру без методов. Каждый класс включает только исходный код для конкретной функции. Эти три класса никак
не зависят друг от друга. То есть любое непреднамеренное дублирование
исключено.

Недостаток такого решения — разработчик теперь должен создавать экземпляры трех классов и следить за ними. Эта проблема часто решается применением шаблона проектирования «Фасад» (Facade), как показано на рис. 7.4.
Класс EmployeeFacade содержит очень немного кода и отвечает за создание
экземпляров трех классов и делегирование вызовов методов.

![image_1.png](image_1.png)

Некоторые разработчики предпочитают держать наиболее важные бизнес-правила как можно ближе к данным. Это можно сделать, сохранив важные
методы в оригинальном классе Employee, и затем использовать этот класс
как фасад для низкоуровневых функций (рис. 7.5).

![image_2.png](image_2.png)

## Заключение

Принцип единственной ответственности (Single Responsibility Principle;
SRP) касается функций и классов, но он проявляется в разных формах на
еще двух более высоких уровнях. На уровне компонентов он превращается
в принцип согласованного изменения (Common Closure Principle; CCP),
а на архитектурном уровне — в принцип оси изменения (Axis of Change),
отвечающий за создание архитектурных границ.