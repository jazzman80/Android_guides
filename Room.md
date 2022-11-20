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
