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
If your fix for issue #53 modified the same part of a file as the hotfix branch, you’ll get a merge conflict that looks something like this:  
**$ git merge iss53**  
> Auto-merging index.html
CONFLICT (content): Merge conflict in index.html  
Automatic merge failed; fix conflicts and then commit the result.  

Git hasn’t automatically created a new merge commit. It has paused the process while you resolve the conflict. If you want to see which files are unmerged at any point after a merge conflict, you can run git status:  
**$ git status**  
> On branch master  
You have unmerged paths.  
(fix conflicts and run "git commit")  
Unmerged paths:  
(use "git add <file>..." to mark resolution)  
both modified: index.html  
no changes added to commit (use "git add" and/or "git commit -a")  
  
Anything that has merge conflicts and hasn’t been resolved is listed as unmerged. Your file contains a section that looks something like this:  
> <<<<<<< HEAD:index.html  
\<div id="footer">contact : email.support@github.com</div>  
=======  
\<div id="footer">  
please contact us at support@github.com  
\</div>  
\>>>>>>> iss53:index.html  

In order to resolve the conflict, you have to either choose one side or the other or merge the contents yourself. For instance, you might resolve this conflict by replacing the entire block with this:  
**\<div id="footer">  
please contact us at email.support@github.com  
\</div>**  
After you’ve resolved each of these sections in each conflicted file, run **git add** on each file to mark it as resolved. Staging the file marks it as resolved in Git.  


If you want to use a graphical tool to resolve these issues, you can run **git mergetool**, which fires up an appropriate visual merge tool and walks you through the conflicts:  
**$ git mergetool**  
> This message is displayed because 'merge.tool' is not configured.  
See 'git mergetool --tool-help' or 'git help config' for more details.  
'git mergetool' will now attempt to use one of the following tools:  
opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse diffmerge ecmerge  
p4merge araxis bc3 codecompare vimdiff emerge  
Merging:  
index.html  
Normal merge conflict for 'index.html':  
{local}: modified file  
{remote}: modified file  
Hit return to start merge resolution tool (opendiff):  

After you exit the merge tool, Git asks you if the merge was successful. If you tell the script that it was, it stages the file to mark it as resolved for you. You can run git status again to verify that all conflicts have been resolved:  
**$ git status**  
> On branch master  
All conflicts fixed but you are still merging.  
(use "git commit" to conclude merge)  
Changes to be committed:  
modified: index.html  

If you’re happy with that, and you verify that everything that had conflicts has been staged, you can type **git commit** to finalize the merge commit.  

## Branch Management
The **git branch** command does more than just create and delete branches. If you run it with no arguments, you get a simple listing of your current branches:  
**$ git branch**  
> iss53  
> \* master  
> testing  

Notice the * character indicates the branch that you currently have checked out.
To see the last commit on each branch, you can run **git branch -v**:  
**$ git branch -v**  
> iss53 93b412c fix javascript issue  
> \* master 7a98805 Merge branch 'iss53'  
> testing 782fd34 add scott to the author list in the readmes  

The useful **--merged** and **--no-merged** options can filter this list to branches that you have or have not yet merged into the branch you’re currently on.  
**$ git branch --merged**  
> iss53  
\* master  

**$ git branch --no-merged**
> testing

## Branching Workflows  
### Long-Running Branches
Many Git developers have a workflow which having only code that is entirely **stable** in their **master branch** — possibly only code that has been or will be released. They have another **parallel branch** named develop or next that they work from or use to test stability — it isn’t necessarily always stable, but **whenever it gets to a stable state, it can be merged into master**.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/workflow1.PNG)
### Topic Branches
Consider an example of doing some work (on **master**), branching off for an issue (**iss91**), working on it for a bit, branching off the second branch to try another way of handling the same thing (**iss91v2**), going back to your master branch and working there for a while, and then branching off there to do some work that you’re not sure is a good idea (**dumbidea** branch). Your commit history will look something like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/workflow2.PNG)  

