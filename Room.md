# Room
## Gradle
Добавляем kapt в секцию плагинов
```
plugins {
    ...
    id 'kotlin-kapt'
}
```
Подключение библиотек
```
implementation 'androidx.room:room-runtime:2.4.3'
implementation 'androidx.room:room-ktx:2.4.3'
kapt('androidx.room:room-compiler:2.4.3')
```
## Описание класса для таблицы БД
С помощью аннотаций отмечаем таблицу и столбцы в ней
```
@Entity(tableName = "user")
data class User(
    @PrimaryKey
    @ColumnInfo(name = "id")
    val id: Int,
    
    @ColumnInfo(name = "first_name")
    val firstName: String,
    
...
)
```
## DAO
Создаётся Data Access Object как интерфейс размеченный аннотациями следующего вида:
```
@Dao
interface UserDao {

    @Query("SELECT * FROM user")
    fun selectAll(): List<User>

    @Insert
    fun insert(user: User)

    @Delete
    fun delete(user: User)

    @Update
    fun update(user: User)
}
```
## Создание базы данных
БД создаётся как абстрактный класс вида:
```
@Database(entities = [User::class], version = 1)
abstract class AppDatabase: RoomDatabase() {
    abstract fun userDao():UserDao
}
```
## Подключение к приложению (при отсутствии DI)
Создаём класс Application и инициализируем экземпляр базы данных
```
class App : Application() {
    lateinit var db: AppDatabase

    override fun onCreate() {
        super.onCreate()

        db = Room.databaseBuilder(
            applicationContext,
            AppDatabase::class.java,
            "db"
        ).build()
    }
}
```
Во время разработки приложения инициализировать БД в памяти, чтобы избежать миграций
```
db = Room.inMemoryDatabaseBuilder(
    applicationContext,
    AppDatabase::class.java
).build()
```
Добавляем в манифест Application
```
<application
    android:name=".App"
    ...
```
## Доступ к DAO
```
val userDao: UserDao = (application as App).db.userDao()
```
