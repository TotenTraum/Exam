# Вопрос 45

## Посредник {collapsible="true"}

Посредник — это поведенческий паттерн проектирования,
который позволяет уменьшить связанность множества
классов между собой, благодаря перемещению этих связей
в один класс-посредник.

![image_28.png](image_28.png)

```Kotlin
// Абстрактный класс Компонента
abstract class Component(val mediator: Mediator) {
    abstract fun send(message: String)
    abstract fun receive(message: String)
}

// Конкретный класс Компонента
class ConcreteComponentA(mediator: Mediator) : Component(mediator) {
    override fun send(message: String) {
        mediator.send(message, this)
    }

    override fun receive(message: String) {
        println("Component A received: $message")
    }
}

// Конкретный класс Компонента
class ConcreteComponentB(mediator: Mediator) : Component(mediator) {
    override fun send(message: String) {
        mediator.send(message, this)
    }

    override fun receive(message: String) {
        println("Component B received: $message")
    }
}

// Интерфейс Посредника
interface Mediator {
    fun send(message: String, sender: Component)
}

// Конкретный класс Посредника
class ConcreteMediator : Mediator {
    private lateinit var componentA: ConcreteComponentA
    private lateinit var componentB: ConcreteComponentB

    fun setComponentA(componentA: ConcreteComponentA) {
        this.componentA = componentA
    }

    fun setComponentB(componentB: ConcreteComponentB) {
        this.componentB = componentB
    }

    override fun send(message: String, sender: Component) {
        if (sender == componentA) {
            componentB.receive(message)
        } else if (sender == componentB) {
            componentA.receive(message)
        }
    }
}

// Клиентский код
fun main() {
    val mediator = ConcreteMediator()

    val componentA = ConcreteComponentA(mediator)
    val componentB = ConcreteComponentB(mediator)

    mediator.setComponentA(componentA)
    mediator.setComponentB(componentB)

    componentA.send("Hello from Component A!")
    componentB.send("Hi from Component B!")
}
```

## Снимок {collapsible="true"}

Снимок — это поведенческий паттерн проектирования,
который позволяет сохранять и восстанавливать прошлые
состояния объектов, не раскрывая подробностей их реализации.

![image_29.png](image_29.png)

```Kotlin
data class GameState(val level: Int, val score: Int)

class Game {
    private var level: Int = 1
    private var score: Int = 0

    fun playLevel() {
        // Логика игры
        score += level * 100
        level++
    }

    fun saveState(): GameState {
        return GameState(level, score)
    }

    fun restoreState(state: GameState) {
        level = state.level
        score = state.score
    }

    fun printStatus() {
        println("Level: $level")
        println("Score: $score")
    }
}

class Caretaker {
    private var savedStates: MutableList<GameState> = mutableListOf()

    fun saveState(state: GameState) {
        savedStates.add(state)
    }

    fun restoreState(): GameState? {
        if (savedStates.isEmpty()) {
            return null
        }
        val lastIndex = savedStates.lastIndex
        val state = savedStates[lastIndex]
        savedStates.removeAt(lastIndex)
        return state
    }
}

fun main() {
    val game = Game()
    val caretaker = Caretaker()

    // Играем несколько уровней и сохраняем состояния
    game.playLevel()
    caretaker.saveState(game.saveState())
    game.playLevel()
    caretaker.saveState(game.saveState())

    // Печатаем текущий статус игры
    game.printStatus()

    // Восстанавливаем предыдущее состояние
    val previousState = caretaker.restoreState()
    if (previousState != null) {
        game.restoreState(previousState)
    }

    // Печатаем статус после восстановления
    game.printStatus()
}
```