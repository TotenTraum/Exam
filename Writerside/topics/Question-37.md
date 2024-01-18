# Вопрос 37

Базовые архитектурные паттерны представляют собой шаблоны проектирования, которые помогают разработчикам создавать стабильные, масштабируемые и легко поддерживаемые программные системы.

## Gateway {collapsible="true"}

Шаблон Gateway представляет собой компонент, который обеспечивает единый интерфейс для взаимодействия между приложением и внешней системой, такой как база данных или веб-сервис. Он выступает в роли посредника между приложением и внешней системой, скрывая детали взаимодействия и предоставляя единый интерфейс для работы с данными.

```Kotlin 
// Интерфейс Gateway, определяющий методы доступа к данным
interface DataGateway {
    fun fetchData(): String
    fun saveData(data: String)
}

// Реализация Gateway для работы с базой данных
class DatabaseGateway : DataGateway {
    override fun fetchData(): String {
        // Логика получения данных из базы данных
        return "Data from database"
    }

    override fun saveData(data: String) {
        // Логика сохранения данных в базу данных
        println("Data saved to database: $data")
    }
}

// Реализация Gateway для работы с веб-сервисом
class WebServiceGateway : DataGateway {
    override fun fetchData(): String {
        // Логика получения данных из веб-сервиса
        return "Data from web service"
    }

    override fun saveData(data: String) {
        // Логика сохранения данных в веб-сервисе
        println("Data saved to web service: $data")
    }
}

// Пример использования Gateway
fun main() {
    val dbGateway: DataGateway = DatabaseGateway()
    val webServiceGateway: DataGateway = WebServiceGateway()

    val dataFromDB = dbGateway.fetchData()
    println("Data from DB: $dataFromDB")

    val dataFromWebService = webServiceGateway.fetchData()
    println("Data from web service: $dataFromWebService")

    dbGateway.saveData("New data")
    webServiceGateway.saveData("New data")
}
```


## Mapper {collapsible="true"}

Шаблон Mapper предназначен для управления преобразованием данных между объектами предметной области и структурами данных, такими как база данных или внешний API. Он помогает изолировать слой доступа к данным от остальных компонентов приложения и обеспечивает гибкость и расширяемость при изменении структуры данных или объектной модели.

```Kotlin

// Объект предметной области
data class User(val id: Int, val name: String, val email: String)

// Маппер для преобразования пользователей в строки и обратно
class UserMapper {
    fun mapToString(user: User): String {
        return "User[id=${user.id}, name=${user.name}, email=${user.email}]"
    }

    fun mapFromString(userString: String): User {
        val id = getValueFromUserString(userString, "id")
        val name = getValueFromUserString(userString, "name")
        val email = getValueFromUserString(userString, "email")
        return User(id, name, email)
    }

    private fun getValueFromUserString(userString: String, key: String): String {
        val regex = "$key=([^,]+)".toRegex()
        val matchResult = regex.find(userString)
        return matchResult?.groupValues?.getOrNull(1) ?: ""
    }
}

// Пример использования маппера
fun main() {
    val user = User(1, "John Doe", "johndoe@example.com")

    val userMapper = UserMapper()

    // Преобразование пользователя в строку
    val userString = userMapper.mapToString(user)
    println("User as string: $userString")

    // Преобразование строки в пользователя
    val parsedUser = userMapper.mapFromString(userString)
    println("Parsed user: $parsedUser")
}
```