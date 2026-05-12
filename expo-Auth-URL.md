Compacted conversationThe issue arises because the redirect URI `exp://192.168.1.92:8081` is not a valid public domain, and Google Cloud Console requires redirect URIs to use a public top-level domain (e.g., `.com`, `.org`). This is a common issue when using Expo Go for development.

### Solution
Expo Go uses a development proxy with the `exp://` scheme, which is not directly supported by Google OAuth. To resolve this, you need to use Expo's **proxy redirect URI** for Google OAuth, which is `https://auth.expo.io/@your-username/your-app-slug`.

#### Steps to Fix:
1. **Find Your Expo Redirect URI**:
   - Replace `your-username` with your Expo account username.
   - Replace `your-app-slug` with the slug of your Expo project (defined in `app.json` or `app.config.js`).

   Example:
   ```
   https://auth.expo.io/@ahmedalsalih/gymbody-app
   ```

2. **Add the Expo Redirect URI to Google Cloud Console**:
   - Go to the **Google Cloud Console**.
   - Navigate to **APIs & Services > Credentials**.
   - Select your OAuth 2.0 client ID.
   - Add the Expo redirect URI (`https://auth.expo.io/@your-username/your-app-slug`) to the **Authorized redirect URIs**.

3. **Update Your Code**:
   - Update the `redirectUri` in your Google sign-in configuration to use the Expo redirect URI.

Would you like me to update your code to use the correct redirect URI, or guide you through any of these steps?
