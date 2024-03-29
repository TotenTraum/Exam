# Вопрос 1

Архитектура – абстракции высокого и низкого уровней и связи между ними. абстракции –
компоненты системы (файлы, классы, модули и т.п. в зависимости от уровня). Базовые ценности
архитектуры:
* Стоимость
* Расширяемость
* Функциональность

## Понятие об архитектуре
Низкоуровневые детали и высокоуровневая структура являются частями одного целого.
Они образуют сплошное полотно, определяющее форму системы. Одно без другого невозможно; нет никакой четкой линии, которая разделяла бы их.
Есть просто совокупность решений разного уровня детализации.

## Цель
Цель архитектуры программного обеспечения — уменьшить человеческие
трудозатраты на создание и сопровождение системы.

Архитектура считается хорошей, если в течение всего периода эксплуатации системы трудозатраты
невелики. Если трудозатраты растут по мере выхода новых версии или расширении системы, то
архитектура считается плохой.

## Плохая архитектура
Плохая архитектура обычно характеризуется набором проблем, которые затрудняют разработку, тестирование, поддержку и модификацию программного обеспечения. Вот некоторые признаки плохой архитектуры:

* Жесткая связанность (Tight Coupling): Компоненты программы сильно зависят друг от друга, и изменение одного компонента может привести к неожиданным побочным эффектам в других компонентах.
* Ломкая наследуемость (Fragile Base Class): Изменение функциональности базового класса может приводить к ошибкам в производных классах, что затрудняет поддержку и модификацию кода.
* Глобальные зависимости (Global Dependencies): Программа полагается на глобальные переменные или состояние, что делает ее сложной для тестирования и понимания.
* Нарушение принципа единственной ответственности (Violation of Single Responsibility Principle): Компоненты или классы выполняют слишком много задач, что делает их сложными для понимания, тестирования и повторного использования.

## Хорошая архитектура
С другой стороны, хорошая архитектура следует ряду принципов и практик, которые делают программное обеспечение более гибким, расширяемым и понятным. Некоторые признаки хорошей архитектуры:
* Разделение ответственности (Separation of Concerns): Компоненты программы имеют четко определенные задачи и ответственности, что упрощает понимание и изменение кода.
* Слабая связанность (Loose Coupling): Компоненты программы слабо связаны друг с другом, что позволяет изменять один компонент без влияния на другие.
* Высокая связность (High Cohesion): Компоненты программы имеют высокую внутреннюю связность, то есть элементы внутри компонента тесно связаны и работают вместе для достижения одной цели.
* Модульность (Modularity): Программа разделена на независимые модули, что упрощает разработку, тестирование и поддержку.
* Использование принципов SOLID: Принципы SOLID (Single Responsibility, Open-Closed, Liskov Substitution, Interface Segregation, Dependency Inversion) являются основой хорошей архитектуры и помогают создавать гибкое и расширяемое программное обеспечение.

