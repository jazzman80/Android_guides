# Recycler View
## Gradle
В Gradle уровня module добавляем
```
// RecyclerView
implementation 'androidx.recyclerview:recyclerview:1.2.1'
```
для работы полезен view binding
```
buildFeatures {
    viewBinding = true
}
```
## Адаптер с автообновлением через DiffUtil
```
import android.view.LayoutInflater
import android.view.ViewGroup
import androidx.recyclerview.widget.DiffUtil
import androidx.recyclerview.widget.RecyclerView

// TODO Рефакторим название класса
class MyAdapter(
    // TODO Устанавливаем правильный тип данных списка Ctrl + Shift + R - автозамена
    private var values: List<MyEntity>
) : RecyclerView.Adapter<MyAdapter.ViewHolder>() {
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        // TODO Прописываем и импортируем биндинг элемента списка
        val binding = MyItemBinding.inflate(LayoutInflater.from(parent.context))
        return ViewHolder(binding)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        // TODO Визуализируем данные на экране
        holder.binding.myView.text = values[position].text
    }

    override fun getItemCount(): Int = values.size

    class ViewHolder(val binding: ItemBinding) : RecyclerView.ViewHolder(binding.root)

    class DuffUtilCallback(
        // TODO Устанавливаем правильный тип данных списков
        private val oldList: List<MyEntity>,
        private val newList: List<MyEntity>
    ) : DiffUtil.Callback(){
        override fun getOldListSize(): Int = oldList.size
        override fun getNewListSize(): Int = newList.size

        override fun areItemsTheSame(oldItemPosition: Int, newItemPosition: Int): Boolean =
            // TODO Прописываем правило равенства элементов списка
            oldList[oldItemPosition].name == newList[newItemPosition].name

        override fun areContentsTheSame(oldItemPosition: Int, newItemPosition: Int): Boolean =
            // TODO Прописываем правило равенства контента элементов списка
            oldList[oldItemPosition].position == newList[newItemPosition].position
    }

    // TODO Устанавливаем правильный тип данных списка
    fun update(newElements: List<MyEntity>) {

        val result = DiffUtil.calculateDiff(
            DuffUtilCallback(
                oldList = values,
                newList = newElements
            )
        )

        values = newElements
        result.dispatchUpdatesTo(this)
    }
```
В переменных фрагмента
```
// TODO Прописать и импортировать правильный тип адаптера
private var adapter: MyAdapter? = null
```
В onViewCreated фрагмента
```
// TODO Прописать правильный тип адаптера и ID RecyclerView
adapter = MyAdapter(emptyList())
binding.recyclerView.adapter = adapter
```
Передачу новых данных делать через update
```
viewModel.elements.observe(
    viewLifecycleOwner
) { newElements ->
    adapter!!.update(newElements)
}
```
## View Adapter
Простейший адаптер
```
import android.view.LayoutInflater
import android.view.ViewGroup
import androidx.recyclerview.widget.RecyclerView
import com.example.sandbox.databinding.ItemBinding

class MyAdapter(
    private val values: List<String>
) : RecyclerView.Adapter<MyAdapter.ViewHolder>() {
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val binding = ItemBinding.inflate(LayoutInflater.from(parent.context))
        return ViewHolder(binding)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.binding.textView.text = values[position]
    }

    override fun getItemCount(): Int = values.size

    class ViewHolder(val binding: ItemBinding) : RecyclerView.ViewHolder(binding.root)
}
```
## DiffUtil Callback
Для автоматического вычисления изменений пишем класс DiffUtilCallback
```
import androidx.recyclerview.widget.DiffUtil

class ElementDuffUtilCallback(
    private val oldList: List<Worker>,
    private val newList: List<Worker>
) : DiffUtil.Callback(){
    override fun getOldListSize(): Int = oldList.size
    override fun getNewListSize(): Int = newList.size

    override fun areItemsTheSame(oldItemPosition: Int, newItemPosition: Int): Boolean =
        oldList[oldItemPosition].name == newList[newItemPosition].name

    override fun areContentsTheSame(oldItemPosition: Int, newItemPosition: Int): Boolean =
        oldList[oldItemPosition].position == newList[newItemPosition].position
}
```
## Подключение во фрагменте
В OnViewCreated
```
binding.myRecyclerView.adapter = MyAdapter(some_data)
```
