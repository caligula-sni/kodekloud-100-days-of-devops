In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]
**Tags:** #bash #tomcat-server 

--------------------------------------------------------------------------
**NARRATIVE** 
--
So well, i guess the task were really getting hard now because this one i thought to be simple enough for me to do alone without but welp i needed claude and we actually did atleast 3-5 attempts on this one, The task was install and configure a tomcat server in given app server and also deploy a given war file on it, so many times i thought i already had it but still an error persist which is the tomcat not configure or setup properly so i had to redo it at times. 

This is claude's analysis of the task: 

Task Summary

**Task:**

- Deploy the Nautilus team's beta Java application on Tomcat, on **App Server 3** (`stapp03`)
- Configure Tomcat to run on port **8087**
- Deploy the provided `ROOT.war` (staged on Jump Host at `/tmp`) so it's reachable directly at the base URL — i.e. ==`curl http://stapp03:8087`== returns the app itself, not a subpath

1. The task began on the jump host, where the war file first had to be transferred over to App Server 3 using ==`scp /tmp/ROOT.war banner@stapp03:/tmp/`==, copying the file securely to the target server's `/tmp` directory under the correct user, `banner`. From there, a remote session was opened into the target server itself with `ssh banner@stapp03`.

2. Once logged into `stapp03`, the next step was checking whether Tomcat was already installed, using ==`rpm -qa | grep -i tomcat`==. This came back empty, confirming a fresh install was needed, so Tomcat and its bundled web apps were installed with ==`sudo yum install -y tomcat tomcat-webapps tomcat-admin-webapps`==.

3. With Tomcat installed, the port configuration came next. Running ==`sudo grep -n "Connector port" /etc/tomcat/server.xml`== located every connector block in the config file along with their line numbers, which made it possible to identify the correct HTTP connector rather than one of the HTTPS ones. The file was then opened with ==`sudo nano /etc/tomcat/server.xml`== to change that connector's port from the ==`default 8080 to 8087`==, and the change was verified afterward by re-running ==`grep -n "Connector port" /etc/tomcat/server.xml`== to confirm the file now reflected port 8087.

4. Before touching the deployment itself, Tomcat was stopped with ==`sudo systemctl stop tomcat`== to avoid a partial or inconsistent deploy while files were being changed underneath it. The stale default application directory was then removed with ==`sudo rm -rf /var/lib/tomcat/webapps/ROOT`==, clearing the way for the new war to deploy properly. The actual `ROOT.war` file was copied into place with ==`sudo cp /tmp/ROOT.war /var/lib/tomcat/webapps/`==, and Tomcat was brought back up with ==`sudo systemctl start tomcat`==, which triggers it to detect and auto-extract the new war on startup.

5. A short pause was added with ==`sleep 5`== to give Tomcat enough time to fully extract the war before checking on it, followed by ==`ls /var/lib/tomcat/webapps/`== to confirm that a fresh ==`ROOT/`== directory had regenerated alongside the war file, indicating the extraction actually happened this time.

6. Finally, the deployment was verified in two stages: first ==locally on the server== itself with ==`curl http://localhost:8087`==, which returned the actual Nautilus application content rather than Tomcat's default page, confirming the app and config were correct independent of any network factors. Then, from the ==jump host==, ==`curl http://stapp03:8087`== was run to replicate exactly what KodeKloud's validator checks — and it returned the same correct application output, confirming the deployment was reachable externally and the task was genuinely complete.

## Issues & Fixes



**1. `curl` returned HTML but the task still failed validation**

- Root cause: HTML output alone isn't proof of a correct deploy — it can just as easily be Tomcat's generic default page if the real app never got extracted
- Fix: manually inspected the returned HTML content and compared it against expected app output; identified it as the stock Tomcat welcome page rather than the Nautilus app

**3. War file wasn't deploying to the base URL despite being copied to `webapps/`**

- Root cause: an existing `ROOT/` directory from the default Tomcat install already occupied that deployment slot, blocking the new `.war` from auto-exploding
- Fix: stopped the Tomcat service, force-removed the stale `ROOT/` directory (`rm -rf`), recopied `ROOT.war` into a clean `webapps/` folder, then restarted Tomcat so it extracted the new war from scratch

**4. Uncertain which `webapps/` path Tomcat was actually using**

- Root cause: Tomcat installed via `yum` uses `/var/lib/tomcat/webapps/`, while a manually-installed tarball version uses `/usr/local/tomcat/webapps/` instead — using the wrong path means the war is copied somewhere Tomcat never looks
- Fix: confirmed the install method first (`rpm -qa | grep tomcat`) before copying anything, establishing this was a yum install and the correct path was `/var/lib/tomcat/webapps/`

--------------------------------------------------------------------------

**TASK** 
--
lets redo it with that in mind, the user here in app server 3 is banner: The `Nautilus` application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in `Stratos DC`. After an internal team meeting, they have decided to use the `tomcat` application server. Based on the requirements mentioned below complete the task:

a. Install `tomcat` server on `App Server 3`.

b. Configure it to run on port `8087`.

c. There is a `ROOT.war` file on `Jump host` at location `/tmp`.  
Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e `curl http://stapp03:8087`

--------------------------------------------------------------------------
**ATTEMPTS** 
--
i did atleast 5 times for this task. 

--------------------------------------------------------------------------
**PROBLEMS** 
--
 1. ==`curl`== returned HTML but the task still failed validation
    Root cause: HTML output alone isn't proof of a correct deploy — it can just as easily be Tomcat's generic default page if the real app never got extracted
 2. ==`.war`== file wasn't deploying to the base URL despite being copied to `webapps/
    Root cause: an existing ==`ROOT/`== directory from the default Tomcat install already occupied that deployment slot, blocking the new ==`.war`== from auto-exploding
