# Navigation with Compose
## Gradle
В Gradle уровня Module добавляем
```
// Navigation
implementation "androidx.navigation:navigation-compose:2.5.3"
```
## Navigation
Создаём файл Navigation со следующей структурой
```
@Composable
fun Navigation(){
    NavHost(navController = rememberNavController(), startDestination = ""){
        composable(""){}
    }
}
```
Добавляем Navigation в MainActivity setContent
```
Navigation()
```
