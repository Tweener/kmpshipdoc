# Get started

<h4>Hey devs, welcome to KMPShip 👋</h4>

This guide will help you set up your environment and get KMPShip running on your machine in just a few steps.

## Requirements

Before running KMPShip, make sure your local environment is ready for Kotlin Multiplatform development.

The easiest way to check your setup is to use [**KDoctor**](https://github.com/Kotlin/kdoctor).

### Run KDoctor

Install KDoctor (if not already installed) and in your terminal, run:

```bash
kdoctor
```

You should get a ✅ All systems are go message. If anything is missing (like Xcode or Android Studio), KDoctor will let you know what to install.

---

## Get the Code

Follow these steps to get KMPShip set up locally:

### Clone the repository

```bash
git clone git@github.com:TweenerLabs/kmp-ship.git [YOUR_APP_NAME]
```

Replace `[YOUR_APP_NAME]` with the desired name for your project folder.

---

### Run the project

Open the project in **Android Studio** and run the app using the provided run configurations:

* To run the **Android** app, start the `androidApp` run configuration.
* To run the **iOS** app, start the `iosApp` run configuration.

Then, open the Xcode workspace located at `iosApp/iosApp.xcworkspace` (not the `.xcodeproj` file), select the iosApp target, and run it on a simulator or device to ensure everything works correctly in Xcode too.

---

Once you've completed these steps, you're ready to dive into KMPShip and start building!
