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
A branch in Git is simply a lightweight movable pointer to one of these commits. The default branch name in Git is **master**. As you start making commits, you’re given a master branch that points to the last commit you made. Every time you commit, the master branch pointer moves forward automatically.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/branch3.PNG)  
### Creating a New Branch 
**$ git branch testing**  
This creates a new pointer to the same commit you’re currently on.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/branch4.PNG)  
How does Git know what branch you’re currently on? It keeps a special pointer called **HEAD**.  
In this case, you’re still on master. The git branch command only created a new branch — it didn’t switch to that branch.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/branch5.PNG)  
You can easily see this by running:  
**$ git log --oneline --decorate**  
> f30ab (HEAD -> master, testing) add feature #32 - ability to add new formats to the central interface  
34ac2 Fixed bug #1328 - stack overflow under certain conditions  
98ca9 The initial commit of my project  
### Switching Branches
To switch to an existing branch, you run the **git checkout** command. Let’s switch to the new testing branch:  
**$ git checkout testing**  
This moves HEAD to point to the testing branch.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/switch1.PNG)  
Well, let’s do another commit:  
**$ vim test.rb  
$ git commit -a -m 'made a change'**  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/switch2.PNG)  
This is interesting, because now your testing branch has moved forward, but your master branch still points to the commit you were on when you ran git checkout to switch branches. Let’s switch back to the master branch:  
**$ git checkout master**  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/switch3.PNG)  
That command did two things. It moved the HEAD pointer back to point to the master branch, and it
reverted the files in your working directory back to the snapshot that master points to. This also means the changes you make from this point forward will diverge from an older version of the project.  
Let’s make a few changes and commit again:  
**$ vim test.rb
$ git commit -a -m 'made other changes'**  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/switch4.PNG)  
If you run **git log --oneline --decorate --graph --all** it will print out the history of your commits, showing where your branch pointers are and how your history has diverged.  
**$ git log --oneline --decorate --graph --all**  
> * c2b9e (HEAD, master) made other changes  
| * 87ab2 (testing) made a change  
|/  
* f30ab add feature #32 - ability to add new formats to the  
* 34ac2 fixed bug #1328 - stack overflow under certain conditions  
* 98ca9 initial commit of my project  

Because a branch in Git is actually a simple file that contains the 40 character SHA-1 checksum of the commit it points to, branches are cheap to create and destroy. Creating a new branch is as quick and simple as writing 41 bytes to a file (40 characters and a newline).

## Basic Branching and Merging




