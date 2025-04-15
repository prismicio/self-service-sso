# How to Set Up Okta with Auth0 via Self-Service SSO

Follow these steps to configure Okta as your Identity Provider (IdP) using the Auth0 self-service SSO setup flow. This guide assumes you’re setting up an **OIDC-based integration** (which is recommended and simpler than SAML).

---

## 🔧 Step 1 – Create an Application in Okta

1. Log in to your Okta Admin Dashboard:  
   [https://<your-okta-domain>.okta.com](https://<your-okta-domain>.okta.com)

2. Go to **Applications** > **Applications**, then click **Create App Integration**.

3. Choose:
   - **Sign-in method**: OIDC - OpenID Connect
   - **Application type**: Web Application
   - Click **Next**

4. Fill in the form:
   - **App name**: Choose something like `MyCompany Auth0 SSO`
   - **Sign-in redirect URIs**:  
     Add:
     ```
     https://<YOUR-AUTH0-TENANT>.auth0.com/login/callback
     ```
     *(Replace with your Auth0 tenant domain or custom domain)*

   - **Sign-out redirect URIs**: *(optional)*  
     Can be your app homepage or dashboard URL.

   - **Assignments**: Choose "Skip group assignment" (or restrict to specific users/groups if needed)

5. Click **Save**

---

## 📋 Step 2 – Gather Required Information

Once the app is created, go to the application settings page and copy:

- **Client ID** → Used in Auth0 config
- **Client Secret** → Used in Auth0 config
- **Okta Domain** → From your URL (e.g. `dev-123456.okta.com` or `company.okta.com`)

You’ll use these in the Auth0 self-service SSO form.

---

## 🌐 Step 3 – Construct the Okta Endpoint URLs

| Field               | Value |
|--------------------|-------|
| **Authorization URL** | `https://<OKTA_DOMAIN>/oauth2/default/v1/authorize` |
| **Token URL**         | `https://<OKTA_DOMAIN>/oauth2/default/v1/token` |
| **Issuer URL**        | `https://<OKTA_DOMAIN>/oauth2/default` |

Make sure you’re using the **default authorization server** unless you’ve created a custom one.

---

## 🔑 Step 4 – Fill Out the Auth0 Self-Service SSO Form

When prompted by the Auth0 self-service SSO setup, fill in the following:

| Field             | Value |
|------------------|-------|
| **Client ID**         | From Okta |
| **Client Secret**     | From Okta |
| **Authorization URL** | As above |
| **Token URL**         | As above |
| **Issuer URL**        | As above |
| **Scopes**            | `openid email profile` |
| **Connection name**   | e.g. `okta-mycompany` *(optional)*

---

## ✅ Step 5 – Test and Complete the Setup

- Click **Test Connection** in the self-service SSO setup.
- You will be redirected to Okta to authenticate.
- Once login succeeds, Auth0 will confirm the connection and save it.

---

## ❓ Frequently Asked Questions (FAQ) & Troubleshooting

### 🔹 The test connection fails with a generic error.
- Make sure all URLs use your actual **Okta domain**, and use `/default` in the paths.
- Ensure the redirect URI matches **exactly** what you entered in Okta.

### 🔹 I get an “option validation failed” error.
- Likely due to:
  - Missing or incorrect field (e.g. `issuer` or `client_id`)
  - Typo in a URL
  - Using `https://okta.com` instead of your **own Okta tenant domain**

### 🔹 I see a 403 or the connection won’t save.
- If you're on a **custom domain**, try using the default Auth0 domain.
- Confirm your Auth0 plan supports Enterprise (SSO) connections.

### 🔹 What scopes are required?
- Use `openid email profile`
- Do not include extra scopes unless you’re sure they’re supported by your Okta app

### 🔹 Can I limit who can use the SSO?
- Yes — in Okta, assign the app only to specific users or groups.
- You can also use rules in Auth0 to restrict login by email domain, group, etc.

---

Let us know if you need help verifying your setup or debugging any specific step!
