# Вопрос 22
## Принцип согласованного изменения

<tip>
В один компонент должны включаться классы, изменяющиеся по одним причинам и в одно время. В разные компоненты должны включаться классы, изменяющиеся в разное время и по разным причинам.
</tip>

Это означает, что классы, которые меняются вместе, должны быть объединены в одну компоненту или модуль, чтобы обеспечить высокую сопровождаемость системы и уменьшить вероятность ошибок.

## Пример
Игровой движок: В случае разработки игрового движка, классы, относящиеся к графике, физике и управлению аудио, должны быть группированы вместе в соответствии с принципом CCP. Изменения в этих классах, связанные с оптимизацией графического движка, внедрением новых физических эффектов или обновлением аудио-движка, скорее всего, будут происходить по одной и той же причине - связанной с функциональностью игрового движка.