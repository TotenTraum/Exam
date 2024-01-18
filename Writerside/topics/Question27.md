# Вопрос 28

Когда речь идет о паттернах сохраняемости (persistence patterns), обычно имеются в виду различные подходы и шаблоны проектирования, используемые для сохранения данных в постоянное хранилище, такое как база данных или файловая система. 

## DataSet {collapsible="true"}
DataSet - это структура данных, представляющая собой коллекцию таблиц, которые содержат данные, а также метаданные, описывающие структуру этих данных. DataSet часто применяется для работы с реляционными базами данных и предоставляет удобный способ хранения, обработки и взаимодействия с данными.

```Kotlin
    import java.util.*

    data class Employee(val id: Int, val name: String, val position: String)
    
    fun main() {
        // Создание DataSet
        val dataSet = DataSet()
    
        // Создание таблицы "Employees"
        val employeesTable = DataTable("Employees")
        employeesTable.addColumn("ID", Int::class.java) // Добавление столбца ID с типом Int
        employeesTable.addColumn("Name", String::class.java) // Добавление столбца Name с типом String
        employeesTable.addColumn("Position", String::class.java) // Добавление столбца Position с типом String
    
        // Добавление данных в таблицу "Employees"
        employeesTable.addRow(listOf(1, "John Doe", "Manager"))
        employeesTable.addRow(listOf(2, "Jane Smith", "Developer"))
        employeesTable.addRow(listOf(3, "Bob Johnson", "Tester"))
    
        // Добавление таблицы "Employees" в DataSet
        dataSet.addTable(employeesTable)
    
        // Получение таблицы "Employees" из DataSet
        val retrievedTable = dataSet.getTable("Employees")
    
        // Вывод данных из таблицы "Employees"
        for (row in retrievedTable.rows) {
            val id = row["ID"] as Int
            val name = row["Name"] as String
            val position = row["Position"] as String
            println("ID: $id, Name: $name, Position: $position")
        }
    }
    
    class DataSet {
        private val tables: MutableMap<String, DataTable> = mutableMapOf()
    
        fun addTable(table: DataTable) {
            tables[table.name] = table
        }
    
        fun getTable(name: String): DataTable? {
            return tables[name]
        }
    }
    
    class DataTable(val name: String) {
        private val columnNames: MutableList<String> = mutableListOf()
        val rows: MutableList<MutableMap<String, Any>> = mutableListOf()
    
        fun addColumn(columnName: String, columnType: Class<*>) {
            columnNames.add(columnName)
        }
    
        fun addRow(values: List<Any>) {
            val row = mutableMapOf<String, Any>()
            for (i in columnNames.indices) {
                row[columnNames[i]] = values[i]
            }
            rows.add(row)
        }
    }
```

## Repository {collapsible="true"}

Репозиторий - это шаблон, который предоставляет абстракцию для доступа к данным. Он скрывает детали работы с источником данных (например, базой данных) и предоставляет унифицированный интерфейс для выполнения операций чтения и записи данных. Репозиторий позволяет изолировать остальную часть приложения от конкретной реализации хранилища данных и делает код более гибким и тестируемым.

```Kotlin
    data class Book(val id: Long, val title: String, val author: String)
    
    interface BookRepository {
        fun findById(id: Long): Book?
        fun findAll(): List<Book>
        fun save(book: Book)
        fun delete(book: Book)
    }
    
    class InMemoryBookRepository : BookRepository {
        private val books: MutableMap<Long, Book> = mutableMapOf()
    
        override fun findById(id: Long): Book? {
            return books[id]
        }
    
        override fun findAll(): List<Book> {
            return books.values.toList()
        }
    
        override fun save(book: Book) {
            books[book.id] = book
        }
    
        override fun delete(book: Book) {
            books.remove(book.id)
        }
    }
```

## Query object {collapsible="true"}

Query Object - это объект, который инкапсулирует логику запроса к базе данных, представляя его в виде объекта с определенными свойствами и методами. Это позволяет абстрагироваться от деталей низкоуровневых запросов SQL и упрощает создание, поддержку и повторное использование запросов.

Преимущества использования Query Object:

Разделение ответственности: Query Object отвечает только за выполнение запроса и получение результатов, отделяя логику запроса от других компонентов приложения.
Повторное использование: Query Object может быть использован повторно в разных частях приложения для выполнения одного и того же запроса.
Удобство тестирования: Query Object может быть легко протестирован, так как его поведение легко изолировать и проверить.

```Kotlin
    interface ICriteria {
        fun generateSQL(): String
    }
    
    class Criteria(private val operator: String, private val fieldName: String, private val obj: Any) : ICriteria {
        override fun generateSQL(): String = "$fieldName $operator $obj"
    }
    
    class PairCriteria(private val operator: String, private val first: Criteria, private val second: Criteria): ICriteria {
        override fun generateSQL(): String =
            "${first.generateSQL()} $operator $second"
    }
    
    
    object SqlBuilder {
        fun equal(fieldName: String, value: Any) = Criteria("=", fieldName, value)
        fun notEqual(fieldName: String, value: Any) = Criteria("!=", fieldName, value)
        fun greater(fieldName: String, value: Any) = Criteria(">", fieldName, value)
        fun less(fieldName: String, value: Any) = Criteria("<", fieldName, value)
        fun lessThat(fieldName: String, value: Any) = Criteria("<=", fieldName, value)
        fun greaterThat(fieldName: String, value: Any) = Criteria(">=", fieldName, value)
        fun and(first: Criteria, second: Criteria) = PairCriteria("and", first, second)
        fun or(first: Criteria, second: Criteria) = PairCriteria("and", first, second)
    }
    
    fun sql( function: SqlBuilder.()->ICriteria) {
        SqlBuilder.function().generateSQL()
    }
```