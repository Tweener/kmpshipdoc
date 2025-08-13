# Activate Firestore Database

To enable database features in your app using Firebase, you need to activate **Cloud Firestore**. Follow these steps to set it up properly.

---

## Step 1: Enable the Firestore API

1. Go to the [Google Cloud Firestore API](https://console.cloud.google.com/apis/api/firestore.googleapis.com).
2. Select your Firebase project from the dropdown at the top.
3. Click on the **Enable** button to activate the Firestore API for your project.

---

## Step 2: Create the Firestore Database

1. Go to [Firebase Console](https://console.firebase.google.com/).
2. Select your Firebase project.
3. Navigate to **Build > Firestore Database** in the left-hand menu.
4. Click **Create database**.
5. Select the Firestore region closest to your target users.
6. Choose the **Production mode** (recommended for live apps).
7. Click **Enable** to complete the setup.

---

## Step 3: Set Firestore security rules

After creating the database:

1. Open the **Rules** tab.
2. Replace the existing rules with the following:

```javascript
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{uid} {
      allow read, write: if request.auth.uid == uid;
    }
  }
}
```

These rules ensure that each authenticated user can only read and write to their own document in the `users` collection.

---

## Step 4: First document creation

You don’t need to manually create the `users` collection.
It will be **automatically created** after the first successful account creation in your app.

Once this happens, you'll see the `users` collection in the Firestore console.
