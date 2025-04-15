# How to Set Up PingFederate with Auth0 via Self-Service SSO (SAML)

This guide walks you through configuring **PingFederate** as a **SAML Identity Provider (IdP)** to connect with **Auth0** using the **Self-Service SSO setup flow**.

---

## 🔧 Step 1 – Set Up a SP Connection in PingFederate

1. Log into your **PingFederate Admin Console**.
2. Navigate to **Identity Provider > SP Connections**, then click **Create New**.
3. Choose **Browser SSO Profiles** and click **Next**.
4. Select **Browser SSO** > **SAML 2.0**, then click **Next**.

---

## 📄 Step 2 – Configure Connection Settings

1. **Connection Name**: e.g. `Auth0 SSO`
2. Optionally set a description and partner ID.
3. Choose **File-Based Metadata** or **Manually Configure Connection** depending on preference.

If manually configuring:
- Set the **ACS URL** (Assertion Consumer Service URL) to:
  ```
  https://<YOUR-AUTH0-TENANT>.auth0.com/login/callback?connection=<CONNECTION_NAME>
  ```
- Set the **SP Entity ID** to:
  ```
  urn:auth0:<YOUR-TENANT-NAME>
  ```

---

## 🔐 Step 3 – Configure Assertion Creation

1. Go to the **Assertion Creation** section.
2. Define **Subject Name Format** as `EmailAddress`.
3. Map the necessary attributes:

| PingFederate Attribute | SAML Attribute  |
|------------------------|-----------------|
| Email Address          | email           |
| First Name             | given_name      |
| Last Name              | family_name     |

4. Under **Attribute Contract**, ensure these are included:
   - `email`
   - `given_name`
   - `family_name`

5. Configure **Signature Policy**:
   - Sign SAML Assertions: **Yes**
   - Signing Certificate: Choose the appropriate cert

---

## 📋 Step 4 – Gather Metadata for Auth0

You will need the following from your PingFederate setup:

| Field                  | Where to Find |
|-----------------------|---------------|
| **SAML Sign-In URL**    | SSO Service Endpoint (SAML 2.0 HTTP-Redirect or POST) |
| **Entity ID**           | IdP Entity ID configured in PingFederate |
| **X.509 Certificate**   | Export the signing certificate in Base64 format |

---

## 🔐 Step 5 – Fill Out the Auth0 Self-Service SSO Form

In the Auth0 self-service SSO flow, provide:

| Field                     | Value |
|--------------------------|-------|
| **Connection name**         | e.g. `pingfed-mycompany` |
| **SAML Issuer (Entity ID)** | PingFederate IdP Entity ID |
| **SAML Sign In URL**        | PingFederate SSO endpoint |
| **X.509 Certificate**       | The Base64 PEM-formatted certificate |
| **Name Identifier Format** | `EmailAddress` |
| **Attributes (optional)**  | email, given_name, family_name |

---

## ✅ Step 6 – Test and Complete the Setup

- Click **Test Connection** in the Auth0 UI.
- You’ll be redirected to the PingFederate login screen.
- After successful authentication, the user is redirected back to Auth0 and the connection is saved.

---

## ❓ FAQ & Troubleshooting

### 🔹 Auth0 says it can’t find the NameID or email.
- Ensure your Subject Name Format is set to `EmailAddress`.
- Verify the email claim is included in the assertion and correctly mapped.

### 🔹 Invalid signature or cert errors.
- Make sure the certificate pasted into Auth0 is the correct **signing certificate** from PingFederate.
- The certificate must include:
  ```
  -----BEGIN CERTIFICATE-----
  ...
  -----END CERTIFICATE-----
  ```

### 🔹 The redirect after login fails.
- Confirm the **ACS URL** in PingFederate exactly matches:
  ```
  https://<YOUR-AUTH0-TENANT>.auth0.com/login/callback?connection=<CONNECTION_NAME>
  ```

### 🔹 Can I restrict access to certain users?
- Yes — apply group membership or role restrictions in PingFederate using access control policies or attribute filters.
- You can also restrict access on Auth0 using Rules or Actions.

---

Let us know if you need help reviewing metadata or troubleshooting SAML assertions!
