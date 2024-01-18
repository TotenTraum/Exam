# Вопрос 44

## Команда {collapsible="true"}

Команда — это поведенческий паттерн проектирования,
который превращает запросы в объекты, позволяя
передавать их как аргументы при вызове методов, ставить
запросы в очередь, логировать их, а также поддерживать
отмену операций.

![image_25.png](image_25.png)

```Kotlin
// Интерфейс команды
interface Command {
    fun execute()
}

// Конкретная реализация команды для движения игрового персонажа влево
class MoveLeftCommand(private val character: Character) : Command {
    override fun execute() {
        character.moveLeft()
    }
}

// Конкретная реализация команды для движения игрового персонажа вправо
class MoveRightCommand(private val character: Character) : Command {
    override fun execute() {
        character.moveRight()
    }
}

// Игровой персонаж, который может выполнять команды
class Character {
    fun moveLeft() {
        println("Character moves left")
        // Логика движения влево
    }

    fun moveRight() {
        println("Character moves right")
        // Логика движения вправо
    }
}

// Игровой объект, который может выступать в роли источника команд
class InputHandler {
    private var leftCommand: Command? = null
    private var rightCommand: Command? = null

    fun setLeftCommand(command: Command) {
        leftCommand = command
    }

    fun setRightCommand(command: Command) {
        rightCommand = command
    }

    fun handleLeftInput() {
        leftCommand?.execute()
    }

    fun handleRightInput() {
        rightCommand?.execute()
    }
}

// Клиентский код
fun main() {
    val character = Character()

    val moveLeftCommand = MoveLeftCommand(character)
    val moveRightCommand = MoveRightCommand(character)

    val inputHandler = InputHandler()

    inputHandler.setLeftCommand(moveLeftCommand)
    inputHandler.setRightCommand(moveRightCommand)

    // Имитация пользовательского ввода
    inputHandler.handleLeftInput()
    inputHandler.handleRightInput()
}
```

## Состояние {collapsible="true"}

Состояние — это поведенческий паттерн проектирования,
который позволяет объектам менять поведение в
зависимости от своего состояния. Извне создаётся
впечатление, что изменился класс объекта.

![image_26.png](image_26.png)

```Kotlin
// Интерфейс состояния
interface State {
    fun handleInput()
    fun update()
    fun render()
}

// Конкретные реализации состояний
class IdleState : State {
    override fun handleInput() {
        println("IdleState: No input handling")
    }

    override fun update() {
        println("IdleState: No updates")
    }

    override fun render() {
        println("IdleState: Render idle animation")
    }
}

class WalkState : State {
    override fun handleInput() {
        println("WalkState: Handling input for walking")
    }

    override fun update() {
        println("WalkState: Updating walking logic")
    }

    override fun render() {
        println("WalkState: Render walking animation")
    }
}

class AttackState : State {
    override fun handleInput() {
        println("AttackState: Handling input for attacking")
    }

    override fun update() {
        println("AttackState: Updating attack logic")
    }

    override fun render() {
        println("AttackState: Render attack animation")
    }
}

// Класс, управляющий состояниями объекта
class StateManager(private var currentState: State) {
    fun setCurrentState(state: State) {
        currentState = state
    }

    fun handleInput() {
        currentState.handleInput()
    }

    fun update() {
        currentState.update()
    }

    fun render() {
        currentState.render()
    }
}

// Клиентский код
fun main() {
    val idleState = IdleState()
    val walkState = WalkState()
    val attackState = AttackState()

    val stateManager = StateManager(idleState)

    // Имитация игрового цикла
    stateManager.handleInput()
    stateManager.update()
    stateManager.render()

    stateManager.setCurrentState(walkState)
    stateManager.handleInput()
    stateManager.update()
    stateManager.render()

    stateManager.setCurrentState(attackState)
    stateManager.handleInput()
    stateManager.update()
    stateManager.render()
}
```

## Итератор {collapsible="true"}

Итератор — это поведенческий паттерн проектирования,
который даёт возможность последовательно обходить
элементы составных объектов, не раскрывая их
внутреннего представления.

![image_27.png](image_27.png)

```Kotlin
// Интерфейс итератора
interface Iterator<T> {
    fun hasNext(): Boolean
    fun next(): T
}

// Класс коллекции объектов
class ObjectCollection {
    private val objects = mutableListOf<GameObject>()

    fun addObject(obj: GameObject) {
        objects.add(obj)
    }

    fun removeObject(obj: GameObject) {
        objects.remove(obj)
    }

    // Внутренний класс итератора
    inner class ObjectIterator : Iterator<GameObject> {
        private var currentIndex = 0

        override fun hasNext(): Boolean {
            return currentIndex < objects.size
        }

        override fun next(): GameObject {
            return objects[currentIndex++]
        }
    }

    // Метод, возвращающий итератор
    fun createIterator(): Iterator<GameObject> {
        return ObjectIterator()
    }
}

// Класс игрового объекта
class GameObject(private val name: String) {
    fun getName(): String {
        return name
    }
}

// Клиентский код
fun main() {
    val collection = ObjectCollection()

    collection.addObject(GameObject("Object 1"))
    collection.addObject(GameObject("Object 2"))
    collection.addObject(GameObject("Object 3"))

    val iterator = collection.createIterator()

    while (iterator.hasNext()) {
        val obj = iterator.next()
        println("Object name: ${obj.getName()}")
    }
}
```