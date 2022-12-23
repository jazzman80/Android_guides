# Recycler View
## Gradle
В Gradle уровня module добавляем
```
implementation 'androidx.recyclerview:recyclerview:1.2.1'
```
для работы полезен view binding
```
buildFeatures {
    viewBinding = true
}
```
## View Adapter
Простейший адаптер
```
class MyAdapter(
    private val values: List<String>
) : RecyclerView.Adapter<MyViewHolder>() {
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val binding = MyItemBinding.inflate(LayoutInflater.from(parent.context))
        return MyViewHolder(binding)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.binding.textView.text = values[position]
    }

    override fun getItemCount(): Int = values.size
}

class MyViewHolder(val binding: MyItemBinding) : RecyclerView.ViewHolder(binding.root)
```
## Подключение во фрагменте
В OnViewCreated
```
binding.photoList.adapter = MyAdapter(some_data)
```
