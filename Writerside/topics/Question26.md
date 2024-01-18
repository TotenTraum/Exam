# Вопрос 27

Когда речь идет о паттернах сохраняемости (persistence patterns), обычно имеются в виду различные подходы и шаблоны проектирования, используемые для сохранения данных в постоянное хранилище, такое как база данных или файловая система. 

## DAO {collapsible="true"}

DAO (Data Access Object) - это паттерн проектирования, который используется для абстрагирования доступа к данным от конкретных источников данных. Он предоставляет единый интерфейс для работы с данными и скрывает детали реализации доступа к базе данных или другим источникам данных.

```Kotlin
    interface UserDao {
        fun createUser(user: User)
        fun getUserById(userId: Int): User?
        fun updateUser(user: User)
        fun deleteUser(user: User)
        // другие методы
    }

    class JdbcUserDao : UserDao {
        override fun createUser(user: User) {
        }
    
        override fun getUserById(userId: Int): User? {
            return null
        }
    
        override fun updateUser(user: User) {
        }
    
        override fun deleteUser(user: User) {
        }
        // другие методы
    }
```

Таким образом, паттерн DAO позволяет абстрагировать доступ к данным от конкретных источников данных и предоставляет единый интерфейс для работы с данными в приложении. Это упрощает тестирование, поддержку и изменение источников данных, поскольку изменения в реализации DAO не затрагивают остальные части приложения, использующие его интерфейс.



## Unit of work {collapsible="true"}
Unit of Work - это паттерн проектирования, который используется для управления транзакционными операциями и изменениями состояния объектов в рамках определенной рабочей сессии. Он позволяет группировать операции над объектами и обеспечивает целостность данных при работе с различными источниками данных, такими как базы данных.

```Kotlin
    class UnitOfWork {
        private val dirtyObjects: MutableList<Any> = mutableListOf()
        private val removedObjects: MutableList<Any> = mutableListOf()
        private val newObjects: MutableList<Any> = mutableListOf()
    
        fun registerDirty(obj: Any) {
            dirtyObjects.add(obj)
        }
    
        fun registerRemoved(obj: Any) {
            removedObjects.add(obj)
        }
        
        fun registerNew(obj: Any) {
            removedObjects.add(obj)
        }
    
        fun commit() {
            // Применить изменения к источникам данных
            // Например, сохранить изменения в базе данных
            dirtyObjects.forEach { obj ->
                // Применить изменения объекта
                // ...
            }
            newObjects.forEach { obj ->
                // Применить изменения объекта
                // ...
            }
            removedObjects.forEach { obj ->
                // Применить изменения объекта
                // ...
            }
        }
    
        fun rollback() {
            // Отменить изменения
            dirtyObjects.clear()
            newObjects.clear()
            removedObjects.clear()
        }
    }
    
    class UserRepository(private val unitOfWork: UnitOfWork) {
        fun createUser(user: User) {
            // Создание пользователя
            // ...
            unitOfWork.registerNew(user)
        }
    
        fun updateUser(user: User) {
            // Обновление пользователя
            // ...
            unitOfWork.registerDirty(user)
        }
    
        // Другие методы репозитория
    }
```