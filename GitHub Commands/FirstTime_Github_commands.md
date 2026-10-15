Use Gitbash for Repo:

Step 1: To check if SSH key is installed:
$ ls -al ~/.ssh  
This will tell if SSH key is already genenrated.
If the list shows things like id_ed25519.pub or id_rsa.pub, this means that we already have an SSH key pair.

Step 2: To copy the public SSH key
To copy the contents of public key file, run this command to display the key in GitBash:
$ cat ~/.ssh/id_ed25519.pub
If RSA key is generated then use:
$ cat ~/.ssh/id_rsa.pub
To copy the key to clipboard automatically use:
$ clip < ~/.ssh/id_ed25519.pub

Step 3: Paste this key into Github

Step 4: Stop Entering passphrase everytie
Start SSH agent in the background
$ eval "$(ssh-agent -s)"
Add private key to agent:
$ ssh-add ~/.ssh/id_ed25519

Step 5: Test the connection:
$ ssh -T git@github.com


Step 6: Make sure that SSH agent is auto restarted every time Gitbash is opened
Open Bash configuration file:
$ nano ~/.bashrc
Paste the autostart script:
# Auto-launch the ssh-agent and load all private keys on Git for Windows
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

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

Press Ctrl + O then press Enter to save the file.
Press Ctrl +X to exit the nano editor


Step 7: Reload Gitbas to apply changes without restarting 
$ source ~/.bashrc

Step 8: Run the Github SSH Test
$ ssh -T git@github.com
If it gives connection timeout, this means port 22 is blocked by firewall.
We can use port 443.

Step 9: Test Port 443 connectivity
$ssh -T -P 443 git@ssh.github.com

Step 10: Configure SSH to use Port 443 permanently
Open the SSH config file 
$ nano ~/.ssh/config
Paste the following text.

Host github.com
Hostname ssh.github.com
Port 443
User git
Press Ctrl + O then press Enter to save the file.
Press Ctrl +X to exit the nano editor

Step 11: Retest normal connection
$ssh -T git@github.com
