# Git Basics  
## Getting a Git Repository
You typically obtain a Git repository in one of two ways:  
1. You can take a local directory that is currently not under version control, and turn it into a Git repository, or  
2. You can clone an existing Git repository from elsewhere.  
In either case, you end up with a Git repository on your local machine, ready for work.
### Initializing a Repository in an Existing Directory
Go to your project directory on your machine and type: ***git init***  
This creates a new subdirectory named .git that contains all of your necessary repository files — a Git repository skeleton.  
If you want to start version-controlling existing files (as opposed to an empty directory), you should probably begin tracking those files and do an initial commit. You can accomplish that with a few ***git add*** commands that specify the files you want to track, followed by a ***git commit***:
* $ git add *.c
* $ git add LICENSE
* $ git commit -m 'initial project version'
### Cloning an Existing Repository
You clone a repository with ***git clone \<url>***. For example, if you want to clone the Git linkable library called libgit2, you can do so like this:  
  * $ git clone https://github.com/libgit2/libgit2
If you want to clone the repository into a directory named something other than libgit2, you can specify the new directory name as an additional argument:  
  * $ git clone https://github.com/libgit2/libgit2 mylibgit
## Recording Changes to the Repository
Remember that each file in your working directory can be in one of two states: ***tracked or untracked***. Tracked files are files that were in the last snapshot; they can be unmodified, modified, or staged. In short, tracked files are files that Git knows about.  
Untracked files are everything else — any files in your working directory that were not in your last snapshot and are not in your staging area.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/lifecycle.PNG)
### Checking the Status of Your Files
The main tool you use to determine which files are in which state is the ***git status*** command.  
If you run this command directly after a clone, you should see something like this:  
> **$ git status**    
> On branch master  
> Your branch is up-to-date with 'origin/master'.  
> nothing to commit, working directory clean  

Let’s say you add a new file to your project, a simple **README** file. If the file didn’t exist before, and you run git status, you see your untracked file like so:  
> **$ echo 'My Project' > README  
> $ git status**  
> On branch master  
> Your branch is up-to-date with 'origin/master'.  
> Untracked files:  
> (use "git add \<file>..." to include in what will be committed)    
> 
> README
> 
> nothing added to commit but untracked files present (use "git add" to track)
### Tracking New Files
In order to begin tracking a new file, you use the command **git add**.  
> **$ git add README**  
If you run your status command again, you can see that your README file is now tracked and staged to be committed:  
> **$ git status**
> On branch master  
> Your branch is up-to-date with 'origin/master'.  
> Changes to be committed:  
> (use "git reset HEAD <file>..." to unstage)  
>   
> new file: README
 
### Staging Modified Files
Let’s change a file that was already tracked. If you change a previously tracked file called **CONTRIBUTING.md** and then run your git status command again, you get something that looks like this:  
> **$ git status**  
> On branch master  
> Your branch is up-to-date with 'origin/master'.  
> Changes to be committed:  
> (use "git reset HEAD <file>..." to unstage)  
>   
> new file: README  
>   
> Changes not staged for commit:  
> (use "git add <file>..." to update what will be committed)  
> (use "git checkout -- <file>..." to discard changes in working directory) 
>  
> modified: CONTRIBUTING.md
>
 
The **CONTRIBUTING.md** file appears under a section named ***“Changes not staged for commit”*** — which means that a file that is tracked has been modified in the working directory but **not yet staged**.  
To stage it, you run the **git add** command.  
> **$ git add CONTRIBUTING.md**  
> **$ git status**  
> On branch master  
> Your branch is up-to-date with 'origin/master'.  
> Changes to be committed:  
> (use "git reset HEAD <file>..." to unstage)  
>   
> new file: README  
> modified: CONTRIBUTING.md  
>   

### Short Status
If you run **git status -s** or **git status --short** you get a far more simplified output from the command:  
> **$ git status -s**  
> M README  
> MM Rakefile  
> A lib/git.rb  
> M lib/simplegit.rb  
> ?? LICENSE.txt  

