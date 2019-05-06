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
> * f30ab add feature #32 - ability to add new formats to the  
> * 34ac2 fixed bug #1328 - stack overflow under certain conditions  
> * 98ca9 initial commit of my project  

Because a branch in Git is actually a simple file that contains the **40 character SHA-1** checksum of the commit it points to, branches are cheap to create and destroy. Creating a new branch is as quick and simple as writing 41 bytes to a file **(40 characters and a newline)**.

## Basic Branching and Merging
### Basic Branching  
First, let’s say you’re working on your project and have a couple of commits already on the master branch:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/basic_branching1.PNG)  
You’ve decided that you’re going to work on issue #53.  
To create a new branch and switch to it at the same time, you can run the **git checkout** command with the **-b** switch:  
**$ git checkout -b iss53**  
> Switched to a new branch "iss53"  

![image](https://github.com/bing1980/Pro-Git/blob/master/img/basic_branching2.PNG)  
You work on your website and do some commits. Doing so moves the iss53 branch forward, because you have it checked out (that is, your HEAD is pointing to it):  
**$ vim index.html**  
**$ git commit -a -m 'added a new footer [issue 53]'**  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/basic_branching3.PNG)  
For now, let’s assume you’ve committed all your changes, so you can switch back to your master branch:  
**$ git checkout master**  
> Switched to branch 'master' 

Next, you have a hotfix to make. Let’s create a hotfix branch on which to work until it’s completed:  
**$ git checkout -b hotfix**  
> Switched to a new branch 'hotfix'

**$ vim index.html**  
**$ git commit -a -m 'fixed the broken email address'**  
> [hotfix 1fb7853] fixed the broken email address
> 1 file changed, 2 insertions(+)  

![image](https://github.com/bing1980/Pro-Git/blob/master/img/basic_branching4.PNG)  
You can run your tests, make sure the hotfix is what you want, and finally merge the hotfix branch back into your master branch to deploy to production. You do this with the **git merge** command:  
**$ git checkout master**  
**$ git merge hotfix**  
> Updating f42c576..3a0874c  
Fast-forward  
index.html | 2 ++  
1 file changed, 2 insertions(+)  

Your change is now in the snapshot of the commit pointed to by the master branch, and you can deploy the fix. 
![image](https://github.com/bing1980/Pro-Git/blob/master/img/basic_branching5.PNG)  
After your super-important fix is deployed, you’re ready to switch back to the work you were doing before you were interrupted. However, first you’ll delete the hotfix branch, because you no longer need it — the master branch points at the same place. You can delete it with the **-d** option to git branch:  
**$ git branch -d hotfix**  
> Deleted branch hotfix (3a0874c).

Now you can switch back to your work-in-progress branch on issue #53 and continue working on it:  
**$ git checkout iss53**  
> Switched to branch "iss53"  

**$ vim index.html  
$ git commit -a -m 'finished the new footer [issue 53]'**  
> [iss53 ad82d7a] finished the new footer [issue 53]
1 file changed, 1 insertion(+)


![image](https://github.com/bing1980/Pro-Git/blob/master/img/basic_branching6.PNG)  

### Basic Merging
Suppose you’ve decided that your issue #53 work is complete and ready to be merged into your master branch. In order to do that, you’ll merge your iss53 branch into master, much like you merged your hotfix branch earlier. All you have to do is check out the branch you wish to merge into and then run the git merge command:  
**$ git checkout master**  
> Switched to branch 'master'  

**$ git merge iss53**  
> Merge made by the 'recursive' strategy.
index.html | 1 +
1 file changed, 1 insertion(+)

In this case, your development history has **diverged** from some older point. Because the commit on the branch you’re on isn’t a direct ancestor of the branch you’re merging in, Git has to do some work.In this case, Git does a simple **three-way merge**, using the two snapshots pointed to by the branch tips and the common ancestor of the two.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/basic_branching7.PNG)  
Instead of just moving the branch pointer forward, Git creates a new snapshot that results from this three-way merge and automatically creates a new commit that points to it. This is referred to as a merge commit, and is special in that it has more than one parent.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/basic_branching8.PNG)   
Now that your work is merged in, you have no further need for the iss53 branch. You can close the ticket in your ticket-tracking system, and delete the branch:  
**$ git branch -d iss53**  

### Basic Merge Conflicts











