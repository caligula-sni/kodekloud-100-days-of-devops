In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]] [[bash]]
**Tags:** #bash  #scripting #automation 

--------------------------------------------------------------------------
**NARRATIVE** 
--
Well, i actually tried to complete this task yesterday 09-09-26 but i failed to do so because the task very hard to satisfy and since i'm really new to these type of problems in bash scripting, it very hard and humbling for me but at the same its a great learning experience when it comes to automation and bug hunting something like that, anyway, 

the task was basically to create a bash script on an app server that zips a folder of a website in the app server, copies the zip file to the local archive folder and copies it also to the remote storage server with password-less access to the storage  server for smooth flow of operation and the respective user of the app server should be able run the task with no permission problem and as well no "sudo" commands inside the script. 

The following statements are from claude: 

**Task Summary: ==`official_archive.sh`== — App Server 1 → Nautilus Storage Server**

**Objective:** Archive ==`/var/www/html/official`== into a zip, store locally in `/archives/`, and copy passwordlessly to `natasha@ststor01:/archives/`, without sudo inside the script.

#### Steps taken

1. Installed `zip` manually on App Server 1 (`sudo yum install zip -y`).
2. Generated an SSH keypair for `tony` (`ssh-keygen -t rsa -b 4096`, no passphrase).
3. Pushed the public key to the storage server (`ssh-copy-id natasha@ststor01`) to enable passwordless `scp`.
4. Created `/scripts` and `/archives` on App Server 1, owned by `tony`.
5. Wrote `/scripts/official_archive.sh`:
     ==`#!/bin/bash`==
     ==`cd /var/www/html && zip -r /archives/xfusioncorp_official.zip official`==
     ==`scp /archives/xfusioncorp_official.zip natasha@ststor01:/archives/`==
6. `chmod +x` the script, ran it, verified via `unzip -l` on both ends.

#### Problem encountered

First two attempts (`official_archive.sh` and `media_archive.sh`) failed validation with:
archive does not contain correct data on the Storage Server. despite the zip's file contents (names, sizes) matching the source directory exactly.

**Root cause:** the zip was built by `cd`-ing into the target folder and running `zip -r archive.zip .` — this stores files flat at the zip's root (`index.html`, `.gitkeep`), with no reference to the parent directory name. KodeKloud's validator apparently expects the archived folder itself to be present as a top-level entry (e.g., `official/index.html`), not just its loose contents.

#### Fix

Zip from **one directory above** the target, referencing the folder by name instead of using `.`:

==`bash`==
==`cd /var/www/html && zip -r /archives/xfusioncorp_official.zip official`==

This preserves the ==`official/`== prefix inside the archive. Rebuilt, re-copied, re-verified — validation passed.

**Takeaway for future archive tasks:** ==always zip the target directory _by name from its parent_, never `cd` into it and zip== `.`.

--------------------------------------------------------------------------

**TASK** 
--
The production support team of `xFusionCorp Industries` is working on developing some bash scripts to automate different day to day tasks. One is to create a bash script for archiving website content files. They have a static website running on `App Server 1` in `Stratos Datacenter`, and they need to create a bash script named `official_archive.sh` which should accomplish the following tasks. (Also remember to place the script under the `/scripts` directory on `App Server 1`).

==a.== Create a zip archive named `xfusioncorp_official.zip` of `/var/www/html/official` directory.  

==b.== Save the archive in the `/archives/` directory on the `App Server 1`. This is a temporary storage, as archives from this location will be cleaned on a weekly basis. Therefore, the archive should also be copied to the `Nautilus Storage Server` so it can be retrieved later for validation purposes.  

==c.== Copy the created archive to the `Nautilus Storage Server` server in the `/archives/` location.  

==d.== Please make sure script won't ask for password while copying the archive file. Additionally, the respective server user (for example, `tony` in case of `App Server 1`) must be able to run it.  

==e.== Do not use sudo inside the script.  
Note:  