Now, let’s say you decide you like the second solution to your issue best (**iss91v2**); and you showed the **dumbidea** branch to your coworkers, and it turns out to be genius. You can throw away the original iss91 branch (**losing commits C5 and C6**) and merge in the other two. Your history then looks like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/workflow3.PNG)  
It’s important to remember when you’re doing all this that these branches are completely local.  

## Remote Branches
You can get a full list of remote references explicitly with git **ls-remote [remote]**, or **git remote show [remote]** for remote branches as well as more information. Nevertheless, a more common way is to take advantage of **remote-tracking** branches.  
Remote-tracking branches are references to the state of remote branches. They’re local references that you can’t move; Git moves them for you whenever you do any network communication, to make sure they accurately represent the state of the remote repository.  

Let’s say you have a Git server on your network at ***git.ourcompany.com***. If you **clone** from this, Git’s clone command automatically names it origin for you, pulls down all its data, creates a pointer to where its master branch is, and names it **origin/master** locally. Git also gives you your own **local master** branch starting at the same place as origin’s master branch, so you have something to work from.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/remote_branch1.PNG)  
If you do some work on your local master branch, and, in the meantime, someone else pushes to *git.ourcompany.com* and updates its master branch, then your histories move forward differently. Also, as long as you stay out of contact with your origin server, your **origin/master** pointer doesn’t move.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/remote_branch2.PNG)  
To synchronize your work with a given remote, you run a **git fetch \<remote>** command (in our case, **git fetch origin**). This command looks up which server “origin” is (in this case, it’s git.ourcompany.com), fetches any data from it that you don’t yet have, and **updates your local database**, moving your **origin/master** pointer to its new, more up-to-date position.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/remote_branch3.PNG)  
Let’s assume you have another internal Git server that is used only for development by one of your sprint teams. This server is at *git.team1.ourcompany.com*. You can add it as a **new remote reference** to the project you’re currently working on by running the **git remote add** command as we covered in Git Basics. Name this remote **teamone**, which will be your shortname for that whole URL.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/remote_branch4.PNG)  
Now, you can run **git fetch teamone** to fetch everything the remote teamone server has that you don’t have yet. Because that server has a subset of the data your origin server has right now, Git fetches no data but **sets a remote-tracking branch** called **teamone/master** to point to the commit that teamone has as its master branch.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/remote_branch5.PNG)  
### Pushing  
When you want to share a branch with the world, you need to push it up to a remote to which you have write access.  
If you have a branch named **serverfix** that you want to work on with others, you can push it up the same way you pushed your first branch. Run **git push \<remote> \<branch>**:  
**$ git push origin serverfix**  
> Counting objects: 24, done.  
Delta compression using up to 8 threads.  
Compressing objects: 100% (15/15), done.  
Writing objects: 100% (24/24), 1.91 KiB | 0 bytes/s, done.  
Total 24 (delta 2), reused 0 (delta 0)  
To https://github.com/schacon/simplegit  
\* [new branch] serverfix -> serverfix  

Git automatically expands the serverfix branchname out to **refs/heads/serverfix:refs/heads/serverfix**, which means, “Take my serverfix local branch and push it to update the remote’s serverfix branch.” You can also do git push origin serverfix:serverfix, which does the same thing. If you didn’t want it to be called serverfix on the remote, you could instead run **git push origin serverfix:awesomebranch** to push your local serverfix branch to the awesomebranch branch on the remote project.  

The next time one of your collaborators fetches from the server, they will get a reference to where the server’s version of serverfix is under the remote branch origin/serverfix:  
**$ git fetch origin**  
> remote: Counting objects: 7, done.  
remote: Compressing objects: 100% (2/2), done.  
remote: Total 3 (delta 0), reused 3 (delta 0)  
Unpacking objects: 100% (3/3), done.  
From https://github.com/schacon/simplegit  
\* [new branch] serverfix -> origin/serverfix  

