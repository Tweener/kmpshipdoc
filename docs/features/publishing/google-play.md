# Publishing on Google Play (Android)

KMPShip comes preconfigured for automated deployment to the Google Play Console using GitHub Actions. However, it still requires a few one-time manual configuration steps to securely connect the app to the Play Console and prepare it for publishing.

---

## Step 1: Keystore setup

To sign your Android app and publish it to the Play Store, you need a secure keystore. This guide walks you through generating the keystore, encoding it, and setting up GitHub Actions secrets.

### 1. Generate a keystore

1. Open **Android Studio**.
2. Navigate to **Build > Generate Signed Bundle / APK**.
3. Choose either **Android App Bundle**, then click **Next**.
4. Click **Create new\...** to generate a new keystore.
5. For the "Key store path", choose:
   ```
   androidApp/config/keystore/my_app.keystore
   ```
6. Fill in the required keystore and key credentials.
7. Click **OK** to create the keystore. Your keystore is now generated.

!!! warning "Never commit your keystore to version control"

    The keystore is already added to the `.gitignore` file by default, but it's crucial to ensure that it remains secure and is not accidentally committed to your repository.

### 2. Encode the keystore for GitHub Actions

To safely upload the keystore to GitHub Actions:

1. Run the following command to base64-encode your keystore:
   ```bash
   base64 -i androidApp/config/keystore/my_app.keystore | pbcopy
   ```
2. Go to your **GitHub repository > Settings > Secrets and variables > Actions**.
3. Create a new **repository secret**:
    * `ANDROID_KEYSTORE_RELEASE_B64`: Paste the base64-encoded string

### 3. Add additional signing secrets

You'll also need to store these additional credentials as GitHub Actions secrets:

* `ANDROID_KEY_ALIAS_RELEASE`: your **key alias** used when creating the keystore
* `ANDROID_KEY_PASSWORD_RELEASE`: your **key password**
* `ANDROID_STORE_PASSWORD_RELEASE`: your **store password**

Once these secrets are configured, your GitHub Actions workflow will be able to sign and publish your app to Google Play securely.

---

## Step 2: Create a Google Cloud Service Account

To enable automated publishing, you need to securely connect GitHub Actions with your Google Play Console using a Service Account. Here's how:

### 2. Create a Service Account in Google Cloud

1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Select your existing project linked to the Play Console or create a new one.
3. Navigate to **IAM & Admin > Service Accounts**.
4. Click **Create service account**.
5. Enter a name (e.g., `play-console-uploader`) and description, then click **Create and continue**.
6. For the role, choose **Service Account User**, then click **Continue**.
7. Skip granting user access and click **Done**.
8. Locate your new service account and copy its **email address** — you'll need it in the next step.

### 3. Generate the JSON key

1. In the same list, click the **Actions (⋮)** menu for your service account just created and select **Manage keys**.
2. Click **Add key > Create new key**.
3. Select **JSON** and click **Create** — this will download a JSON file to your computer. Keep it secure.

### 4. Grant access in Google Play Console

1. Open your [Google Play Console](https://play.google.com/console).
2. Go to **Users and permissions > Invite new users**.
3. Paste the service account’s email.
4. Under **App permissions**, select your app.
5. Under **Account permissions**, you can either:
    * Select **Admin** for full access
    * Grant only the minimum required permissions:
        * View app information and download bulk reports (read-only)
        * Manage testing tracks and edit tester lists
        * Manage production releases, app signing, and Play Games Services
        * Click **Invite user** to finish.

The service account is now ready to interact with the Play Console API.

---

## Step 3: Store the JSON key

Once downloaded, place the `.json` file at the following location in your project:

```
androidApp/google-play-uploader.json
```

---

## Step 4: Configure CI/CD

### Configure Java SDK version

1. Go to your GitHub repository settings → **Secrets and variables** → **Actions** → **Variables** tab.
2. Create a new **repository variable**:
    * `JAVA_JDK_VERSION`: `21` (or the version you are using)

### Configure GitHub Actions secrets

1. Encode the `.json` key as base64:

    ```bash
    base64 -i androidApp/google-play-uploader.json | pbcopy
    ```

2. Go to your GitHub repository settings → **Secrets and variables** → **Actions** → **Secrets** tab.
3. Create a new **repository secret**:
    * `GOOGLE_PLAY_UPLOADER_JSON_KEY_B64`: Paste the base64 string you copied

This will be used by your CI/CD pipeline to deploy the app update automatically.

---

## Publish from your machine <small>(Optional)</small>

To publish a release update manually from your local machine, you can use the release script for Android only:

```bash
./release.sh android
```

---

!!! success "Google Play Console setup complete"

    You have successfully set up the automated publishing process for your app to the Google Play Store. You can now push updates directly from your CI/CD pipeline or manually from your local machine.
