In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]] [[bash]]
**Tags:** #bash #mariadb 

--------------------------------------------------------------------------
**NARRATIVE** 
--
This 9th task didnt go so well for me because i didnt understand the whole process and let duckAI do the work and i feel guilty because i know didnt understand that much on how duckAI troubleshooted the task. 

Despite that though, i still want to learn from its commands its used and how he handled it

The task about troubleshooting a database server because based on the context the nautilus database is having trouble because mariadb is down, mariadb is a database for linux so first i did is seek the database, 

I thought i might try the app servers but as expected they wont work but eventually i found out that the database has the user peter and host name stdb01 and labeled as database server so i was certain it was this.

I remotely connected to it using ssh and check the status of mariadb, i found out that its not running, and that the exit=code error starts showing thats when i consulted duck AI and start looking for the error logs and all  


1. **Checked the MariaDB service status.**

MariaDB was installed, but it failed during startup.

Important message:

Main process exited, code=exited, status=1/FAILURE

This meant that the MariaDB process started but then stopped because of an error.

2. **Checked which startup steps succeeded.**

These steps completed successfully:

`mariadb-check-socket ... status=0/SUCCESS`

`mariadb-prepare-db-dir ... status=0/SUCCESS`

This meant that the socket check and database directory preparation were successful. The problem happened later, inside the main MariaDB process.

3. **Found a stale socket file.**

The logs showed:

Socket file /var/lib/mysql/mysql.sock exists.

No process is using /var/lib/mysql/mysql.sock

This meant that an old socket file was left behind from a previous MariaDB process. Since no process was using it, the file was safe to remove.

Command used:

`sudo rm -f /var/lib/mysql/mysql.sock`

4. **Tried starting MariaDB again.**

MariaDB still failed after the socket file was removed.

This showed that the stale socket file was not the main problem.

5. **Checked the MariaDB error log.**

The systemd output did not show the detailed error, so the MariaDB error log was checked.

The important error was:

==`Can't create/write to file '/run/mariadb/mariadb.pid'`==
==`Permission denied`==


6. **Identified the real problem.**

MariaDB was able to start InnoDB, initialize its database engine, and create its socket.

However, ==`/run`== existed, but the parent directory `/run/mariadb` was missing or inaccessible.

Because `/run/mariadb` ==did not exist or did not have the correct ownership, permissions, or SELinux context==, MariaDB ==could not create the PID file== inside it:

`/run/mariadb/mariadb.pid` 

The fix was to create the missing parent directory and give it the correct ownership, permissions, and SELinux context so the `mysql` user could create the PID file.

The runtime directory had ==incorrect or missing permissions, ownership, or SELinux context==.

7. **Repaired the runtime directory.**

Commands used

`sudo mkdir -p /run/mariadb`

Created the /run/mariadb directory.

`sudo chown mysql:mysql /run/mariadb`

Changed the owner and group to mysql.

`sudo chmod 755 /run/mariadb`

Set the directory permissions so the mysql user could access it.

`sudo restorecon -Rv /run/mariadb`

Restored the correct SELinux security context.

8. **Started MariaDB again.**

Commands used:

`sudo systemctl start mariadb`

Started the MariaDB service.

`sudo systemctl status mariadb --no-pager -l`

Confirmed that MariaDB was running successfully.

9. **Understood the difference between failed and disabled.**

Failed meant that MariaDB tried to start but stopped because of an error.

In this case, the error was ==`permission denied`== when creating the ==`PID file`==.

Disabled meant that MariaDB was not configured to start automatically during system boot.

Disabled did not cause the startup failure.

==The final cause was that MariaDB did not have permission to create:==

==/run/mariadb/mariadb.pid==

The ==problem== was fixed by creating the directory, assigning it to the mysql user, setting the correct permissions, and restoring the SELinux context.


--------------------------------------------------------------------------

**TASK** 
--
The task about troubleshooting a database server because based on the context the nautilus database is having trouble because mariadb is down

--------------------------------------------------------------------------
**ATTEMPTS** 
--
I only  did 1 attempt for this one 

--------------------------------------------------------------------------
**PROBLEMS** 
--
1. I didnt know the name of the database server so i tried connecting to app server because thats what i usually use but failed
2. i did not know what or where are configs, error logs of mariadb
3. the unused socket that was suspected as the culprit wasnt the problem
4. there was an ownership or security issues on the real problem which not mariadb not being able to create its mariadb.pid or process id

   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. I look at the given documentation of machines in the stratos datacenter found stdb01 which is a database, i tried connecting to it and it was succcess so i found the database server
2. i use duckAI to help me find the configs and error logs of mariadb
3. We eventually found a problem of unused socket but found it wasnt the real problem because the issue persisted when i tried to start mariadb so we search deeper
4. duckAI created the missing parent directory, gave it a power ownership and proper SELinux security and that solved the problem

--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. **DuckAI** - providing the most effective commands for the problem
2. **SSH** - remotely connect the database server
3. **Nano** - inline text-editor


**Commands Used in Sequence**

