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


## What is Git ?

### Snapshots, Not Differences
The major difference between Git and any other VCS (Subversion and friends included) is the way Git thinks about its data.
These other systems (CVS, Subversion, Perforce, Bazaar, and so on) think of the information they store as a set of files and the changes made to each file over time (this is commonly described as delta-based versiongitol).)

![image](https://github.com/bing1980/Pro-Git/blob/master/img/store_data_others.PNG)

Git doesn’t think of or store its data this way. Instead, Git thinks of its data more like a series of snapshots of a miniature filesystem.

![image](https://github.com/bing1980/Pro-Git/blob/master/img/store_data_git.PNG)

### Nearly Every Operation Is Local
Most operations in Git need only local files and resources to operate — generally no information is needed from another computer on your network. If you’re used to a CVCS where most operations have that network latency overhead, this aspect of Git will make you think that the gods of speed have blessed Git with unworldly powers. Because you have the entire history of the project right there on your local disk, most operations seem almost instantaneous.

### Git Has Integrity
The mechanism that Git uses for this checksumming is called a SHA-1 hash. This is a 40-character string composed of hexadecimal characters (0–9 and a–f) and calculated based on the contents of a file or directory structure in Git. A SHA-1 hash looks something like this: 
      <br/>**24b9da6552252987aa493b52f8696cd6d3b00373**</br>
You will see these hash values all over the place in Git because it uses them so much. In fact, Git stores everything in its database not by file name but by the hash value of its contents.

### The Three States
Git has three main states that your files can reside in: ***committed, modified**, and **staged**:*
* Committed means that the data is safely stored in your local database.
* Modified means that you have changed the file but have not committed it to your database yet.
* Staged means that you have marked a modified file in its current version to go into your next commit snapshot.

This leads us to the three main sections of a Git project: the Git directory, the working tree, and the staging area.

![image](https://github.com/bing1980/Pro-Git/blob/master/img/three_states.PNG)

The basic Git workflow goes something like this:
1. You modify files in your working tree.
2. You selectively stage just those changes you want to be part of your next commit, which adds only those changes to the staging area.
3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.

## First-Time Git Setup
Git comes with a tool called **git config** that lets you get and set configuration variables that control all aspects of how Git looks and operates.  
You can view all of your settings and where they are coming from:
<br/>**git config --list --show-origin**</br>

