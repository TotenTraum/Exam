# Вопрос 8

Диаграммы вариантов использования (Use Case Diagrams) - это графическое представление функциональных требований к системе, которые описывают, как актеры (пользователи) взаимодействуют с системой для достижения определенных целей. Они помогают визуализировать основные функции и поведение системы из перспективы пользователей.
Вот основные элементы, используемые в диаграммах вариантов использования и их назначение:
* Актер (Actor): Представляет роль или внешнюю сущность, которая взаимодействует с системой. Актеры могут быть пользователями, другими системами, внешними устройствами или даже временными событиями. Примеры: Пользователь, Администратор, Внешняя система.
* Вариант использования (Use Case): Представляет функциональность или поведение системы, которое достигается через взаимодействие между актерами и системой. Варианты использования описывают конкретные действия, которые система выполняет для пользователя или другого актера. Примеры: Создать заказ, Войти в систему, Отправить уведомление.
* Отношение включения (Include): Используется для обозначения включения одного варианта использования в другой. Оно указывает, что функциональность одного варианта использования включается в другой вариант использования. Пример: Вариант использования "Отправить уведомление" включает вариант использования "Сформировать уведомление".
* Отношение расширения (Extend): Используется для обозначения возможности расширения одного варианта использования другим. Оно указывает, что определенное поведение может быть расширено другим поведением при определенных условиях. Пример: Вариант использования "Создать заказ" может быть расширен вариантом использования "Добавить товар в заказ".
* Система (System): Представляет саму систему, для которой создается диаграмма вариантов использования. Обычно система изображается в виде прямоугольника, окруженного актерами, и содержит все варианты использования, связанные с данной системой.
