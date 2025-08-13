# Get started

<h4>Hey devs, welcome to KMPShip 👋</h4>

This guide will help you set up your environment and get KMPShip running on your machine in just a few steps.

---

## Requirements

Before running KMPShip, make sure your local environment is ready for Kotlin Multiplatform development. The easiest way to check your setup is to use [**KDoctor**](https://github.com/Kotlin/kdoctor).

### Run KDoctor

Install KDoctor (if not already installed) and in your terminal, run:

```bash
kdoctor
```

You should get a <span class="text-green">[✓]</span> on all diagnostics and the following message if no issues are found: `Your operation system is ready for Kotlin Multiplatform Mobile Development!`

If anything is missing (like Xcode or Android Studio), KDoctor will let you know what to install.

---

## Get the code

Follow these steps to get KMPShip set up locally.

### Clone the repository

```bash
git clone git@github.com:TweenerLabs/kmp-ship.git [YOUR_APP_NAME]
cd [YOUR_APP_NAME]
```

Replace `[YOUR_APP_NAME]` with the desired name for your project folder.

### Configure Git remotes <small>(Pro Plan)</small>

!!! info "Pro plan required"

    This step is only required for Pro Plan users. If you're using the Free Plan, you can skip this section.

To set up your project with Git, you need to configure the remote repositories. This allows you to push your changes to your own repository and keep track of updates from the original KMPShip repository.

- First, rename the default Git remote to `upstream`:

```bash
git remote rename origin upstream
```

- Now, add your own repository as the new `origin`:

```bash
git remote add origin [YOUR_SSH_REMOTE_REPO_URL]
```

Replace `[YOUR_SSH_REMOTE_REPO_URL]` with the SSH URL of your own GitHub repository (ie. `git@github.com:Org/my-repo.git`).

- Remove the upstream tracking information:

```bash
git branch --unset-upstream
```

- Finally, push the project to your own repository:

````bash
git push -u origin main
````

### Keep your project up to date <small>(Pro Plan)</small>

!!! info "Pro plan required"

    This step is only required for Pro Plan users. If you're using the Free Plan, you can skip this section.

To ensure your project stays up to date with the latest changes from the original KMPShip repository, you can regularly pull updates from the `upstream` remote:

```bash
git fetch upstream
git merge upstream/main
```

!!! tip

    Make sure to **regularly keep** the `main` branch in sync with the original KMPShip repository. This way, you can easily pull updates and new features as they are released.

---

## Create a Firebase Project

KMPShip relies on Firebase for services like authentication, remote config, and analytics. You’ll need to create a Firebase project and connect both your Android and iOS apps.

Follow this step-by-step guide:  
👉 [Create a Firebase Project](tutorials/create-firebase-project.md)

---

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

## Run the project

Open the project in **Android Studio** and run the app using the provided run configurations:

* To run the **Android** app, start the `androidApp` run configuration.
* To run the **iOS** app, start the `iosApp` run configuration.

Then, to make sure it also works on Xcode, open the Xcode workspace located at `iosApp/iosApp.xcodeproj`, select the iosApp target, and run it on a simulator or device to ensure everything works correctly in Xcode too.

---

!!! success "Next Steps"

    Congratulations! 👏 The initial setup is done and you already have your app up and running on Android and iOS.
    Now, you can proceed with your app [customization](/customization/).
