In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]  [[bash]]
**Tags:** #bash #ansible  #automation 

--------------------------------------------------------------------------
**NARRATIVE** 
--
Today was the 8th task and surprisingly, the task is not that complex because its just about installing a tool called "Ansible" but ofcourse i still needed help for the commands i needed to use in order to complete the task, this is the context of the task: 

During the weekly meeting, the Nautilus DevOps team discussed about the automation and configuration management solutions that they want to implement. While considering several options, the team has decided to go with Ansible for now due to its simple setup and minimal pre-requisites. The team wanted to start testing using Ansible, so they have decided to use jump host as an Ansible controller to test different kind of tasks on rest of the servers.

Task: 
Install ansible version 4.9.0 on Jump host using pip3 only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

I have an idea on how to install tool but since it said to only pip3 that kinda changed the whole idea so i needed resources and use duckAI on how to install it, it gave me a different command at first so i give duckAI the whole situation of the task and there it gave me the right command that finishes the task. 

--------------------------------------------------------------------------

**TASK** 
--
Install ansible version 4.9.0 on Jump host using pip3 only. Make sure Ansible binary is available globally on this system, i.e all users on this system are able to run Ansible commands.

--------------------------------------------------------------------------
**ATTEMPTS** 
--
I did 2 attempts here because the first one failed due to wrong version installation. 

--------------------------------------------------------------------------
**PROBLEMS** 
--
1. I have an idea how to install ansible but since it needed to be installed using pip3 that changed my idea on how to install it
2. duckAI gave a different command at first that lead me to giving the 2 attemps on the task

   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. I used duckAI to find out what relevant commands i need to input in order to satisfy the task
2. I gave duckAI the whole context of the task so that it can give me the more relevant and effective command to use which is `sudo -H pip3 install ansible==4.9.0`
   which i will explain later 
--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. **DuckAI** -  providing me the most effective commands based on the task
2. **pip3** - is the package manager used to install Python 3 libraries and applications

**Commands Used** 
1. `sudo -H pip3 install ansible==4.9.0`
     sudo - runs the command as root 
     -H - tells `sudo` to use the root user’s home directory instead of your personal home directory while installing Ansible.
     `pip3` — runs the Python 3 package installer.
     `install` — tells `pip3` to install a package.
     `ansible==4.9.0` — requests the Ansible community package version `4.9.0` exactly.
2. `ansible --version` 
    shows the `ansible-core==2.11.12` which is the ansible engine or core for 
    the version `4.9.0` which confirms the successful installation of the tool. 
3. `command -v ansible`
     shows the directory of the command ansible
4. `sudo chmod a+rx /usr/local/bin/ansible` 
     `sudo` - to run the command with root privileges
     `chmod` - modify a file permission
     `a+rx` - give all "a" users the ability to "r" read and "x" execute the file
     `/usr/local/bin/asn` - this was the target file of chmod 