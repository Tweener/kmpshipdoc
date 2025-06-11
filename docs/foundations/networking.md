# Networking

---

## Configuration

All networking is configured using [**Ktor**](https://ktor.io/), a multiplatform networking client.

* Centralized configuration for HTTP clients
* Support for logging, headers, and interceptors
* Easily integrates with the dependency injection setup

You can check the `KtorHttpClientFactory` class in the `Data` layer.

---

## API Requests

The project includes a preconfigured architecture for making safe API calls using:

* **Ktor client** with serializers
* Structured API service interfaces
* Error handling and response mapping

These layers ensure that networking code remains predictable, reusable, and decoupled from the rest of your app logic.
