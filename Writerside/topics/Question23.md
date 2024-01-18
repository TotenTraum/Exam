# Вопрос 23

## Принцип совместного повторного использования

<tip>
Не вынуждайте пользователей компонента зависеть от того, чего им не требуется.
</tip>

Принцип CRP подразумевает, что классы, которые часто используются вместе, должны находиться в одной компоненте или модуле, чтобы облегчить повторное использование и уменьшить зависимости между компонентами системы.

## Пример
Компоненты пользовательского интерфейса: Если у вас есть компоненты пользовательского интерфейса, которые часто используются в различных частях системы, то они могут быть объединены в одну компонентную библиотеку или модуль. Например, кнопки, текстовые поля и списки выбора, которые используются в различных формах и окнах приложения, могут быть сгруппированы вместе в одну библиотеку пользовательского интерфейса.