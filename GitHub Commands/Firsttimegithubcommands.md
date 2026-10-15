# GitHub SSH Setup & Git Workflow Guide (Git Bash)

A step-by-step guide to setting up SSH keys, configuring the SSH agent, resolving port blockages, and managing daily Git workflows using **Git Bash**.

---

## 🛠️ Step-by-Step SSH Setup

### Step 1: Check for Existing SSH Keys
Check if you already have an SSH key pair generated on your machine:
```bash
ls -al ~/.ssh
```
> **Note:** If you see files named `id_ed25519.pub` or `id_rsa.pub`, an SSH key pair already exists.

---

### Step 2: Copy the Public SSH Key
Display the public key in your terminal to copy it manually:

* **For Ed25519 key:**
  ```bash
  cat ~/.ssh/id_ed25519.pub
  ```
* **For RSA key:**
  ```bash
  cat ~/.ssh/id_rsa.pub
  ```

**Or copy directly to your clipboard (Windows):**
```bash
clip < ~/.ssh/id_ed25519.pub
```

---

### Step 3: Add Key to GitHub
1. Go to **GitHub** $\rightarrow$ **Settings** $\rightarrow$ **SSH and GPG keys**.
2. Click **New SSH Key**.
3. Paste your public key and click **Add SSH key**.

---

### Step 4: Add Private Key to SSH Agent
Avoid typing your passphrase every time by adding your key to the SSH agent:

1. **Start the SSH agent in the background:**
   ```bash
   eval "$(ssh-agent -s)"
   ```
2. **Add your private key:**
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```

---

### Step 5: Test the Connection
Verify that GitHub can authenticate your key:
```bash
ssh -T git@github.com
```

---

### Step 6: Auto-Start SSH Agent in Git Bash
To automatically start `ssh-agent` and load your keys every time Git Bash opens:

1. **Open the Bash configuration file:**
   ```bash
   nano ~/.bashrc
   ```
2. **Paste the following auto-start script:**
   ```bash
   # Auto-launch the ssh-agent and load all private keys on Git for Windows
   env=~/.ssh/agent.env

   agent_load_env () { 
       test -f "$env" && . "$env" >| /dev/null ; 
   }

   agent_start () { 
       (umask 077; ssh-agent >| "$env")
       . "$env" >| /dev/null ; 
   }

   agent_load_env

   # agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
   agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

   if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
       agent_start
       ssh-add
   elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
       ssh-add
   fi

   unset env
   ```
3. **Save and exit `nano`:**
   * Press `Ctrl + O`, then hit `Enter` to save.
   * Press `Ctrl + X` to exit.

---

### Step 7: Apply Configuration Changes
Reload `.bashrc` without restarting Git Bash:
```bash
source ~/.bashrc
```

---

## 🔒 Troubleshooting Connection Timeouts (Port 443 Fallback)

If running `ssh -T git@github.com` results in a connection timeout, port 22 is likely blocked by your firewall. You can route SSH traffic over HTTPS port 443 instead.

### Step 8 & 9: Test Connectivity over Port 443
```bash
ssh -T -p 443 git@ssh.github.com
```

### Step 10: Permanently Configure SSH for Port 443
1. **Open or create the SSH config file:**
   ```bash
   nano ~/.ssh/config
   ```
2. **Paste the following configuration:**
   ```config
   Host github.com
       Hostname ssh.github.com
       Port 443
       User git
   ```
3. **Save and exit `nano`:**
   * Press `Ctrl + O`, hit `Enter`, then press `Ctrl + X`.

### Step 11: Retest Connection
```bash
ssh -T git@github.com
```

---

## 🚀 Everyday Git Workflow

### 1. Clone the Repository
```bash
git clone git@github.com:chandantaluja/Lane-Keep-Assistance_ClassicAutosar_Fusa.git
```

### 2. Check Repository Status
```bash
git status
```

### 3. Commit Changes
```bash
git commit -m "Adding git commands that are used"
```

### 4. Push to Main Branch
```bash
git push origin main
```