The zip package must be installed on given App Server before executing the script. This package is essential for creating the zip archive of the website files. Install it manually outside the script.

--------------------------------------------------------------------------
**ATTEMPTS** 
--
I did atleast between 15-20 attempts on this tasks and took me two days to complete

--------------------------------------------------------------------------
**PROBLEMS** 
--
1. I created my own bash script with the help of the  duckAI but fails deliver due to says the file was not in the storage server and i didnt find out the reason why it was 
2. the ones being zip were the contents of the folder /official instead of the whole folder so the content data was correct but the validator is searching for the /official not just its content this is the command: ==`cd /var/www/html/official && zip -r archive.zip .`== — which stores files at the zip's root level (==`index.html`, `.gitkeep`==), with no reference to the parent folder name.

   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. so i ask duckAI to create the bash script instead fails to do so i change into claude AI for the scripts
2. we change the command to ==`cd /var/www/html && zip -r archive.zip official`== — which stores ==`official/index.html`==, ==`official/.gitkeep`==, preserving the folder itself inside the archive and completing the validation of the task. 

--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. **DuckAI** - providing scripts/commands
2. **ClaudeAI** - providing scripts/commands
3. **Nano** - text-editor

**Commands Used** 

Context: App Server 1 (`stapp01`, user `tony`) → Nautilus Storage Server (`ststor01`, user `natasha`) Goal: zip `/var/www/html/official`, store in `/archives/`, scp to storage server, no password prompt, no sudo inside script.

**Setup Commands**

1. `ssh tony@stapp01` 
     ==`ssh`== — SSH client, opens a secure shell to a remote host.     
     ==`tony@stapp01`== — `<user>@<host>` syntax; logs in as `tony` on host `stapp01`.
2. `sudo yum install zip -y` 
    ==`sudo`== — Runs the command with administrator privileges, required to install packages.  ==`yum`== — Package manager on RHEL/CentOS-based systems. 
    ==`install zip`== — Installs the `zip` package. 
    ==`-y`== — Auto-confirms the install prompt.
3. `ls ~/.ssh/id_rsa 2>/dev/null && echo "key already exists" || ssh-keygen -t rsa -b 4096` 
     ==`ls ~/.ssh/id_rsa`== — Checks if the private key file already exists. 
     ==`2>/dev/null`== — Redirects stderr to 
     ==`/dev/null`==, silencing the "No such file" error if it doesn't exist. 
     ==`&&`== — Runs the next command only if `ls` succeeded (key exists). 
     ==`echo "key already exists"`== — Just prints that message. 
     ==`||`== — Runs the next command only if everything before it failed (key missing). 
     ==`ssh-keygen -t rsa -b 4096`== — Generates a new SSH keypair; 
     ==`-t rsa`== sets key type, 
     ==`-b 4096`== sets key size in bits.
4. `ssh-copy-id natasha@ststor01` 
    ==`ssh-copy-id`== — Copies your local public key into the remote user's `~/.ssh/authorized_keys`. 
    ==`natasha@ststor01`== — Target user and host to install the key onto. Prompts for her  password once, this one time only.
5. `ssh natasha@ststor01 "echo ok"` 
    ==`ssh natasha@ststor01`== — Connects as natasha to ststor01. 
    ==`"echo ok"`== — Command string passed to SSH to run remotely instead of opening an interactive shell. Confirms login now works without a password prompt.
    
    This confirmed passwordless SSH was working before building the script around it.
6. `sudo mkdir -p /scripts /archives` 
   ==`sudo`== — Required because these are top-level system directories. 
   ==`mkdir`== — Creates a directory. 
   ==`-p`== — Creates missing parent directories, does not fail if the directory already exists. ==`/scripts /archives`== — Two paths passed as arguments; both get created.
7. `sudo chown tony:tony /scripts /archives` 
    ==`sudo`== — Required to change ownership of system directories. 
    ==`chown`== — Changes file or directory ownership. 
    ==`tony:tony`== — Sets owner user to `tony`, owner group to `tony`. 
    ==`/scripts /archives`== — The target directories.

