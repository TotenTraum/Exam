# Вопрос 34

Паттерны распределенного взаимодействия - это архитектурные и проектировочные концепции, которые помогают обеспечить эффективное взаимодействие между компонентами распределенных систем. Они предоставляют рекомендации и best practices для решения распространенных проблем, связанных с распределенным взаимодействием, таких как управление сетевыми протоколами, обмен сообщениями, обеспечение надежности и т. д.

## Remote facade {collapsible="true"}

Remote Facade — это паттерн проектирования, который позволяет скрыть сложность распределенной системы и предоставить простой интерфейс для удаленного взаимодействия с ней.

Основная цель Remote Facade состоит в том, чтобы предоставить единый точку входа для клиентов, которые хотят взаимодействовать с распределенной системой. Вместо того чтобы клиентам приходилось напрямую взаимодействовать с различными удаленными компонентами и протоколами, Remote Facade предоставляет упрощенный интерфейс, скрывая детали взаимодействия.

Service Layer - это еще один шаблон проектирования, он представляет собой слой в архитектуре приложения, который инкапсулирует бизнес-логику и предоставляет упрощенный интерфейс для взаимодействия с этой логикой. Service Layer позволяет разделить логику приложения на более мелкие и управляемые компоненты, что делает ее более модульной и тестируемой.

Remote Facade и Service Layer имеют схожие цели: обеспечить упрощенный интерфейс для клиентов и скрыть сложности внутренней системы. Однако Remote Facade сосредоточен на предоставлении интерфейса для удаленного взаимодействия, в то время как Service Layer фокусируется на инкапсуляции бизнес-логики и предоставляет интерфейс для взаимодействия с этой логикой внутри одного приложения.

![image_12.png](image_12.png)

### Пример

```Kotlin
interface IRemoteFacade {
    fun login(username: String, password: String): Boolean
    fun fetchData(): List<String>
    fun performAction(data: String)
}

// Remote Facade
class RemoteFacadeServer: IRemoteFacade {
    fun login(username: String, password: String): Boolean {
        // Логика аутентификации пользователя удаленной системы
        // ...
        return true
    }

    fun fetchData(): List<String> {
        // Логика получения данных из удаленной системы
        // ...
        return listOf("Data 1", "Data 2", "Data 3")
    }

    fun performAction(data: String) {
        // Логика выполнения действия в удаленной системе
        // ...
        println("Performing action with data: $data")
    }
}

class RemoteFacadeAPI: IRemoteFacade {
    fun login(username: String, password: String): Boolean {
        return HttpClient(...)
    }

    fun fetchData(): List<String> {
        return HttpClient(...)
    }

    fun performAction(data: String) {
        ...
    }
}

// Пример использования Remote Facade
fun main() {
    // Создание Remote Facade
    val remoteFacade = RemoteFacadeAPI()

    // Использование Remote Facade для взаимодействия с удаленной системой
    val loggedIn = remoteFacade.login("username", "password")
    if (loggedIn) {
        val data = remoteFacade.fetchData()
        println("Received data: $data")

        remoteFacade.performAction("Some action data")
    } else {
        println("Login failed.")
    }
}
```

## DTO {collapsible="true"}
Data Transfer Object (DTO) — это шаблон проектирования, описанный в книге "Шаблоны корпоративных приложений" Мартина Фаулера. Он используется для передачи данных между компонентами системы или между клиентом и сервером.

Основная идея DTO состоит в том, чтобы создать объект данных, который инкапсулирует определенный набор данных и предоставляет упрощенный интерфейс для доступа к этим данным. DTO позволяет собрать все необходимые данные в один объект и передать его между компонентами системы без необходимости передавать каждое поле отдельно.

```Kotlin 
    // Data Transfer Object (DTO)
data class UserDTO(
    val id: Long,
    val username: String,
    val email: String
)

// Пример использования Data Transfer Object
fun main() {
    // Создание экземпляра UserDTO
    val userDTO = UserDTO(1, "john_doe", "john@example.com")

    // Использование полей DTO
    println("User ID: ${userDTO.id}")
    println("Username: ${userDTO.username}")
    println("Email: ${userDTO.email}")

    // Передача DTO между компонентами системы
    processUser(userDTO)
}

// Функция для обработки UserDTO
fun processUser(userDTO: UserDTO) {
    // Обработка данных пользователя
    println("Processing user ${userDTO.username} with ID ${userDTO.id}")
    // ...
}
```