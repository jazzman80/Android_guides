# Навигация
## Gradle
В Gradle уровня Module добавляем
```
implementation 'androidx.navigation:navigation-fragment-ktx:2.5.3'
implementation 'androidx.navigation:navigation-ui-ktx:2.5.3'
```
## NavGraph
В папке Navigation создаём nav_graph   
Подключаем к Activity через NavHostFragment   
Используем графический интерфейс nav_graph
## SafeArgs
Рекомендуемый способ навигации   
В самый верх Gradle уровня Module добавляем
```
buildscript {
    repositories {
        google()
    }
    dependencies {
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:2.5.3"
    }
}
```
В раздел plugins добавляем
```
    id 'androidx.navigation.safeargs.kotlin' version("2.5.3")
```
## Переходы
```
    val action = MainFragmentDirections.actionMainFragmentToNewTaskFragment()
    findNavController().navigate(action)
```
