# Вопрос 20

Принцип инверсии зависимости (Dependency Inversion Principle; DIP) утверждает, что наиболее гибкими получаются системы, в которых зависимости
в исходном коде направлены на абстракции, а не на конкретные реализации.

Как следствие, стабильными называются такие архитектуры, в которых
вместо зависимостей от переменчивых конкретных реализаций используются зависимости от стабильных абстрактных интерфейсов. Это следствие
сводится к набору очень простых правил:
* Не ссылайтесь на изменчивые конкретные классы. Ссылайтесь на абстрактные интерфейсы. Это правило применимо во всех языках, независимо от устройства системы типов. Оно также накладывает важные ограничения на создание объектов и определяет преимущественное использование шаблона «Абстрактная фабрика».
* Не наследуйте изменчивые конкретные классы. Это естественное следствие из предыдущего правила, но оно достойно отдельного упоминания. Наследование в языках со статической системой типов является самым строгим и жестким видом отношений в исходном коде; следовательно, его следует использовать с большой осторожностью. Наследование в языках с динамической системой типов влечет меньшее количество проблем, но все еще остается зависимостью, поэтому дополнительная предосторожность никогда не помешает.
* Не переопределяйте конкретные функции. Конкретные функции часто требуют зависимостей в исходном коде. Переопределяя такие функции, вы не устраняете эти зависимости — фактически вы наследуете их. Для управления подобными зависимостями нужно сделать функцию абстрактной и создать несколько ее реализаций.
* Никогда не ссылайтесь на имена конкретных и изменчивых сущностей. В действительности это всего лишь перефразированная форма самого принципа.

![image_6.png](image_6.png)