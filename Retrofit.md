# Retrofit
## Gradle
В Gradle уровня Module добавляем
```
    //Retrofit Moshi
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.moshi:moshi-kotlin:1.13.0'
    implementation 'com.squareup.retrofit2:converter-moshi:2.9.0'
    kapt("com.squareup.moshi:moshi-kotlin-codegen:1.14.0")
```
## Entity
Пример создания вложенной сущности
```
@JsonClass(generateAdapter = true)
data class Response(
    @Json(name = "results") val result: List<User>
)

@JsonClass(generateAdapter = true)
data class User(
    @Json(name = "name") val name: Name,
    @Json(name = "email") val email: String,
    @Json(name = "phone") val phone: String,
    @Json(name = "cell") val cell: String,
    @Json(name = "picture") val picture: Picture
)

@JsonClass(generateAdapter = true)
data class Name(
    @Json(name = "first") val firstName: String,
    @Json(name = "last") val lastName: String
)

@JsonClass(generateAdapter = true)
data class Picture(
    @Json(name = "large") val imageUrl: String
)
```
## Сервис
Для работы необходимо создать сервис вида
```
import retrofit2.Call
import retrofit2.Retrofit
import retrofit2.create
import retrofit2.http.GET
import retrofit2.http.Query

interface RetrofitService {
    @GET("api/character")
    fun loadList(@Query("page") page: Int): Call<Response>

    companion object{
        const val pageSize = 20

        val retrofit by lazy {
            Retrofit
                .Builder()
                .baseUrl("https://rickandmortyapi.com")
                .build()
                .create<RetrofitService>()
        }
    }

}

class Response(
    val results: List<Character>
)
```
## Запрос из ViewModel
```
    fun getUser() {
        viewModelScope.launch {
            _user.value = RetrofitService.getUser.getUser().result.first()
        }
    }
```
