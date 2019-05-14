# Git on the Server
The preferred method for collaborating with someone is to set up an intermediate repository that you both have access to, and push to and pull from that.    
Running a Git server is fairly straightforward. First, you choose which **protocols** you want your server to support.  
A remote repository is generally a bare repository — a Git repository that has no working directory. Because the repository is only used as a collaboration point, there is no reason to have a snapshot checked out on disk; it’s just the Git data. In the simplest terms, a bare repository is the contents of your project’s .git directory and nothing else.  
## The Protocols  
