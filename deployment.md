# Azure App Service Deployment Guide

Follow these steps to deploy your Flask application to Azure Web Apps.

## Step 1: Create the Web App
1. Search for **"App Services"** in the Azure Portal.
2. Click **+ Create**.
3. Select **Web App** (NOT "Web App + Database" - we already have a database!).
4. **Basics Tab**:
    - **Subscription**: Select yours.
    - **Resource Group**: Select `postcard-app-rg` (the same one we used before).
    - **Name**: Enter a unique name (e.g., `postcard-app-efe-2026`).
    - **Publish**: Select **Code**.
    - **Runtime stack**: Select **Python 3.11** (or 3.12 if you prefer).
    - **Operating System**: **Linux**.
    - **Region**: Same as before (e.g., `West Europe`).
    - **Pricing Plan**:
        - Click **Change size** (or "Explore pricing plans").
        - Select **Free F1** (if available) or **Basic B1** (for better performance/testing).
4. Click **Review + create**, then **Create**.
5. Wait for deployment, then **Go to resource**.

## Step 2: Configure Environment Variables
The cloud server needs to know your secrets (Database URL, Storage Key) just like your local computer did.

1. On your Web App page, look at the left menu under **Settings**.
2. Click **Environment variables**.
3. Click **+ Add** (or "Add parameter") for each of these settings:
    
    | Name | Value |
    |------|-------|
    | `DATABASE_URL` | *Copy this from your local `.env` file* |
    | `AZURE_STORAGE_CONNECTION_STRING` | *Copy this from your local `.env` file* |
    | `AZURE_CONTAINER_NAME` | `postcards` |
    | `SCM_DO_BUILD_DURING_DEPLOYMENT` | `true` |

4. Click **Apply** then **Confirm/Save** at the bottom (Azure UI varies slightly here, ensure they are saved!).

## Step 3: Configure Startup Command (Optional)
*Note: Azure often detects Flask apps automatically. If your deployment fails, try adding this.*

1. In the left menu, under **Settings**, click **Configuration**.
2. Go to the **General settings** tab.
3. Find **Startup Command**.
4. Enter this command:
    ```bash
    python -m flask run --host=0.0.0.0 --port=8000
    ```
5. Click **Save**.

## Step 4: Deploy from GitHub
1. In the left menu, under **Deployment**, click **Deployment Center**.
2. **Source**: Select **GitHub**.
3. **Authorize** your GitHub account if needed.
4. **Organization**: Select your username.
5. **Repository**: Select `cloud_project`.
6. **Branch**: `main`.
7. **Authentication**: Select **Basic authentication** (Simpler for students).
8. Click **Save**.

Azure will now pull your code and start building it.
- Click on the **Logs** tab in Deployment Center to watch the progress.
- Once it says "Success", go to the **Overview** page and click the **Default domain** link to see your app live!
