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
