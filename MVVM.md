# MVVM
## ViewModel
### Gradle
В Gradle уровня Module необходимо имплементировать следующие библиотеки

```
// MVVM
implementation 'androidx.fragment:fragment-ktx:1.5.4'
implementation 'androidx.lifecycle:lifecycle-viewmodel-ktx:2.5.1'
implementation 'androidx.lifecycle:lifecycle-livedata-ktx:2.5.1'
```

### Создание ViewModel
ViewModel реализуется как класс, унаследованный от ViewModel
```
class MainViewModel : ViewModel() {}
```

### Реализация ViewModel в Activity или Fragment
ViewModel объявляется, как переменная
```
class MainFragment : Fragment() {
    private val viewModel: MainViewModel by viewModels()
...
}
```

## Организация потоков данных
Согласно модели MVVM внутри ViewModel организуются потоки данных, на которые подписываются View.
Потоки организуются двумя основными способами - через LiveData или StateFlow
### LiveData
Во ViewModel

```
private val _currentScrambledWord = MutableLiveData<String>()
val currentScrambledWord: LiveData<String>
   get() = _currentScrambledWord
```
    
Изменяем данные

```
private fun getNextWord() {
 ...
   } else {
       _currentScrambledWord.value = String(tempWord)
       ...
   }
}
```

В onViewCreated фрагмента подписываемся на изменение данных

```
viewModel.currentScrambledWord.observe(viewLifecycleOwner,
   { newWord ->
       binding.textViewUnscrambledWord.text = newWord
   })
```

### StateFlow
Во ViewModel

```
private val _state = MutableStateFlow<State>(State.Inactive)
val state: StateFlow<State> get() = _state.asStateFlow()
```
Изменяем данные

```
_state.value = State.InProgress
```
В onViewCreated фрагмента подписываемся на изменение данных

```
viewLifecycleOwner.lifecycleScope.launchWhenStarted {
            viewModel.state.collect{}
```
## Добавление зависимостей во ViewModel
Для правильного функционирования ViewModel создаётся ***только с помощью ViewModel Factory***!
### ViewModel

```
class MyViewModel(
    private val myRepository: MyRepository,
    private val savedStateHandle: SavedStateHandle
) : ViewModel() {
    companion object {
        val Factory: ViewModelProvider.Factory = viewModelFactory {
            initializer {
                val savedStateHandle = createSavedStateHandle()
                val myRepository = (this[APPLICATION_KEY] as MyApplication).myRepository
                MyViewModel(
                    myRepository = myRepository,
                    savedStateHandle = savedStateHandle
                )
            }
        }
    }
}
```

### Инициализация

```
private val viewModel: MyViewModel by viewModels { MyViewModel.Factory }
```


