# Remote config

KMPShip integrates [Firebase Remote Config](https://firebase.google.com/docs/remote-config) to allow you to dynamically change the behavior and appearance of your app without requiring users to download an update.

With Remote Config, you can:

* Enable or disable features remotely
* Customize app behavior for different audiences
* Roll out features gradually
* Experiment with A/B testing

---

## Using Remote Config in KMPShip

KMPShip provides a structured and type-safe way to work with Remote Config values by categorizing them into **Feature Flags** and **App Configuration Parameters**.

### Create your keys in Firebase Remote Config

Open your project in Firebase console and navigate to the Remote Config section:
```
https://console.firebase.google.com/u/0/project/{yourProjectId}/config/env/firebase
```

Replace `{yourProjectId}` with your actual Firebase project ID.

Here, you can define your keys and their default values.

### Declare your keys in KMPShip

All keys defined in your Firebase Remote Config console must be added to the `RemoteConfigKey` enum class:

```kotlin
enum class RemoteConfigKey(val value: String) {
    EXAMPLE_BOOLEAN_KEY("my_boolean_key"),
    EXAMPLE_STRING_KEY("my_string_key"),
}
```

---

## Feature Flags <small>(Boolean values)</small>

Boolean Remote Config values are considered **Feature Flags** in KMPShip.

### Steps to add a Feature Flag:

1. Add a new value to the `FeatureFlag` enum class
   Location: `com.example.myapp.domain.entity`
2. Add a mapping in the `RemoteConfigFeatureFlagModelMapper` class
   Location: `com.example.myapp.data.source.firebase.remoteconfig.mapper`
3. Use `GetFeatureFlagUseCase` from your ViewModel
   Location: `com.example.myapp.domain.usecase.settings`

### Example:

1. Define in Firebase Remote Config:

    ```json
    "onboarding_enabled": true
    ```

2. Update `RemoteConfigKey` enum class:

    ```kotlin
    enum class RemoteConfigKey(val value: String) {
        ONBOARDING_ENABLED("onboarding_enabled"),
    }
    ```
   
3. Update `FeatureFlag` enum class:

    ```kotlin
    enum class FeatureFlag(val defaultValue: Boolean) {
        ONBOARDING_ENABLED(true),
    }
    ```

4. Update `RemoteConfigFeatureFlagModelMapper` mapper:

    ```kotlin
    class RemoteConfigFeatureFlagModelMapper : EntityToModelMapper<FeatureFlag, RemoteConfigModel> {
    
        override fun convertToModel(entity: FeatureFlag): RemoteConfigModel {
            val key = when (entity) {
                FeatureFlag.ONBOARDING_ENABLED -> RemoteConfigKey.ONBOARDING_ENABLED
            }
    
            return RemoteConfigModel(key = key)
        }
    }
    ```

5. Use in ViewModel:

    ```kotlin
    val isOnboardingEnabled = getFeatureFlagUseCase.execute(
        getFeatureFlagUseCase.InputParams(
            featureFlag = FeatureFlag.ONBOARDING_ENABLED
        )
    )
    ```

---

## App Configuration Parameters <small>(Strings, Numbers, Arrays...)</small>

All other Remote Config value types are considered general **App Configuration Parameters**.

### Steps to add an App Configuration key:

1. Add a value to the `AppConfiguration` data class
   Location: `com.example.myapp.domain.entity`
2. Add mapping in `AppConfigurationRepositoryImpl`
   Location: `com.example.myapp.data.repository`
3. Use `AppConfigurationRepository` to access the value
   Location: `com.example.myapp.domain.usecase.config` (e.g. `GetAppRatingAskPeriodMonthsUseCase`)

### Example:

1. Define in Firebase Remote Config:

    ```json
    "app_rating_ask_period_months": "3" // Period between two requests to show in-app review/ratings (in months)
    ```

2. Update `RemoteConfigKey` enum class:

    ```kotlin
    enum class RemoteConfigKey(val value: String) {
        APP_RATING_ASK_PERIOD_MONTHS("app_rating_ask_period_months"),
    }
    ```

3. Update `AppConfiguration` data class:

    ```kotlin
    data class AppConfiguration(
        val appRatingAskPeriodMonths: Int,
    )
    ```

4. Update `AppConfigurationRepositoryImpl` repository implementation:

    ```kotlin
    class AppConfigurationRepositoryImpl(
        private val localAppConfigurationDataSource: LocalAppConfigurationDataSource,
        private val remoteConfigDataSource: FirebaseRemoteConfigDataSource,
    ) : AppConfigurationRepository {
    
        override suspend fun getAppConfiguration(): AppConfigurationRepository.OutputParams.GetAppConfiguration {
            if (localAppConfigurationDataSource.appConfiguration == null) {
                val appRatingAskPeriodMonths = remoteConfigDataSource.getLong(key = RemoteConfigModel(key = RemoteConfigKey.APP_RATING_ASK_PERIOD_MONTHS).key.value, defaultValue = 3)
    
                localAppConfigurationDataSource.appConfiguration = AppConfiguration(
                    appRatingAskPeriodMonths = appRatingAskPeriodMonths.toInt(),
                )
            }
    
            return AppConfigurationRepository.OutputParams.GetAppConfiguration(appConfiguration = localAppConfigurationDataSource.appConfiguration!!)
        }
    }
    ```

5. Use in code:

    ```kotlin
    val months = appConfigurationRepository.getAppConfiguration().appConfiguration.appRatingAskPeriodMonths
    ```

---

!!! success "Remote Config setup complete"
    
    You have successfully set up Remote Config in your KMPShip project. You can now manage feature flags and app configuration parameters dynamically through Firebase.
