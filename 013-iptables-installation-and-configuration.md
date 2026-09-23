eIn Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]
**Tags:** #bash #iptables #firewall 

--------------------------------------------------------------------------
**NARRATIVE** 
--
ive actually tried doing this task but to no avail due to me being busy and also failing the first  attemps today tho, claude helped me redo the task, 

The task problem was the apache port was open to all in the network and allowing any one in the network to access it, the task demands me to close the apache port to all connections except lbr port or the load balancer server which distributes traffic in the network.  

The task gave me steps on the praaaaaaaaaaaocedures i need to complete which 
1. install iptables and its services and depedencies - which is a firewall manager for linux 
2. block the port of apache to everyone except lbr server
3. configure the rule to remain and survive reboot 

on my first few attempts last week, although i dont quite remember the task i think i was having problem with the internet connection and the terminal provided by kodekloud and i think one of the app servers were actually having problems that or my commands were not the most best or the most effective for the task given.

in my second attemp today,  claude help me accomplished but we tried other attemps first such creating an ansible playbook that can automate the task of blocking and configurating the firewall but due to errors and other connection problems i totally dump the idea of using ansible for the task.

then i used direct commands instead which worked but due to my ignorance i had to actually make around 5 more attemps in order to complete it, first error i made is that i just copied whatever claude gave before even checking the ip address and current port of the apache have matched, well in kodekloudwhen you redo a task, the variables such as ip addresses and port changes so thats a lot errors persisted because of my ignorance of that. 

after that i actually had another error is for the saving or boot-proofing of the rules applied to the iptable, i didnt include a command given by claude causing this attempt to fail, the step that i skipped was actually the sole command responsible for making the rule remain so consulted claude and using the troubleshooting commands we were able to shoot down the problem and claude createa an alternative way to bypass the problem and then i was able to conmplete the problem. 

In each app server i was able to install iptable and its dependencies, configure the iptable for apache, using a command to for the apache port to accept the ip address of lbr server which will be accepted first and after that will be the command that will enable the apache port to block or reject all tcp connections in the network but since the the lbr server command was input first, this will create an exception rule to the rejection rule of apache port. Iptables check the rules by their order, so if it is written that first that lbr server is accepted to the apache port then it will create a exception to the rejection rule after it. After that other iptables services was used in order to save the rule for it to survive the reboot and some commands are used too for vefication of the commands used. after applying all that to the app server i was able to successfully complete the task.  

--------------------------------------------------------------------------

**TASK** 
--

The task problem was the apache port was open to all in the network and allowing any one in the network to access it, the task demands me to close the apache port to all connections except lbr port or the load balancer server which distributes traffic in the network.  

The task gave me steps on the procedures i need to complete which 
1. install iptables and its services and depedencies - which is a firewall manager for linux 
2. block the port of apache to everyone except lbr server
3. configure the rule to remain and survive reboot 

--------------------------------------------------------------------------
**ATTEMPTS** 
--
i did atleast  10 attempts for this task 

--------------------------------------------------------------------------
**PROBLEMS** 
--
1. I have unstable internet connection and terminals of kodekloud platform is having problems
2. i had errors mismatching and using the template given by claude for the ip addresses and ports that is used for the task due to my ignorance
3. i had problem because i was missing or ignoring a step that actually plays a critical role for saving the rule and surviving the reboot 

   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. i actually rested for a bit here atleast a week and done the task after which i think also fixed the unstable internet connection and also the unstable terminal of kodekloud
2. claude helped me by suggesting a command called getent and the hostname which echoes the ip address of the given  hostname
3. claude helped me trouble shoot the error commands such as permission denied and other missing dependencies and gave me an alternative command which worked well. 
--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. ClaudeAI - provisioning of commands
2. Iptables - firewall for linux

**Commands Used** 
1. `getent hosts stlb01`
    ==`getent`== - Queries the system's configured name resolution sources (NSS — `/etc/hosts`, DNS, etc.)
    ==`hosts`== - Tells `getent` to search the **hosts database** specifically (name → IP resolution)
    ==`stlb01`== - The hostname being resolved — returns its IP if found in `/etc/hosts` or DNS
2. `sudo dnf install iptables iptables-services -y`
     ==`sudo`== - Run as root — package installation requires elevated privileges
     ==`dnf`== - Fedora/RHEL package manager
     ==`install`== - Subcommand to install packages
     ==`iptables`== - Core firewall utility (rule management binary)
     ==`iptables-services`==Provides the ==`iptables`== systemd service + ==`/etc/sysconfig/iptables`== config path for persistence
     ==`-y`== - Auto-confirm the install prompt, skips manual "y/n"
3. `sudo iptables -A INPUT -p tcp -s 10.244.234.247 --dport 8087 -j ACCEPT`
     ==`-A INPUT`== - **Append** this rule to the **INPUT** chain (rules for traffic _incoming_ to this host)
     ==`-p tcp`== - Match only **TCP** protocol traffic
     ==`-s 10.244.234.247`== - **Source** — only match packets originating from this IP (the LBR host)
     ==`--dport 8087`== - **Destination port** — only match traffic targeting port 8087 (Apache)
     ==`-j ACCEPT`== - **Jump** target — if matched, **allow** the packet through
4. `sudo iptables -A INPUT -p tcp --dport 8087 -j REJECT`
     ==`-A INPUT`== - Append to INPUT chain again
     ==`-p tcp`==  - Match TCP traffic
     ==`--dport 8087`== - Match traffic to port 8087 — **no 
     ==`-s`==  - this time**, so it matches _any_ source
     ==`-j REJECT`== - Block the packet, and (unlike `DROP`) send back an ICMP "port unreachable" response
5. `sudo iptables-save | sudo tee /etc/sysconfig/iptables`
     ==`iptables-save`== - Dumps the **current live rule set** (in-memory) to stdout, in restorable format
     ==`|`==Pipe — sends that output into the next command
     ==`sudo tee /etc/sysconfig/iptables`==Writes the piped input to this file (the path `iptables.service` reads on boot); 
     ==`tee`== is used instead of ==`>`== because ==`sudo`== needs to apply to the _write_, not just the read side of a redirect
6. `sudo systemctl enable iptables sudo systemctl start iptables`
     ==`systemctl enable iptables`==Creates a symlink so ==`iptables.service`== **starts automatically on every boot**
     
     ==`systemctl start iptables`==Starts the service **immediately**, applying rules from ==`/etc/sysconfig/iptables`== right now
7. `sudo iptables -L INPUT -n --line-numbers`
     ==`-L INPUT`==  - **List** rules, scoped to the INPUT chain only
     ==`-n`== - **Numeric** output — shows raw IPs/ports instead of resolving hostnames/service names (faster, avoids DNS lookups)
     ==`--line-numbers`== - Prefixes each rule with its position/order in the chain — critical for confirming ACCEPT sits above REJECT 