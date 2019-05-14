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
