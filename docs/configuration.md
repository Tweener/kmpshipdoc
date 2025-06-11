# Configuration

Before you dive into using KMPShip’s features, there are a few essential configuration steps to complete. These ensure that everything is set up correctly so you can enable services like authentication, analytics, and notifications later on.

This configuration is required and should be done **right after setup**, and **before attempting to use any feature module**.

---

## Create Developer Accounts

!!! note

    You can skip this step if you already have developer accounts set up for your Android and iOS apps.

To publish your app, you’ll need accounts on the Google Play Console and App Store Connect. These are required for signing, uploading builds, and configuring store metadata.

If you haven’t done this yet, follow these quick guides:

- [Google Play Developer Account](tutorials/stores/google-play-developer-account.md)
- [Apple Developer Account (App Store Connect)](tutorials/stores/app-store-connect-account.md)

---

## Create your app on the stores

After creating your developer accounts, you need to create your app on both the Google Play Console and App Store Connect. This is necessary for configuring store metadata, uploading builds, and managing releases.

Follow these guides to create your app on each platform:

- [Create your app on Google Play Console](https://support.google.com/googleplay/android-developer/answer/9859152)
- [Create your app on App Store Connect](https://developer.apple.com/help/app-store-connect/create-an-app-record/add-a-new-app/)

---

## Create a Firebase Project

KMPShip relies on Firebase for services like authentication, remote config, and analytics. You’ll need to create a Firebase project and connect both your Android and iOS apps.

Follow this step-by-step guide:  
👉 [Create a Firebase Project](tutorials/create-firebase-project.md)

---

!!! success "Next Steps"

    Now that you have completed the initial configuration, you can proceed to set up specific features like authentication, notifications, and analytics.  
    Check out the [Features](features/index.md) section for more details on each feature and how to implement them in your app.