**Troubleshoot Commands**
1. `sudo systemctl status mariadb.service`
    `sudo` — Runs the command with administrator privileges.
    `systemctl` — Controls and queries systemd services.
    `status` — Displays the current state, recent logs, and process information for a service.
    `mariadb.service` — The exact systemd unit name for MariaDB.
    The `.service` suffix is optional, so this is equivalent:
2. `sudo journalctl -u mariadb.service -b -n 100 --no-pager`
     `sudo` — Runs with elevated privileges so all relevant logs can be read.
      `journalctl` — Reads logs collected by systemd’s journal.
      `-u mariadb.service` — Filters the output to only this systemd unit.
     `-b` — Shows messages from the current boot.
     `-n 100` — Displays the most recent 100 log entries.
     `--no-pager` — Prints the output directly instead of opening it in `less` or another pager.

    The command showed service startup messages, but not the detailed `mariadbd` error. That is why the MariaDB log was checked next.
3. `nano /var/log/mariadb/mariadb.log`
     `nano` — A terminal text editor.
      `/var/log/mariadb/mariadb.log` — The path to MariaDB’s error log.

    There were no command-line options here. The file was opened for viewing or editing. For simply reading the end of a log, this is usually safer and more convenient
4. `sudo rm -f /var/lib/mysql/mysql.sock`
      `sudo` — Required because the file belongs to the database service.
     `rm` — Removes files.
      `-f` — Force removal:
        Does not ask for confirmation.
         Does not report an error if the file is already absent.
               `/var/lib/mysql/mysql.sock`  The Unix socket file used for local MariaDB connections. 
    This was safe in this case because the service log confirmed that no process was using the socket. The command removed only the socket file, not the database files.
5. `sudo systemctl start mariadb`
      `sudo` — Administrator privileges are required to start a system service.
     `systemctl` — Communicates with systemd.
     `start` — Requests that systemd start the service.
     `mariadb` — The MariaDB service unit. 
6. `sudo systemctl status mariadb --no-pager -l`
      `sudo` — Runs with administrator privileges.
      `systemctl` — Queries systemd and manager of services. 
      `status` — Displays service state and recent messages.
      `mariadb` — The MariaDB service.
      `--no-pager` — Keeps output in the terminal.
      `-l` — Shows full, untruncated lines. Without `-l`, long lines may be shortened with `

   This confirmed that the actual server process exited with: ==`status=1/FAILURE`==
7. `sudo tail -n 100 /var/log/mariadb/mariadb.log`
     `sudo` — Allows access to the protected log file.
     `tail` — Displays the end of a file.
     `-n 100` — Displays the final 100 lines.
     `/var/log/mariadb/mariadb.log` — MariaDB’s error log.

    This revealed the decisive error:  
    ==`Can't create/write to file '/run/mariadb/mariadb.pid'Permission denied`==
    
    The PID file records MariaDB’s running process ID. MariaDB could not create it because the runtime directory was missing, incorrectly owned, incorrectly permissioned, or had an incorrect SELinux context.
**Fix Commands**
8. `sudo mkdir -p /run/mariadb`
      `sudo` — Required because `/run` is controlled by the system.
      `mkdir` — Creates a directory.
     `-p` - Creates missing parent directories and does not fail if the directory already exists.
    `/run/mariadb` - The directory where MariaDB stores temporary runtime files such as its PID file 
    and socket.
    
    `/run` is a temporary runtime filesystem. Its contents can disappear after a reboot.
9. `sudo chown mysql:mysql /run/mariadb`
       `sudo` — Required to change ownership of a system directory.
      `chown` — Changes file or directory ownership.
      `mysql:mysql` — Sets:
       owner user: `mysql`
       owner group: `mysql`
     `/run/mariadb` — The target directory.

   ==MariaDB== normally runs as the ==`mysql` system user==. Giving this user ==ownership== allows it to create ==`/run/mariadb/mariadb.pid`.==
10. `sudo chmod 755 /run/mariadb`
     `sudo` — Administrator privileges.
     `chmod` — Changes permissions.
      `755` — The numeric permission mode:
      
     First digit `7`: owner can read, write, and enter/search the directory.
     Second digit `5`: group can read and enter/search the directory.
     Third digit `5`: others can read and enter/search the directory.
     
     `/run/mariadb` — The target directory.
11. `sudo restorecon -Rv /run/mariadb`
      `sudo` — Required to change security contexts.
     `restorecon` — Restores SELinux labels based on the system’s policy.
     `-R` — Recursive; applies the operation to the directory and its contents.
     `-v` — Verbose; displays what it changes.
     `/run/mariadb` — The directory whose SELinux context is restored.

    ==Linux== permissions and ==SELinux== permissions are ==separate==. Even if `mysql:mysql` ownership is correct, SELinux can still block MariaDB from writing to the directory.
**Testing Commands**
12. `sudo systemctl start mariadb`
      `sudo` — Administrator privileges are required to start a system service.
     `systemctl` — Communicates with systemd.
     `start` — Requests that systemd start the service.
     `mariadb` — The MariaDB service unit. 
13. 1. `sudo systemctl status mariadb`
    `sudo` — Runs the command with administrator privileges.
    `systemctl` — Controls and queries systemd services.
    `status` — Displays the current state, recent logs, and process information for a service.
    `mariadb` — The exact systemd unit name for MariaDB.