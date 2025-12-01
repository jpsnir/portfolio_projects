# Gcalcli Setup Guide: A Complete Walkthrough

This guide documents the process of setting up `gcalcli` on a Linux system, including creating an isolated Python environment to avoid system conflicts, generating the necessary Google API credentials, and creating a convenient wrapper script for easy execution.

## Part 1: Installing `pyenv` and a Supported Python Version

The core of the problem with `gcalcli` is that its dependencies require a modern version of Python. Using `pyenv` allows us to install a new Python version without affecting the system's default Python.

### 1.1. Install `pyenv`
`pyenv` is a Python version manager. It's installed using the following command:
```bash
curl https://pyenv.run | bash
```

### 1.2. Configure Your Shell for `pyenv`
For `pyenv` to work, its initialization script must be added to your shell's startup file (`~/.bashrc` for bash).
```bash
echo -e '
# Pyenv configuration
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
```
After this, you need to restart your shell for the changes to take effect.

### 1.3. Install a Supported Python Version
We will install Python 3.11.5, which is compatible with `gcalcli` and its dependencies.
```bash
pyenv install 3.11.5
```


## Part 2: Setting Up the `gcalcli` Environment

### 2.1. Create a `pyenv` Virtual Environment
This creates an isolated environment named `gcalcli-env` using the Python version we just installed.
```bash
pyenv virtualenv 3.11.5 gcalcli-env
```

### 2.2. Install `gcalcli`
We install `gcalcli` directly into this new environment.
```bash
~/.pyenv/versions/gcalcli-env/bin/pip install gcalcli
```


## Part 3: Google API Credentials Setup

`gcalcli` needs to be authorized to access your Google Calendar. This requires creating personal API credentials.

### 3.1. Create a Google Cloud Project
Go to the [Google Cloud Console Project Creator](https://console.cloud.google.com/projectcreate) and create a new project.

### 3.2. Enable the Google Calendar API
1.  Navigate to the [API Library](https://console.cloud.google.com/apis/library).
2.  Search for "Google Calendar API" and enable it for your project.

### 3.3. Create OAuth 2.0 Credentials
1.  Navigate to the [Credentials page](https://console.cloud.google.com/apis/credentials).
2.  Click **"+ CREATE CREDENTIALS"** -> **"OAuth client ID"**.
3.  If prompted, configure the **OAuth consent screen**:
    *   Choose **"External"**.
    *   Fill in the required fields (App name, support email, developer contact).
    *   Save and continue through the other steps.
4.  Create the OAuth client ID:
    *   **Application type:** "Desktop app".
    *   Click **"Create"**.
5.  Copy the **"Client ID"** and **"Client Secret"** that appear.

### 3.4. Publish the App
To avoid an `access_denied` error, publish your app:
1.  Go to the [OAuth consent screen](https://console.cloud.google.com/apis/credentials/consent).
2.  Click **"PUBLISH APP"**.


## Part 4: Creating a Convenient Runner Script

To avoid manually activating the `pyenv` environment each time, we create a wrapper script.

### 4.1. Create the Script
Create a file named `gcal` in `~/.local/bin/` with the following content:
```bash
#!/bin/bash
# Activate the pyenv virtual environment
export PYENV_VERSION="gcalcli-env"

# Run the gcalcli command with all provided arguments
~/.pyenv/versions/gcalcli-env/bin/gcalcli "$@"
```

### 4.2. Make the Script Executable
```bash
chmod +x ~/.local/bin/gcal
```


## Part 5: First-Time Authentication

Now, run the authentication command using your new script and the credentials from Part 3.

```bash
gcal auth --client-id YOUR_CLIENT_ID_HERE --client-secret YOUR_CLIENT_SECRET_HERE
```
Follow the instructions in your browser to complete the authentication.

## Part 6: Basic Usage

You can now use `gcal` to interact with your calendar.
```bash
# List all calendars
gcal list

# Show today's agenda
gcal agenda
```

## Part 7: Desktop Notifications (Optional)

You can set up a `cron` job to get desktop reminders.

1.  Open your crontab: `crontab -e`
2.  Add this line, replacing `"Your Calendar Name"` with a name from `gcal list`:
```
*/5 * * * * DISPLAY=:0 DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/$(id -u)/bus /home/jagatpreet/.local/bin/gcal --calendar "Your Calendar Name" remind | xargs -I {} notify-send "Upcoming Event" "{}"
```
This will check for upcoming events every 5 minutes and create a desktop pop-up.

