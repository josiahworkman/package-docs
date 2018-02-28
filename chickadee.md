#Installing Apps using APT
Installing apps is super simple with a package manager like apt. Apt is included in all default Ubuntu installations. Package managers.

###Install Base Apps
`sudo apt install samtools trimmomatic picard-tools fastqc exonerate mummer ncbi-blast+ bowtie bowtie2 trinity repeatmasker-recon r-base python-biopython bioperl tophat`

The code above should install the base versions of the apps known to work on Ubuntu 16.04 (Xenieal). In addtition to installing those apps, the command above should also install dependencies for the packages. You can print a list of information about the installed packages by typing `apt show <package-name>`

or for everything:
`apt show samtools trimmomatic picard-tools fastqc exonerate mummer ncbi-blast+ bowtie bowtie2 trinity repeatmasker-recon r-base python-biopython bioperl tophat`

If necessary, we can hold packages from updating automatically. In other words we can lock the list of applications above in their current state using the commands below:
###Freeze Versions
sudo apt-mark hold <package-name>
###UnFreeze
sudo apt-mark unhold <package-name>

##Custom Installation
It's generally preferred to install custom packages in the /opt/ directory. When compiling a package, you are generally prompted to pick an installation directory which you can set to /opt/package-name/. 
