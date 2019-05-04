# Git Branching
Branching means you diverge from the main line of development and continue to do work without messing with that main line. The way Git branches is incredibly lightweight, making branching operations nearly instantaneous, and switching back and forth between branches generally just as fast.  
## Branches in a Nutshell
When you make a commit, Git stores a commit object that contains a pointer to the snapshot of the content you staged. This object also contains the author’s name and email address, the message that you typed, and pointers to the commit or commits that directly came before this commit (its parent or parents): zero parents for the initial commit, one parent for a normal commit, and multiple parents for a commit that results from a merge of two or more branches.  
To visualize this, let’s assume that you have a directory containing three files, and you stage them all and commit. Staging the files computes a checksum for each one (the SHA-1 hash we mentioned in Getting Started), stores that version of the file in the Git repository (Git refers to them as blobs), and adds that checksum to the staging area:  
**$ git add README test.rb LICENSE  
$ git commit -m 'The initial commit of my project'**  
Your Git repository now contains five objects: three blobs (each representing the contents of one of the three files), one tree that lists the contents of the directory and specifies which file names are stored as which blobs, and one commit with the pointer to that root tree and all the commit metadata.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/branch1.PNG)  
If you make some changes and commit again, the next commit stores a pointer to the commit that came immediately before it:
![image](https://github.com/bing1980/Pro-Git/blob/master/img/branch2.PNG)  
A branch in Git is simply a lightweight movable pointer to one of these commits. The default branch name in Git is master. As you start making commits, you’re given a master branch that points to the last commit you made. Every time you commit, the master branch pointer moves forward automatically.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/branch3.PNG)  
### Creating a New Branch 
