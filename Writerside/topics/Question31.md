# Вопрос 32

Паттерны представления данных и обработки ввода - это шаблоны проектирования, которые помогают структурировать и управлять данными в пользовательском интерфейсе и обрабатывать ввод пользователя. Эти паттерны часто используются при разработке приложений, где требуется взаимодействие с данными, и включают такие архитектурные концепции, как Model-View-Controller (MVC), Model-View-ViewModel (MVVM), и другие.

MVC (Model-View-Controller) - это популярный паттерн проектирования, используемый для разделения компонентов в приложении на три отдельных слоя: модель (Model), представление (View) и контроллер (Controller). Каждый из этих слоев выполняет определенные функции и отвечает за свою часть логики приложения.
* Модель (Model) представляет собой слой данных и бизнес-логики приложения. Он отвечает за хранение данных, их обработку и манипуляцию. Модель не зависит от других слоев и предоставляет интерфейс для работы с данными.
* Представление (View) отвечает за отображение данных модели пользователю. Он представляет данные в удобной для пользователя форме, такой как графический интерфейс или веб-страница. Представление получает данные от модели и отображает их, а также обрабатывает пользовательский ввод и отправляет его контроллеру.
* Контроллер (Controller) является посредником между моделью и представлением. Он получает пользовательский ввод от представления, обрабатывает его и взаимодействует с моделью для получения или обновления данных. Контроллер также отвечает за передачу обновленных данных представлению для их отображения.

```Kotlin
// Модель (Model)

data class User(val id: Int, val name: String)

class UserRepository {
    private val users = mutableListOf<User>()

    fun addUser(user: User) {
        users.add(user)
    }

    fun getUserById(id: Int): User? {
        return users.find { it.id == id }
    }
}

// Представление (View)

interface UserView {
    fun showUserDetails(user: User)
    fun showErrorMessage(message: String)
}

class ConsoleUserView {
    fun showUserDetails(user: User) {
        println("User Details:")
        println("ID: ${user.id}")
        println("Name: ${user.name}")
    }

    fun showErrorMessage(message: String) {
        println("Error: $message")
    }
}

class HtmlUserView {
    fun showUserDetails(user: User): String {
        return """
            <html>
            <body>
                <h1>User Details:</h1>
                <p>ID: ${user.id}</p>
                <p>Name: ${user.name}</p>
            </body>
            </html>
        """.trimIndent()
    }

    fun showErrorMessage(message: String): String {
        return """
            <html>
            <body>
                <h1>Error:</h1>
                <p>$message</p>
            </body>
            </html>
        """.trimIndent()
    }
}

// Контроллер (Controller)

class UserController(private val userRepository: UserRepository, private val userView: UserView) {
    fun addUser(id: Int, name: String) {
        val newUser = User(id, name)
        userRepository.addUser(newUser)
    }

    fun getUserDetails(id: Int) {
        val user = userRepository.getUserById(id)
        if (user != null) {
            userView.showUserDetails(user)
        } else {
            userView.showErrorMessage("User not found")
        }
    }
}

// Пример использования

fun main() {
    val userRepository = UserRepository()
    val userView = UserView()
    val userController = UserController(userRepository, userView)

    userController.addUser(1, "John Doe")
    userController.addUser(2, "Jane Smith")

    userController.getUserDetails(1)
    userController.getUserDetails(3) // User not found
}
    
```