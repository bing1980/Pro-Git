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


