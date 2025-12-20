# Payments <small>(Scale Plan)</small>

!!! info "Scale Plan required"

    This feature is only available in the Scale Plan.

KMPShip uses [RevenueCat](https://www.revenuecat.com/) to manage in-app purchases and subscriptions across platforms. Before using the payments feature, a few steps are required to configure your
store accounts and RevenueCat dashboard.

---

## 1. Create in-app purchases & subscriptions

You must create your in-app purchases and subscriptions in both stores:

### App Store Connect (iOS)

* Create an App ID and enable in-app purchases.
* Set up your subscriptions and pricing.
* Official documentation: [Apple Store Connect - Create and configure products](https://www.revenuecat.com/docs/getting-started/entitlements/ios-products)

### Google Play Console (Android)

* Create a new product (subscription or one-time purchase).
* Assign pricing and billing period.
* Official documentation: [Google Play - Create and configure products](https://www.revenuecat.com/docs/getting-started/entitlements/android-products)

---

## 2. Configure RevenueCat

RevenueCat acts as a unified backend for both platforms. Here’s what you need to do:

1. Sign up at [RevenueCat](https://app.revenuecat.com/) and create a new project.
2. Add your app identifiers (ie. `com.example.myapp`) for iOS and Android.
3. Create offerings and link your store products (by using their product IDs).
4. Copy your API keys (one for Android and one for iOS).

Official setup guide: [RevenueCat - Getting Started](https://www.revenuecat.com/docs/getting-started)

---

## 3. Add API Keys to `local.properties`

Add your RevenueCat API keys to the `local.properties` file:

```properties
REVENUECAT_ANDROID_API_KEY=your_android_api_key_here
REVENUECAT_IOS_API_KEY=your_ios_api_key_here
```

These values will be injected at build time and used by the shared code to initialize the RevenueCat SDK.

---

## 4. Add In-App Purchase Capability <small>(iOS only)</small>

In Xcode:

* Select your project target.
* Go to **Signing & Capabilities**.
* Click the “+ Capability” button.
* Add **In-App Purchase**.

---

!!! success "Payments setup complete"

    You have successfully configured payments for both Android and iOS using RevenueCat. Your app is now ready to offer and manage in-app purchases and subscriptions.

---

## 5. Use RevenueCat in your code

To abstract RevenueCat and isolate business logic, KMPShip provides a set of use cases located at:

```
com.example.app.domain.usecase.payment
```

These use cases are meant to be injected into your ViewModel and used directly in your business logic.

* `GetPaywallUseCase`: Fetches the current paywall configuration including available products and pricing.
* `GetRedeemUrlUseCase`: Returns a URL the user can open to redeem a purchase or subscription offer (e.g., after a promo or discount).
* `PurchaseProductUseCase`: Starts the purchase flow for a given product ID.
* `RestorePurchaseUseCase`: Triggers restoration of previously purchased products.
* `HasUserActiveSubscriptionUseCase`: Checks if the user currently has an active subscription.
* `GetCurrentActiveSubscriptionUseCase`: Returns the details of the currently active subscription, if any.
* `WasUserPreviouslySubscribedUseCase`: Checks if the user had an active subscription at any point in the past.
* `GetSubscriptionsDiscountsUseCase`: Fetches available subscription discounts or introductory offers.

This clean separation makes your code easier to maintain and test, while ensuring platform logic remains hidden within the repository and SDK layers.
