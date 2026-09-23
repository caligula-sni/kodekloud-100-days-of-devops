In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]  [[bash]]
**Tags:** #bash #scripting 

--------------------------------------------------------------------------
**NARRATIVE** 
--
The 4th task of the challenge was familiar to me this time, the team of developers created an sh file that can automate processes but the problem was that the sh file cannot be executed in the app server 2 without the right permissions to user. 

So this time i already had an idea on what to do and what commands to use which is ls -l to check the current permission of the file and also chmod which is a tool used to change permissions of the files and who can use it.

So as usual, i sshed into the stapp02 server, i located the sh file which is /tmp/xfusioncorp.sh?
i did the "ls -l" command on the file and found out that no one can read nor execute it so i did consult duck.ai and ask what command should i use in order to give all users the ability to read and execute the sh file and it gave me "chmod a+rx xfusioncorp.sh" which the command did the task successfully after i check the sh file with "ls -l" again.

--------------------------------------------------------------------------

**TASK** 
--
The task this time was to give all users in the appserver or stapp02 the permission the execute the sh file located in /tmp/xfusioncorp.sh 

--------------------------------------------------------------------------
**ATTEMPTS** 
--
I once again did only one attempt on the task

--------------------------------------------------------------------------
**PROBLEMS** 
--
1. I know the concept of the solution that i will be doing but i dont know the exact chmod arguments that i need to use in order give all users in the server the permission to execute the sh file
2. The problem in the task was how are you going to give all the users in the app server 2 the permission to execute the sh file

   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. I used duck.ai once again for me the know the right command i have to use in order to give all the users the permission to execute the sh file
2. I use "ls-l" command first to check the current permission of users to the sh file found that no one can read nor execute the file and then i used "chmod" with "a+rx" which means change modify the sh file to give all users the ability to read and execute the sh file. 
--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. **Duck.AI** - the source of linux commands 
2. **CHMOD** - a linux tool used for changing file permissions
   
**Commands Used** 
1. ls -l 
     **ls** - list information
     **-l** - list detailed information 
2. sudo chmod a+rx xfusioncorp.sh
     **sudo** - to run the command with root privileges
     **chmod** - modify a file permission
     **a+rx** - give all "a" users the ability to "r" read and "x" execute the file
     **xfusioncorp.sh** - this was the target file of chmod 