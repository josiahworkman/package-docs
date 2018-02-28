#Installing Apps on Ubuntu

##APT
[APT Package Manager](https://help.ubuntu.com/lts/serverguide/apt.html "Apt Ubuntu Documentation") is a command line package manager with numerous functions to track, update and manage applications on the Unix operating systems. 

###Know the basics:
Install a package:
`sudo apt install <package-name>`
Remove a package:
`sudo apt remove <package-name>`
Adding the `--purge` option to `apt remove` will remove the package configuration files as well. Use with caution.
Update Package Index:
`sudo apt update`
Upgrade Packages:
`sudo upgrade`

###Install Base Apps
`sudo apt install samtools trimmomatic picard-tools fastqc exonerate mummer ncbi-blast+ bowtie bowtie2 trinity repeatmasker-recon r-base python-biopython bioperl tophat`

The code above should install the base versions of the apps known to work on Ubuntu 16.04 (Xenieal). In addition to installing those apps, the command above should also install dependencies for the packages. You can print a list of information about the installed packages by typing `apt show <package-name>`

or for everything:
`apt show samtools trimmomatic picard-tools fastqc exonerate mummer ncbi-blast+ bowtie bowtie2 trinity repeatmasker-recon r-base python-biopython bioperl tophat`

If necessary, we can hold packages from updating automatically. In other words we can lock the list of applications above in their current state using the commands below:
###Freeze Versions
`sudo apt-mark hold <package-name>`
###UnFreeze
`sudo apt-mark unhold <package-name>`

##Custom Installation
It's generally preferred to install custom packages in the /opt/ directory. When compiling a package, you are generally prompted to pick an installation directory which you can set to /opt/package-name/. 

In most cases java applications and compiled softwares can live in the /opt/ directory but it's a pain to manually type the paths to those applications

Add shortcuts to applications:
`export PATH=$PATH:/path/to/your/super/cool/folder/bin`

Alias:
`alias someShortcode="java -jar /opt/javaApplication/myJavaApp.jar"`

Update your session path:
`source ~/.profile`