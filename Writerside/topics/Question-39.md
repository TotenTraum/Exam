# Вопрос 39

## Фабричный метод {collapsible="true"}

Фабричный метод — это порождающий паттерн проектирования, который определяет общий интерфейс для создания объектов в суперклассе, позволяя подклассам изменять тип создаваемых объектов.

Паттерн Фабричный метод предлагает создавать объекты не
напрямую, используя оператор new, а через вызов особого
фабричного метода. Не пугайтесь, объекты всё равно будут
создаваться при помощи new, но делать это будет фабричный метод.

![image_13.png](image_13.png)

```Kotlin
// Абстрактный класс, представляющий продукт
abstract class Product {
    abstract fun operation(): String
}

// Конкретный класс продукта
class ConcreteProductA : Product() {
    override fun operation(): String {
        return "ConcreteProductA"
    }
}

// Конкретный класс продукта
class ConcreteProductB : Product() {
    override fun operation(): String {
        return "ConcreteProductB"
    }
}

// Абстрактный класс фабрики
abstract class Creator {
    abstract fun createProduct(): Product

    fun someOperation(): String {
        val product = createProduct()
        return "Creator: Created ${product.operation()}"
    }
}

// Конкретная реализация фабрики
class ConcreteCreatorA : Creator() {
    override fun createProduct(): Product {
        return ConcreteProductA()
    }
}

// Конкретная реализация фабрики
class ConcreteCreatorB : Creator() {
    override fun createProduct(): Product {
        return ConcreteProductB()
    }
}

// Пример использования фабричного метода
fun main() {
    val creatorA: Creator = ConcreteCreatorA()
    val productA: Product = creatorA.createProduct()
    println(creatorA.someOperation()) // Вывод: Creator: Created ConcreteProductA

    val creatorB: Creator = ConcreteCreatorB()
    val productB: Product = creatorB.createProduct()
    println(creatorB.someOperation()) // Вывод: Creator: Created ConcreteProductB
}
```

## Абстрактная фабрика {collapsible="true"}

Абстрактная фабрика — это порождающий паттерн
проектирования, который позволяет создавать семейства
связанных объектов, не привязываясь к конкретным
классам создаваемых объектов.

![image_14.png](image_14.png)

```Kotlin
// Абстрактный класс продукта A
abstract class AbstractProductA {
    abstract fun operationA(): String
}

// Конкретная реализация продукта A1
class ConcreteProductA1 : AbstractProductA() {
    override fun operationA(): String {
        return "ConcreteProductA1"
    }
}

// Конкретная реализация продукта A2
class ConcreteProductA2 : AbstractProductA() {
    override fun operationA(): String {
        return "ConcreteProductA2"
    }
}

// Абстрактный класс продукта B
abstract class AbstractProductB {
    abstract fun operationB(): String
}

// Конкретная реализация продукта B1
class ConcreteProductB1 : AbstractProductB() {
    override fun operationB(): String {
        return "ConcreteProductB1"
    }
}

// Конкретная реализация продукта B2
class ConcreteProductB2 : AbstractProductB() {
    override fun operationB(): String {
        return "ConcreteProductB2"
    }
}

// Абстрактная фабрика
abstract class AbstractFactory {
    abstract fun createProductA(): AbstractProductA
    abstract fun createProductB(): AbstractProductB
}

// Конкретная реализация абстрактной фабрики 1
class ConcreteFactory1 : AbstractFactory() {
    override fun createProductA(): AbstractProductA {
        return ConcreteProductA1()
    }

    override fun createProductB(): AbstractProductB {
        return ConcreteProductB1()
    }
}

// Конкретная реализация абстрактной фабрики 2
class ConcreteFactory2 : AbstractFactory() {
    override fun createProductA(): AbstractProductA {
        return ConcreteProductA2()
    }

    override fun createProductB(): AbstractProductB {
        return ConcreteProductB2()
    }
}

// Клиентский код
fun main() {
    val factory1: AbstractFactory = ConcreteFactory1()
    val productA1: AbstractProductA = factory1.createProductA()
    val productB1: AbstractProductB = factory1.createProductB()
    println(productA1.operationA()) // Вывод: ConcreteProductA1
    println(productB1.operationB()) // Вывод: ConcreteProductB1

    val factory2: AbstractFactory = ConcreteFactory2()
    val productA2: AbstractProductA = factory2.createProductA()
    val productB2: AbstractProductB = factory2.createProductB()
    println(productA2.operationA()) // Вывод: ConcreteProductA2
    println(productB2.operationB()) // Вывод: ConcreteProductB2
}
```
