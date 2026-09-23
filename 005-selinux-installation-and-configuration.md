In Evaluation **Honesty**, **Clarity** and **Organized** should always be kept in mind. 
**Evaluation System:** Narrative Task Attempts Problems Solutions
**Related Links:**   [[kodekloud-100-days-devops]]  [[debian server]]  [[homelab]] [[tech]]  [[bash]]
**Tags:** #bash #selinux 

--------------------------------------------------------------------------
**NARRATIVE** 
--
The task was not that confusing but definitely new to me, the task was asking me to install the right dependencies for SELinux - Security Enhance linux which is a security system that controls what programs and users are allowed to do even when normal file permissions would permit it. 

After installing it, the task asked me to configure the config file of SELinux by making it permanently disabled and saving that config file.

So i started working, i sshed into the app server 3 based on the task and  tried using "apt" "install" which is what i know based on the machines that i use but it does not work and turns out as i use duck ai once again, the server im working on is probably not debian so i tried the command "uname -r" and indeed its a different machine and so duckAI suggest using "sudo dnf install" + the dependencies so turns out, dnf was the package manager of RHEL and fedora based linux machines.

Once ive installed SElinux, i configured it using nano /etc/selinux/config and replacing the "selinux=enforing" to "selinux=disabled" after that, i check if what i did was enough to satisfy the task and fortunately it did. 

--------------------------------------------------------------------------

**TASK** 
--
Following a security audit, the xFusionCorp Industries security team has opted to enhance application and server security with SELinux. To initiate testing, the following requirements have been established for `App server 3` in the `Stratos Datacenter:`  

1. Install the required `SELinux` packages.
2. Permanently disable SELinux for the time being; it will be re-enabled after necessary configuration changes.
3. No need to reboot the server, as a scheduled maintenance reboot is already planned for tonight.
4. Disregard the current status of SELinux via the command line; the final status after the reboot should be `disabled`.

--------------------------------------------------------------------------
**ATTEMPTS** 
--
I only needed 1 attempt for this one 

--------------------------------------------------------------------------
**PROBLEMS** 
--
1. the usual "apt" that i know wasnt working 
2.  i dont know the dependencies of SELinux i need to install
3. i didnt know where the config file of teh SELinux is 
4. how to permanently disable the SELinux config file?


   
--------------------------------------------------------------------------
**SOLUTIONS** 
--
1. i used "uname -r " to check the os version and it wasnt debian so its probably RHEL or Fedora that has a different package manager which is "dnf" so i used "sudo dnf install"
2. i used duckAI to once again give me the appropriate and required dependencies for SELinux 
3. i used duckAI to give me the location on where the config file 
4. as ive read the config file, it was relatively easy, i just needed to overwrite the existing "SELinux = enforcing" to "SELinux = disabled"
--------------------------------------------------------------------------
**TOOLS AND COMMANDS** 
--
**Tools Used**
1. **DuckAI** - for linux guidance and info
2. **Nano** - cli text editor for config files
3. **DNF** - RHEL and Fedora machines package manager
4. **Uname** - for checking machine os information

**Commands Used** 
1. uname -r
     **uname** - displays system info
     **-r** - displays kernel
2. sudo dnf install selinux-policy selinux-policy-targeted policycoreutils policycoreutils-python-utils
     **sudo** - to run the command as root 
     **dnf** - the package manager of RHEL and Fedora machines like apt 
     **install** - to get the required softwares prerequisite to SELinux
     **dependencies** - the policies and other softwares that make SELinux works
3. sudo nano /etc/selinux/config
     **sudo -** to run the command as root
     **nano -** to edit or create new text file
     **etc/selinux/config -** the location of the config file to be edited 