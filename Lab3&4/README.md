## DWP Deployment Plan ##

This plan will cover deployment from a local enviornment, to a staging enviornment, to a production enviornment.

*This Plan assumes that you already have oh-my-zsh plugin for for your local development enviorment.*

    $ git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
    
    

### Configure Staging Enviornment *Ubuntu 12.04.3 x64*###

**Create sudo user**

    $ ssh root@your.ip.address 
    $ enter your password   
    $ sudo adduser username
    
**Configure user defaults, then add to sudo group**

    $ sudo adduser username sudo
    
*Logout as root, and login as new sudo user.*

**Update and upgrade Package system**

    $ sudo apt-get update
    $ sudo apt-get upgrade
    $ sudo apt-get update
    
**Update and upgrade os level packages (*with safe-upgrade!*)**

    $ sudo aptitude update
    $ sudo aptitude safe-upgrade
    $ sudo reboot

**Install Apache, restart**

    $ sudo apt-get install apache2
    $ sudo service apache2 restart
    
**Config Apache localhost, subdirectory linking, and symlinks**

    $ sudo pico /etc/apache2/conf.d/security
    
*Uncomment Directory tag, add **Options FollowSymLinks** inside tag, and **ServerName localhost** at bottom of file*

**Restart Apache**

    $ sudo service apache2 restart

**Change ownership of www directory**

    $ sudo chmod username /var/www
    
**Install Git Core**

    $ sudo apt-get install git-core
    $ git config --global user.name "name"
    $ git config --global user.email "email"
    $ git config --list




## Configure Git Directories (**Staging to Production**) ##

**Create Repos directory under var/**

    $ sudo mkdir /var/repos/
    
**Change ownership to repos directory**

    $ sudo chown YourUserName /var/repos
    
**Inside of the repos, create a site director.git, instatiate a version control of git, in the master branch as default.**

    $ cd /var/repos && mkdir SiteName.git && cd SiteName.git
    $ git init --bare   (no source files needed)

**Set up hook to sync branch**

    $ cd /var/repos/SiteName.git/hooks/
    
**Create hook to force push directory contents on commit**

    $ sudo pico post-receive
    
    (TYPE IN HOOK FILE)
      #!/bin/sh
      GIT_WORK_TREE=/var/www git checkout -f

**Give Permissions to execute**

    $ sudo chmod +x hooks/post-receive
    
**Set up remote to push to Production Server**

     $ git remote add productionRemoteName ssh://YourUserName@production.id.address/var/repos/SiteName.git
     
* **To view remotes** 

    $ pico .git/config
    
    
 * **To push to production server**
    
    $ git push productionRemoteName master
    


## Create Local Staging Remote ##

Change to your local development git directory
    
Set up local remote to Staging server 

    $ git remote add stagingRemoteName ssh://YourUserName@staging.ip.address/var/repos/SiteName.git
    
* **To view remotes** 

    $ pico .git/config
    


    
 Once you make a change to this directory, and it works properly:
 
 * You want add the changes
    
    $ git add -A (for overiding the whole directory)
    
 
 * Then commit
    
    $ git commit -m "Commit message inside here"
    
    
 * Then push to server
    
    $ git push stagingRemoteName master
    


### COMMUNICATE WITH YOUR TEAM ###

Notify the DWP Private group on slack of successful deployment with a link to the IP address of the server and your repo’s github url.