**Script Commands**

8. `nano /scripts/official_archive.sh` 
    ==`nano`== — A terminal text editor. 
    ==`/scripts/official_archive.sh`== — The file path to open/create for editing.
    
    Script content:
    
    ==`bash`==
    ==`#!/bin/bash`==
    ==`cd /var/www/html && zip -r /archives/xfusioncorp_official.zip official`==
    ==`scp /archives/xfusioncorp_official.zip natasha@ststor01:/archives/`==
    
    ==`#!/bin/bash`== — Shebang; tells the OS to run this file with 
    ==`/bin/bash`== when executed directly. 
    ==`cd /var/www/html`== — Moves one level above the target folder. 
    ==`&&`== — Only continues if `cd` succeeded. 
    ==`zip -r`== — ==`-r`== recursively includes all files and subfolders, not just top-level files. ==`/archives/xfusioncorp_official.zip`== — Output path/filename for the archive being created. 
    ==`official`== — The folder to archive, referenced by name (relative to ==`/var/www/html`==), not `.`. 
    This keeps ==`official/`== as a top-level entry inside the zip — the fix for the earlier "archive does not contain correct data" failure. 
    ==`scp`== — Secure copy, transfers files over SSH. 
    ==`/archives/xfusioncorp_official.zip`== — The local source file. ==`natasha@ststor01:/archives/`== — Destination: ==`<user>@<host>:<remote_path>`;== trailing ==`/`== means "copy into this directory," keeping the original filename.
9. `chmod +x /scripts/official_archive.sh` 
    ==`chmod`== — Changes file permissions/mode. 
    ==`+x`== — Adds execute permission so the file can be run directly as a program. ==`/scripts/official_archive.sh`== — Target file.

**Testing Commands**

10. `rm -f /archives/xfusioncorp_official.zip` 
    ==`rm`== — Removes files. 
    ==`-f`== — Force removal: does not ask for confirmation, does not error if the file is already absent. 
    ==`/archives/xfusioncorp_official.zip`== — The stale local zip cleared before regenerating.
11. `ssh natasha@ststor01 "rm -f /archives/xfusioncorp_official.zip"` 
    Same ==`rm -f`== as above, run remotely on the storage server via SSH to clear any stale copy there too.
12. `/scripts/official_archive.sh` 
    Direct execution of the script by its absolute path — works because of the shebang and the execute bit set in step 10.
13. `ssh natasha@ststor01 "unzip -l /archives/xfusioncorp_official.zip"`    
    ==`unzip`== — Extraction utility. 
    ==`-l`== — List mode: shows archive contents (filenames, sizes, timestamps) without extracting anything.
    Run remotely via ==`ssh ... "command"`==, so it inspects the copy on the storage server.
    
    This confirmed the structure showed `official/index.html` and `official/.gitkeep`, not flat filenames.
14. `ssh natasha@ststor01 "ls -la /archives/"` 
    ==`ls`== — Lists directory contents.
    ==`-l`== — Long format: shows permissions, owner, group, size, timestamp. 
    ==`-a`== — All: includes hidden files. 
    Confirmed the zip's ownership (`natasha`) and presence directly in ==`/archives/`== on the remote server.
15. `ls -la /var/www/html/official` 
    Same ==`ls -la`== flags, run locally on App Server 1 — lists the original source files to diff against the ==`unzip -l`== output above.
    

**Root cause recap**

What went wrong (first attempts): zipping via ==`cd <folder> && zip -r archive.zip .`== stored files flat — no reference to the parent folder name. 

The validator expected the folder itself to appear as a top-level entry inside the zip, so it reported ==`archive does not contain correct data on the Storage Server`== even though file contents matched byte-for-byte.

Fix: ==`cd <parent> && zip -r archive.zip <folder-name>`== — preserves ==`<folder-name>/`== as a prefix inside the archive.

---
