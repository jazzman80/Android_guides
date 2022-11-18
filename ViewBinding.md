# View Binding
## Gradle
В Gradle уровня Module добавляем
```
buildFeatures{
    viewBinding true
}
```
## Реализация в активити

## Реализация в фрагменте
```
private var _binding: FragmentMainBinding? = null
private val binding get() = _binding!!
    
override fun onCreateView(
    inflater: LayoutInflater, container: ViewGroup?,
    savedInstanceState: Bundle?
): View? {
    _binding = FragmentMainBinding.inflate(inflater)
    return binding.root
}
```
