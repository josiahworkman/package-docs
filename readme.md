# Managing Ubuntu Server 16.04 Overview

## Installing Packages with APT
[APT Package Manager](https://help.ubuntu.com/lts/serverguide/apt.html "Apt Ubuntu Documentation") is a command line package manager with numerous functions to track, update and manage applications on the Unix operating systems.

### Know the basics:
Install a package:
`sudo apt install <package-name>`

Remove a package:
`sudo apt remove <package-name>`
Adding the `--purge` option to `apt remove` will remove the package configuration files as well. Use with caution.

Update Package Index:
`sudo apt update`

List installed packages:
`sudo apt list --installed`

### Install Base Apps
Apps available in the apt package manager can be searched using the `apt search <package-name>` command. Ex. searching `apt search sssd` would reveal all packages with the 'sssd' name in the title or description. Searching for apps in the default package manager should also reveal the version number and help identify if the package you are looking for is indeed the one you need.

Given the list of desired apps, I was able to find the following using the apt package manager.
`sudo apt install samtools trimmomatic picard-tools fastqc exonerate mummer ncbi-blast+ bowtie bowtie2 trinity repeatmasker-recon r-base python-biopython bioperl tophat`

The desired package `bcftools` is dependent on `samtools` and needs to be installed afterwards. After the above list of packages has been run, install bcftools: `sudo apt install bcftools`

The code above should install the base versions of the apps known to work on Ubuntu 16.04 (Xenieal). In addition to installing those apps, the command above should also install dependencies for the packages. You can print a list of information about the installed packages by typing `apt show <package-name>`

or for everything:
`apt show samtools trimmomatic picard-tools fastqc exonerate mummer ncbi-blast+ bowtie bowtie2 trinity repeatmasker-recon r-base python-biopython bioperl tophat bcftools`

If necessary, we can hold packages from updating automatically. In other words we can lock the list of applications above in their current state using the commands below:
### Freeze Apps
To suspend the automatic updates of an application, run the command: `sudo apt-mark hold <package-name>`

or for everything: `sudo apt-mark hold samtools trimmomatic picard-tools fastqc exonerate mummer ncbi-blast+ bowtie bowtie2 trinity repeatmasker-recon r-base python-biopython bioperl tophat bcftools`

### Un-Freeze Apps
To update an app which has been suspended from updates, run the command: `sudo apt-mark unhold <package-name>`

### Apps with other sources
Some apps may require the addition of binary file repository in order to update to the latest version. Apt uses a file that lists 'sources' from which packages can be obtained. The default list is stored in /etc/apt/sources.list

Some packages like R require an additional source repository to be listed in this file. [R-Project's Website]("https://cran.r-project.org/bin/linux/ubuntu/") has a nice writeup detailing the process as it relates to R.

After adding the R update repository to /etc/apt/sources.list the R package should be upgradable.

Update apt's known packages:
`sudo apt update`

Update R Package:
`sudo apt install r-base r-base-dev`

### General Upkeep
The `apt-mark` command has several useful features for listing installed applications. Type `apt-mark help` for more information. Typing `apt-mark showhold` would reveal which applications are currently prevented from updating when `sudo apt upgrade` is invoked from the command line or automatic Cron tasks.

### Custom Application Installation
It's generally preferred to install custom packages in the /opt/ directory. When compiling a package, you are generally prompted to pick an installation directory which you can set to /opt/package-name/.

In most cases java applications and compiled softwares can live in the /opt/ directory but it's a pain to manually type the paths to those applications

Add shortcuts to applications:
`export PATH=$PATH:/path/to/your/super/cool/folder/bin`

Alias:
`alias someShortcode="java -jar /opt/javaApplication/myJavaApp.jar"`

Update your session path:
`source ~/.profile`

## User Management

### Adding Users
To add a user the `adduser` command accomplishes the combined tasks of setting passwords and creating a home directory. For most new users, it's best to set a temporary password and have that user set their own password on the first login.

`sudo adduser user1234` Complete the prompts and a new user will have been created.

`sudo adduser user1234 sshlogin` Add new user to the sshlogin group.

`sudo chage -d 0 user1234` Enforce password change

### Groups
In most Ubuntu installations there are several configured groups. To get a list of all groups on a system run `less /etc/group`. Running the `groups` command from terminal will display witch groups the current terminal user is currently a member of.

In general, if several users will work on a project it's worthwhile to create a new group and assign group permissions to a directory object.

`sudo addgroup project1` to create the 'project1' group

`sudo adduser user1234 project1` to add user1234 to the new group.

### Group Permissions
By default, users are created and assigned to the primary group of their own user account. Users can join secondary groups as shown above but they can not join two primary groups. If two users were part of the same secondary group, they could access and edit the same files in an external directory.

The table below is a theoretical list of mounting points on a Ubuntu system. Bin is a system created point, data0 and data1 are additional points created manually.

| Mount Point   | Permissions   | User Owner  | Group Owner |
| ------------- |-------------- | ----------- |------------ |
| bin           | drwxr-xr-x    | root        | root		|
| data0     	| drwxrwsr-x    | root        |	project0	|
| data1		 	| drwxrwsrwx    | root        |	project1	|

In the table above, you'll notice the permissions for data0/data1 differ from the bin directory. Both data0/data1 directories have had group owners set explicitly. In addition the drwxrwsr-x permissions tell us that the group setgid has been enabled on this directory. With the setgid bit enabled, executable files with this bit set will run with effective gid set to the gid of the file owner - in this case the group owner is included. Google linux permissions and setgid to gain a better understanding of the concept. In sum, we can set permissions on a directory so that anyone within a group can edit and view the same files.

In the example above, users within project0 have read write execute permissions within data0. As long as a user is in the group project0, they can create, edit and delete files and folders within data0.

By contrast, data1 has been set with 755 permissions, anyone, regardless of their group affiliation has access to read, write and execute data here. data1 is also set with the setgid under the group category, any user within the group project1 can read write and execute files and folders within data1.

There are several ways to setup a directory, they are easier to setup before data is dumped into the directory. Plan ahead of time.

Before diving out access to a folder consider the following. Who requires access? Should this be restricted?

If we were to setup data2 in on this theoretical server I might create a new group for the owners of that data. If the data is to be accessed by many users, it might still make sense to create a project2 group to help associate the data with it's respective project.

All that said, most of these data folders are setup on the real server. Changing the permissions to reflect group ownership should be the last task before use.

### Change Ownership & Setting setgid Bit
Once you've identified a directory and group changing permissions can be accomplished using the `chown` & `chmod` commands.

**Change Ownership** `sudo chown -R :mySwellGroup /myAmazingFolder` this would change the group ownership on the 'myAmazingFolder' directory to 'mySwellGroup'

**setgid bit** `sudo chmod 2775 /myAmazingFolder` to setup the setgid bit on this directory. Note: You've probably seen `chmod 775 foo` before. The 2 preceding the 775 essentially enforces inheritance - Any files or subdirectories in 'myAmazingFolder' will inherit the group id (mySwellGroup).

Without setting this bit, files and directory within a directory would be owned by each user creating files or directories. Enforcing inheritance and groups should help keep things organized.

### Mass edit permissions
To edit the permissions of many objects the `find` command can be used in tandem with `-exec` command option. If you wanted to set inheritance on a directory, you would need to change the existing files and folders in that directory first.

`find /data1 -type d -exec chmod 775 {} \;` search for everything within /data1 and identify directories then execute chmod to change their permissions.

`find /data1 -type f -exec chmod 664 {} \;` search for everything within /data1 and identify files then execute chmod to change their permissions.

After recursively setting those permissions, you could then set the ownership and setgid bits to enable inheritance for future files.
`find /data1 -type d -exec chmod 2775 {} \;`
