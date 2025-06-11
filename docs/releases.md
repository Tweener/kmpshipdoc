# Releases

When you're ready to publish a new version of your app to the stores, follow these simple steps to trigger the release automation.

---

## Step 1. Update the version name

In your shared codebase, open the `ProjectConfiguration.kt` file and update the version name:

```kotlin
const val versionName = "1.0.1"
```

Change it to the version number you want to release (e.g., `1.1.0`, `1.2.3`, etc.). This version will be used on both Android and iOS stores.

---

## Step 2. Create a new GitHub release

Once the version name is updated and committed:

1. Go to your GitHub repository.
2. Click on **Releases** > **Draft a new release**.
3. Tag the release using the same version number (e.g., `v1.1.0`).
4. Fill in the release title and optional changelog.
5. Click **Publish release**.

This will automatically trigger the GitHub Actions workflows:

* `publishAppStore.yml` → for **iOS deployment** to App Store Connect.
* `publishGooglePlay.yml` → for **Android deployment** to Google Play Console.

---

Once triggered, the CI/CD pipelines will take care of building, signing, and uploading your app to the respective stores.

---

!!! success "Release Process Complete"

    Your app has been successfully released to both the App Store and Google Play Store. Users can now download the latest version from their respective stores.
