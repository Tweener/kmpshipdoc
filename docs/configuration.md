# Configuration

Before you dive into using KMPShip’s features, there are a few essential configuration steps to complete. These ensure that everything is set up correctly so you can enable services like authentication, analytics, and notifications later on.

This configuration is required and should be done **right after setup**, and **before attempting to use any feature module**.

## Create Developer Accounts

!!! note

    You can skip this step if you already have developer accounts set up for your Android and iOS apps.

To publish your app, you’ll need accounts on the Google Play Console and App Store Connect. These are required for signing, uploading builds, and configuring store metadata.

If you haven’t done this yet, follow these quick guides:

- [Google Play Developer Account](tutorials/stores/google-play-developer-account.md)
- [Apple Developer Account (App Store Connect)](tutorials/stores/app-store-connect-account.md)

## Set up a Firebase Project

KMPShip relies on Firebase for services like authentication, remote config, and analytics. You’ll need to create a Firebase project and connect both your Android and iOS apps.

Follow this step-by-step guide:  
👉 [Create a Firebase Project](tutorials/create-firebase-project.md)
