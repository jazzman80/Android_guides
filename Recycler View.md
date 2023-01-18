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
## Адаптер с автообновлением
```
class PhotoAdapter(
    private val context: Context,
    values: List<Photo>
) : RecyclerView.Adapter<PhotoAdapter.MyViewHolder>() {

    private var values = values.toMutableList()

    @SuppressLint("NotifyDataSetChanged")
    fun setData(newValues: List<Photo>){
        values = newValues.toMutableList()
        notifyDataSetChanged()
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val binding = ItemPhotoBinding.inflate(LayoutInflater.from(parent.context))
        return MyViewHolder(binding)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        Glide.with(context)
            .load(values[position].path.toUri())
            .placeholder(R.drawable.ic_outline_camera_24)
            .into(holder.binding.photoView)
    }

    override fun getItemCount(): Int = values.size

    class MyViewHolder(val binding: ItemPhotoBinding) : RecyclerView.ViewHolder(binding.root)
}
```
Передачу новых данных делать через setData
```
        viewModel.photoList.observe(viewLifecycleOwner
        ) {
            photoList = it
            adapter.setData(photoList)
        }
```
## View Adapter
Простейший адаптер
```
import android.view.LayoutInflater
import android.view.ViewGroup
import androidx.recyclerview.widget.RecyclerView

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
binding.myRecyclerView.adapter = MyAdapter(some_data)
```
