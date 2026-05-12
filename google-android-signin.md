To get the `EXPO_PUBLIC_GOOGLE_ANDROID_CLIENT_ID`, follow these steps:

### 1. **Go to Google Cloud Console**
   - Open [Google Cloud Console](https://console.cloud.google.com/).

### 2. **Select Your Project**
   - Ensure you're in the project where you set up the OAuth consent screen and created the credentials for your app.

### 3. **Navigate to Credentials**
   - In the left-hand menu, go to **APIs & Services > Credentials**.

### 4. **Create or Locate Android OAuth Client**
   - If you already have an Android OAuth client:
     - Look for a credential with the **type** "OAuth 2.0 Client IDs" and **name** indicating Android.
     - Click on it to view details.
   - If you don't have one:
     - Click **Create Credentials > OAuth Client ID**.
     - Select **Android** as the application type.
     - Enter your app's **package name** (e.g., `com.troam.gymbody`) and **SHA-1 certificate fingerprint**.

### 5. **Get the SHA-1 Fingerprint**
   - Run the following command in your terminal to generate the SHA-1 for your debug keystore:
     ```bash
     keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
     ```
   - Copy the SHA-1 fingerprint from the output.

### 6. **Save and Copy the Client ID**
   - After creating the Android OAuth client, copy the **Client ID** from the details page.

### 7. **Add to .env**
   - Paste the client ID into your .env file:
     ```dotenv
     EXPO_PUBLIC_GOOGLE_ANDROID_CLIENT_ID=your-client-id-here
     ```

Let me know if you need help with any of these steps!
