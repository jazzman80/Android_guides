# Paging
## Gradle
Подключаем в Gradle уровня Module
```
// Paging
implementation 'androidx.paging:paging-runtime-ktx:3.1.1'
```
## Data Source
```
import androidx.paging.PagingSource
import androidx.paging.PagingState
import ru.iflex.mobile.module23.entity.Character

class DataSource(
    private val backend: RetrofitService,
) : PagingSource<Int, Character>() {
    override suspend fun load(
        params: LoadParams<Int>
    ): LoadResult<Int, Character> {
        return try {
            // Start refresh at page 1 if undefined.
            val nextPageNumber = params.key ?: 1
            val response = backend.loadList(nextPageNumber)
            LoadResult.Page(
                data = response.results,
                prevKey = null,
                nextKey = nextPageNumber + 1
            )
        } catch (e: Exception) {
            LoadResult.Error(e)
        }
    }

    override fun getRefreshKey(state: PagingState<Int, Character>): Int? {
        return state.anchorPosition?.let { anchorPosition ->
            val anchorPage = state.closestPageToPosition(anchorPosition)
            anchorPage?.prevKey?.plus(1) ?: anchorPage?.nextKey?.minus(1)
        }
    }
}
```
## Adapter
```
import android.graphics.Color
import android.view.LayoutInflater
import android.view.ViewGroup
import androidx.paging.PagingDataAdapter
import androidx.recyclerview.widget.DiffUtil
import androidx.recyclerview.widget.RecyclerView
import com.bumptech.glide.Glide
import ru.iflex.mobile.module23.databinding.ListItemBinding
import ru.iflex.mobile.module23.entity.Character

class CharacterAdapter:
    PagingDataAdapter<Character, CharacterAdapter.ViewHolder>(CharacterComparator) {
    override fun onCreateViewHolder(
        parent: ViewGroup,
        viewType: Int
    ): ViewHolder {
        val binding = ListItemBinding.inflate(LayoutInflater.from(parent.context))
        return ViewHolder(binding)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val item = getItem(position)
        if (item != null) {
            with(holder.binding) {
                name.text = item.name
                species.text = item.species
                status.text = item.status

                when (item.status) {
                    "Alive" -> {
                        circle.setColorFilter(Color.GREEN)
                    }
                    "Dead" -> {
                        circle.setColorFilter(Color.RED)
                    }
                    "unknown" -> {
                        circle.setColorFilter(Color.GRAY)
                    }
                }

                location.text = item.location.name

                Glide
                    .with(avatar)
                    .load(item.image)
                    .into(avatar)
            }
        }

    }

    class ViewHolder(val binding: ListItemBinding) : RecyclerView.ViewHolder(binding.root)

    object CharacterComparator : DiffUtil.ItemCallback<Character>() {
        override fun areItemsTheSame(oldItem: Character, newItem: Character): Boolean {
            return oldItem.id == newItem.id
        }

        override fun areContentsTheSame(oldItem: Character, newItem: Character): Boolean {
            return oldItem == newItem
        }
    }
}
```
## Внутри ViewModel
```
    private val retrofitService = RetrofitService.retrofit

    val characterList = Pager(
        config = PagingConfig(
            pageSize = 20,
            enablePlaceholders = false,
        ),
        pagingSourceFactory = { DataSource(retrofitService) }
    ).flow.cachedIn(viewModelScope)
```
## Внутри фрагмента onViewCreated
```
        val pagingAdapter = CharacterAdapter()
        val recyclerView = binding.recyclerView
        recyclerView.adapter = pagingAdapter

        viewLifecycleOwner.lifecycleScope.launch {
            viewModel.characterList.collectLatest {
                pagingAdapter.submitData(it)
            }
        }

        viewLifecycleOwner.lifecycleScope.launch{
            pagingAdapter.loadStateFlow.collectLatest {loadStates ->
                binding.progressBar.isVisible = loadStates.refresh is LoadState.Loading
                binding.recyclerView.isVisible = loadStates.refresh !is LoadState.Loading
                binding.errorPanel.isVisible = loadStates.refresh is LoadState.Error
            }
        }

        binding.reloadButton.setOnClickListener {
            pagingAdapter.refresh()
        }
```
