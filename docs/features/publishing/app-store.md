# Publishing on App Store Connect (iOS)

KMPShip includes a preconfigured Fastlane setup for automating your iOS app deployment to App Store Connect. To use it, you’ll need to complete a few initial configuration steps, such as creating your app in App Store Connect and generating an API key for authentication.

---

## Step 1: Create an API Key for App Store Connect

Fastlane uses this API key to authenticate and submit builds.

1. In [App Store Connect](https://appstoreconnect.apple.com/), go to **Users and Access**.
2. Open the **Keys** tab.
3. Click **Generate API Key**.
4. Provide a name (e.g., `ci-upload-key`) and select the role **App Manager**.
5. Click **Generate**, then download the `.p8` file.
6. Note the **Issuer ID**, **Key ID**, and keep the `.p8` file safe — you’ll need these for CI/CD.

---

## Step 2: Publish from Your Machine (Optional)

You can use Fastlane to publish locally:

```bash
cd iosApp
fastlane release
```

---

## Step 3: Configure CI/CD

To publish automatically via GitHub Actions:

1. Base64 encode the `.p8` key file:
    ```bash
    base64 -i AuthKey_XXXXXX.p8 | pbcopy
    ```
2. Add the following repository secrets in GitHub:
    * `APP_STORE_API_KEY_BASE64` – the base64 string of your `.p8` file
    * `APP_STORE_ISSUER_ID` – the Issuer ID from App Store Connect
    * `APP_STORE_KEY_ID` – the Key ID from App Store Connect

These secrets will be used by the GitHub Actions workflow to authenticate with App Store Connect and publish your updates.

---

!!! success "Store publishing setup complete"

    You have successfully set up the automated publishing process for your app to the Google Play Store and App Store Connect. You can now push updates directly from your CI/CD pipeline or manually from your local machine.
