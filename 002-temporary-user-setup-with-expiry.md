In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions, Tools and Commands
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]  [[bash]]
**Tags:** #bash 

--------------------------------------------------------------------------
**NARRATIVE** 
--
Well today i was given a bit of variation of the task yesterday which is to create a user with extra arguments, today i was given a task which is to create a temporary user.

This user will have an expiry date so it the user will be deleted in due established time and date.

So since im just a self-learned linux enthusiast of course i didnt had any idea of the commands that i need to use in order to create a temporary user and so i consulted duck.ai again and help me find the relevant commands to be used.  

so my first attempt going well, the task was to create a temporary user named ammar in the application server 3, i actually did it, no errors what so ever, i sshed into the app server 3 using the credentials provided by the tasks, ssh banner@stapp03 and then i deployed the command sudo useradd -m -e 2027-02-17 -s /bin/.bash ammar and it did worked.

For some reason tho, the system did not accept my work and so i tried doing the task again now being the user named jim, so i sshed and deployed the same command and the system checked and it was confirmed, so thats a bit weird but atleast im progressing on this program, learning new commands that can be used in real-world IT. 

--------------------------------------------------------------------------

**TASK** 
--

The task was to create a temporary user with an expiry date in one of the application server



--------------------------------------------------------------------------
**ATTEMPTS** 
--
 I did total of 2 attempts
The first attemp user was named ammar with an exprity date of 2027-02-17 in appserver 3
The second attempt user was named jim with an expiry date of 2024-04 in appserver 3

--------------------------------------------------------------------------
**PROBLEMS** 
--

1. since im self-learned linux enthusiast, i didnt have any idea of the commands that will be going to use
2. the program have a problem wherein my first were not accepted although ive confirmed that they are correct but second attempts are accepted instead 
3. the task demanded on creating a temporary user 
   
--------------------------------------------------------------------------
SOLUTIONS 
--
1. for problem 1 as usual i researched using duck.ai on what commands to be used and also what do they in every argument
2. i tried doing the task amd this time it worked and im successful with the task
3. by utilizing the command  useradd with extra arguments such -m for home directory, -e and date for giving it an expiry date and -s for giving it a shell

--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. **SSH Secure Shell** - to remotely access the appserver3
2. **Bash** - platform and language deploy the commands
3. **Duck.Ai** - for linux commands guide 
   
**Commands Used**
1. shh banner@stapp03
2. sudo useradd -m -e 2027-02-17 -s /bin/bash jim 
     **sudo** - run with admin privilege
     **useradd** - create user 
     **-m**  - create a home directory for the user
     **-e 2027-02-17** - sets an accouint expiration date 
     **-s /bin/bash** - sets bash as the users login shell
     **jim** - is the temporary user 
3. command -v /usr/sbin/bash 
    tells you which command would run when you type a command name in your shell 
