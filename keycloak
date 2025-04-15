# How to Set Up Keycloak with Auth0 via Self-Service SSO

Follow these steps to configure **Keycloak** as your Identity Provider using **Auth0’s Self-Service SSO** setup flow. This guide uses the **OIDC (OpenID Connect)** protocol, which is supported natively by Keycloak.

---

## 🔧 Step 1 – Create a Client in Keycloak

1. Log into your Keycloak Admin Console.
2. Select the appropriate **Realm** where your users reside.
3. Go to **Clients**, then click **Create client**.
4. Fill out the form:
   - **Client type**: OpenID Connect
   - **Client ID**: Choose something like `auth0-oidc`
   - Click **Next**

5. On the next screen:
   - **Root URL**: `https://<YOUR-AUTH0-TENANT>.auth0.com`
   - **Valid redirect URIs**:
     ```
     https://<YOUR-AUTH0-TENANT>.auth0.com/login/callback
     ```
   - **Home URL** and **Web origins**: (Optional)
   - Click **Save**

6. In the **Settings** tab, ensure the following:
   - **Access type**: `confidential`
   - **Standard flow enabled**: `ON`
   - **Direct access grants enabled**: `ON`
   - **Service accounts enabled**: `OFF`

7. Click **Save**, then go to the **Credentials** tab.
   - Copy the **Client Secret**

---

## 📋 Step 2 – Gather Required Information

| Field         | Where to Find |
|---------------|----------------|
| **Client ID**     | From Keycloak Client settings |
| **Client Secret** | From Keycloak > Client > Credentials tab |
| **Keycloak URL**  | Your base Keycloak server URL |

---

## 🌐 Step 3 – Build the Required Endpoint URLs

| Field               | Value |
|--------------------|-------|
| **Authorization URL** | `https://<KEYCLOAK_DOMAIN>/realms/<REALM_NAME>/protocol/openid-connect/auth` |
| **Token URL**         | `https://<KEYCLOAK_DOMAIN>/realms/<REALM_NAME>/protocol/openid-connect/token` |
| **Issuer URL**        | `https://<KEYCLOAK_DOMAIN>/realms/<REALM_NAME>` |

Replace `<KEYCLOAK_DOMAIN>` and `<REALM_NAME>` with your actual values.

---

## 🔐 Step 4 – Fill Out the Auth0 Self-Service SSO Form

When prompted in the Auth0 self-service setup, enter:

| Field             | Value |
|------------------|-------|
| **Client ID**         | From Keycloak |
| **Client Secret**     | From Keycloak |
| **Authorization URL** | From Step 3 |
| **Token URL**         | From Step 3 |
| **Issuer URL**        | From Step 3 |
| **Scopes**            | `openid email profile` |
| **Connection name**   | e.g. `keycloak-myrealm` *(optional)* |

---

## ✅ Step 5 – Test and Complete the Setup

- Click **Test Connection** in Auth0.
- You will be redirected to Keycloak to authenticate.
- After logging in, you'll return to your app and the connection will be saved.

---

## ❓ Frequently Asked Questions (FAQ) & Troubleshooting

### 🔹 The test connection fails with a generic error.
- Double-check that the **redirect URI** is correctly set in Keycloak.
- Make sure **Issuer URL**, **Token URL**, and **Auth URL** are correct and match your Keycloak configuration.

### 🔹 I get an “option validation failed” error.
- This usually indicates a typo or formatting error in one of the URLs.
- Ensure the realm and domain in the URLs are correct.
- Make sure the client is set to **confidential** and not public.

### 🔹 I see a 403 or the connection won’t save.
- If using a custom Auth0 domain, try using the default Auth0 domain.
- Make sure your Auth0 plan allows enterprise (SSO) connections.

### 🔹 What scopes are required?
- Use: `openid email profile`
- Make sure Keycloak is configured to release these claims in the user info/token response.

### 🔹 Can I restrict who can log in?
- Yes, in Keycloak you can assign users to roles or groups and control which users are allowed to use the OIDC client.
- You can also implement restrictions in Auth0 using Actions or Rules.

---

Let us know if you need help verifying your configuration or setting up test users!
