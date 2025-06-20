# Networking

---

## Ktor HTTP Client

All networking is configured using [**Ktor**](https://ktor.io/), a multiplatform networking client.

* Centralized configuration for HTTP clients
* Support for logging, headers, and interceptors
* Easily integrates with the dependency injection setup

You can check the `KtorHttpClientFactory` class in the `Data` layer.

---

## Create an HTTP Client

To initialize a custom `HttpClient`, use the `KtorHttpClientFactory` provided in the `data` module:

```kotlin
val client = KtorHttpClientFactory.create(
    baseUrl = "example.com", // Replace with your API base URL
    protocol = URLProtocol.HTTPS,
    path = "v1", // Optional, resolves to "https://example.com/v1"
    readTimeout = 15_000 // Optional
)
```

This function centralizes common configuration, including logging, headers, and timeouts.

---

### Define an API Service

Create a service class that takes the `HttpClient` as a constructor parameter. Here's a generic example that performs a GET request:

```kotlin title="ExampleApiService.kt"
class ExampleApiService(
    private val httpClient: HttpClient
) {

    suspend inline fun <reified T> get(
        url: String,
        queryParams: Map<String, String>? = null
    ): Result<T> = try {
        val response = httpClient
            .get {
                url {
                    path(url)
                    queryParams?.forEach { parameters.append(it.key, it.value) }
                }
            }
            .body<T>()

        Result.success(response)
    } catch (throwable: Throwable) {
        Result.failure(throwable)
    }
}
```

This pattern abstracts networking logic into a reusable and testable format.

---

### Example: Marvel API Integration

A real-world usage can be found in `MarvelApiService`, which shows how to use the `ExampleApiService` pattern to query the [Marvel Comics API](https://developer.marvel.com/).

This is how you create an HTTPClient:

```kotlin title="DataSourceModule.kt"
single {
    MarvelApiService(
        httpClient = KtorHttpClientFactory.create(baseUrl = "gateway.marvel.com/v1/public", readTimeout = 3000L),
        publicApiKey = "{use your public key here}",
        privateApiKey = "{use your private key here}",
    )
}
factoryOf(::MarvelApiDataSource)
```

And this is how to use the `MarvelApiService` to fetch comics:

```kotlin title="MarvelApiDataSource.kt"
class MarvelApiDataSource(
    private val marvelApiService: MarvelApiService,
) {

    companion object {
        private const val API_QUERY_PARAM_OFFSET = "offset"
        private const val API_QUERY_PARAM_LIMIT = "limit"

        private const val API_PATH_COMICS = "comics"
    }

    suspend fun getComics(offset: Int? = null, limit: Int? = null): Result<List<MarvelApiComicModel>> =
        marvelApiService
            .get<MarvelApiComicModel>(
                url = API_PATH_COMICS,
                queryParams = mutableMapOf<String, String>().apply {
                    offset?.let { put(API_QUERY_PARAM_OFFSET, it.toString()) }
                    limit?.let { put(API_QUERY_PARAM_LIMIT, it.toString()) }
                },
            )
            .fold(
                onSuccess = { Result.success(it.data.results) },
                onFailure = { Result.failure(it) }
            )
}
```
