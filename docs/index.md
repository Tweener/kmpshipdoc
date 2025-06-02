# Get started

<h4>Hey devs, welcome to KMPShip 👋</h4>

This guide will help you set up your environment and get KMPShip running on your machine in just a few steps.

## Requirements

Before running KMPShip, make sure your local environment is ready for Kotlin Multiplatform development. The easiest way to check your setup is to use [**KDoctor**](https://github.com/Kotlin/kdoctor).

### Run KDoctor

Install KDoctor (if not already installed) and in your terminal, run:

```bash
kdoctor
```

You should get a <span class="text-green">[✓]</span> on all diagnostics and the following message if no issues are found: `Your operation system is ready for Kotlin Multiplatform Mobile Development!`

If anything is missing (like Xcode or Android Studio), KDoctor will let you know what to install.

## Get the Code

Follow these steps to get KMPShip set up locally:

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

## Run the project

Open the project in **Android Studio** and run the app using the provided run configurations:

* To run the **Android** app, start the `androidApp` run configuration.
* To run the **iOS** app, start the `iosApp` run configuration.

Then, to make sure it also works on Xcode, open the Xcode workspace located at `iosApp/iosApp.xcodeproj`, select the iosApp target, and run it on a simulator or device to ensure everything works correctly in Xcode too.

---

Congratulations! 👏 The initial setup is done and you already have your app up and running on Android and iOS.
