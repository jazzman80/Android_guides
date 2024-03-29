# Activity
## Compose
Для работы с Compose
```
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            // TODO
        }
    }
}
```
## Подключение ViewBinding
В Gradle уровня Module прописываем ViewBinding
```
buildFeatures {
    viewBinding = true
}
```
## Layout
Создаём layout вида "activity_main.xml", это необходимо для создания viewBinding классов
## Создание Activity
Создаём класс и наследуем его от AppComPatActivity, реализуем OnCreate и binding
```
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity

class MainActivity: AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
    }
}
```
## Manifest
В манифесте прописываем activity с точкой входа
```
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```
И без точки входа
```
<activity android:name=".MainActivity"/>
```
## Activity с Databinding
```
        val binding : ActivityMainBinding =
            DataBindingUtil.setContentView(this, R.layout.activity_main)
```
