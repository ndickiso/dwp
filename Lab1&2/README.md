## DWP Project Deployment Plan ##


### This a plan to deploy your project from a local git directory to a git directory on your server ###


*This Plan assumes that you already have git core installed on your server, and oh-my-zsh plugin for terminal*

    $ git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
    
    $ apt-get install git-core

#### * Ignore this step if you already have a git directiry set up to push to on server * ####

## Configure Git Directories (**Server**) ##

Create Repos directory under var/

    $ sudo mkdir /var/repos/
    
Change ownership to repos directory

    $ sudo chown YourUserName /var/repos
    
Inside of the repos, create a site director.git, instatiate a version control of git, in the master branch as default.

    $ cd /var/repos && mkdir SiteName.git && cd SiteName.git
    
    $ git init --bare   (no source files needed)

Set up hook to sync branch

    $ cd /var/repos/SiteName.git/hooks/
    
Create hook

    $ sudo pico post-receive
    
    TYPE
      #!/bin/sh
      GIT_WORK_TREE=/var/www git checkout -f

Give Permissions to execute

    $ sudo chmod +x hooks/post-receive
    

## Create git directory (**Locally**)##

Inside of the directory you want to pull from, instatiate a version control of git, in the master branch as default.

    $ git init

This will create a hidden file .git, you can access it's config from inside the directory:

    $ pico .git/config 
    
Set up remote to server 

    $ git remote add remoteName ssh://YourUserName@IPAddress/var/repos/SiteName.git
    
*You will call this remote name (the word after add) when pushing to this remote*

    
 Once you make a change to this directory, and it works properly:
 
 * You want add the changes
    
    $ git add -A (for overiding the whole directory)
    
 
 * Then commit
    
    $ git commit -m "Commit message inside here"
    
    
 * Then push to server
    
    $ git push portfolio master
    


### COMMUNICATE WITH YOUR TEAM ###

Notify the DWP Private group on slack of successful deployment with a link to the IP address of the server and your repo’s github url.
