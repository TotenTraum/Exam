# Вопрос 35

Паттерны распределенного взаимодействия - это архитектурные и проектировочные концепции, которые помогают обеспечить эффективное взаимодействие между компонентами распределенных систем. Они предоставляют рекомендации и best practices для решения распространенных проблем, связанных с распределенным взаимодействием, таких как управление сетевыми протоколами, обмен сообщениями, обеспечение надежности и т. д.

### CQRS

CQRS (Command Query Responsibility Segregation) — это шаблон проектирования, впервые описанный в книге "Шаблоны корпоративных приложений" Мартина Фаулера и предназначенный для управления запросами и командами в приложении.

Основная идея CQRS состоит в том, чтобы разделить операции чтения (query) и операции записи (command) в отдельные модели или компоненты. Вместо того, чтобы использовать единую модель для обработки всех операций, CQRS предлагает использовать разные модели и механизмы для обработки различных операций.

В рамках CQRS операции чтения обрабатываются отдельно от операций записи. Это позволяет оптимизировать каждую сторону — операции чтения могут использовать оптимизированные модели данных и хранилища для получения данных, в то время как операции записи могут использовать отдельную модель для обработки команд и обновления состояния системы.

```Kotlin
// Модель для операций чтения (Query)
data class Product(
    val id: String,
    val name: String,
    val price: Double,
    val quantity: Int
)

// Модель для операций записи (Command)
data class CreateProductCommand(
    val name: String,
    val price: Double,
    val quantity: Int
)

// Класс для обработки операций чтения (Query Handler)
class ProductQueryHandler(private val databaseUrl: String, private val username: String, private val password: String) {
    fun getProductById(id: String): Product? {
        val connection = DriverManager.getConnection(databaseUrl, username, password)
        val statement = connection.createStatement()
        val query = "SELECT * FROM products WHERE id = '$id'"
        val resultSet = statement.executeQuery(query)
        val product = if (resultSet.next()) {
            mapResultSetToProduct(resultSet)
        } else {
            null
        }
        resultSet.close()
        statement.close()
        connection.close()
        return product
    }

    private fun mapResultSetToProduct(resultSet: ResultSet): Product {
        val id = resultSet.getString("id")
        val name = resultSet.getString("name")
        val price = resultSet.getDouble("price")
        val quantity = resultSet.getInt("quantity")
        return Product(id, name, price, quantity)
    }
}

// Класс для обработки операций записи (Command Handler)
class ProductCommandHandler(private val databaseUrl: String, private val username: String, private val password: String) {
    fun createProduct(command: CreateProductCommand) {
        val connection = DriverManager.getConnection(databaseUrl, username, password)
        val statement = connection.createStatement()
        val query = "INSERT INTO products (name, price, quantity) VALUES ('${command.name}', ${command.price}, ${command.quantity})"
        statement.executeUpdate(query)
        statement.close()
        connection.close()
        println("Product ${command.name} created with price ${command.price} and quantity ${command.quantity}")
    }
}

// Пример использования CQRS с базой данных
fun main() {
    val databaseUrl = "jdbc:mysql://localhost:3306/mydb"
    val username = "root"
    val password = "password"

    val queryHandler = ProductQueryHandler(databaseUrl, username, password)
    val commandHandler = ProductCommandHandler(databaseUrl, username, password)

    // Операция чтения
    val product = queryHandler.getProductById("123")
    if (product != null) {
        println("Product ID: ${product.id}")
        println("Name: ${product.name}")
        println("Price: ${product.price}")
        println("Quantity: ${product.quantity}")
    } else {
        println("Product not found")
    }

    // Операция записи
    val createProductCommand = CreateProductCommand("New Product", 49.99, 5)
    commandHandler.createProduct(createProductCommand)
}
```
