# Git on the Server
The preferred method for collaborating with someone is to set up an intermediate repository that you both have access to, and push to and pull from that.    
Running a Git server is fairly straightforward. First, you choose which **protocols** you want your server to support.  
A remote repository is generally a bare repository — a Git repository that has no working directory. Because the repository is only used as a collaboration point, there is no reason to have a snapshot checked out on disk; it’s just the Git data. In the simplest terms, a bare repository is the contents of your project’s .git directory and nothing else.  
## The Protocols  
Git can use four distinct protocols to transfer data: **Local, HTTP, Secure Shell (SSH) and Git**.  
### Local Protocol  
The most basic is the Local protocol, in which the remote repository is in another directory on the **same host**. This is often used if everyone on your team has access to a shared filesystem such as an **NFS mount**.  
To clone a repository like this, or to add one as a remote to an existing project, use the path to the repository as the **URL**. For example, to clone a local repository, you can run something like this:  
**$ git clone /srv/git/project.git**  
or：  
**$ git clone file:///srv/git/project.git**  
To add a local repository to an existing Git project, you can run something like this:  
**$ git remote add local_proj /srv/git/project.git**  
### The HTTP Protocols  
Git can communicate over HTTP using two different modes: the **Smart HTTP** protocol and the older way as **Dumb HTTP**.  
#### Smart HTTP
It has probably become the most popular way to use Git now, since it can be set up to both serve anonymously like the git:// protocol, and can also be pushed over with authentication and encryption like the SSH protocol.  
In fact, for services like GitHub, the URL you use to view the repository online is the same URL you can use to clone and, if you have access, push over.  
#### Dumb HTTP  
If the server does not respond with a Git HTTP smart service, the Git client will try to fall back to the simpler Dumb HTTP protocol.  
The Dumb protocol expects the bare Git repository to be served like normal files from the **web server**. Basically, all you have to do is put a bare Git repository under your HTTP document root and set up a specific **post-update hook**, and you’re done.  
To allow read access to your repository over HTTP, do something like this:  
> **$ cd /var/www/htdocs/  
$ git clone --bare /path/to/git_project gitproject.git  
$ cd gitproject.git  
$ mv hooks/post-update.sample hooks/post-update  
$ chmod a+x hooks/post-update**  

The post-update hook that comes with Git by default runs the appropriate command (git update-server-info) to make HTTP fetching and cloning work properly. This command is run when you push to this repository (over SSH perhaps); then, other people can clone via something like:  
**$ git clone https://example.com/gitproject.git**  
### The SSH Protocol
A common transport protocol for Git when self-hosting is over SSH. This is because SSH access to servers is already set up in most places — and if it isn’t, it’s easy to do. SSH is also an authenticated network protocol and, because it’s ubiquitous, it’s generally easy to set up and use.  
To clone a Git repository over SSH, you can specify an ssh:// URL like this:  
**$ git clone ssh://[user@]server/project.git**  
Or you can use the shorter scp-like syntax for the SSH protocol:  
**$ git clone [user@]server:project.git**  
### The Git Protocol
This is a special daemon that comes packaged with Git; it listens on a dedicated port **(9418)** that provides a service similar to the SSH protocol, but with absolutely **no authentication**. In order for a repository to be served over the Git protocol, you must create a
**git-daemon-export-ok** file — the daemon won’t serve a repository without that file in it — but, other than that, there is no security.  

## Getting Git on a Server
In order to initially set up any Git server, you have to export an existing repository into a new **bare repository** — a repository that doesn’t contain a working directory.  
In order to clone your repository to create a new bare repository, you run the clone command with the **--bare** option like this:  
**$ git clone --bare my_project my_project.git**  
> Cloning into bare repository 'my_project.git'...  
> done.  

This is roughly equivalent to something like:  
**$ cp -Rf my_project/.git my_project.git**  
### Putting the Bare Repository on a Server  
Now that you have a bare copy of your repository, all you need to do is put it on a server and set up your protocols.  
Let’s say you’ve set up a server called git.example.com to which you have SSH access, and you want to store all your Git repositories under the /srv/git directory.  
**$ scp -r my_project.git user@git.example.com:/srv/git**  
At this point, other users who have SSH-based **read access** to the /srv/git directory can **clone** your repository by running:    
**$ git clone user@git.example.com:/srv/git/my_project.git**  
If a user SSHs into a server and has **write access** to the /srv/git/my_project.git directory, they will also automatically have **push access**.  
Git will automatically **add group write permissions** to a repository properly if you run the **git init** command with the **--shared** option:  
**$ ssh user@git.example.com  
$ cd /srv/git/my_project.git  
$ git init --bare --shared**  
## Generating Your SSH Public Key
If you don’t have these files (or you don’t even have a .ssh directory), you can create them by running a program called ssh-keygen, which is provided with the SSH package on Linux/macOS systems and comes with Git for Windows:  
**$ ssh-keygen -o**  
## Setting Up the Server
First, you create a git user account and a **.ssh** directory for that user.  
**$ sudo adduser git  
$ su git  
$ cd  
$ mkdir .ssh && chmod 700 .ssh  
$ touch .ssh/authorized_keys && chmod 600 .ssh/authorized_keys**  
Next, you need to add some developer SSH public keys to the authorized_keys file for the git user.  
You just append them to the git user’s authorized_keys file in its .ssh directory:  
**$ cat /tmp/id_rsa.john.pub >> ~/.ssh/authorized_keys  
$ cat /tmp/id_rsa.josie.pub >> ~/.ssh/authorized_keys**  
Now, you can set up an empty repository for them by running git init with the --bare option, which initializes the repository without a working directory:  
**$ cd /srv/git  
$ mkdir project.git  
$ cd project.git  
$ git init --bare    
Initialized empty Git repository in /srv/git/project.git/**  
Then, John or Josiecan push the first version of their project into that repository by adding it as a remote and pushing up a branch.  
(for example: on John's computer)  
**$ cd myproject  
$ git init  
$ git add .  
$ git commit -m 'initial commit'  
$ git remote add origin git@gitserver:/srv/git/project.git  
$ git push origin master**  
At this point, the others can clone it down and push changes back up just as easily:  
**$ git clone git@gitserver:/srv/git/project.git  
$ cd project  
$ vim README  
$ git commit -am 'fix for the README file'  
$ git push origin master**  
## Git Daemon
In any case, the Git protocol is relatively easy to set up. Basically, you need to run this command in a daemonized manner:  
**$ git daemon --reuseaddr --base-path=/srv/git/ /srv/git/**  
The --reuseaddr option allows the server to restart without waiting for old connections to time out, while the --base-path option allows people to clone projects without specifying the entire path, and the path at the end tells the Git daemon where to look for repositories to export.  
Since systemd is the most common init system among modern Linux distributions, you can use it for that purpose. Simply place a file in **/etc/systemd/system/git-daemon.service** with these contents:  
> [Unit]  
Description=Start Git Daemon  
[Service]  
ExecStart=/usr/bin/git daemon --reuseaddr --base-path=/srv/git/ /srv/git/  
Restart=always  
RestartSec=500ms  
StandardOutput=syslog  
StandardError=syslog  
SyslogIdentifier=git-daemon  
User=git  
Group=git  
[Install]  
WantedBy=multi-user.target  

Finally, you’ll run systemctl enable git-daemon to automatically start the service on boot.
Next, you have to tell Git which repositories to allow unauthenticated Git server-based access to. You can do this in each repository by creating a file named **git-daemon-export-ok**.  
**$ cd /path/to/project.git  
$ touch git-daemon-export-ok**  

## Smart HTTP
