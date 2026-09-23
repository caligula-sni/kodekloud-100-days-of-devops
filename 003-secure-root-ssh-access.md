In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]  [[bash]]
**Tags:** #bash #ssh 

--------------------------------------------------------------------------
**NARRATIVE** 
--
Today, the 3rd day i was given a task to secure the ssh root acess which means prevent anyone accessing a remote server through ssh from getting escalated privileges being the user "root"

So before i started the task i already had ideas on how to do the task but still dont have any idea of the commands that i need to use in order to accomplish the task.

I ask duck.ai about the task and what commands should i use and pretty much, the commands were simple and straight forward and i was familiar with them so i did the task. 

The task was one but will be operated in 3 app servers which are the stapp01, stapp02, stapp03 which are application servers that i've with before with the previous tasks. 

So in each and every server i did these steps, i first ssh into the application server using the given user and password of the server, and then deployed this command "sudo nano /etc/ssh/sshd_config" which opens up nano (text editor) and displays all of the configuration of the ssh service.  

After that, all i need to do was disable the PermitRootLogin from "yes" to "no" and after that i saved the config file and closed nano and then reloaded and restarted ssh using "sudo sshd -t" and "sudo systemctl reload sshd". I just repeated that operation for the remaining 2 app server and task was accomplished on my attempt for the first time. 


--------------------------------------------------------------------------

**TASK** 
--
The task was secure the ssh root acess which means prevent anyone accessing a server through ssh from getting escalated privileges being the user "root" and this task was to be repeated on 3 application servers including stapp01, stapp02, stapp03

--------------------------------------------------------------------------
**ATTEMPTS** 
--
For the first time, i was able to complete the task in one attempt 

--------------------------------------------------------------------------
**PROBLEMS** 
--

1. I know the concept on how to solve the task but i don't know the exact commands to use and i don't know much of the config file directories of linux services such as ssh 
2. how do you prevent an ssh user connected to a server from accessing user root?
   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. I use Duck.AI once again to help me know the right commands for the task and what config file i need to overwrite in order to accomplish the tasks as well as how to restart and reload the sshd service 
2. Disabling the permitrootlogin line from /etc/ssh/sshd_config by replacing yes to no
--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used** 
1. **Duck.Ai** - for linux commands guide 
2. **SSH** - to securely connect to the servers i needed to operate in
3. **Nano** - a built-in terminal text edito
   
**Commands Used** 
1. sudo nano /etc/ssh/sshd_config
 **sudo** - to run the command with root privilegel
 **nano** - to open or create a text file
 **/etc/ssh/sshd_config** - location of  the sshd_config file
2. sudo sshd -t 
 **sudo** - to run a command with root privileges
 **sshd** - the name of the ssh service or daemon
 **-t** - test if there configuration errors
3. sudo systemctl reload ssh
 **sudo** - runs the command with root privileges
 **systemctl** - services or daemon command controller
 **reload** - tells the service to reread its config without halting or starting the service 
 **ssh** - name of the ssh daemon or service 