# Store publishing

KMPShip provides a streamlined way to publish your app updates to both the Google Play Store and Apple App Store. This guide will walk you through the steps required to set up automated deployments for your Android and iOS apps.

## Prerequisite: Create Developer Accounts

!!! note

    You can skip this step if you already have both Google Play Console and App Store Connect accounts created and set up. 

Before setting up automated deployments, you must first complete a **manual setup** for both app stores.

If you haven't done this yet, follow the linked guide to create your developer accounts before continuing:

👉 [Create stores accounts](../tutorials/stores/)

## Android (Google Play)

### Step 1: Create a Google Cloud Service Account

To enable automated publishing, you need to securely connect GitHub Actions with your Google Play Console using a Service Account. Here's how:

#### 1. Generate a keystore

Before continuing, make sure your Android app is signed with a release keystore:

1. Open **Android Studio**.
2. Navigate to **Build > Generate Signed Bundle / APK**.
3. Choose either **APK** or **Android App Bundle**, then click **Next**.
4. Click **Create new\...** to generate a new keystore.
5. For the "Key store path", choose `androidApp/config/keystore/my_app.keystore`.
6. Fill in the required keystore and key credentials.
7. Click **OK** to create the keystore.

Your keystore is now generated and saved securely. It wil be used for signing your app.

!!! warning "Never commit your keystore to version control"

    The keystore is already added to the `.gitignore` file by default, but it's crucial to ensure that it remains secure and is not accidentally committed to your repository.

#### 2. Create a Service Account in Google Cloud

* Go to the [Google Cloud Console](https://console.cloud.google.com/).
* Select your existing project linked to the Play Console or create a new one.
* Navigate to **IAM & Admin > Service Accounts**.
* Click **Create service account**.
* Enter a name (e.g., `play-console-uploader`) and description, then click **Create and continue**.
* For the role, choose **Service Account User**, then click **Continue**.
* Skip granting user access and click **Done**.
* Locate your new service account and copy its **email address** — you'll need it in the next step.

#### 3. Generate the JSON Key

* In the same list, click the **Actions (⋮)** menu for your service account just created and select **Manage keys**.
* Click **Add key > Create new key**.
* Select **JSON** and click **Create** — this will download a JSON file to your computer. Keep it secure.

#### 4. Grant Access in Google Play Console

* Open your [Google Play Console](https://play.google.com/console).
* Go to **Users and permissions > Invite new users**.
* Paste the service account’s email.
* Under **App permissions**, select your app.
* Under **Account permissions**, you can either:

    * Select **Admin** for full access
    * Grant only the minimum required permissions:

        * View app information and download bulk reports (read-only)
        * Manage testing tracks and edit tester lists
        * Manage production releases, app signing, and Play Games Services
          * Click **Invite user** to finish.

The service account is now ready to interact with the Play Console API.

### Step 2: Store the JSON key

Once downloaded, place the `.json` file at the following location in your project:

```
androidApp/google-play-uploader.json
```

### Step 3: Publish from your machine (Optional)

To publish a release update manually from your local machine:

```bash
./gradlew :androidApp:publishReleaseBundle
```

### Step 4: Configure CI/CD

If you want to publish automatically via GitHub Actions:

1. Encode the `.json` key as base64:

    ```bash
    base64 -i androidApp/google-play-uploader.json | pbcopy
    ```

2. Go to your GitHub repository settings → **Secrets and variables** → **Actions**.
3. Create a new **repository secret**:

* **Name**: `GOOGLE_PLAY_UPLOADER_JSON_KEY_B64`
* **Value**: (Paste the base64 string you copied)

This will be used by your CI/CD pipeline to deploy the app update automatically.

## iOS (App Store Connect)

---

!!! success "Store publishing setup complete"

    You have successfully set up the automated publishing process for your app to the Google Play Store and App Store Connect. You can now push updates directly from your CI/CD pipeline or manually from your local machine.
