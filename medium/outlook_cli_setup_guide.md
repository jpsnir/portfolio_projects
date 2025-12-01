# Outlook on the Command Line: A Setup Guide for CLI for Microsoft 365

This guide explains how to set up the "CLI for Microsoft 365", a powerful command-line tool for interacting with Microsoft 365 services, including Outlook Calendar, on a Linux system.

## Part 1: Installation

### 1.1. Prerequisite: Check for `npm`
The CLI for Microsoft 365 is installed using `npm` (Node Package Manager). First, check if it's installed:
```bash
npm --version
```
If this command returns a version number, you can proceed. If not, you will need to install Node.js and npm first.

### 1.2. Install the CLI
Use `npm` to install the package globally on your system. The `-g` flag makes the `m365` command available system-wide.
```bash
npm install -g @pnp/cli-microsoft365
```


## Part 2: Azure App Registration

Similar to Google, the CLI needs a unique Application ID to identify itself to Microsoft's services. You create this in the Azure portal.

### 2.1. Navigate to App Registrations
Go to the [Azure portal App Registrations page](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) and sign in.

### 2.2. Create a New Registration
1.  Click **"+ New registration"**.
2.  On the registration page:
    *   **Name:** Give it a descriptive name (e.g., "MyM365CLI").
    *   **Supported account types:** Select the option for **"Accounts in any organizational directory... and personal Microsoft accounts..."**.
    *   **Redirect URI:** Select **"Public client/native (mobile & desktop)"** and use `http://localhost`.
3.  Click **"Register"**.

### 2.3. Copy the Application (client) ID
From the "Overview" page of your new app registration, copy the **"Application (client) ID"**. You will need this for the setup.



## Part 3: Configuration and Login

### 3.1. Initial Setup
The CLI has a setup command to configure defaults. Running it interactively is the easiest way to provide your Application ID.
```bash
m365 setup
```
Follow the prompts, and when asked, provide the **Application (client) ID** you copied in the previous step.

### 3.2. Create a Client Secret
The login process also requires a "Client Secret".
1.  In your Azure App Registration, go to the **"Certificates & secrets"** section.
2.  Click **"+ New client secret"**.
3.  Give it a description, choose an expiration period, and click **"Add"**.
4.  **Immediately copy the "Value"** of the secret. It is only shown once.

### 3.3. Final Login
Now, perform the login using the `"secret"` authentication type, providing both your App ID and the secret you just created.

```bash
m365 login --authType secret --appId YOUR_APP_ID_HERE --secret YOUR_CLIENT_SECRET_HERE
```
Replace the placeholders with your actual credentials. If successful, this will authenticate the CLI.



## Part 4: Basic Usage (Example)

Once you are successfully logged in, you can start interacting with your Outlook calendar. For example, to get a list of events from your calendar for a specific period, you would use a command like this:

```bash
m365 outlook calendar event list --startDateTime 2025-11-29T00:00:00Z --endDateTime 2025-12-05T00:00:00Z
```
You can adjust the dates to specify the time range you are interested in.


---

## Next Steps (as of 2025-11-29)

This setup is currently paused. The last step was to create a **Client Secret**. The next step is to run the final login command below, replacing the placeholders with your credentials.

```bash
m365 login --authType secret --appId YOUR_APP_ID_HERE --secret YOUR_CLIENT_SECRET_HERE
```