In this case, you don’t have a new serverfix branch — you have only an origin/serverfix pointer that you can’t modify.  
To merge this work into your current working branch, you can run **git merge origin/serverfix**. If you want your own serverfix branch that you can work on, you can base it off your remotetracking branch:  
**$ git checkout -b serverfix origin/serverfix**  
> Branch serverfix set up to track remote branch serverfix from origin.  
Switched to a new branch 'serverfix'  

### Tracking Branches
Checking out a local branch from a remote-tracking branch automatically creates what is called a “tracking branch”.Tracking branches are
local branches that have a direct relationship to a remote branch.    
When you **clone** a repository, it generally automatically creates a master branch that tracks **origin/master**.  
The simple case is the example you just saw, running **git checkout -b \<branch> \<remote>/\<branch>**. This is a common enough operation that Git provides the --track shorthand:  
**$ git checkout --track origin/serverfix**  
> Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'

To set up a local branch with a different name than the remote branch, you can easily use the first version with a different local branch name:  
**$ git checkout -b sf origin/serverfix**  
> Branch sf set up to track remote branch serverfix from origin.
Switched to a new branch 'sf'

If you already have a local branch and want to set it to a remote branch you just pulled down, or want to change the upstream branch you’re tracking:  
**$ git branch -u origin/serverfix**  
> Branch serverfix set up to track remote branch serverfix from origin.

If you want to see what tracking branches you have set up, you can use the **-vv** option to git branch.  
**$ git branch -vv**  
> iss53 7e424c3 [origin/iss53: ahead 2] forgot the brackets  
master 1ae2a45 [origin/master] deploying index fix  
\* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it  
testing 5ea463a trying something new  

So here we can see that our iss53 branch is tracking **origin/iss53** and is **“ahead”** by two, meaning that we have two commits locally that are not pushed to the server. We can also see that our master branch is tracking **origin/master** and is up to date. Next we can see that our serverfix branch is tracking the server-fix-good branch on our teamone server and is **ahead by three and behind by one**, meaning that there is one commit on the server we haven’t merged in yet and three commits locally that we haven’t pushed. Finally we can see that our testing branch is not tracking any remote branch.  

If you want totally up to date ahead and behind numbers, you’ll need to fetch from all your remotes right before running this. You could do that like this:  
**$ git fetch --all; git branch -vv** 

### Pulling
There is a command called **git pull** which is essentially a **git fetch** immediately followed by a **git merge** in most cases.  
Git pull will look up what server and branch your current branch is tracking, fetch from that server and then try to merge in that remote branch.  

### Deleting Remote Branches
Suppose you’re done with a remote branch, you can delete a remote branch using the **--delete** option to **git push**. If you want to
delete your **serverfix** branch from the server, you run the following:  
**$ git push origin --delete serverfix**  
> To https://github.com/schacon/simplegit  
\- [deleted] &nbsp;&nbsp;&nbsp; serverfix  

## Rebasing
In Git, there are two main ways to integrate changes from one branch into another: the **merge** and the **rebase**.  
### The Basic Rebase
Compare to basic merge diagram as bellow:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/rebase1.PNG)  
However, there is another way: you can take the patch of the change that was introduced in C4 and reapply it on top of C3. In Git, this is called **rebasing**. With the rebase command, you can take all the changes that were committed on one branch and replay them on a different branch.  
For this example, you would check out the experiment branch, and then rebase it onto the master branch as follows:  
**$ git checkout experiment**  
**$ git rebase master**  
> First, rewinding head to replay your work on top of it...  
Applying: added staged command  

![image](https://github.com/bing1980/Pro-Git/blob/master/img/rebase2.PNG)  
At this point, you can go back to the master branch and do a fast-forward merge.  
**$ git checkout master**  
**$ git merge experiment**  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/rebase3.PNG)  




















