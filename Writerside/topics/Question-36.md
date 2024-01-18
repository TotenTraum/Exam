# Вопрос 36

Паттерны распределенного взаимодействия - это архитектурные и проектировочные концепции, которые помогают обеспечить эффективное взаимодействие между компонентами распределенных систем. Они предоставляют рекомендации и bewst practices для решения распространенных проблем, связанных с распределенным взаимодействием, таких как управление сетевыми протоколами, обмен сообщениями, обеспечение надежности и т. д.

## Event Sourcing

Event Sourcing (журналирование событий) — это паттерн проектирования, который заключается в сохранении всех изменений состояния системы в виде событий. Вместо того чтобы сохранять только текущее состояние объекта, система сохраняет все события, которые произошли с объектом, и восстанавливает его состояние путем повторного применения всех событий.

* Преимущества Event Sourcing:
* История изменений: Event Sourcing сохраняет полную историю изменений состояния системы, что позволяет восстановить состояние на любой момент времени.
* Аудит и отладка: Благодаря сохранению всех событий, Event Sourcing позволяет анализировать и аудитировать систему, а также упрощает отладку проблем и ошибок.
* Воспроизводимость: События могут быть воспроизведены в любой последовательности, что упрощает повторное создание состояния системы и разработку сценариев тестирования.
* Гибкость и эволюция: Event Sourcing позволяет легко вносить изменения в систему, добавлять новые функции и взаимодействие, не нарушая существующую логику.

```Kotlin 
import java.time.LocalDateTime

// Модель события
data class TaskEvent(val taskId: String, val timestamp: LocalDateTime, val eventType: String)

// Модель состояния задачи
data class Task(val id: String, val title: String, val completed: Boolean)

// Класс для управления задачами с использованием Event Sourcing
class TaskManager {
    private val events: MutableList<TaskEvent> = mutableListOf()

    // Метод для создания новой задачи
    fun createTask(taskId: String, title: String) {
        val event = TaskEvent(taskId, LocalDateTime.now(), "TaskCreated")
        events.add(event)
        println("Task '$title' created with ID '$taskId'")
    }

    // Метод для обновления состояния задачи
    fun updateTask(taskId: String, completed: Boolean) {
        val event = TaskEvent(taskId, LocalDateTime.now(), "TaskUpdated")
        events.add(event)
        println("Task '$taskId' updated (completed: $completed)")
    }

    // Метод для восстановления состояния задачи из событий
    fun restoreTaskState(taskId: String): Task? {
        val taskEvents = events.filter { it.taskId == taskId }.sortedBy { it.timestamp }
        if (taskEvents.isEmpty()) {
            return null
        }

        var taskState = Task(taskId, "", false)
        for (event in taskEvents) {
            taskState = applyEvent(taskState, event)
        }

        return taskState
    }

    // Вспомогательный метод для применения события к состоянию задачи
    private fun applyEvent(task: Task, event: TaskEvent): Task {
        return when (event.eventType) {
            "TaskCreated" -> Task(task.id, task.title, task.completed)
            "TaskUpdated" -> Task(task.id, task.title, true)
            else -> task
        }
    }
}

fun main() {
    val taskManager = TaskManager()

    // Создание задачи
    taskManager.createTask("1", "Buy groceries")

    // Обновление задачи
    taskManager.updateTask("1", true)

    // Восстановление состояния задачи
    val restoredtask = taskManager.restoreTaskState("1")
    if (restored != null) {
        println("Restored task:")
        println("ID: ${restored.id}")
        println("Title: ${restored.title}")
        println("Completed: ${restored.completed}")
    } else {
        println("Task not found")
    }
}
```
