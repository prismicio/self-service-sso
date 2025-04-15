# How to Set Up Google Workspace with Auth0 via Self-Service SSO (SAML)

This guide walks you through setting up **Google Workspace** (formerly G Suite) as a **SAML Identity Provider (IdP)** for **Auth0** using the **Self-Service SSO flow**.

---

## 🔧 Step 1 – Set Up a Custom SAML App in Google Admin Console

1. Go to [admin.google.com](https://admin.google.com) and log in with your admin account.
2. Navigate to **Apps > Web and mobile apps**.
3. Click **Add App > Add custom SAML app**.
4. Set an App name (e.g. `Auth0 SSO`) and click **Continue**.

### Step 1.1 – Download Google IdP Info
- **Option 1**: Download the **IDP metadata file** (recommended)
- **Option 2**: Copy the following manually:
  - **SSO URL** (SAML Login URL)
  - **Entity ID** (Issuer)
  - **Certificate** (X.509 certificate)

You'll use these in Auth0.

---

## 📄 Step 2 – Enter ACS and Entity ID in Google

In the Google Admin setup wizard:

1. **ACS URL**:
   ```
   https://<YOUR-AUTH0-TENANT>.auth0.com/login/callback?connection=<CONNECTION_NAME>
   ```
2. **Entity ID**:
   ```
   urn:auth0:<YOUR-TENANT-NAME>
   ```
3. **Start URL**: *(optional)* Leave empty or set to your app's login page.
4. **Name ID Format**: `EMAIL`
5. **Name ID**: `Basic Information > Primary email`
6. Click **Continue**

---

## 💡 Step 3 – Add Attribute Mapping (Optional)

You can map user profile fields to be sent to Auth0. Recommended claims:

| Google Directory Field     | SAML Attribute  |
|----------------------------|-----------------|
| Primary email              | email           |
| First name                 | given_name      |
| Last name                  | family_name     |

Click **Finish** to save the app.

Then, enable the app for users:
1. In the app settings, click **User Access**.
2. Choose **ON for everyone** (or specific org units).

---

## 📋 Step 4 – Gather Info for Auth0 Setup

From the previous steps, you should now have:

| Field                | Source |
|---------------------|--------|
| **SAML Sign-in URL**   | Google Admin Console |
| **Entity ID**          | Google Admin Console |
| **X.509 Certificate**  | Google Admin Console (download or copy) |

---

## 🔐 Step 5 – Fill Out the Auth0 Self-Service SSO Form

When prompted in the Auth0 self-service setup, fill in:

| Field                   | Value |
|------------------------|-------|
| **Connection name**       | e.g. `google-mycompany` |
| **SAML Issuer (Entity ID)** | `urn:auth0:<YOUR-TENANT>` |
| **SAML Sign In URL**       | From Google (SSO URL) |
| **X.509 Signing Certificate** | From Google (paste the full certificate) |
| **Name Identifier Format** | `EmailAddress` |
| **Attributes**             | email, given_name, family_name (optional) |

---

## ✅ Step 6 – Test and Complete the Setup

- Click **Test Connection** in Auth0.
- You'll be redirected to Google for authentication.
- Once successful, Auth0 will save the connection.

---

## ❓ FAQ & Troubleshooting

### 🔹 Auth0 says it can't find the email/NameID.
- Ensure that `Primary email` is set as the Name ID in Google.
- Double-check attribute mappings for `email`, `given_name`, etc.

### 🔹 The certificate is rejected or invalid.
- Make sure the X.509 certificate is copied **exactly**, including:
  ```
  -----BEGIN CERTIFICATE-----
  ...
  -----END CERTIFICATE-----
  ```
- Or use the metadata XML file upload if supported.

### 🔹 Users can’t access the app.
- Check that the app is enabled for the correct users or org units in Google Admin.

### 🔹 Redirect fails after login.
- Double-check the ACS URL:
  ```
  https://<AUTH0-TENANT>.auth0.com/login/callback?connection=<CONNECTION_NAME>
  ```
- Confirm the Entity ID matches what you set in Google.

---

Need help verifying your metadata or debugging a login issue? Let us know!
