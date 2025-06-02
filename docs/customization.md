# Customization

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

## Customize your theme

Check out [Czan - Theme cuztomization](https://www.czan.dev/setup/#theme-customization) documentation to learn how to customize your theme, including colors, typography, shapes, dark mode, and more.
