# Navigation

KMPShip uses [Voyager](https://github.com/adrielcafe/voyager) as its multiplatform navigation library.

---

## App entry point

The main entry point for navigation logic is the `App` composable.

* On **Android**, it is called from `MainActivity`
* On **iOS**, it is launched via `MainViewController`

`App` sets up the root screen of the app: `MainScreen`.

---

## Root screen: `MainScreen`

`MainScreen` is the starting point of the app. It handles logic when the app is freshly launched, or a notification is clicked, etc.

By default, `MainScreen` displays a bottom navigation bar with 3 tabs:

* `HomeTab` → `HomeScreen`
* `FavoritesTab` → `FavoritesScreen`
* `ProfileTab` → `ProfileScreen`

You can freely **add**, **remove**, or **rename** tabs to match your own application needs.

Each tab is linked to a corresponding screen, and each screen has its own ViewModel to manage state and logic.

---

## Screen definition and structure

All screens are defined in the following package:

```
com.tweener.kmpship.presentation.screen
```

![Presentation package hierarchy](/assets/images/presentation-packages.png)


For better organization, each screen is split into 4 subpackages:

### `di`

Contains a Koin module that declares all the dependencies needed for the screen.

### `mapper`

Handles mapping between domain entities and UI models.

### `model`

Defines the UI-specific models used by this screen.

### `ui`

Contains the entire UI hierarchy for the screen. This includes:

* **component**: All reusable UI components that can be composed to build templates.
* **template**: The layout skeleton of the screen composed by components. It has no logic and no reference to a ViewModel. It is purely UI.
* **screen** (also known as **page**): The entry point of the screen. It connects the `ViewModel` with the `template` and serves as the bridge between logic and UI.

---

## ViewModels

Each `*Screen` is backed by a `*ViewModel`. For example:

* `HomeScreen` ↔ `HomeViewModel`
* `FavoritesScreen` ↔ `FavoritesViewModel`

ViewModels serve as the link between the `domain` and `presentation` layers. They expose the current UI state, handle user interactions, and trigger use cases.

---

## Navigating Between Screens

To navigate to a new screen, use the `push` function from Voyager’s navigator. This allows you to add a new screen to the navigation stack.

### Example Usage

In most cases, you'll use the root navigator, like this:

```kotlin
navigator.push(NewScreen)
```

### Real Example from `HomeScreen`

```kotlin
@Composable
fun HomeScreen() {
    val viewModel: HomeViewModel = koinInject()
    val navigator = LocalNavigator.currentOrThrow.findRootNavigator()

    viewModel.openDetailScreen.subscribe { id ->
        navigator.push(item = DetailScreen(id = id))
    }
}
```

This setup allows your ViewModel to emit navigation events which are handled by the screen using `subscribe {}`.

---

With this structure, KMPShip gives you a modular and scalable foundation to manage navigation across your app.
