# Вопрос 47

## Цепочка обязанностей {collapsible="true"}

Цепочка обязанностей — это поведенческий паттерн
проектирования, который позволяет передавать запросы
последовательно по цепочке обработчиков. Каждый
последующий обработчик решает, может ли он обработать
запрос сам и стоит ли передавать запрос дальше по цепи.

![image_32.png](image_32.png)

```Kotlin
abstract class Handler {
    private var nextHandler: Handler? = null

    fun setNext(handler: Handler) {
        nextHandler = handler
    }

    fun handleRequest(request: GameRequest) {
        if (canHandle(request)) {
            processRequest(request)
        } else {
            nextHandler?.handleRequest(request)
        }
    }

    abstract fun canHandle(request: GameRequest): Boolean
    abstract fun processRequest(request: GameRequest)
}

class PlayerHandler : Handler() {
    override fun canHandle(request: GameRequest): Boolean {
        return request.type == GameRequest.Type.PLAYER_ACTION
    }

    override fun processRequest(request: GameRequest) {
        println("PlayerHandler: Handling player action request")
        // Логика обработки действий игрока
    }
}

class EnemyHandler : Handler() {
    override fun canHandle(request: GameRequest): Boolean {
        return request.type == GameRequest.Type.ENEMY_ACTION
    }

    override fun processRequest(request: GameRequest) {
        println("EnemyHandler: Handling enemy action request")
        // Логика обработки действий врага
    }
}

class GameRequest(val type: Type) {
    enum class Type {
        PLAYER_ACTION,
        ENEMY_ACTION
    }
}

fun main() {
    val playerHandler = PlayerHandler()
    val enemyHandler = EnemyHandler()

    playerHandler.setNext(enemyHandler)

    val playerRequest = GameRequest(GameRequest.Type.PLAYER_ACTION)
    playerHandler.handleRequest(playerRequest)

    val enemyRequest = GameRequest(GameRequest.Type.ENEMY_ACTION)
    playerHandler.handleRequest(enemyRequest)
}
```

## Стратегия {collapsible="true"}

Стратегия — это поведенческий паттерн проектирования,
который определяет семейство схожих алгоритмов и
помещает каждый из них в собственный класс, после чего
алгоритмы можно взаимозаменять прямо во время
исполнения программы.

![image_33.png](image_33.png)

```Kotlin
interface AttackStrategy {
    fun attack()
}

class MeleeAttackStrategy : AttackStrategy {
    override fun attack() {
        println("Performing melee attack!")
        // Логика выполнения ближней атаки
    }
}

class RangedAttackStrategy : AttackStrategy {
    override fun attack() {
        println("Performing ranged attack!")
        // Логика выполнения дальней атаки
    }
}

class Player(private val attackStrategy: AttackStrategy) {
    fun performAttack() {
        attackStrategy.attack()
    }
}

fun main() {
    val meleeStrategy = MeleeAttackStrategy()
    val rangedStrategy = RangedAttackStrategy()

    val playerWithMeleeAttack = Player(meleeStrategy)
    playerWithMeleeAttack.performAttack()

    val playerWithRangedAttack = Player(rangedStrategy)
    playerWithRangedAttack.performAttack()
}
```

## Шаблонный метод {collapsible="true"}

Шаблонный метод — это поведенческий паттерн
проектирования, который определяет скелет алгоритма,
перекладывая ответственность за некоторые его шаги на
подклассы. Паттерн позволяет подклассам переопределять
шаги алгоритма, не меняя его общей структуры.

![image_34.png](image_34.png)

```Kotlin
abstract class Game {
    fun play() {
        initialize()
        startGame()
        endGame()
    }

    abstract fun initialize()
    abstract fun startGame()
    abstract fun endGame()
}

class Chess : Game() {
    override fun initialize() {
        println("Chess: Initializing the game...")
    }

    override fun startGame() {
        println("Chess: Starting the game...")
    }

    override fun endGame() {
        println("Chess: Ending the game...")
    }
}

class TicTacToe : Game() {
    override fun initialize() {
        println("Tic Tac Toe: Initializing the game...")
    }

    override fun startGame() {
        println("Tic Tac Toe: Starting the game...")
    }

    override fun endGame() {
        println("Tic Tac Toe: Ending the game...")
    }
}

fun main() {
    val chess = Chess()
    chess.play()

    val ticTacToe = TicTacToe()
    ticTacToe.play()
}
```