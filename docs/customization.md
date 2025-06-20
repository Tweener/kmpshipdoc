# Customize your app

## Give your app a name

To ensure your app has a unique identity in the app stores and on users' devices, you’ll want to give it a name and set a custom package/bundle ID.

This is done by running the `renameProject` Gradle task, which will automatically handle the necessary changes across your project files.

!!! warning "Important"

    Make sure to commit your changes before running the rename task. This task will modify multiple files, and it's best to have a backup in case you need to revert.

The task has the following parameters:

* `projectName` (**required**): The new name for your project (e.g. `MyApp`).
* `packageName` (**required**): The new base package (e.g. `com.example.myapp`).
* `dryRun` (**optional**): If present, performs a dry run to preview changes without applying them.

<h4>Example Usage</h4>

```bash
./gradlew renameProject -PprojectName=MyApp -PpackageName=com.example.myapp -PdryRun
```

Remove `-PdryRun` when you're ready to apply the changes and run the task again, you should see the message `Task completed successfully!` in the terminal.

Go back to **Android Studio** and click on **"Sync Project with Gradle Files"** (the elephant icon) to ensure all changes are fully applied.

---

## Theme

Check out [Czan - Theme cuztomization](https://www.czan.dev/setup/#theme-customization) documentation to learn how to customize your theme, including colors, typography, shapes, dark mode, and more.

--- 

## App icon

Updating the app icon to reflect your brand identity is one of the first things you'll want to do.

### Android

The easiest way to change the app icon on Android is by using **Image Asset Studio** in Android Studio.

**Steps:**

1. Open **Android Studio**.
2. In the **Project view**, right-click the `androidApp` folder.
3. Select **New > Image Asset**.
4. In the **Asset Type** dropdown, choose either:
    * **Image** (for a PNG file), or
    * **Vector** (for an SVG file).
5. Upload your app icon and configure the foreground and background layers.
6. Optionally customize padding, scaling, and shape.
7. Click **Next**, then **Finish**.
   This will automatically generate icons in all required `mipmap-` folders.

8. Your app icon is referenced in `AndroidManifest.xml`:
   ```xml title="AndroidManifest.xml"
   <application
       ...   
       android:icon="@mipmap/ic_launcher"
       ...
   >
   ```

![Android Studio Image Asset](/assets/images/android-studio-image-asset.png)

---

### iOS

iOS uses an asset catalog (`Assets.xcassets`) to manage app icons.

**Steps:**

1. Prepare your icon assets in all required sizes. You can generate them using a tool like:

    * [MakeAppIcon](https://makeappicon.com/)
    * [appicon.co](https://appicon.co)

2. Open the iOS project in Xcode (`iosApp/iosApp.xcodeproj`).

3. In the **Project Navigator**, expand `iosApp > Assets.xcassets > AppIcon`.

4. Drag and drop the generated icon images into the correct slots in the AppIcon set.

5. Your app icon is referenced in your target’s **Build Settings** > **Asset Catalog Compiler - Options** > **Primary App Icon Set Name**.

![Xcode App Icon](/assets/images/xcode-app-icon.png)
