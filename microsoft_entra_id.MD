# How to Set Up Microsoft Entra ID (Azure AD) with Auth0 via Self-Service SSO

Follow these steps to configure Microsoft Entra ID (formerly Azure AD) as your Identity Provider for SSO with Auth0 using the self-service setup flow.

---

## 🔧 Step 1 – Register an Application in Microsoft Entra ID

1. Go to the [Azure Portal](https://portal.azure.com) and open **Microsoft Entra ID**.
2. Navigate to **App registrations**, then click **New registration**.
3. Fill in the form:
   - **Name**: Choose a name like "MyCompany SSO".
   - **Supported account types**: Select **Accounts in this organizational directory only**.
   - **Redirect URI**:
     ```
     https://<YOUR-AUTH0-TENANT>.auth0.com/login/callback
     ```
     Replace with your actual Auth0 tenant domain or custom domain.

4. Click **Register**.

---

## 📋 Step 2 – Gather Required App Information

Once the app is registered, go to its **Overview** and copy the following:

- **Application (client) ID** → This will be used as the **Client ID**.
- **Directory (tenant) ID** → Used to build the URLs below.

Then go to **Certificates & secrets**, and:
- Create a new **Client Secret**
- Copy the value (you’ll need it for Auth0)

---

## 🌐 Step 3 – Build the Microsoft Entra URLs

| Field               | Value |
|--------------------|-------|
| **Authorization URL** | `https://login.microsoftonline.com/<TENANT_ID>/oauth2/v2.0/authorize` |
| **Token URL**         | `https://login.microsoftonline.com/<TENANT_ID>/oauth2/v2.0/token` |
| **Issuer URL**        | `https://login.microsoftonline.com/<TENANT_ID>/v2.0` |

Replace `<TENANT_ID>` with the **Directory (tenant) ID** from earlier.

---

## 🔑 Step 4 – Fill Out the Auth0 Self-Service SSO Form

When prompted by the Auth0 self-service SSO setup, fill in the following fields:

| Field             | Value |
|------------------|-------|
| **Client ID**         | Paste the Application (client) ID |
| **Client Secret**     | Paste the secret you just created |
| **Authorization URL** | From Step 3 |
| **Token URL**         | From Step 3 |
| **Issuer URL**        | From Step 3 |
| **Scopes**            | `openid email profile` (must include `openid`) |
| **Connection name**   | Choose a name like `entra-mycompany` *(optional)* |

---

## ✅ Step 5 – Test and Complete the Setup

- Click **Test Connection** in the setup flow.
- You should be redirected to Microsoft to log in.
- After login, you’ll return to the app and the connection will be saved.

---

## ❓ Frequently Asked Questions (FAQ) & Troubleshooting

### 🔹 The test connection fails with a generic error.
- Check that the client ID and secret are correct and don’t contain extra spaces.
- Make sure the redirect URI in Entra ID exactly matches what Auth0 uses (`/login/callback`).
- Confirm the tenant ID is correct in all URLs.

### 🔹 I get an “option validation failed” error.
- One of the URLs is likely malformed.
  - Use the `v2.0` versions.
  - No trailing slashes.
  - No parameters like `?scope=...` inside the URL.
- Double-check that you selected the correct strategy (OIDC, not SAML).

### 🔹 I see a 403 error or the connection fails to save.
- If you're using a custom domain (e.g., `auth.company.com`), try using your default Auth0 domain (e.g., `your-tenant.us.auth0.com`).
- Your Auth0 plan might not support Enterprise connections; verify you're on **Developer Pro** or higher.

### 🔹 What scopes are required?
- Use `openid email profile` — `openid` is mandatory.
- Add others like `offline_access` if you need refresh tokens.

### 🔹 Can we restrict who can log in?
- Yes — in Entra ID, you can configure **User assignment required**.
- You can also restrict access based on email/domain/group using Auth0 Actions or Rules.

---

Let us know if you need help verifying your setup or adjusting your configuration!
