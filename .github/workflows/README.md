
## Step 1: Register Moodle as an App in Azure AD

1. Go to **[Azure Portal](https://portal.azure.com)** → **Microsoft Entra ID** → **App registrations** → **+ New registration**

2. App Details:
   - **Name**: `Moodle LMS`
   - **Supported account types**: `Accounts in this organizational directory only (Single tenant)`
   - **Redirect URI (Web)**: [`https://your-moodle-site/auth/oidc/`](https://your-moodle-site/auth/oidc/)

3. Click Register

<p align="center">
  <img src="https://github.com/user-attachments/assets/7a13c18e-a2f4-416d-9d3d-824bcdebfbff" 
       alt="Moodle OIDC Settings" 
       width="800" 
       style="border:2px solid #0078D4; border-radius:8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);"/>
</p>


> **<span style="color:red;">Note: Save the Application (client) ID and Directory (tenant) ID</span>**

-----------------------------------------------------------------------------------------------------------------------------------------------
_______________________________________________________________________________________________________________________________________________

## Step 2: Configure API Permissions

1. Open the app you just registered.

2. Navigate to **API Permissions** → **+ Add a permission** → **Microsoft Graph** → **Delegated permissions**

3. Add the following permissions:
   - `openid`
   - `profile`
   - `email`
   - `User.Read`


Image 

4. Click **Grant admin consent for Default Directory** to approve these for your tenant.

Image 

> **<span style="color:red;">Note: Admin consent is required for SSO to work</span>**


-----------------------------------------------------------------------------------------------------------------------------------------------
_______________________________________________________________________________________________________________________________________________


## Step 3: Create Client Secret

1. Go to: **Certificates & secrets** → **+ New client secret**

   - **Description**: `Azure AD`
   - **Expires**: `24 months`

2. **Copy the `Value`** (e.g., `F9S8Q~XXXXXXXXXXXXXXXXX-WZsA~XXXXX`)  
> **<span style="color:red;">Warning: This is shown only once. If lost, delete and recreate.</span>**


-----------------------------------------------------------------------------------------------------------------------------------------------
_______________________________________________________________________________________________________________________________________________


## Step 4: Install OpenID Connect Plugin in Moodle

1. Download the ZIP file from:  
   **[moodle.org/plugins/auth_oidc](https://moodle.org/plugins/auth_oidc)**

2. Go to **Site administration -> plugins -> Drag ZIP package**
   (`https://your-moodle-site/admin/tool/installaddon/index.php`)

3. **Upload the ZIP** → Click **Install plugin from the ZIP file**

4. Follow prompts → **Plugin installed successfully**

> **<span style="color:red;">Note: Do not extract the ZIP — upload as-is</span>**

-----------------------------------------------------------------------------------------------------------------------------------------------
_______________________________________________________________________________________________________________________________________________

## Step 5: Configure OIDC in Moodle

Go to: **Site administration → Plugins → Authentication → OpenID Connect**

| Field                        | Value |
|------------------------------|-------|
| **IdP Type**                 | `Microsoft identity platform (v2.0)` |
| **Application ID**           | *(Your Application (client) ID)* |
| **Client authentication method** | `Secret` |
| **Client Secret**            | *(Paste your secret here)* |
| **Authorization Endpoint**   | `https://login.microsoftonline.com/<tenant-id>/oauth2/v2.0/authorize` |
| **Token Endpoint**           | `https://login.microsoftonline.com/<tenant-id>/oauth2/v2.0/token` |
| **Scope**                    | `openid profile email` |
| **Redirect URI**             | `https://your-moodle-site/auth/oidc/` |

> **<span style="color:red;">Warning: Replace `<tenant-id>` with your Directory (tenant) ID</span>**


-----------------------------------------------------------------------------------------------------------------------------------------------
_______________________________________________________________________________________________________________________________________________


## Step 6: Enable Advanced Features

Go to: **Site administration → Plugins → Authentication → OpenID Connect → Settings**

| Feature               | Setting                     |
|-----------------------|-----------------------------|
| **Force redirect**    | `Yes`                       |
| **Silent Login Mode** | `Yes`                       |
| **Login Flow**        | `Authorization Code Flow`   |

> **<span style="color:red;">Note: Silent Login works best when users are already signed into Microsoft</span>**

-----------------------------------------------------------------------------------------------------------------------------------------------
_______________________________________________________________________________________________________________________________________________

## Step 7: Enable Single Sign-Out

Go to: **Site administration → Plugins → Authentication → OpenID Connect → Settings**

| Field                        | Value |
|------------------------------|-------|
| **Single Sign Out**          | `Yes` |
| **IdP Logout Endpoint**      | `https://login.microsoftonline.com/<tenant-id>/oauth2/v2.0/logout` |
| **Front-channel Logout URL** | `https://your-moodle-site/auth/oidc/logout.php` |

> **Azure AD → App → Authentication → Add Front-channel logout URL**  
> **<span style="color:red;">Warning: Replace `<tenant-id>` with your Directory (tenant) ID</span>**


-----------------------------------------------------------------------------------------------------------------------------------------------
_______________________________________________________________________________________________________________________________________________

## Step 8: Auto-Sync User Profiles

Go to: **Site administration → Plugins → Authentication → OpenID Connect → Settings**

| Field       | Mapping       | Update Local         | Lock      |
|-------------|---------------|----------------------|-----------|
| **First name** | `given_name` | `On creation & login` | `Unlocked` |
| **Last name**  | `family_name`| `On creation & login` | `Unlocked` |
| **Email**      | `email`      | `On creation & login` | `Unlocked` |

> **<span style="color:red;">Note: Profile fields are synced from Azure AD on every login</span>**


-----------------------------------------------------------------------------------------------------------------------------------------------
_______________________________________________________________________________________________________________________________________________


## Step 9: Test the Flow

1. Visit: [`https://your-moodle-site`](https://your-moodle-site)

2. Click **"Login with Microsoft"**

3. **Authenticate** → Enter credentials → Complete **MFA**

4. **Success** → User auto-created in Moodle → Profile synced

5. Click **Logout** → Signed out of **Moodle + Microsoft**

> **<span style="color:red;">Warning: If login fails, check Client Secret, Redirect URI, and user assignment in Azure AD</span>**

-----------------------------------------------------------------------------------------------------------------------------------------------
_______________________________________________________________________________________________________________________________________________


