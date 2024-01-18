# Вопрос 29

Когда речь идет о паттернах сохраняемости (persistence patterns), обычно имеются в виду различные подходы и шаблоны проектирования, используемые для сохранения данных в постоянное хранилище, такое как база данных или файловая система. 

Active Record - это шаблон, который связывает объекты с реляционной базой данных. Каждая запись в базе данных представляется объектом Active Record, который имеет свойства, соответствующие столбцам таблицы, а также методы для сохранения, обновления и удаления записи. Active Record инкапсулирует логику доступа к данным и предоставляет простой интерфейс для работы с базой данных.

Преимущества использования Active Record:
* Простота использования: Active Record предоставляет простой и интуитивно понятный интерфейс для доступа к данным, что упрощает разработку и поддержку кода.
* Инкапсуляция данных и логики: Active Record инкапсулирует данные и методы работы с ними в одном объекте, что способствует легкости понимания и поддержки кода.
* Прозрачность: Active Record скрывает детали взаимодействия с базой данных, позволяя разработчикам работать с данными как с обычными объектами.

```Kotlin
data class Task(
    val id: Long,
    var name: String,
    var status: TaskStatus,
    var priority: TaskPriority
) {
    fun save() {
        // Логика сохранения задачи в базе данных
        Database.saveTask(this)
    }

    fun update() {
        // Логика обновления задачи в базе данных
        Database.updateTask(this)
    }

    fun delete() {
        // Логика удаления задачи из базы данных
        Database.deleteTask(this)
    }

    companion object {
        fun findById(id: Long): Task? {
            return Database.getTaskById(id)
        }

        fun findAll(): List<Task> {
            return Database.getAllTasks()
        }
    }
}

object Database {
    private val tasks = mutableListOf<Task>()

    fun saveTask(task: Task) {
        tasks.add(task)
        println("Task saved: $task")
    }

    fun updateTask(task: Task) {
        val existingTask = tasks.find { it.id == task.id }
        existingTask?.let {
            it.name = task.name
            it.status = task.status
            it.priority = task.priority
            println("Task updated: $task")
        }
    }

    fun deleteTask(task: Task) {
        tasks.removeIf { it.id == task.id }
        println("Task deleted: $task")
    }

    fun getTaskById(id: Long): Task? {
        return tasks.find { it.id == id }
    }

    fun getAllTasks(): List<Task> {
        return tasks.toList()
    }
}

// Использование Active Record
fun main() {
    val task1 = Task(1, "Построить диаграмму классов", TaskStatus.IN_PROGRESS, TaskPriority.HIGH)
    task1.save()

    val task2 = Task(2, "Реализовать функциональность поиска", TaskStatus.TODO, TaskPriority.MEDIUM)
    task2.save()

    val allTasks = Task.findAll()
    println("All Tasks: $allTasks")

    val taskToUpdate = Task.findById(1)
    taskToUpdate?.let {
        it.name = "Implement search functionality"
        it.update()
    }

    val taskToDelete = Task.findById(2)
    taskToDelete?.delete()
}
```