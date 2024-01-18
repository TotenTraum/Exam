# Вопрос 40

## Строитель {collapsible="true"}

Строитель — это порождающий паттерн проектирования, который позволяет создавать сложные объекты пошагово. Строитель даёт возможность использовать один и тот же код строительства для получения разных представлений объектов.

![image_15.png](image_15.png)

```Kotlin
// Класс продукта
class Product(
    val partA: String,
    val partB: String,
    val partC: String
) {
    fun printParts() {
        println("Part A: $partA")
        println("Part B: $partB")
        println("Part C: $partC")
    }
}

// Интерфейс Строителя
interface Builder {
    fun buildPartA()
    fun buildPartB()
    fun buildPartC()
    fun getProduct(): Product
}

// Конкретная реализация Строителя
class ConcreteBuilder : Builder {
    private val product = Product("", "", "")

    override fun buildPartA() {
        product.partA = "Part A"
    }

    override fun buildPartB() {
        product.partB = "Part B"
    }

    override fun buildPartC() {
        product.partC = "Part C"
    }

    override fun getProduct(): Product {
        return product
    }
}

// Директор
class Director(private val builder: Builder) {
    fun construct() {
        builder.buildPartA()
        builder.buildPartB()
        builder.buildPartC()
    }
}

// Пример использования паттерна Строитель
fun main() {
    val builder: Builder = ConcreteBuilder()
    val director = Director(builder)

    director.construct()
    val product: Product = builder.getProduct()
    product.printParts()
}
```

## Прототип {collapsible="true"}

Прототип — это порождающий паттерн проектирования,
который позволяет копировать объекты, не вдаваясь в
подробности их реализации.

![image_16.png](image_16.png)

```Kotlin
// Прототип
abstract class Prototype : Cloneable {
    abstract fun clone(): Prototype
}

// Конкретный прототип
class ConcretePrototype(private val data: String) : Prototype() {
    override fun clone(): Prototype {
        return ConcretePrototype(data)
    }

    fun printData() {
        println("Data: $data")
    }
}

// Пример использования паттерна Прототип
fun main() {
    val prototype: Prototype = ConcretePrototype("Prototype")
    val clone1: Prototype = prototype.clone()
    val clone2: Prototype = prototype.clone()

    (clone1 as ConcretePrototype).printData() // Вывод: Data: Prototype
    (clone2 as ConcretePrototype).printData() // Вывод: Data: Prototype
}
```

## Одиночка {collapsible="true"}

Одиночка — это порождающий паттерн проектирования,
который гарантирует, что у класса есть только один
экземпляр, и предоставляет к нему глобальную
точку доступа.

![image_17.png](image_17.png)

```Kotlin
// Класс одиночки
class Singleton private constructor() {
    companion object {
        private var instance: Singleton? = null

        fun getInstance(): Singleton {
            if (instance == null) {
                instance = Singleton()
            }
            return instance as Singleton
        }
    }

    fun doSomething() {
        println("Doing something...")
    }
}

// Пример использования паттерна Одиночка
fun main() {
    val singleton1 = Singleton.getInstance()
    val singleton2 = Singleton.getInstance()

    println(singleton1 === singleton2) // Вывод: true

    singleton1.doSomething() // Вывод: Doing something...
    singleton2.doSomething() // Вывод: Doing something...
}
```