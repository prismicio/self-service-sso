# How to Set Up ADFS with Auth0 via Self-Service SSO (SAML)

This guide explains how to configure **ADFS (Active Directory Federation Services)** as a **SAML Identity Provider (IdP)** for **Auth0** using the **Self-Service SSO setup flow**.

---

## 🔧 Step 1 – Set Up a Relying Party Trust in ADFS

1. Open the **ADFS Management Console**.
2. Go to **Trust Relationships > Relying Party Trusts**, then click **Add Relying Party Trust**.
3. Choose **Claims aware** and click **Start**.
4. Select **Enter data about the relying party manually**, then click **Next**.
5. Set a **Display name**, e.g. `Auth0 SSO`, and click **Next**.
6. Choose **AD FS profile** and continue.
7. In the **Configure Certificate** step, skip it unless required.
8. On the **Configure URL** step:
   - Check **Enable support for the SAML 2.0 WebSSO protocol**.
   - Enter the SAML Assertion Consumer Service (ACS) URL from Auth0:
     ```
     https://<YOUR-AUTH0-TENANT>.auth0.com/login/callback?connection=<CONNECTION_NAME>
     ```
9. In **Configure Identifiers**, add this:
   ```
   urn:auth0:<YOUR-TENANT-NAME>
   ```
10. Finish the wizard and open the **Edit Claim Rules** dialog.

---

## 🔹 Step 2 – Add Claim Rules

1. Click **Add Rule**, select **Send LDAP Attributes as Claims**, then:
   - **Attribute store**: Active Directory
   - Map the following:
     | LDAP Attribute     | Outgoing Claim Type |
     |--------------------|----------------------|
     | E-Mail-Addresses   | email                |
     | Given-Name         | given_name           |
     | Surname            | family_name          |

2. Click **Add Rule** again, choose **Transform an Incoming Claim**, then:
   - **Incoming claim type**: `email`
   - **Outgoing claim type**: `Name ID`
   - **Outgoing name ID format**: `Email`

Click **Finish** and apply your rules.

---

## 📋 Step 3 – Gather Required Information for Auth0

You will need:

| Field             | Where to Find |
|------------------|----------------|
| **SAML IdP URL**  | ADFS Federation Metadata or login URL (e.g. `https://<adfs-domain>/adfs/ls/`) |
| **SAML Certificate** | Found in ADFS > Service > Certificates |
| **Entity ID**     | Your configured relying party identifier, e.g. `urn:auth0:<tenant>` |

Export the **ADFS signing certificate** in Base64 format.

---

## 🔐 Step 4 – Fill Out the Auth0 Self-Service SSO Form

When prompted in the self-service setup, provide the following:

| Field                   | Value |
|------------------------|-------|
| **Connection name**       | e.g. `adfs-mycompany` |
| **SAML Issuer (Entity ID)** | `urn:auth0:<tenant>` (matches what you entered in ADFS) |
| **SAML Sign In URL**       | ADFS SAML endpoint, e.g. `https://<adfs-domain>/adfs/ls/` |
| **X.509 Signing Certificate** | Paste the full Base64 cert from ADFS |
| **Name Identifier Format** | `EmailAddress` |
| **Mappings / Attributes**  | email, given_name, family_name (optional, depends on claim rules) |

---

## ✅ Step 5 – Test and Complete the Setup

- Click **Test Connection**.
- You should be redirected to your ADFS login page.
- After successful authentication, you'll be redirected to your app and the connection will be saved.

---

## ❓ FAQ & Troubleshooting

### 🔹 The test connection fails with "Invalid Signature".
- Make sure the **certificate you pasted** in Auth0 matches the ADFS token-signing cert.
- Confirm there are no formatting issues (extra characters, missing `-----BEGIN CERTIFICATE-----` etc).

### 🔹 ADFS login page shows but the redirect fails.
- Ensure the **ACS URL** is exactly what Auth0 expects:
  ```
  https://<AUTH0-TENANT>.auth0.com/login/callback?connection=<CONNECTION_NAME>
  ```

### 🔹 Auth0 can't find the NameID or email.
- Double-check claim rules in ADFS: the `Name ID` should be mapped to the user's email.
- Use the **SAML Tracer** browser extension to debug the assertion.

### 🔹 What SAML NameID format should we use?
- Recommended: `EmailAddress`
- Auth0 can also accept `Persistent` or `Unspecified`, but `Email` is most common.

### 🔹 Can I limit which users can log in?
- Yes — in ADFS, assign access only to users or groups in the relying party trust.
- You can also restrict access on the Auth0 side using Rules or Actions.

---

Need help reviewing the metadata or debugging the login? We're happy to assist!

