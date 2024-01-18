# Вопрос 43

## Фасад {collapsible="true"}

Фасад — это структурный паттерн проектирования, который предоставляет простой интерфейс к сложной системе классов, библиотеке или фреймворку.

![image_22.png](image_22.png)

```Kotlin
// Подсистема 1
class Subsystem1 {
    fun operation1() {
        println("Subsystem1: operation1()")
    }
}

// Подсистема 2
class Subsystem2 {
    fun operation2() {
        println("Subsystem2: operation2()")
    }
}

// Подсистема 3
class Subsystem3 {
    fun operation3() {
        println("Subsystem3: operation3()")
    }
}

// Фасад
class Facade(private val subsystem1: Subsystem1, private val subsystem2: Subsystem2, private val subsystem3: Subsystem3) {
    fun operation() {
        subsystem1.operation1()
        subsystem2.operation2()
        subsystem3.operation3()
    }
}

// Пример использования паттерна Фасад
fun main() {
    val subsystem1 = Subsystem1()
    val subsystem2 = Subsystem2()
    val subsystem3 = Subsystem3()

    val facade = Facade(subsystem1, subsystem2, subsystem3)
    facade.operation()
}
```

## Легковес {collapsible="true"}

Легковес — это структурный паттерн проектирования, который позволяет вместить бóльшее количество объектов в отведённую оперативную память. Легковес экономит память, разделяя общее состояние объектов между собой, вместо хранения одинаковых данных в каждом объекте. 

![image_23.png](image_23.png)

```Kotlin
// Интерфейс легковеса для графического объекта
interface Graphic {
    fun render(position: Vector2)
}

// Конкретная реализация легковеса для графического объекта
class Image(private val filename: String) : Graphic {
    override fun render(position: Vector2) {
        // Загрузка изображения из файла
        // Отрисовка изображения в заданной позиции
        println("Rendering image '$filename' at position $position")
    }
}

// Контекст игры
class Game {
    private val graphics: MutableList<Pair<Graphic, Vector2>> = mutableListOf()

    fun addGraphic(graphic: Graphic, position: Vector2) {
        graphics.add(Pair(graphic, position))
    }

    fun render() {
        for ((graphic, position) in graphics) {
            graphic.render(position)
        }
    }
}

// Вспомогательный класс для представления позиции
data class Vector2(val x: Float, val y: Float)

// Пример использования шаблона Легковес в игровой разработке
fun main() {
    val game = Game()

    val image1 = Image("image1.png")
    val image2 = Image("image2.png")
    val image3 = Image("image1.png") // Используется та же картинка, что и image1

    game.addGraphic(image1, Vector2(100f, 100f))
    game.addGraphic(image2, Vector2(200f, 200f))
    game.addGraphic(image3, Vector2(300f, 300f))

    game.render()
}
```

## Заместитель {collapsible="true"}

Заместитель — это структурный паттерн проектирования, который позволяет подставлять вместо реальных объектов специальные объекты-заменители. Эти объекты перехватывают вызовы к оригинальному объекту, позволяя
сделать что-то до или после передачи вызова оригиналу.

![image_24.png](image_24.png)

```Kotlin
// Интерфейс реального объекта
interface Resource {
    fun load()
    fun draw()
}

// Реализация реального объекта
class RealResource(private val filename: String) : Resource {
    init {
        load()
    }

    override fun load() {
        println("Loading resource '$filename'")
        // Загрузка ресурса (например, текстуры или модели)
    }

    override fun draw() {
        println("Drawing resource '$filename'")
        // Отрисовка ресурса
    }
}

// Заместитель
class ResourceProxy(private val filename: String) : Resource {
    private var resource: RealResource? = null

    override fun load() {
        if (resource == null) {
            resource = RealResource(filename)
        }
    }

    override fun draw() {
        if (resource != null) {
            resource!!.draw()
        } else {
            println("Resource '$filename' is not loaded yet")
        }
    }
}

// Клиентский код
fun main() {
    val resource1 = ResourceProxy("resource1.png")
    resource1.load()
    resource1.draw()

    val resource2 = ResourceProxy("resource2.png")
    resource2.draw() // Загрузка ресурса произойдет при первой отрисовке

    resource1.draw() // Ресурс уже загружен и будет отрисован
}
```