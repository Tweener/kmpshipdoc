# Get started

<h4>Hey devs, welcome to KMPShip 👋</h4>

This guide will help you get the code and set up KMPShip on your machine in just a few steps.

---

## Get the code

Follow these steps to get KMPShip set up locally.

### Clone the repository

```bash
git clone git@github.com:TweenerLabs/kmp-ship.git [YOUR_APP_NAME]
cd [YOUR_APP_NAME]
```

Replace `[YOUR_APP_NAME]` with the desired name for your project folder.

### Configure Git remotes <small>(Complete Plan)</small>

!!! info "Complete plan required"

    This step is only required for Complete Plan users. If you're using the Starter Plan, you can skip this section.

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

### Keep your project up to date <small>(Complete Plan)</small>

!!! info "Complete plan required"

    This step is only required for Complete Plan users. If you're using the Starter Plan, you can skip this section.

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

## Configure local properties

Before running the setup script, set up your local configuration file:

1. Rename the example file to create your local properties:

```bash
mv local.properties.example local.properties
```

This file contains all the environment variables you can configure for your project, including signing keys, API credentials, and other secrets needed for builds and releases.

!!! tip "Keep it secure"

    The `local.properties` file is already included in `.gitignore` to prevent committing sensitive credentials to your repository.

---

## Run the setup script

Now that you have the code and Firebase configured, run the setup script to configure your project.

!!! warning "Important"

    Make sure to commit your changes before running the setup script. This script will modify multiple files, and it's best to have a backup in case you need to revert.

1. Make the script executable (first time only):

```bash
chmod +x scripts/setup.sh
```

2. Run the script and follow the interactive prompts:

```bash
./scripts/setup.sh
```

The script will:

- Check your development environment (installs and runs kdoctor to verify Xcode, Android Studio, etc.)
- Ask if you want to run in **preview mode** (dry-run) to see changes without applying them
- Guide you through configuring your project name and package name
- Optionally rename package structure in source code (your choice during setup)

After the script completes, go back to **Android Studio** and click on **"Sync Project with Gradle Files"** (the elephant icon) to ensure all changes are fully applied.

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
