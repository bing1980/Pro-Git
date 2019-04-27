## Version Control System (VCS)
Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.
It allows you to revert selected files back to a previous state, revert the entire project back to a previous state, compare changes 
over time, see who last modified something that might be causing a problem, who introduced an issue and when, and more.

### Local Version Control Systems
Local VCSs that had a simple database that kept all changes to files under revision control.
![image](https://github.com/bing1980/Pro-Git/blob/master/img/lvcs.PNG)

### Centrialized Version Control Systems
Centrialized Version Control Systems(CVSs) have a single server that contains all the versioned files, and a number of clients that check out files from that central place. The downside of CVSs is that once the server goes down, nobody can collabrate or save versioned changes.
![image](https://github.com/bing1980/Pro-Git/blob/master/img/cvcs.PNG)


### Distributed Version Control Systems
In a DVCS (such as Git, Mercurial, Bazaar or Darcs), clients don’t just check out the latest snapshot of the files; rather, they fully mirror the repository, including its full history. Thus, if any server dies, and these systems were collaborating via that server, any of the client repositories can be copied back up to the server to restore it. Every clone is really a full backup of all the data.
![image](https://github.com/bing1980/Pro-Git/blob/master/img/dvcs.PNG)
