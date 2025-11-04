# Publishing on App Store Connect (iOS)

KMPShip includes a preconfigured Fastlane setup for automating your iOS app deployment to App Store Connect. To use it, you’ll need to complete a few initial configuration steps, such as creating your app in App Store Connect and generating an API key for authentication.

---

## Step 1: Create an API key for App Store Connect

Fastlane uses this API key to authenticate and submit builds.

1. In [App Store Connect](https://appstoreconnect.apple.com/), go to **Users and Access**.
2. Open the **Keys** tab.
3. Click **Generate API Key**.
4. Provide a name (e.g., `ci-upload-key`) and select the role **App Manager**.
5. Click **Generate**, then download the `.p8` file.
6. Note the **Issuer ID**, **Key ID**, and keep the `.p8` file safe — you’ll need these for CI/CD.
7. Base64 encode the `.p8` key file:
    ```bash
    base64 -i AuthKey_XXXXXX.p8 | pbcopy
    ```
8. Add the following repository secrets in GitHub:
    * `APP_STORE_CONNECT_API_KEY_B64`: the base64 string of your `.p8` file
    * `APP_STORE_CONNECT_API_ISSUER_ID`: the Issuer ID from App Store Connect
    * `APP_STORE_CONNECT_API_KEY_ID`: the Key ID from App Store Connect

These secrets will be used by the GitHub Actions workflow to authenticate with App Store Connect and publish your updates.

---

## Step 2: Create a Distribution certificate

To sign and ship iOS builds, you need an Apple Distribution certificate.

1. Open [Apple Developer Portal](https://developer.apple.com/account/resources/certificates/list).
2. Go to **Certificates**, then click the **+** icon.
3. Select **Apple Distribution** and click **Continue**.
4. Follow the instructions to upload a Certificate Signing Request (CSR). You can create a CSR using Keychain Access on macOS:
    - Open **Keychain Access**
    - Go to **Certificate Assistant > Request a Certificate from a Certificate Authority**
    - Save the CSR to disk
5. After uploading the CSR, download the generated `.cer` certificate.
6. Double-click the `.cer` file to install it into your Keychain.
7. Export the certificate along with the private key as a `.p12` file:
    - In **Keychain Access**, right-click the certificate > **Export** > choose `.p12`
8. Base64 encode the `.p12` file:
    ```bash
    base64 -i YourCertificate.p12 | pbcopy
    ```
9. Add the following repository secrets in GitHub:
    * `IOS_APP_CERTIFICATE_P12_B64`: the base64 string of your `.p12` file
    * `IOS_APP_CERTIFICATE_P12_PASSWORD`: the password used when exporting the `.p12`

---

## Step 3: Create provisioning profiles

Provisioning profiles link your distribution certificate to your app ID and enable app signing.

1. Go to the [Apple Developer Portal](https://developer.apple.com/account/resources/profiles/list).
2. Click the **+** icon to create a new provisioning profile.
3. Choose **App Store** as the distribution method and click **Continue**.
4. Select the **App ID** that matches your project’s bundle identifier.
5. Choose the **distribution certificate** you created earlier.
6. Name the profile (e.g., `MyApp_AppStore_Profile`) and click **Generate**.
7. Download the `.mobileprovision` file.
8. Base64 encode the file:
    ```bash
    base64 -i MyApp_AppStore_Profile.mobileprovision | pbcopy
    ```
9. Add it as a GitHub secret:
    * `IOS_PROVISIONING_PROFILE_B64`: the base64 string of your `.mobileprovision` file
    * `IOS_PROVISIONING_PROFILE_NAME`: the name of your provisioning profile (e.g., `iosApp`)

---

## Step 5: Configure CI/CD

### Configure Xcode version

1. Go to your GitHub repository settings → **Secrets and variables** → **Actions** → **Variables** tab.
2. Create a new **repository variable**:
   * `XCODE_VERSION`: `16.2` (or the version you are using)

---

## Publish from your machine <small>(Optional)</small>

You can use Fastlane to publish locally:

```bash
cd iosApp
fastlane release
```

---

!!! success "Aoo Store Connect setup complete"

    You have successfully set up the automated publishing process for your app to the App Store Connect. You can now push updates directly from your CI/CD pipeline or manually from your local machine.
