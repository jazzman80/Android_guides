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
private const val BASE_URL = "https://randomuser.me"

object RetrofitService {
    private val retrofit =
        Retrofit.Builder().baseUrl(BASE_URL).addConverterFactory(MoshiConverterFactory.create())
            .build()

    val getUser: GetUser = retrofit.create(
        GetUser::class.java
    )
}

interface GetUser {
    @GET("api")
    suspend fun getUser(): Response
}
```
## Запрос из ViewModel
```
    fun getUser() {
        viewModelScope.launch {
            _user.value = RetrofitService.getUser.getUser().result.first()
        }
    }
```

@JsonClass(generateAdapter = true)
data class Name(
    @Json(name = "first") val firstName: String,
    @Json(name = "last") val lastName: String
)

@JsonClass(generateAdapter = true)
data class Picture(
    @Json(name = "large") val imageUrl: String
)