New files that aren’t tracked have a **??** next to them, new files that have been added to the staging area have an **A**, modified files have an **M** and so on.  
So for example in that output, the **README** file is modified in the working directory but not yet staged, while the **lib/simplegit.rb** file is modified and staged. The **Rakefile** was modified, staged and then modified again, so there are changes to it that are both staged and unstaged.
### Ignoring Files
Often, you’ll have a class of files that you don’t want Git to automatically add or even show you as being untracked. In such cases, you can create a file listing patterns to match them named **.gitignore**. Here is an example .gitignore file:  
> **$ cat .gitignore**  
> \*.[oa]    
> \*~
The first line tells Git to ignore any files ending in “.o” or “.a” — object and archive files that may be the product of building your code. The second line tells Git to ignore all files whose names end with a tilde (~).  

The rules for the patterns you can put in the .gitignore file are as follows:  
* Blank lines or lines starting with # are ignored.
* Standard glob patterns work, and will be applied recursively throughout the entire working tree.
* You can start patterns with a forward slash (/) to avoid recursivity.
* You can end patterns with a forward slash (/) to specify a directory.
* You can negate a pattern by starting it with an exclamation point (!).

### Viewing Your Staged and Unstaged Changes
If you want to know exactly what you changed, not just which files were changed — you can use the **git diff** command.  
That command compares what is in your working directory with what is in your staging area. The result tells you **the changes you’ve made that you haven’t yet staged.**:
> **$ git diff**  
> diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md  
> index 8ebb991..643e24f 100644  
> --- a/CONTRIBUTING.md  
> +++ b/CONTRIBUTING.md  
> @@ -65,7 +65,8 @@ branch directly, things can get messy.  
> Please include a nice description of your changes when you submit your PR;  
> ..... ......

If you want to see what you’ve staged that will go into your next commit, you can use **git diff --staged**. This command compares your staged changes to your last commit:  
> **$ git diff --staged**  
> diff --git a/README b/README  
> new file mode 100644  
> index 0000000..03902a1  
> --- /dev/null  
> +++ b/README  
> @@ -0,0 +1 @@  
> +My Project  

and **git diff --cached** to see what **you’ve staged so far** (--staged and --cached are synonyms).
### Committing Your Changes
Now that your staging area is set up the way you want it, you can commit your changes. Remember that anything that is still unstaged — any files you have created or modified that you haven’t run **git add** on since you edited them — **won’t go into this commit**.  
**$ git commit**  
(shows in editor)    
> Please enter the commit message for your changes. Lines starting  
> with '#' will be ignored, and an empty message aborts the commit.  
> On branch master  
> Your branch is up-to-date with 'origin/master'.  
>   
> Changes to be committed:  
> new file: README  
> modified: CONTRIBUTING.md  

you can pass the **-v** option to git commit. Doing so also puts the diff of your change in the editor.  
Alternatively, you can type your commit message inline with the commit command by specifying it after a **-m** flag, like this:  
**$ git commit -m "Story 182: Fix benchmarks for speed"**  
> [master 463dc4f] Story 182: Fix benchmarks for speed  
> 2 files changed, 2 insertions(+)  
> create mode 100644 README  

### Skipping the Staging Area
Adding the **-a** option to the git commit command makes Git automatically stage every file that is already tracked before doing the commit, letting you skip the git add part:  
**$ git commit -a -m "added new benchmarks"**  
> [master 83e38c7] added new benchmarks  
> 1 file changed, 5 insertions(+), 0 deletions(-)  

Notice how you don’t have to run git add on the CONTRIBUTING.md file in this case before you commit.  
That’s because the **-a flag includes all changed files**.
### Removing Files
To remove a file from Git, you have to remove it from your tracked files (more accurately, remove it from your staging area) and then commit. The **git rm command** does that, and also removes the file from your working directory so you don’t see it as an untracked file the next time around.  
**$ git rm PROJECTS.md**  
> rm 'PROJECTS.md'  

**$ git status**  
> On branch master  
Your branch is up-to-date with 'origin/master'.  
Changes to be committed:  
(use "git reset HEAD <file>..." to unstage)  
deleted: PROJECTS.md  
 