3.  uncertain which ==`webapps/`== path Tomcat was actually using
    Root cause: Tomcat installed via ==`yum` uses `/var/lib/tomcat/webapps/`==, while a manually-installed tarball version uses ==`/usr/local/tomcat/webapps/`== instead — using the wrong path means the war is copied somewhere Tomcat never looks

   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. Fix: manually inspected the returned HTML content and compared it against expected app output; identified it as the stock Tomcat welcome page rather than the Nautilus app
2. Fix: stopped the Tomcat service, force-removed the stale ==`ROOT/`== directory (==`rm -rf`==), recopied ==`ROOT.war`== into a clean `webapps/` folder, then restarted Tomcat so it extracted the new war from scratch
3. Fix: confirmed the install method first (==`rpm -qa | grep tomcat`==) before copying anything, establishing this was a yum install and the correct path was ==`/var/lib/tomcat/webapps/`==

--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. **ClaudeAI** - for providing commands
2. **Tomcat Server** - the server for the root.war
3. **Nano** - editing server config files
4. **SSH** - Remote access to tehe app server

**Commands Used** 
1. `scp /tmp/ROOT.war banner@stapp03:/tmp/` 
      ==`scp`== — secure copy, transfers files over SSH
     ==`/tmp/ROOT.war`== — source file path (on jump host)
     ==`banner@stapp03`== — target host, connecting as user `banner`
     ==`:/tmp/`== — destination directory on the remote server
2. **`ssh banner@stapp03`**
    ==`ssh`== — opens a remote shell session
    ==`banner@stapp03`== — login as user `banner` on host `stapp03`
3. `rpm -qa | grep -i tomcat`
     ==`rpm`== — RPM package manager (query/install tool for RHEL-based systems)
     ==`-q`== — query mode
     ==`-a`== — query **all** installed packages
     ==`|`== — pipe: sends output of first command into the next
     ==`grep`== — searches text for a pattern
     ==`-i`== — case-insensitive match
     ==`tomcat`== — the search string
4. `sudo yum install -y tomcat tomcat-webapps tomcat-admin-webapps`
    ==`sudo`== — run as root (installing packages needs elevated privileges)
    ==`yum`== — package manager for RHEL/CentOS
    ==`install`== — subcommand to install packages
    ==`-y`== — auto-answer "yes" to install prompts (non-interactive)
    ==`tomcat`== — core Tomcat server package
    ==`tomcat-webapps`== — default bundled web apps
    ==`tomcat-admin-webapps`== — manager/host-manager admin apps
5. `sudo grep -n "Connector port" /etc/tomcat/server.xml` 
    ==`sudo`== — root privileges to read a system config file
    ==`grep`== — search text
    ==`-n`== — show line numbers with matches
    ==`"Connector port"`== — the search string (in quotes because it has a space)
    ==`/etc/tomcat/server.xml`== — file being searched (Tomcat's main config)
6. `sudo nano /etc/tomcat/server.xml` 
     ==`sudo`== — root privileges to edit a system file
     ==`nano`== — terminal text editor
     ==`/etc/tomcat/server.xml`== — file being opened for editing
     (no flags used — just opens the file directly)
7. **`grep -n "Connector port" /etc/tomcat/server.xml`**
    Same as command 5, run without `sudo` since only reading is needed post-edit — used to verify the port change was saved
8. **`sudo systemctl stop tomcat`**
     ==`sudo`== — root privileges to manage services
     ==`systemctl`== — controls systemd services
     ==`stop`== — subcommand to stop a running service
     ==`tomcat`== — the service name/unit being targeted
9.  **`sudo rm -rf /var/lib/tomcat/webapps/ROOT`** 
     ==`sudo`== — root privileges (system-owned directory)
     ==`rm`== — remove files/directories
     ==`-r`== — recursive (needed to delete a directory and its contents)
     ==`-f`== — force (no confirmation prompts, ignores nonexistent-file errors)
     ==`/var/lib/tomcat/webapps/ROOT`== — the stale default app directory being deleted
10. `sudo cp /tmp/ROOT.war /var/lib/tomcat/webapps/`
     ==`sudo`== — root privileges to write into the webapps directory
     ==`cp`== — copy command
     ==`/tmp/ROOT.war`== — source file
     ==`/var/lib/tomcat/webapps/`== — destination directory (must be exact name `ROOT.war` to deploy at base URL)
11. `sudo systemctl start tomcat`
     Same structure as command 8, but ==`start`== instead of ==`stop`== — brings the service back up so it detects and extracts the new war
12.  `sleep 5`
     ==`sleep`== — pauses execution
     ==`5`== — number of seconds to wait (gives Tomcat time to auto-extract the war before checking)
13. **`ls /var/lib/tomcat/webapps/`**
     ==`ls`== — lists directory contents
     ==`/var/lib/tomcat/webapps/`== — directory being listed (no flags — plain listing was enough to confirm `ROOT/` regenerated)
14. **`curl http://localhost:8087`** 
    ==`curl`== — sends an HTTP request and prints the response
    ==`http://localhost:8087`== — target URL: local loopback, port 8087 (tests Tomcat directly on the server itself)
15. **`curl http://stapp03:8087`** (run from jump host) 
     Same ==`curl`== command, but target is the hostname `stapp03` instead of ==`localhost`== — tests reachability over the network, matching exactly what the task's validator checks