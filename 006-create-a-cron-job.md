In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]  [[bash]]
**Tags:** #bash #cronjob  #automation 

--------------------------------------------------------------------------
**NARRATIVE** 
--
This is the 6th tasked i was given, it is to create a cron job on the 3 same RHEL/Fedora based app servers with the given cron text that i have to run for the `root user` which is `*/5 * * * * echo helllo > /tmp/cron_text` 

I have  encountered cron job before when i was experimenting with my homelab but im encountering it now on a new level because i was tasked to create my own. 

So i remotely accessed one of the app servers and installed cronie using the "dnf install". After that i didnt whats the name of the cronie daemon or service so i had to use duck AI to help me. 

After that i started cronie using the `systemctl start crond` command and check if its really running using the `systemctl status crond` command. I didnt know how to create an cron process so i asked duckAI again and presented `sudo crontab -e` which  is a command used to create cron process with `-e` for editing mode and `sudo` to create a the cron job to the `root user` and after deploying that command i was put into a vim-like environment so it was a bit new to me but still i was able to input the text required for the cron job and after that, i saved it and exited then duckAI suggested `sudo crontab -l` to verify if the cron job i just created is in the list of running crons of cronie and it was, and one thing tho, if the command was just `crontab -l` , it will not show the cron job i created because it has no `sudo` , and its because `sudo` refers to the `root user` to which i did the cron job in the first place. 

I repeated all of that process to the remaining 2 app servers and the it work smoothly, earning me the completion of the 6th task of kodekloud 100 days of devOps

--------------------------------------------------------------------------

**TASK** 
--
This is the 6th tasked i was given, it is to create a cron job on the 3 same RHEL/Fedora based app servers with the given cron text that i have to run for the `root user` which is `*/5 * * * * echo helllo > /tmp/cron_text`

--------------------------------------------------------------------------
**ATTEMPTS** 
--
I was able to complete the task in 1 attempt 

--------------------------------------------------------------------------
**PROBLEMS** 
--
1. I dont know what commands to use in order to create a cron job using cronie
2. I dont know what command to use in order to save a cron job in a vim 
3. i dont know what commad to use in order to verify a cron job for root 

   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. I used duckAI to give me the proper command to use which is `sudo crontab -e` 
2. i used duckAI to know how to exit vim which is `Esc + :wq + Enter` 
3. duckAI provided info on how to verify the use root cron job  using `sudo crontab -l`

--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. **DuckAI** - command provider and guidance 
2. **SSH** - remote access to servers 
3. **Cronie** - RHEL/Fedora cron job manager software tool

**Commands Used** 
1. `sudo crontab -e` 
     **sudo** - to run the command as root 
     **crontab** - the command for managing scheduled jobs
     **-e** - editing mode    
     - deploying this command with **sudo** , will result into the cron job  
     being  exclusive for the root user. 
2. `sudo crontab -l` 
     **sudo** - to run the command as root 
     **crontab** - the command for managing scheduled jobs
     **-l** - list the cron jobs for the root user 
3. `sudo systemctl start crond`
 **sudo** - runs the command with root privileges
 **systemctl** - services or daemon command controller
 **start** - to start the given daemon or service 
 **crond** - name of the ssh daemon or service    
4. `sudo systemctl status crond` 
 **sudo** - runs the command with root privileges
 **systemctl** - services or daemon command controller
 **status**  -  to check the current status of the given daemon or service
 **crond** - name of the ssh daemon or service for cronie
5. `*/5 * * * * echo hello > /tmp/cron_text`
     `*/5 * * * *` - means minute hour day-of-month month day-of-week command
        `*/5`  - 1st asterisk with /5 means every 5 minutes
         `*` - 2nd asterisk with no number means every hour
         `*` - 3rd asterisk with no number means every day of the month
         `*` - 4th asterisk with no number means every month
         `*` - 5th asterisk with no number means every day of the week 
         Schedule Time Run - 12:00, 12:05, 12:10, 12:15, 12:20
     **echo** - prints text 
     **hello** - text printed by echo 
      **">"**   - edirects output to a file 
      **/tmp/cron_text** - the destination file 




