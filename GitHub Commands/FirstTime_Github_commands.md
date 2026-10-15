# GitHub SSH Setup & Git Workflow Guide (Git Bash)

A step-by-step guide to setting up SSH keys, configuring the SSH agent, resolving port blockages, and managing daily Git workflows using **Git Bash**.

---

## 🛠️ Step-by-Step SSH Setup

### Step 1: Check for Existing SSH Keys
Check if you already have an SSH key pair generated on your machine:
```bash
ls -al ~/.ssh

Note: If you see files named id_ed25519.pub or id_rsa.pub, an SSH key pair already exists.
Step 2: Copy the Public SSH Key
Display the public key in your terminal to copy it manually:
For Ed25519 key: Bashcat ~/.ssh/id_ed25519.pub
For RSA key: Bashcat ~/.ssh/id_rsa.pub
Or copy directly to your clipboard (Windows): Bashclip < ~/.ssh/id_ed25519.pub
Step 3: Add Key to GitHubGo to GitHub $\rightarrow$ Settings $\rightarrow$ SSH and GPG keys.Click New SSH Key.Paste your public key and click Add SSH key.Step 4: Add Private Key to SSH AgentAvoid typing your passphrase every time by adding your key to the SSH agent:Start the SSH agent in the background:Basheval "$(ssh-agent -s)"
Add your private key:Bashssh-add ~/.ssh/id_ed25519
Step 5: Test the ConnectionVerify that GitHub can authenticate your key:Bashssh -T git@github.com
Step 6: Auto-Start SSH Agent in Git BashTo automatically start ssh-agent and load your keys every time Git Bash opens:Open the Bash configuration file:Bashnano ~/.bashrc
Paste the following auto-start script:Bash# Auto-launch the ssh-agent and load all private keys on Git for Windows
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
Save and exit nano:Press Ctrl + O, then hit Enter to save.Press Ctrl + X to exit.Step 7: Apply Configuration ChangesReload .bashrc without restarting Git Bash:Bashsource ~/.bashrc
🔒 Troubleshooting Connection Timeouts (Port 443 Fallback)If running ssh -T git@github.com results in a connection timeout, port 22 is likely blocked by your firewall. You can route SSH traffic over HTTPS port 443 instead.Step 8 & 9: Test Connectivity over Port 443Bashssh -T -p 443 git@ssh.github.com
Step 10: Permanently Configure SSH for Port 443Open or create the SSH config file:Bashnano ~/.ssh/config
Paste the following configuration:Code snippetHost github.com
    Hostname ssh.github.com
    Port 443
    User git
Save and exit nano:Press Ctrl + O, hit Enter, then press Ctrl + X.Step 11: Retest ConnectionBashssh -T git@github.com
🚀 Everyday Git Workflow1. Clone the RepositoryBashgit clone git@github.com:chandantaluja/Lane-Keep-Assistance_ClassicAutosar_Fusa.git
2. Check Repository StatusBashgit status
3. Commit ChangesBashgit commit -m "Adding git commands that are used"
4. Push to Main BranchBashgit push origin main

---

### How to update your README file via terminal:

1. **Open `README.md` in nano:**
   ```bash
   nano README.md
Paste the raw Markdown content inside.Save and close (Ctrl + O $\rightarrow$ Enter $\rightarrow$ Ctrl + X).Commit and push:Bashgit add README.md
git commit -m "Update README with SSH setup guide"
git push origin main
