# Configure the app

This guide will walk you through the configuration steps to make the KMPShip boilerplate your own.

---

## App's name and package

To get started with your KMPShip project, you'll need to configure the app's name and package. This is a crucial step to ensure your app has a unique identity in the app stores and on users' devices.

!!! warning "Important"

    Make sure to commit your changes before running the rename task. This task will modify multiple files, and it's best to have a backup in case you need to revert.

This is done by running the `renameProject` Gradle task, which will automatically handle the necessary changes across your project files.

The task has the following parameters:

* `projectName` (**required**): The new name for your project (e.g. `MyApp`).
* `packageName` (**required**): The new base package (e.g. `com.example.myapp`).
* `dryRun` (**optional**): If present, performs a dry run to preview changes without applying them.

<h4>Example Usage</h4>

```bash
./gradlew renameProject -PprojectName=MyApp -PpackageName=com.example.myapp -PdryRun
```

Remove `-PdryRun` when you're ready to apply the changes.

---

After running the task with the `dryRun` parameter, you should see the message `Task completed successfully!` in the terminal.

Go back to **Android Studio** and click on **"Sync Project with Gradle Files"** (the elephant icon) to ensure all changes are fully applied.

---

