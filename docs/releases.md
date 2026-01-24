# Releases

When you're ready to publish a new version of your app to the stores, you can choose between two release methods: automated releases via GitHub Actions or manual releases from your local machine.

Both Android and iOS release processes are powered by [Fastlane](https://fastlane.tools/), which automates building, signing, and uploading your app to the respective stores.

---

## Update the version name

Before releasing, update the version name in your shared codebase. Open the `ProjectConfiguration.kt` file and update the version:

```kotlin
const val versionName = "1.0.1"
```

Change it to the version number you want to release (e.g., `1.1.0`, `1.2.3`, etc.). This version will be used on both Android and iOS stores.

---

## Method 1: Release via GitHub Actions (Recommended)

This is the recommended approach for most teams as it provides a consistent CI/CD pipeline and audit trail.

### Step 1. Commit and push your changes

Make sure your version update is committed and pushed to your repository:

```bash
git add .
git commit -m "Bump version to 1.0.1"
git push origin main
```

### Step 2. Create a new GitHub release

Once the version name is updated and committed:

1. Go to your GitHub repository.
2. Click on **Releases** > **Draft a new release**.
3. Tag the release using the same version number (e.g., `v1.1.0`).
4. Fill in the release title and optional changelog.
5. Click **Publish release**.

This will automatically trigger the GitHub Actions workflows:

* `publishAppStore.yml` → for **iOS deployment** to App Store Connect.
* `publishGooglePlay.yml` → for **Android deployment** to Google Play Console.

Once triggered, the CI/CD pipelines will take care of building, signing, and uploading your app to the respective stores.

!!! success "Release Process Complete"

    Your app has been successfully released to both the App Store and Google Play Store. Users can now download the latest version from their respective stores.

---

## Method 2: Release from local machine

For quick releases or when you need to bypass CI/CD, you can use the `release.sh` script to release directly from your local machine.

### Prerequisites

Before running the release script, ensure you have the required credentials configured in your `local.properties` file or as environment variables.

#### Required properties for Android

```properties
# Google Play
GOOGLE_PLAY_UPLOADER_JSON_KEY_B64=<base64-encoded-json-key>

# Keystore configuration
ANDROID_KEYSTORE_RELEASE_B64=<base64-encoded-keystore>
ANDROID_KEYSTORE_RELEASE_FILE=my_app.keystore
ANDROID_KEY_ALIAS_RELEASE=<your-key-alias>
ANDROID_KEY_PASSWORD_RELEASE=<your-key-password>
ANDROID_STORE_PASSWORD_RELEASE=<your-store-password>
```

#### Required properties for iOS

```properties
# App Store Connect API
APP_STORE_CONNECT_API_KEY_B64=<base64-encoded-p8-key>
APP_STORE_CONNECT_API_KEY_ID=<your-key-id>
APP_STORE_CONNECT_API_ISSUER_ID=<your-issuer-id>

# iOS certificates and provisioning
IOS_PROVISIONING_PROFILE_B64=<base64-encoded-provisioning-profile>
IOS_PROVISIONING_PROFILE_NAME=<provisioning-profile-name>
IOS_APP_CERTIFICATE_P12_B64=<base64-encoded-p12-certificate>
IOS_APP_CERTIFICATE_P12_PASSWORD=<certificate-password>
```

### Running the release script

From your project root, run the release script with the desired platform and track/destination:

```bash
# Usage: ./scripts/release.sh [android|ios|all] [track/destination] [ios_destination]
```

=== "Android"

    ```bash
    # Android releases (track: internal, alpha, beta, or production)
    ./scripts/release.sh android              # internal track (default)
    ./scripts/release.sh android production   # production track
    ./scripts/release.sh android beta         # beta track
    ./scripts/release.sh android alpha        # alpha track
    ```

    This will release to Google Play Console on the specified track.

=== "iOS"

    ```bash
    # iOS releases (destination: testflight or production)
    ./scripts/release.sh ios                  # TestFlight (default)
    ./scripts/release.sh ios production       # App Store production
    ```

    This will release to App Store Connect with the specified destination.

=== "Both platforms"

    ```bash
    # Both platforms with defaults (Android: internal, iOS: testflight)
    ./scripts/release.sh all

    # Both platforms with custom settings
    # Second param: Android track, Third param: iOS destination
    ./scripts/release.sh all production production   # Android to production, iOS to production
    ./scripts/release.sh all beta testflight         # Android to beta, iOS to testflight
    ```

    This will release to both Google Play and App Store Connect sequentially.

### What the script does

The release script automates the following steps:

**For Android:**

1. Decodes and sets up the release keystore
2. Decodes the Google Play uploader JSON key
3. Verifies Ruby and Fastlane installation
4. Runs Fastlane `release` lane to build and publish to Google Play Console

**For iOS:**

1. Verifies Ruby and Fastlane installation
2. Decodes and sets up the App Store Connect API key
3. Sets up iOS provisioning profiles and certificates
4. Runs Fastlane `release` lane to build and upload to App Store Connect

!!! tip "Ruby requirement"

    Both Android and iOS releases require Ruby 3.1 or later, as Fastlane is used for the release process on both platforms. Make sure Ruby is installed on your machine before running releases.

!!! warning "Keystore and certificates"

    Keep your keystore files, certificates, and provisioning profiles secure. Never commit them directly to your repository. Always use base64-encoded values in `local.properties` or environment variables.
