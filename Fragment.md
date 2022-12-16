# Fragment
## View Binding
В Gradle уровня Module прописываем
```
buildFeatures {
    viewBinding = true
}
```
И зависимости для работы binding
```
implementation 'androidx.fragment:fragment-ktx:1.5.4'
```
## Layout
Создаём layout вида "fragment_main.xml", это необходимо для создания классов binding
## Класс Fragment
Создаём класс фрагмента и наследуем его от Fragment
```
class MainFragment : Fragment() {

    private var _binding: FragmentMainBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        _binding = FragmentMainBinding.inflate(inflater)
        return binding.root
    }
    
    override fun onDestroy() {
        super.onDestroy()
        _binding = null
    }
}
```
