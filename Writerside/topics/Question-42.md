# Вопрос 42

## Компоновщик {collapsible="true"}

Компоновщик — это структурный паттерн проектирования, который позволяет сгруппировать множество объектов в древовидную структуру, а затем работать с ней так, как будто это единичный объект.

![image_20.png](image_20.png)

```Kotlin
// Общий интерфейс компонента
interface Component {
    fun operation()
}

// Конкретная реализация компонента: Лист
class Leaf(private val name: String) : Component {
    override fun operation() {
        println("Leaf: $name")
    }
}

// Конкретная реализация компонента: Контейнер
class Composite(private val name: String) : Component {
    private val components = mutableListOf<Component>()

    fun add(component: Component) {
        components.add(component)
    }

    fun remove(component: Component) {
        components.remove(component)
    }

    override fun operation() {
        println("Composite: $name")
        for (component in components) {
            component.operation()
        }
    }
}

// Пример использования паттерна Компоновщик
fun main() {
    val root = Composite("Root")
    root.add(Leaf("Leaf 1"))

    val branch1 = Composite("Branch 1")
    branch1.add(Leaf("Leaf 2"))
    branch1.add(Leaf("Leaf 3"))

    val branch2 = Composite("Branch 2")
    branch2.add(Leaf("Leaf 4"))

    root.add(branch1)
    root.add(branch2)

    root.operation()
}
```

## Декоратор {collapsible="true"}

Декоратор — это структурный паттерн проектирования,
который позволяет динамически добавлять объектам новую
функциональность, оборачивая их в полезные «обёртки».

![image_21.png](image_21.png)

```Kotlin
// Общий интерфейс компонента
interface Component {
    fun operation()
}

// Конкретная реализация компонента
class ConcreteComponent : Component {
    override fun operation() {
        println("ConcreteComponent: operation()")
    }
}

// Базовый класс декоратора
abstract class Decorator(private val component: Component) : Component {
    override fun operation() {
        component.operation()
    }
}

// Конкретный декоратор: Декоратор A
class ConcreteDecoratorA(component: Component) : Decorator(component) {
    override fun operation() {
        super.operation()
        addedBehavior()
    }

    private fun addedBehavior() {
        println("ConcreteDecoratorA: addedBehavior()")
    }
}

// Конкретный декоратор: Декоратор B
class ConcreteDecoratorB(component: Component) : Decorator(component) {
    override fun operation() {
        super.operation()
        addedState()
    }

    private fun addedState() {
        println("ConcreteDecoratorB: addedState()")
    }
}

// Пример использования паттерна Декоратор
fun main() {
    val component: Component = ConcreteComponent()
    val decoratorA: Component = ConcreteDecoratorA(component)
    val decoratorB: Component = ConcreteDecoratorB(decoratorA)

    decoratorB.operation()
}
```