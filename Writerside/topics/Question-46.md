# Вопрос 46

## Наблюдатель {collapsible="true"}

Наблюдатель — это поведенческий паттерн проектирования, который создаёт механизм подписки,
позволяющий одним объектам следить и реагировать на
события, происходящие в других объектах.

![image_30.png](image_30.png)

```Kotlin
interface Observer {
    fun update(event: GameEvent)
}

interface Observable {
    fun addObserver(observer: Observer)
    fun removeObserver(observer: Observer)
    fun notifyObservers(event: GameEvent)
}

data class GameEvent(val type: String, val data: Any?)

class Player : Observer {
    override fun update(event: GameEvent) {
        when (event.type) {
            "EnemySpawned" -> {
                val enemyType = event.data as String
                println("Player: Enemy $enemyType spawned!")
            }
            "PlayerDied" -> {
                println("Player: Oh no, I died!")
            }
            // Обработка других типов событий
        }
    }
}

class Game : Observable {
    private val observers: MutableList<Observer> = mutableListOf()

    override fun addObserver(observer: Observer) {
        observers.add(observer)
    }

    override fun removeObserver(observer: Observer) {
        observers.remove(observer)
    }

    override fun notifyObservers(event: GameEvent) {
        for (observer in observers) {
            observer.update(event)
        }
    }

    fun spawnEnemy(enemyType: String) {
        // Логика спауна врага
        val event = GameEvent("EnemySpawned", enemyType)
        notifyObservers(event)
    }

    fun playerDied() {
        // Логика смерти игрока
        val event = GameEvent("PlayerDied", null)
        notifyObservers(event)
    }
}

fun main() {
    val game = Game()
    val player = Player()

    game.addObserver(player)

    game.spawnEnemy("Zombie")
    game.playerDied()

    game.removeObserver(player)
}
```

## Посетитель {collapsible="true"}

Посетитель — это поведенческий паттерн проектирования,
который позволяет добавлять в программу новые операции,
не изменяя классы объектов, над которыми эти операции
могут выполняться.

![image_31.png](image_31.png)
```Kotlin
interface GameObject {
    fun accept(visitor: GameObjectVisitor)
}

interface GameObjectVisitor {
    fun visit(player: Player)
    fun visit(enemy: Enemy)
    // Добавьте здесь другие методы visit для различных игровых объектов
}

class Player : GameObject {
    override fun accept(visitor: GameObjectVisitor) {
        visitor.visit(this)
    }

    fun play() {
        println("Player: Playing the game...")
    }
}

class Enemy : GameObject {
    override fun accept(visitor: GameObjectVisitor) {
        visitor.visit(this)
    }

    fun attack() {
        println("Enemy: Attacking the player...")
    }
}

class Game : GameObject {
    private val gameObjects: MutableList<GameObject> = mutableListOf()

    fun addGameObject(gameObject: GameObject) {
        gameObjects.add(gameObject)
    }

    override fun accept(visitor: GameObjectVisitor) {
        for (gameObject in gameObjects) {
            gameObject.accept(visitor)
        }
    }
}

class GameAnalyzer : GameObjectVisitor {
    override fun visit(player: Player) {
        println("Game Analyzer: Analyzing player...")
        player.play()
    }

    override fun visit(enemy: Enemy) {
        println("Game Analyzer: Analyzing enemy...")
        enemy.attack()
    }
}

fun main() {
    val game = Game()
    val player = Player()
    val enemy = Enemy()
    val analyzer = GameAnalyzer()

    game.addGameObject(player)
    game.addGameObject(enemy)

    game.accept(analyzer)
}
```