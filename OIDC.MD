# How to Set Up a Custom OIDC Provider with Auth0 via Self-Service SSO

This guide explains how to connect **Auth0** to a **custom OpenID Connect (OIDC)** Identity Provider using the **Self-Service SSO setup flow**. This is useful when your IdP is not one of the pre-integrated options (like Okta, Google, or Azure), but supports standard OIDC endpoints.

---

## 🔧 Step 1 – Gather OIDC Provider Information

From your custom OIDC Identity Provider, collect the following:

| Field                  | Description |
|------------------------|-------------|
| **Client ID**           | Issued when you register an OIDC app on your provider |
| **Client Secret**       | Secret used with the client ID to authenticate to the token endpoint |
| **Authorization URL**   | OIDC endpoint for login (typically `/authorize`) |
| **Token URL**           | OIDC endpoint to exchange code for tokens (typically `/token`) |
| **Issuer URL**          | Issuer identifier (OIDC discovery endpoint or static base URL) |
| **User Info URL** *(optional)* | Used to fetch user profile (often included in discovery) |

If your provider supports OIDC Discovery, you can find this info at:
```
https://<your-provider>/.well-known/openid-configuration
```

---

## 🔐 Step 2 – Fill Out the Auth0 Self-Service SSO Form

In the Auth0 self-service SSO UI, choose **Custom OIDC** or **OIDC Identity Provider**, then enter the following:

| Field             | Value |
|------------------|-------|
| **Connection name**     | Choose a unique name (e.g. `custom-oidc-myidp`) |
| **Client ID**           | From your provider |
| **Client Secret**       | From your provider |
| **Authorization URL**   | e.g. `https://myidp.com/oauth2/authorize` |
| **Token URL**           | e.g. `https://myidp.com/oauth2/token` |
| **Issuer URL**          | e.g. `https://myidp.com/` or discovery issuer |
| **Scopes**              | `openid email profile` *(recommended)* |

Optional (if not handled via discovery):
- **User Info URL**: e.g. `https://myidp.com/userinfo`
- **JWKS URL**: If using a static public key set

---

## ✅ Step 3 – Test and Complete the Setup

- Click **Test Connection** in Auth0.
- You will be redirected to your custom OIDC provider's login page.
- Once login is successful, Auth0 will complete the connection.

---

## ❓ FAQ & Troubleshooting

### 🔹 The test connection fails with "invalid_client".
- Check that the **Client ID** and **Client Secret** are valid.
- Ensure the **redirect URI** in your IdP matches:
  ```
  https://<YOUR-AUTH0-TENANT>.auth0.com/login/callback
  ```
- The Auth0 redirect URI must be registered in your custom IdP.

### 🔹 Auth0 says "Issuer mismatch" or discovery fails.
- Ensure the **Issuer URL** exactly matches the `iss` field in the ID token.
- If your provider doesn’t support discovery, you may need to manually enter all endpoint URLs.

### 🔹 Claims are missing (email, profile).
- Confirm that your IdP is including standard OIDC claims in the ID token or via the userinfo endpoint.
- You may need to configure the IdP to include these scopes and claims.

### 🔹 Can I use PKCE or custom response types?
- Auth0 handles OIDC login using `code` flow by default.
- Custom flows (e.g. `id_token token`) are not supported in this flow.

### 🔹 Can I restrict access to certain users?
- Yes — you can filter users in your IdP or apply Rules or Actions in Auth0 based on domain, group, or email.

---

Let us know if you need help reviewing your OIDC config or debugging token exchange!