You may want to keep the file on your hard drive but not have Git track it anymore:  
**$ git rm --cached README**

### Moving Files
If you want to rename a file in Git, you can run something like:  
**$ git mv file_from file_to**  
for example:  
**$ git mv README.md README  
$ git status**  
> On branch master  
Your branch is up-to-date with 'origin/master'.  
Changes to be committed:  
(use "git reset HEAD <file>..." to unstage)  
renamed: README.md -> README  
 
## Viewing the Commit History
By default, with no arguments, **git log** lists the commits made in that repository in reverse chronological order.  
**$ git log**  
> commit ca82a6dff817ec66f44342007202690a93763949  
Author: Scott Chacon <schacon@gee-mail.com>  
Date: Mon Mar 17 21:52:11 2008 -0700  
changed the version number  

> commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7  
Author: Scott Chacon <schacon@gee-mail.com>  
Date: Sat Mar 15 16:40:33 2008 -0700  
removed unnecessary test  

One of the more helpful options is **-p or --patch**, which shows the difference (the patch output) introduced in each commit. You can also limit the number of log entries displayed, such as using **-2** to show only the last two entries.  
**$ git log -p -2**  
If you want to see some abbreviated stats for each commit, you can use the **--stat** option:  
**$ git log --stat**  
Another really useful option is **--pretty**. This option changes the log output to **formats** other than the default.  
**$ git log --pretty=oneline**  or  
**$ git log --pretty=format:"%h - %an, %ar : %s"**  
### Limiting Log Output
The **time-limiting** options such as **--since** and **--until** are very useful. For example, this command gets the list of commits made in the last two weeks:  
**$ git log --since=2.weeks**  
Another really helpful filter is the **-S** option, which takes a string and shows only those commits that changed the number of occurrences of that string. For instance, if you wanted to find the last commit that added or removed a reference to a specific function, you could call:  
**$ git log -S function_name**  

## Undoing Things
If you want to redo last commit, make the additional changes you forgot, stage them, and commit again using the **--amend** option:  
**$ git commit --amend**  
As an example, if you commit and then realize you forgot to stage the changes in a file you wanted to add to this commit, you can do something like this:  
**$ git commit -m 'initial commit'  
$ git add forgotten_file  
$ git commit --amend**  
### Unstaging a Staged File
To unstage the CONTRIBUTING.md file:  
**$ git reset HEAD CONTRIBUTING.md**  
> Unstaged changes after reset:  
> M CONTRIBUTING.md  
### Unmodifying a Modified File
To give up last changes of CONTRIBUTING.md:  
**$ git checkout -- CONTRIBUTING.md**

## Working with Remotes
Remote repositories are versions of your project that are hosted on the Internet or network somewhere. You can have several of them, each of which generally is either read-only or read/write for you. Collaborating with others involves managing these remote repositories and pushing and pulling data to and from them when you need to share work.  
### Showing Your Remotes
To see which remote servers you have configured, you can run the **git remote** command.  
If you’ve cloned your repository, you should at least see **origin** — that is the default name Git gives to the server you cloned from:  
**$ git clone https://github.com/schacon/ticgit  
$ cd ticgit  
$ git remote  
origin**    
You can also specify -v, which shows you the URLs that Git has stored for the shortname to be used when reading and writing to that remote:    
**$ git remote -v**  
> origin https://github.com/schacon/ticgit (fetch)  
> origin https://github.com/schacon/ticgit (push)  
### Adding Remote Repositories
To add a new remote Git repository as a shortname you can reference easily, run **git remote add \<shortname> \<url>**: 

**$ git remote add pb https://github.com/paulboone/ticgit**  
**$ git remote -v**    
> origin https://github.com/schacon/ticgit (fetch)  
origin https://github.com/schacon/ticgit (push)  
pb https://github.com/paulboone/ticgit (fetch)  
pb https://github.com/paulboone/ticgit (push)  

