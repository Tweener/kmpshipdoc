# Authentication

KMPShip includes built-in support for Firebase Authentication using [:simple-github: Passage](https://github.com/Tweener/passage/), allowing you to easily implement user authentication in your app.

---

## Prerequisite: Create a Firebase Project

!!! note

    You can skip this step if you already have a Firebase project set up. 

Before configuring authentication, you must have a Firebase project already set up.

If you haven't done this yet, follow the linked guide and connect both your Android and iOS apps before continuing:

👉 [Create a Firebase Project](../tutorials/create-firebase-project.md)

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

To support Apple Sign-In in your app, you must:

1. In the [Firebase Console](https://console.firebase.google.com/), go to **Authentication > Sign-in method**
2. Enable **Apple** as a provider.
3. Configure Apple Sign-In for iOS:

     1. Go to the [Apple Developer Portal](https://developer.apple.com/account/resources/identifiers/list) and:
          - Create a **Services ID**
          - Create a **Key** with **Sign in with Apple** enabled
          - Link both to your app’s bundle identifier

     2. In Firebase:
          - Add your **Services ID**
          - Upload the **Key ID** and **Private Key**
          - Set your **Team ID** (found in the [Apple Developer Account](https://developer.apple.com/account) settings)

          - In Xcode:
            - Make sure the **Sign In with Apple** capability is enabled in your app’s target under **Signing & Capabilities**
            - Confirm the app’s **bundle identifier** matches the one registered in the Apple Developer Portal

---

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
