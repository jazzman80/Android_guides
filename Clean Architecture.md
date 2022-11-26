# Clean Architecture
Реализация через Hilt
## Project Gradle
В Gradle уровня Project добавляем
```
plugins {
  ...
  id("com.google.dagger.hilt.android") version "2.44" apply false
}
```
## Module Gradle
В Gradle уровня Module добавляем kapt
```
plugins {
    ...
    id 'org.jetbrains.kotlin.kapt'
    id 'com.google.dagger.hilt.android'
}
```
И зависимости
```
implementation 'com.google.dagger:hilt-android:2.44'
kapt 'com.google.dagger:hilt-android-compiler:2.44'
```
Добавляем в корень
```
kapt {
  correctErrorTypes = true
}
```
## Application
Для реализации Hilt необходим класс Application с аннотацией
```
@HiltAndroidApp
class App : Application() { ... }
```
Зарегистрировать в manifest
```
<application
    android:name=".App"
    ...
/>
```
## Fragment, Activity и ViewModel
Fragment и Activity необходимо помечать аннотацией @AndroidEntryPoint.
ViewModel - @HiltViewModel

## Внедрение зависимостей
Конструкторы всех классов, учавствующих в зависисмостях, отметить как @Inject constructor(), даже если конструктор пустой!
