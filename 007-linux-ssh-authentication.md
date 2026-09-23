In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]  [[bash]]
**Tags:**  #bash #ssh 

--------------------------------------------------------------------------
**NARRATIVE** 
--

Today was the 7th task and it was one of the most new to me, the idea of the task was for user thor of jump host to create a password-less ssh authentication for all 3 sudo users from the 3 app servers which are steve, tony and banner.  

So for context, this is the whole situation  

The system admins team of xFusionCorp Industries has set up some scripts on jump host that run on regular intervals and perform operations on all app servers in Stratos Datacenter. To make these scripts work properly we need to make sure the thor user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e tony for app server 1). Based on the requirements, perform the following:  

**Task:**  
Set up a password-less authentication from user thor on jump host to all app servers through their respective sudo users.  

So the purpose of the task was to ensure the smooth operation of the said scripts from jump host servers into the app servers.  

I didnt have any idea on how to do this since i am new to ssh configurations and key generation so i ask for help using duckAI once again.  

The steps it gaved were three  

**Generate Password-less SSH key**  
using this command: `ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa`  
which is the exact key generation command that creates two files  
`~/.ssh/id_rsa # Private key`  
`~/.ssh/id_rsa.pub # Public key`  

The private key must remain secret. The public key is copied to the servers.  
When ssh-keygen asks for a passphrase, leaving it empty allows automated scripts to use the key without an interactive passphrase prompt.  

`chmod 700 ~/.ssh`  
`chmod 600 ~/.ssh/id_rsa`  
`chmod 644 ~/.ssh/id_rsa.pub`  
these commands to change and modify the keys for public use of ssh keys  

**Copying the public key**   

`ssh-copy-id -i ~/.ssh/id_rsa.pub tony@stapp01`  
The command normally asks for Tony’s password once. It then adds your public key to: **/home/tony/.ssh/authorized_keys**  
After that, SSH can authenticate using your private key on the jump host.  
These commands perform the same operation for the other servers:  

**Testing the Connection**  

`ssh -o PasswordAuthentication=no tony@stapp01 hostname`  
Normally, this command would open an interactive shell. Because hostname is included, SSH instead runs that command on the remote server and displays its hostname, then disconnects. 
 
The important part is:  
`-o PasswordAuthentication=no`  

It verifies that key authentication works. If the key is configured correctly, the command succeeds without asking for a password. If key authentication is not working, SSH fails instead of falling back to a password prompt.

--------------------------------------------------------------------------

**TASK** 
--
Set up a password-less authentication from user thor on jump host to all app servers through their respective sudo users.  


--------------------------------------------------------------------------
**ATTEMPTS** 
--
I only needed 1 attempt in order to complete the task with the help of duckAI

--------------------------------------------------------------------------
**PROBLEMS** 
--
1. I didnt know how to how to create or generate a ssh key gen with no password  
2. I didnt know how to connect give thor from jump host to have password less access to the app servers using the password less key gen  
3. I didnt know how test the connection of the user jump host to the app servers
   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. I use duckAI to give me commands on how to create or generate a ssh key gen with no password  
2. I use duckAI to give me the commands on how to give thor from jump host to have password less access to the app servers using the password less key gen  
3. I use duckAI to give me commands on how to test the connection of the user jump host to the app servers

--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. **DuckAI** - used to provide me the right commands  
2. **ssh keygen/ssh-copy-id** - used to generate and configure SSH keys  
3. **Chmod** - used to change the public and private keys ownership and permissions
   
**Commands Used** 

*Password-less SSH key generation command:* 
1. `ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa`  
    
    `ssh-keygen` - creates and manages SSH authentication keys.  
    `-t rsa` - specifies the key type: RSA.  
    `-b 4096` - specifies the key size: 4,096 bits. A larger key generally provides stronger RSA security.  
    `-f ~/.ssh/id_rsa` - specifies where to save the key.  
    **~** means the current user’s home directory.  
    For thor, it is usually /home/thor.  
    `.ssh-` is the directory used for SSH configuration and keys.  
    `id_rsa` - is the private-key filename.  

    This creates two files:  

   `~/.ssh/id_rsa # Private key`  
   `~/.ssh/id_rsa.pub # Public key`  
   
   since this keygen are passwordless, the following 2 prompts should be left blank  
   
*The three permission commands:* 
2. `chmod 700 ~/.ssh`  
    `chmod` - changes permissions.  
    The number 700 means:  
    `First 7`: the owner has read, write, and execute permissions.  
    `Second 0`: the group has no permissions.  
    `Third 0`: other users have no permissions.  
    
    For a directory, execute means the owner can enter or access files inside it. 
    The .ssh directory should not be accessible to other users.  

2. `chmod 600 ~/.ssh/id_rsa`  
   
    This protects the private key (id_rsa)  
    
    **600 means:**  
    6 Owner: read and write  
    0 Group: no permissions  
    0 Other users: no permissions  
    
    The private key must not be readable by other users besides the owner who can read and write permissions in it.  

2. `chmod 644 ~/.ssh/id_rsa.pub`  
   
    This sets permissions on the public key.  
    
    644 means:  
    6 Owner: read and write  
    4 Group: read  
    4 Other users: read  
    
   It is safe for the public key to be readable because it is not secret.  

*The three permission commands can be summarized as:*  
1. **.ssh directory 700**  - `chmod 700 ~/.ssh`  
2. **Private key 600**  - `chmod 600 ~/.ssh/id_rsa`
3. **Public key 644**  - `chmod 644 ~/.ssh/id_rsa.pub` 

*Copying the public to app server users:* 
5. `ssh-copy-id -i ~/.ssh/id_rsa.pub tony@stapp01`  
   
    `ssh-copy-id` - installs your public SSH key on a remote account.  
    `-i ~/.ssh/id_rsa.pub` - specifies the public-key file to copy.  
    `tony@stapp01` - identifies the remote login:  
    `tony` - is the username on the remote server.  
    `@` - separates the username from the server name.  
    `stapp01` - is the hostname of the first app server.  
    
    The command normally asks for Tony’s password once.  
    It then adds your public key to: `/home/tony/.ssh/authorized_keys`  
    
    After that, SSH can authenticate using your private key on the jump host.  

*Testing the connection between jump host and app server* 
6. `ssh -o PasswordAuthentication=no tony@stapp01 hostname`  
   
    `ssh` - starts a secure remote shell connection.  
    `-o` - allows you to provide an SSH configuration option directly on the command line.  
    `PasswordAuthentication=no` - tells SSH not to use password authentication.  
    `tony@stapp01` - means connect to stapp01 as user tony.  
    `hostname` - is the remote command to execute.  
    
    The important part is: `-o PasswordAuthentication=no`  
    
    It verifies that key authentication works. If the key is configured correctly, the command succeeds without asking for a password.  
    
    If key authentication is not working, SSH fails instead of falling back to a password prompt.