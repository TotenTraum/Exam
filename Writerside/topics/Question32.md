# Вопрос 33

Паттерны представления данных и обработки ввода - это шаблоны проектирования, которые помогают структурировать и управлять данными в пользовательском интерфейсе и обрабатывать ввод пользователя. Эти паттерны часто используются при разработке приложений, где требуется взаимодействие с данными, и включают такие архитектурные концепции, как Model-View-Controller (MVC), Model-View-ViewModel (MVVM), и другие.

MVVM (Model-View-ViewModel) - это архитектурный шаблон проектирования, который является эволюцией паттерна MVC. Он позволяет разделить компоненты приложения на три основных слоя: модель (Model), представление (View) и модель представления (ViewModel). MVVM акцентирует внимание на отделении логики представления от бизнес-логики и упрощает управление состоянием приложения.

Вот краткое описание каждого из слоев в MVVM:
* Модель (Model) представляет собой слой данных и бизнес-логики приложения, такой же, как в паттерне MVC. Он отвечает за получение, обработку и хранение данных.
* Представление (View) отвечает за отображение данных модели и обработку пользовательского ввода, также как в паттерне MVC. Однако, в MVVM, представление имеет более ограниченные обязанности и не содержит логику представления данных. Вместо этого, оно связывается с моделью представления (ViewModel) для получения данных и уведомления о событиях.
* Модель представления (ViewModel) является адаптером между моделью и представлением. Он содержит логику, необходимую для отображения данных модели в представлении и обработки пользовательского ввода. ViewModel также отвечает за управление состоянием представления и обновление модели при необходимости. Он обычно реализуется с использованием паттерна наблюдателя (Observer) или событийного программирования.

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

// Модель представления (ViewModel)

import androidx.lifecycle.ViewModel

class UserViewModel(private val userRepository: UserRepository) : ViewModel() {
    private var userDetails: User? = null
    private var errorMessage: String? = null

    fun addUser(id: Int, name: String) {
        val newUser = User(id, name)
        userRepository.addUser(newUser)
    }

    fun getUserDetails(id: Int) {
        val user = userRepository.getUserById(id)
        if (user != null) {
            userDetails = user
            errorMessage = null
        } else {
            userDetails = null
            errorMessage = "User not found"
        }
    }

    fun getUserDetailsLiveData(): User? {
        return userDetails
    }

    fun getErrorMessageLiveData(): String? {
        return errorMessage
    }
}

// Представление (View)

import androidx.lifecycle.Observer

class UserView(private val userViewModel: UserViewModel) {
    private val userDetailsObserver = Observer<User?> { userDetails ->
        if (userDetails != null) {
            showUserDetails(userDetails)
        } else {
            showErrorMessage(userViewModel.getErrorMessageLiveData())
        }
    }

    fun showUserDetails(user: User) {
        println("User Details:")
        println("ID: ${user.id}")
        println("Name: ${user.name}")
    }

    fun showErrorMessage(errorMessage: String?) {
        println("Error: $errorMessage")
    }

    fun bind() {
        userViewModel.getUserDetailsLiveData()?.let { userDetails ->
            showUserDetails(userDetails)
        }

        userViewModel.getErrorMessageLiveData()?.let { errorMessage ->
            showErrorMessage(errorMessage)
        }

        userViewModel.getUserDetailsLiveData()?.observeForever(userDetailsObserver)
        userViewModel.getErrorMessageLiveData()?.observeForever { errorMessage ->
            showErrorMessage(errorMessage)
        }
    }

    fun unbind() {
        userViewModel.getUserDetailsLiveData()?.removeObserver(userDetailsObserver)
    }
}

// Пример использования

fun main() {
    val userRepository = UserRepository()
    val userViewModel = UserViewModel(userRepository)
    val userView = UserView(userViewModel)

    userView.bind()

    userViewModel.addUser(1, "John Doe")
    userViewModel.addUser(2, "Jane Smith")

    userViewModel.getUserDetails(1)

    userViewModel.getUserDetails(3) // User not found

    userView.unbind()
}
```