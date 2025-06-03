# Create a Firebase Project

To enable features like authentication, analytics, or remote config in your app, you’ll need to create a Firebase project and link both your Android and iOS apps to it.

## Steps to get started

1. **Go to the Firebase Console**  
   Visit [https://console.firebase.google.com](https://console.firebase.google.com) and sign in with your Google account.

2. **Create a new project**  
   Click “Add project” and follow the steps. You can skip Google Analytics setup if you don’t need it.

## Add your Android app

1. In your new Firebase project, click “Add app” and choose the Android icon.
2. Enter your app’s package name (e.g. `com.example.myapp`) — this must match your actual Android package.
3. Download the `google-services.json` file and place it in:
   ```
   androidApp/app/google-services.json
   ```
   This will replace the placeholder file already in that location.

## Add your iOS app

1. Still in the Firebase Console, click “Add app” and choose the iOS icon.
2. Enter your iOS app’s **bundle ID** (e.g. `com.example.myapp`) — this must match exactly.
3. Download the `GoogleService-Info.plist` file and place it in:
   ```
   iosApp/GoogleService-Info.plist
   ```
   This will replace the placeholder file already included in the project.

## Need help?

You can refer to the official guide for detailed steps:  
👉 [Firebase: Add Firebase to your project](https://firebase.google.com/docs/projects/learn-more)

---

!!! success "Firebase project created successfully"

    You have successfully created a Firebase project and linked both your Android and iOS apps. You can start using Firebase services in your shared code.
