# Authentication <small>(Scale Plan)</small>

KMPShip includes built-in support for Firebase Authentication using [:simple-github: Passage](https://github.com/Tweener/passage/), allowing you to easily implement user authentication in your app.

!!! info "Scale Plan required"

    This feature is only available in the Scale Plan.

---

## Prerequisite

### Create a Firebase Project

!!! note

    You can skip this step if you already have a Firebase project set up. 

Before configuring authentication, you must have a Firebase project already set up.

If you haven't done this yet, follow the linked guide and connect both your Android and iOS apps before continuing:

👉 [Create a Firebase Project](../tutorials/create-firebase-project.md)

### Activate Firestore Database

!!! note

    You can skip this step if you already activated Firestore Database.

To use KMPShip's authentication features, you need to activate the Firestore Database in your Firebase project.

If you haven't done this yet, follow this step-by-step guide:

👉 [Activate Firestore Database](../tutorials/activate-firestore-database.md)

---

## Supported providers

KMPShip supports the following authentication providers out of the box:

- [Google Sign In](#google-sign-in)
- [Apple Sign In](#apple-sign-in-ios-only)
- [Email & Password](#email-password)

None of these providers are mandatory — you can enable only the ones you need.

---

## Google Sign In

1. In the [Firebase Console](https://console.firebase.google.com/), go to **Authentication > Sign-in method**
2. Enable **Google** as a provider.
3. Copy the **Web client ID** provided by Firebase.

### Android setup

Store the Web client ID in your `local.properties` file like this:

```properties
GOOGLE_SIGN_IN_WEB_CLIENT_ID={Web client ID provided by Firebase}
```

This value will be injected automatically into your Android build.

### iOS setup

1. Open the `GoogleService-Info.plist` file downloaded from Firebase.
2. Copy the values of the following keys:
     - `CLIENT_ID`
     - `REVERSED_CLIENT_ID`

3. Open your app’s `Info.plist` file and add the following keys:

```xml
<key>GIDClientID</key>
<string>YOUR_GOOGLE_SIGN_IN_WEB_CLIENT_ID</string>

<key>CFBundleURLTypes</key>
<array>
  <dict>
    <key>CFBundleURLSchemes</key>
    <array>
      <string>YOUR_REVERSED_GOOGLE_SIGN_IN_WEB_CLIENT_ID</string>
    </array>
  </dict>
</array>
```

Replace:

- `YOUR_GOOGLE_SIGN_IN_WEB_CLIENT_ID` with the `CLIENT_ID` value
- `YOUR_REVERSED_GOOGLE_SIGN_IN_WEB_CLIENT_ID` with the `REVERSED_CLIENT_ID` value

---

## Apple Sign-In <small>(iOS only)</small>

To support **Sign in with Apple** in your iOS app using Firebase Authentication, follow these steps carefully:

### Step 1: Create a Service ID in Apple Developer Portal

1. Visit [Service IDs](https://developer.apple.com/account/resources/identifiers/list/serviceId) and click the **"+"** button.
2. Ensure **Service IDs** is selected, then click **Continue**.
3. Fill out the form:

    * **Description**: `MyProject Apple Sign In` (replace `MyProject` with your actual project name)
    * **Identifier**: `com.example.myapp.apple.signin` (replace `com.example.myapp` with your app's package name)
4. Click **Continue**, then on the next screen:

    * Enable **Sign In with Apple**
    * Click **Configure**
    * In **Primary App ID**, select your app’s registered App ID (the one tied to your bundle ID)
    * Click **Save**
    * Under **Domains and Subdomains**, add your website domain (e.g., `kmpship.app`)
    * Under **Return URLs**, enter:

      ```
      https://YOUR_FIREBASE_PROJECT_ID.firebaseapp.com/__/auth/handler
      ```

      *(Replace `YOUR_FIREBASE_PROJECT_ID` with your actual Firebase project ID)*
   
    * Click **Next**, then **Done**
    * Finally, click **Continue**, then **Save**

### Step 2: Create a Private Key for Apple Sign-In

1. Visit [Keys](https://developer.apple.com/account/resources/authkeys/list) and click the **"+"** button.
2. Enter a name for your key (e.g., `MyProject Apple Auth Key`)
3. Enable **Sign in with Apple** and click **Configure**.
4. In the **Primary App ID** dropdown, select the same App ID you chose earlier.
5. Click **Save**, then **Continue**.
6. Click **Register**, then **Download** the `.p8` private key.
7. **Save this file securely** — you won't be able to download it again.

### Step 3: Enable and configure Apple provider in Firebase

1. In the [Firebase Console](https://console.firebase.google.com/), go to **Authentication > Sign-in method**
2. Enable **Apple** as a provider.
3. Fill out the following fields using the values from previous steps:

    * **Services ID**: the Service ID you created in Step 1
    * **Apple Team ID**: your Apple Team ID (found at [Apple Developer > Account](https://developer.apple.com/account) → **Membership Details**)
    * **Key ID**: the ID from your newly created key in Step 2
    * **Private key**: paste the content of the downloaded `.p8` file
4. Click **Save**

### Step 4: Enable Sign in with Apple in Xcode

1. Open your project in Xcode.
2. Select your app target > **Signing & Capabilities**.
3. Click **“+ Capability”**, then add **Sign In with Apple**.
4. Ensure the **bundle identifier** matches the one used in your Apple Developer configuration.

## Email & Password

1. In the [Firebase Console](https://console.firebase.google.com/), go to **Authentication > Sign-in method**
2. Enable **Email/Password** as a provider.

Once enabled, KMPShip provides:

- A built-in **Sign up** and **Sign in** form for email/password users.
- Support for **password reset**: users can receive a password recovery email and reset their password directly in the app.
- Support for **email verification**: users can verify their email address after signing up.
- Firebase error handling for common issues (e.g., invalid credentials, user not found).

---

!!! success "Authentication setup complete"

    You have successfully configured authentication in your KMPShip app. Users can now sign in using Google, Apple, or email/password methods.
