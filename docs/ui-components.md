# UI components

KMPShip comes with a set of reusable UI components powered by the [Czan Design System](https://czan.dev/), such as buttons, cards, text fields, images and more.

These components help you build consistent, modern interfaces with minimal effort, for both Android and iOS using [Compose Multiplatform](https://www.jetbrains.com/compose-multiplatform/).

## Explore components

You can explore the full component library, usage examples, and API reference directly on the Czan website:

👉 [https://czan.dev/](https://czan.dev/)

## How to Use

All components are already integrated into the project and ready to use in your UI code:

```kotlin
import com.tweener.czan.designsystem.atom.button.Button

@Composable
fun MyScreen() {
    Column {
        Button(
            modifier = Modifier.fillMaxWidth(),
            text = stringResource(resource = Res.string.hello_world),
            size = ButtonSize.BIG,
            style = ButtonStyle.PRIMARY,
            enabled = true,
            loading = false,
            onClick = { /* Handle button click */ },
        )
    }
}
```

You’re free to customize components or build your own, but using the Czan system helps maintain design consistency throughout your app.
