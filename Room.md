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