Now you can use the string **pb** on the command line in lieu of the whole URL. For example, if you want to fetch all the information that Paul has but that you don’t yet have in your repository, you can run **git fetch pb**:  
**$ git fetch pb**  
> remote: Counting objects: 43, done.  
remote: Compressing objects: 100% (36/36), done.   
remote: Total 43 (delta 10), reused 31 (delta 5)   
Unpacking objects: 100% (43/43), done.  
From https://github.com/paulboone/ticgit  
> * [new branch] master -> pb/master  
> * [new branch] ticgit -> pb/ticgit  

### Fetching and Pulling from Your Remotes
To get data from your remote projects, you can run:  
**$ git fetch \<remote>**  
The command goes out to that remote project and pulls down all the data from that remote project that you don’t have yet. After you do this, you should have references to all the branches from that remote, which you can merge in or inspect at any time.  
**git fetch origin** fetches any new work that has been pushed to that server since you **cloned (or last fetched from)** it.  
Running **git pull** generally fetches data from the server you originally cloned from and automatically tries to merge it into the code you’re currently working on.  

### Pushing to Your Remotes
When you have your project at a point that you want to share, you have to push it upstream. The command for this is simple:  
**git push \<remote> \<branch>.**  
If you want to push your master branch to your origin server:  
**$ git push origin master**  

### Inspecting a Remote
If you want to see more information about a particular remote, you can use the **git remote show \<remote>** command.  
**$ git remote show origin**  
> * remote origin  
Fetch URL: https://github.com/schacon/ticgit  
Push URL: https://github.com/schacon/ticgit  
HEAD branch: master  
Remote branches:  
master tracked  
dev-branch tracked  
Local branch configured for 'git pull':  
master merges with remote master  
Local ref configured for 'git push':  
master pushes to master (up to date)  

### Renaming and Removing Remotes
You can run **git remote rename** to change a remote’s shortname:  
**$ git remote rename pb paul**   
> $ git remote  
> origin  
> paul

If you want to remove a remote for some reason:  
**$ git remote remove paul**  
> $ git remote  
> origin 

## Tagging
Like most VCSs, Git has the ability to tag specific points in a repository’s history as being important.  
Typically, people use this functionality to mark release points (v1.0, v2.0 and so on).  
### Listing Your Tags
Listing the existing tags in Git is straightforward. Just type **git tag** (with optional -l or --list):  
**$ git tag**  
> v1.0  
> v2.0  
### Creating Tags
Git supports two types of tags: lightweight and annotated.  
A lightweight tag is very much like a branch that doesn’t change — it’s just a pointer to a specific commit.  
Annotated tags, however, are stored as full objects in the Git database.  
* **Annotated Tags**
Creating an annotated tag in Git is simple. The easiest way is to specify **-a** when you run the tag command:  
**$ git tag -a v1.4 -m "my version 1.4"**  
**$ git tag**  
  > v0.1
  > v1.3
  > v1.4  
The -m specifies a tagging message, which is stored with the tag.  
You can see the tag data along with the commit that was tagged by using the **git show** command  
* **Lightweight Tags**  
To create a lightweight tag, **don’t supply** any of the -a, -s, or -m options, just provide a tag name:  
**$ git tag v1.4-lw**  
**$ git tag**  
  > v0.1  
    v1.3  
    v1.4  
    v1.4-lw  

### Tagging Later
You can also tag commits after you’ve moved past them. Suppose your commit history looks like this:  
**$ git log --pretty=oneline**  
> 15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'  
> **9fceb02**d0ae598e95dc970b74767f19372d61af8 **updated rakefile**

Suppose you forgot to tag the project at v1.2, which was at the **“updated rakefile”** commit:  
**$ git tag -a v1.2 9fceb02**  
**$ git show v1.2**  
> tag v1.2  
Tagger: Scott Chacon <schacon@gee-mail.com>  
Date: Mon Feb 9 15:32:16 2009 -0800  
version 1.2  
commit 9fceb02d0ae598e95dc970b74767f19372d61af8  
Author: Magnus Chacon <mchacon@gee-mail.com>  
Date: Sun Apr 27 20:43:35 2008 -0700  
updated rakefile  
...  

### Sharing Tags




