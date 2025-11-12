
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




# This is a basic workflow to help you get started with Actions

name: README.md

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the "main" branch
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v4

      # Runs a single command using the runners shell
      - name: Run a one-line script
        run: echo Hello, world!

      # Runs a set of commands using the runners shell
      - name: Run a multi-line script
        run: |
          echo Add other actions to build,
          echo test, and deploy your project.
