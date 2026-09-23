
In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions,  Tools and Commands
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]  [[bash]] 
**Tags:** #bash 

--------------------------------------------------------------------------
**NARRATIVE** 
--
So the task was very simple, it was to create a user with a non-interactive shell on remote application server.  The user being named "ravi" was the one i needed to create a non-interactive shell for in the appserver1. 

So by this time i know how to use the secure shell or ssh to securely connect to a remote machine such as the application provided by the task. 

But i have another problem, i forgot how to create a user in linux so i consulted duckai and immediately gave me answers, so i tried ssh-ing to appserver1 and i did connect and then i tried creating the said user but to no avail.

The problem was that the user named "ravi" already existed in the server and so i was confused, i exited the lab and  retried the opening the task once again. 

After the opening the task, its still the same but now with a different user now named "jim" and a different server now appserver2.

I tried the steps once again and now it was a success, i was able to complete the first task. 

--------------------------------------------------------------------------

**TASK** 
--

So the task was very simple, it was to create a user with a non-interactive shell on remote application server.  The user being named "ravi" was the one i needed to create a non-interactive shell for in the appserver1. 

--------------------------------------------------------------------------
**ATTEMPTS** 
--

I did total of 2 attempts for this task 

--------------------------------------------------------------------------
**PROBLEMS** 
--

There are two problems that have arised in this task
1. I forgot the commands used in order to create or add a user in a linux environment
2. There was an error in the platform after creating the user named "ravi" in the appserver1,
   it appears that the said user have already been created so i was not successful in my task .
3. How do you create a user with a non-interactive shell?
   
--------------------------------------------------------------------------
SOLUTIONS 
--
The solution for problem 1 is that i used duck.ai to help me see the commands needed to used in order to create a user with a non-interactive shell in a linux environment

The solution for problem 2 is that i exited the task and the lab (linux terminal) and retried doing it once more, this time was successful 

The solution for problem 3 was the create it like a normal user but instead using -s /usr/bin/ you will use -s /usr/sbin/nologin 

--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. **SSH Secure Shell** - used to remotely access app servers
2. **Bash** - use to deply commands
3. **Duck.Ai** - for linux commands guide 
   
**Commands Used** 
1. ssh user@host 
2. sudo useradd -m -s /usr/sbin/nologin username 
     **sudo** - run with admin privilege
     **useradd** - create user 
     **-m**  - create a home directory for the user
     **-s /user/sbin/bash** - sets bash as the users login shell in that directory
     **jim** - is the user with non-interactive shell 
3. command -v /usr/sbin/bash 
    tells you which command would run when you type a command name in your shell