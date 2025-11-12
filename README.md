<p>
  <h1 align="center" style="color: #0078D4; font-size: 42px; font-weight: 900; margin: 20px 0; text-shadow: 0 4px 8px rgba(0,0,0,0.2);">
    Azure AD + Moodle OIDC Integration
  </h1>
</p>

---



## Step 1: Register Moodle as an App in Azure AD

1. Go to **[Azure Portal](https://portal.azure.com)** → **Microsoft Entra ID** → **App registrations** → **+ New registration**

2. App Details:
   - **Name**: `Moodle LMS`
   - **Supported account types**: `Accounts in this organizational directory only (Single tenant)`
   - **Redirect URI (Web)**: [`https://your-moodle-site/auth/oidc/`](https://your-moodle-site/auth/oidc/)

3. Click Register

<p align="center">
  <img src="https://github.com/user-attachments/assets/7fca737a-17f2-4756-beee-60b4e87ee685"
       alt="Azure App Registration"
       width="800"
       style="border:2px solid #0078D4; border-radius:8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);"/>
  <br><em>Azure AD App Registration — save the <strong>Application (client) ID</strong> and <strong>Directory (tenant) ID</strong> for Moodle configuration</em>
</p>


> **<span style="color:red;">Note: Save the Application (client) ID and Directory (tenant) ID</span>**

<p align="center">
  <img src="https://github.com/user-attachments/assets/173ab667-82d2-420b-80c8-c2e2f30c02dd"
       alt="Moodle OIDC Advanced Settings"
       width="800"
       style="border:2px solid #0078D4; border-radius:8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);"/>
  <br><em>Advanced OIDC settings — enable <strong>Force redirect</strong>, <strong>Silent Login</strong>, and <strong>Authorization Code Flow</strong></em>
</p>


-----------------------------------------------------------------------------------------------------------------------------------------------


## Step 2: Configure API Permissions

1. Open the app you just registered.

2. Navigate to **API Permissions** → **+ Add a permission** → **Microsoft Graph** → **Delegated permissions**

3. Add the following permissions:
   - `openid`
   - `profile`
   - `email`
   - `User.Read`
  

<p align="center">
  <img src="https://github.com/user-attachments/assets/f4e1a974-0610-468f-8e8f-92775dfeb9a8"
       alt="Azure AD API Permissions"
       width="800"
       style="border:2px solid #0078D4; border-radius:8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);"/>
  <br><em>Configure <strong>Delegated permissions</strong> in Microsoft Graph: <code>openid</code>, <code>profile</code>, <code>email</code>, <code>User.Read</code></em>
</p>


4. Click **Grant admin consent for Default Directory** to approve these for your tenant.



<p align="center">
  <img src="https://github.com/user-attachments/assets/948e12dc-7977-4e69-9088-fee4871d6bd4"
       alt="Azure AD Client Secret Creation"
       width="800"
       style="border:2px solid #0078D4; border-radius:8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);"/>
  <br><em>Create a <strong>Client Secret</strong> — <strong>copy the Value immediately</strong> (it disappears after you leave the page)</em>
</p>


> **<span style="color:red;">Note: Admin consent is required for SSO to work</span>**


-----------------------------------------------------------------------------------------------------------------------------------------------



## Step 3: Create Client Secret

1. Go to: **Certificates & secrets** → **+ New client secret**

   - **Description**: `Azure AD`
   - **Expires**: `24 months`


<p align="center">
  <img src="https://github.com/user-attachments/assets/48c2c7be-41a7-4c3f-8422-a7611a8b6ad9"
       alt="Moodle Plugin Upload"
       width="800"
       style="border:2px solid #0078D4; border-radius:8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);"/>
  <br><em>Upload the <strong>OpenID Connect ZIP file</strong> directly — <strong>do not extract</strong> it first</em>
</p>


2. **Copy the `Value`** (e.g., `F9S8Q~XXXXXXXXXXXXXXXXX-WZsA~XXXXX`)  
> **<span style="color:red;">Warning: This is shown only once. If lost, delete and recreate.</span>**


<p align="center">
  <img src="https://github.com/user-attachments/assets/ab059778-a285-4166-b75c-92bd63edd398"
       alt="Moodle OIDC Configuration"
       width="800"
       style="border:2px solid #0078D4; border-radius:8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);"/>
  <br><em>Moodle OpenID Connect settings — enter your <strong>Client ID</strong>, <strong>Client Secret</strong>, and <strong>v2.0 endpoints</strong></em>
</p>


-----------------------------------------------------------------------------------------------------------------------------------------------



## Step 4: Install OpenID Connect Plugin in Moodle

1. Download the ZIP file from:  
   **[moodle.org/plugins/auth_oidc](https://moodle.org/plugins/auth_oidc)**

2. Go to **Site administration -> plugins -> Drag ZIP package**
   (`https://your-moodle-site/admin/tool/installaddon/index.php`)

3. **Upload the ZIP** → Click **Install plugin from the ZIP file**

4. Follow prompts → **Plugin installed successfully**

> **<span style="color:red;">Note: Do not extract the ZIP — upload as-is</span>**

-----------------------------------------------------------------------------------------------------------------------------------------------


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



## Step 6: Enable Advanced Features

Go to: **Site administration → Plugins → Authentication → OpenID Connect → Settings**

| Feature               | Setting                     |
|-----------------------|-----------------------------|
| **Force redirect**    | `Yes`                       |
| **Silent Login Mode** | `Yes`                       |
| **Login Flow**        | `Authorization Code Flow`   |

> **<span style="color:red;">Note: Silent Login works best when users are already signed into Microsoft</span>**

-----------------------------------------------------------------------------------------------------------------------------------------------


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


## Step 8: Auto-Sync User Profiles

Go to: **Site administration → Plugins → Authentication → OpenID Connect → Settings**

| Field       | Mapping       | Update Local         | Lock      |
|-------------|---------------|----------------------|-----------|
| **First name** | `given_name` | `On creation & login` | `Unlocked` |
| **Last name**  | `family_name`| `On creation & login` | `Unlocked` |
| **Email**      | `email`      | `On creation & login` | `Unlocked` |

> **<span style="color:red;">Note: Profile fields are synced from Azure AD on every login</span>**


-----------------------------------------------------------------------------------------------------------------------------------------------



## Step 9: Test the Flow

1. Visit: [`https://your-moodle-site`](https://your-moodle-site)

2. Click **"Login with Microsoft"**

3. **Authenticate** → Enter credentials → Complete **MFA**

4. **Success** → User auto-created in Moodle → Profile synced

5. Click **Logout** → Signed out of **Moodle + Microsoft**

> **<span style="color:red;">Warning: If login fails, check Client Secret, Redirect URI, and user assignment in Azure AD</span>**

-----------------------------------------------------------------------------------------------------------------------------------------------



<div align="center" style="background: #f8f9fa; padding: 20px; border-radius: 12px; border: 2px solid #0078D4; box-shadow: 0 4px 12px rgba(0,0,0,0.1); max-width: 600px; margin: 30px auto;">
  <h3 style="color: #00BCF2; margin: 0 0 10px;">Get in Touch</h3>
  <p style="margin: 5px 0; color: #333;">
    <strong>Harsha Jayawardhana</strong>
  </p>
  <p style="margin: 10px 0;">
    <a href="mailto:alex.rivera@example.com" style="color: #D83B01; text-decoration: none;">harshakunkuma98@gmail.com</a> • 
    <a href="https://linkedin.com/in/alexrivera" target="_blank">LinkedIn</a> • 
    <a href="https://github.com/alexrivera" target="_blank">GitHub</a>
  </p>
  <p style="margin: 5px 0; font-size: 14px; color: #555;">
    <em>Love open-source? Let’s collaborate!</em>
  </p>
</div>

