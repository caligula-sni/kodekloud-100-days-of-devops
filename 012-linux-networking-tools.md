In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]
**Tags:** #bash #networking #iptables #firewall

--------------------------------------------------------------------------
**NARRATIVE** 
--
This the 12th task of the kodekloud devOps and i only did atleast 2-3 attemts on this which is pretty fast in my state, The task is simple, which is determine which app server is having trouble with the apache service, troubleshoot it and make sure the jump host server can connect to it. 

The task suggested me to use telnet and so i did used telnet to determine which app server is having trouble with apache using the telnet stapp01 5003, stapp01 being the server hostname and 5003 the port of apche and i repeated accross all the app server i was able to determine that app server 1 or stapp01 was the one having trouble because its the only server i wasnt able to connect on its apache port using telnet. 

so i sshed into the app server 1 and using systemctl status and journal ctl, i found out that the apache port 5003 is being used by an email service, using claude i was able to disable the email service and made apache up and running again using the port 5003 but another issue persist when  i found that i cannot connect the jump host server to the app server apache port i just fixed, so when claude analyze the error which says no route to host, thats when it diagnosed that it was a firewall issue.

so i went to the app server 1 once again and using iptable command i was able to see that some rule is blocking the port so using claude i was able to fix it by adding another rule that overwrites the reject rule for that port and i was able to complete the  task 

--------------------------------------------------------------------------

**TASK** 
--
Our monitoring tool has reported an issue in `Stratos Datacenter`. One of our app servers has an issue, as its Apache service is not reachable on port `5003` (which is the Apache port). The service itself could be down, the firewall could be at fault, or something else could be causing the issue.

Use tools like `telnet`, `netstat`, etc. to find and fix the issue. Also make sure Apache is reachable from the jump host without compromising any security settings.

Once fixed, you can test the same using command `curl http://stapp01:5003` command from jump host.  
`Note:` Please do not try to alter the existing `index.html` code, as it will lead to task failure.

--------------------------------------------------------------------------
**ATTEMPTS** 
--
i did only 5 attempts for this one

--------------------------------------------------------------------------
**PROBLEMS** 
--
1. i did knew how to see the error logs of the httpd.service using systemctl status and journal ctl but iwasnt able to determine what was the problem
2. i can curl the apache service on its own server but for some reason it cant be curl on the jump host server so i sent claude the error 

   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. I used claude to determine what problems was causing it and it was an email server using the port so i disabled the email service, enabling the apache server to run again
2. claude found out that its a firewall problem so we check and indeed it was a firewall problem where some rule is blocking the apache port from being connected from the jump host so claude added a new rule overwriting the old rule and making it work. 
--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. telnet
2. claude
3. iptable
4. ss 

**Commands Used** 
1. sudo systemctl status httpd
2.  `sudo journalctl -u httpd.service -b -n 100 --no-pager`
     `sudo` — Runs with elevated privileges so all relevant logs can be read.
      `journalctl` — Reads logs collected by systemd’s journal.
      `-u mariadb.service` — Filters the output to only this systemd unit.
     `-b` — Shows messages from the current boot.
     `-n 100` — Displays the most recent 100 log entries.
     `--no-pager` — Prints the output directly instead of opening it in `less` or another pager.
3. sudo ss -tulnp | grep 5001
     `-t`TCP sockets
     `-u`UDP sockets
     `-l`Listening sockets only (services waiting for connections, not established ones)
     `-n`Numeric — show port numbers and IPs instead of resolving service/host names
     `-p`Show the **process** (name + PID) that owns each socket — requires 
     `sudo` since normal users can't see other users' process info
4. sudo systemctl stop sendmail
5. sudo systemctl disable sendmail
6. sudo ss -tulnp | grep 5001
     `-ss` - socket statistics 
     `-t`TCP sockets
     `-u`UDP sockets
     `-l`Listening sockets only (services waiting for connections, not established ones)
     `-n`Numeric — show port numbers and IPs instead of resolving service/host names
     `-p`Show the **process** (name + PID) that owns each socket — requires 
     `sudo` since normal users can't see other users' process info
8. sudo systemctl start httpd
9. curl http://localhost: 5003 
10. sudo iptables -L -n --line-numbers
     `-L`List the rules currently loaded
     `-n`Numeric output — show IPs/ports as numbers instead of resolving hostnames/service names (faster, and avoids DNS lookups)
     `--line-numbers` - Show the rule's position number in each chain — needed so you can target a specific rule (for inserting/deleting)
11. sudo iptables -I INPUT 5 -p tcp --dport 5003 -j ACCEPT
     `-I INPUT 5` → "put this rule at position 5 in the INPUT chain" (placement)
     `-p tcp --dport 5003` → "match packets that are TCP going to port 5003" (the condition)
     `-j ACCEPT` → "if it matches, accept it"
12. curl http://stapp01: 5003