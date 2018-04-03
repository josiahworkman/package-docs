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

Upgrade Packages:
`sudo upgrade`

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
In most Ubuntu installations there are several configured groups. Running the `groups` command from terminal will display the groups available for users to join. 

In general, if several users will work on a project it's worthwhile to create a new group and assign group permissions to a folder object. 

`sudo addgroup project1` to create the 'project1' group

`sudo adduser user1234 project1` to add user1234 to the new group.

### Group Permissions
By default, users are created and assigned to the primary group of their own user account. Users can join secondary groups as shown above but they can not join two primary groups. If two users were part of the same secondary group, they could access and edit the same files in an external directory. 

.
├── bin
├── boot
├── data0
├── data1
├── data2
├── data3
├── data4
├── data5
├── dev
├── etc
├── home
├── initrd.img -> boot/initrd.img-4.4.0-116-generic
├── initrd.img.old -> boot/initrd.img-4.4.0-112-generic
├── lib
├── lib64
├── lost+found
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── scratch
├── snap
├── srv
├── sys
├── tmp
├── usr
├── var
├── vmlinuz -> boot/vmlinuz-4.4.0-116-generic
└── vmlinuz.old -> boot/vmlinuz-4.4.0-112-generic