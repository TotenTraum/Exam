# Вопрос 17

Принцип можно описать так:

<tip>
Программные сущности должны быть открыты для расширения и закрыты
для изменения.
</tip>
Иными словами, должна иметься возможность расширять поведение программных сущностей без их изменения.

Его цель — сделать систему легко расширяемой и обезопасить ее от
влияния изменений. Эта цель достигается делением системы на компоненты
и упорядочением их зависимостей в иерархию, защищающую компоненты
уровнем выше от изменений в компонентах уровнем ниже.