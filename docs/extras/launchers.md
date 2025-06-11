# Launchers

KMPShip comes with a set of prebuilt launchers designed to abstract away platform differences when triggering native platform actions.

**Location:** `com.example.myapp.presentation._internal.launcher`.

---

## MobileStoreSubscriptionLauncher

Opens the user’s app store page where they can manage their subscriptions. It handles both platforms automatically:

* **Android**: opens the Google Play Store
* **iOS**: opens the App Store

### Usage

In a screen (for example, `ManageSubscriptionPlanScreen`), inject the launcher and use it like this:

```kotlin
@Composable
fun ManageSubscriptionPlanScreen() {
    // Other UI components...

    val mobileStoreSubscriptionLauncher: MobileStoreSubscriptionLauncher = koinInject()

    viewModel.openMobileStoreSubscriptionScreen.subscribe { packageName ->
        mobileStoreSubscriptionLauncher.open(packageName)
    }
}
```

---

## EmailClientLauncher

Opens the default email client on the user's device. This is useful when prompting the user to check their inbox for a magic link after signing into the app.

### Usage

In a screen (for example, `NeedEmailVerificationScreen`), use the launcher in a `@Composable` like this:

```kotlin
@Composable
fun NeedEmailVerificationScreen() {
    // Other UI components...

    val emailClientLauncher = rememberEmailClientLauncher()
    emailClientLauncher.openEmailClient()
}
```

---

## EmailComposerLauncher

Opens the default email client and creates a new email with pre-filled fields like recipient, subject, and body. This is ideal for providing quick feedback or contact links in your app.

### Usage

In a screen (for example, `SendEmailScreen`), inject the launcher and use it like this:

```kotlin
@Composable
fun SendEmailScreen() {
    // Other UI components...

    val emailComposerLauncher: EmailComposerLauncher = koinInject()
    
    viewModel.sendEmail.subscribe { emailDetails ->
        emailComposerLauncher.composeEmail(recipients = emailDetails.recipients, subject = emailDetails.subject, body = emailDetails.body) 
    }
}
```

---

## InAppReviewLauncher

Opens the native **in-app review** popup so users can rate and review your app from within.

### Usage

In a screen (for example, `HomeScreen`), inject the launcher and use it in a `@Composable` like this:

```kotlin
@Composable
fun HomeScreen() {
    // Other UI components...

    val inAppReviewLauncher: InAppReviewLauncher = koinInject()

    inAppReviewLauncher.bindToView() // Required to bind the launcher to the view lifecycle

    viewModel.requestInAppRewview.subscribe { 
        inAppReviewLauncher.requestInAppReview() 
    }
}
```

!!! info "Important"
    
    The system controls whether the review prompt is shown or not. You can trigger it, but it’s up to the OS to decide if the prompt appears. This ensures users are not overwhelmed with frequent review requests.
