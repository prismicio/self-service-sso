# How to Set Up a Custom SAML Identity Provider with Auth0 via Self-Service SSO

This guide walks you through configuring **a custom SAML Identity Provider (IdP)** with **Auth0** using the **Self-Service SSO setup flow**. This is suitable for any IdP that supports standard SAML 2.0 (e.g., Shibboleth, Gluu, custom ADFS environments).

---

## 🔧 Step 1 – Gather IdP Information

From your Identity Provider, collect the following:

| Field                     | Description |
|--------------------------|-------------|
| **SAML Sign-In URL**       | Also called SSO URL or Login URL |
| **SAML Entity ID**         | The IdP Issuer |
| **X.509 Signing Certificate** | Used to validate SAML assertions |
| **Attributes** (optional)    | Claims sent in the assertion (e.g., `email`, `name`) |

You can either get this info from your IdP administrator or export it from their metadata XML.

---

## 🔐 Step 2 – Fill Out the Auth0 Self-Service SSO Form

In the Auth0 self-service SSO flow, select **SAML** or **Custom SAML**, and provide the following fields:

| Field                     | Value |
|--------------------------|-------|
| **Connection name**         | e.g. `custom-saml-clientx` |
| **SAML Sign-In URL**        | From your IdP metadata |
| **SAML Issuer (Entity ID)** | From your IdP metadata |
| **X.509 Certificate**       | Paste the PEM-encoded cert |
| **Name Identifier Format** | `EmailAddress` *(recommended)* |
| **Attributes (optional)**  | e.g., `email`, `given_name`, `family_name` |

*Tip:* Auth0 expects the SAML assertion to contain a `NameID`, ideally mapped to the user’s email address.

---

## 📋 Step 3 – Configure Your SAML IdP

In your SAML IdP, create a new **Service Provider (SP)** connection or application for Auth0. Use these values:

| Field                 | Value |
|----------------------|-------|
| **ACS URL**            | `https://<YOUR-AUTH0-TENANT>.auth0.com/login/callback?connection=<CONNECTION_NAME>` |
| **SP Entity ID**       | `urn:auth0:<YOUR-TENANT>` |
| **NameID Format**      | `EmailAddress` |
| **RelayState**         | *(optional)* Leave blank or configure for app routing |
| **Signed Assertions**  | Recommended: Yes |
| **Signed Responses**   | Optional, depends on IdP config |

Add attributes (claims) to the assertion if needed:
- `email`
- `given_name`
- `family_name`

---

## ✅ Step 4 – Test and Complete the Setup

- In Auth0, click **Test Connection**.
- You will be redirected to your SAML IdP.
- After login, you will return to Auth0 and the connection will be saved.

---

## ❓ FAQ & Troubleshooting

### 🔹 Auth0 can't find the NameID or email.
- Make sure the SAML assertion contains a `NameID` and that it's mapped to the user’s email.
- Use a browser plugin like **SAML-tracer** to inspect the SAML response.

### 🔹 Invalid certificate or signature error.
- Confirm the signing certificate is correctly exported in PEM format:
  ```
  -----BEGIN CERTIFICATE-----
  ...
  -----END CERTIFICATE-----
  ```

### 🔹 Redirect or callback fails.
- Double-check that the **ACS URL** is exactly:
  ```
  https://<AUTH0-TENANT>.auth0.com/login/callback?connection=<CONNECTION_NAME>
  ```

### 🔹 Can I support IdP-initiated login?
- Yes. Auth0 supports both **SP-initiated** and **IdP-initiated** SAML login.
- Ensure the `connection` name is included in the ACS URL, and the IdP is configured to use the correct SP Entity ID.

### 🔹 Can I restrict which users can log in?
- Yes — filter users on the IdP side using group membership or send claims to Auth0 for evaluation via Rules or Actions.

---

Let us know if you’d like help reviewing your metadata or debugging your assertion!
