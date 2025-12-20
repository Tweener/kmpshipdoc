# Notifications <small>(MVP Plan)</small>

!!! info "MVP Plan required"

    This feature is available starting from the MVP Plan.

KMPShip supports both **local** and **push notifications** using [:simple-github: Alarmee](https://github.com/Tweener/alarmee).
This powerful abstraction handles platform differences and gives developers full control over how and when to notify users.

---

## Prerequisite: Create a Firebase Project

!!! note

    You can skip this step if you already have a Firebase project set up. 

Before configuring notifications, you must have a Firebase project already set up.

If you haven't done this yet, follow the linked guide and connect both your Android and iOS apps before continuing:

👉 [Create a Firebase Project](../tutorials/create-firebase-project.md)

---

## Local notifications

You can schedule, cancel, or immediately send local notifications across both Android and iOS.

### Android configuration

Before scheduling any local notification on Android, you need to define **notification channels** and customize the notifications. This is done in the following file:

```
com.tweener.kmpship.presentation._internal.libs.alarmee.AlarmeeConfiguration.android.kt
```

In that file, add your custom channels to the `notificationChannels` list:

```kotlin
actual fun createAlarmeePlatformConfiguration(): AlarmeePlatformConfiguration =
    notificationIconResId = R.drawable.ic_notification,
notificationIconColor = androidx.compose.ui.graphics.Color.Red, // Defaults to Color.Transparent is not specified
notificationChannels = listOf(
    AlarmeeNotificationChannel(
        id = "dailyNewsChannelId",
        name = "Daily news notifications",
        importance = NotificationManager.IMPORTANCE_HIGH,
        soundFilename = "notifications_sound",
    ),
    AlarmeeNotificationChannel(
        id = "breakingNewsChannelId",
        name = "Breaking news notifications",
        importance = NotificationManager.IMPORTANCE_LOW,
    ),
    // List all the notification channels you need here
)
```

### iOS configuration

You don't need to do anything special to configure local notifications on iOS, as KMPShip already includes the necessary setup.

### Scheduling a local notification

Once configured, you can schedule a local notification using the `MobileAlarmeeService`. For example:

```kotlin
val alarmeeService: MobileAlarmeeService = koinInject()

alarmeeService.local.schedule(
    alarmee = Alarmee(
        uuid = "myOneOffAlarmId",
        notificationTitle = "🎉 Congratulations! You've schedule a one-off Alarmee!",
        notificationBody = "This is the notification that will be displayed 10 seconds from now.",
        scheduledDateTime = LocalDateTime.now().plus(10.seconds),
        androidNotificationConfiguration = AndroidNotificationConfiguration(
            priority = AndroidNotificationPriority.DEFAULT,
            channelId = "dailyNewsChannelId",
        ),
        iosNotificationConfiguration = IosNotificationConfiguration(),
    )
)
```

### Cancel a scheduled notification

To cancel a scheduled local notification, you can use the `cancel` function, passing the Alarmee's `uuid`:

```kotlin
alarmeeService.local.cancel(uuid = "myAlarmId")
```

### Send an immediate notification

You can also send a notification immediately without scheduling it. This is useful for urgent messages or alerts:

```kotlin
alarmeeService.local.immediate(
    alarmee = Alarmee(
        uuid = "myAlarmId",
        notificationTitle = "🚀 Congratulations! You've pushed an Alarmee right now!",
        notificationBody = "This notification will be displayed right away",
        androidNotificationConfiguration = AndroidNotificationConfiguration(
            // Required configuration for Android target only (this parameter is ignored on iOS)
            priority = AndroidNotificationPriority.DEFAULT,
            channelId = "immediateChannelId",
        ),
        iosNotificationConfiguration = IosNotificationConfiguration(),
    )
)
```

For more advanced usage and configuration options, please refer to the [Alarmee GitHub repository](https://github.com/Tweener/alarmee).

---

## Push notifications

Push notifications are powered by **Firebase Cloud Messaging (FCM)** and are pre-integrated in KMPShip.

#### Android configuration

You don't need to do anything special to configure push notifications on Android, as KMPShip already includes the necessary setup. However, you must ensure that your Firebase project is correctly
linked to your Android app.

#### iOS configuration

To support push notifications on iOS, you need to configure Apple Push Notification service (APNs) and link it to Firebase:

1. **Generate an APNs Key**:

    * Sign in to your [Apple Developer Account](https://developer.apple.com/account/).
    * Go to **Certificates, Identifiers & Profiles**.
    * Select **Keys** > click the **+** icon to create a new key.
    * Enter a name (e.g., "KMPShip APNs Key") and enable **Apple Push Notifications service (APNs)**.
    * Click **Configure**.
    * Make sure to select **Sandbox & Production** as the environment and click **Save**.
    * Click **Continue** and **Register**.
    * Click **Download** to download the `.p8` file and store it safely — you won't be able to download it again.
    * Note the **Key ID** and your **Team ID** (from your Apple Developer account overview).

2. **Upload APNs Key to Firebase**:

    * Open your [Firebase Console](https://console.firebase.google.com/).
    * Go to your project > **Project Settings** > **Cloud Messaging** tab.
    * Under **Apple app configuration**, click **Upload** next to APNs authentication key.
    * Upload the `.p8` file, and enter the **Key ID** and **Team ID**.
    * Save your changes.

Once this is done, Firebase will be able to send push notifications to your iOS app.

### Receiving push messages

When a push message is received on the device, KMPShip automatically displays a notification using the **title**, **body**, and any extra data from the FCM payload.

### Getting the Firebase token

If you need to retrieve the device’s FCM token (e.g., to send it to your own backend server), you can access it like this:

```kotlin
val scope = rememberCoroutineScope()
val alarmeeService: MobileAlarmeeService = koinInject()

scope.launch {
    val token = alarmeeService.push.getToken().getOrNull()
    println("Firebase current token: $token")
}
```

This token is unique to the device and can be refreshed by Firebase at any time, so you should handle token updates accordingly.

---

!!! success "Notifications setup complete"

    You have successfully configured both local and push notifications in your KMPShip app. Users can now receive timely updates and alerts, enhancing engagement and user experience.
