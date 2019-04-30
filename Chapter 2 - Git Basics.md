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
### Recording Changes to the Repository
Remember that each file in your working directory can be in one of two states: ***tracked or untracked***. Tracked files are files that were in the last snapshot; they can be unmodified, modified, or staged. In short, tracked files are files that Git knows about.  
Untracked files are everything else — any files in your working directory that were not in your last snapshot and are not in your staging area.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/lifecycle.PNG)
#### Checking the Status of Your Files
The main tool you use to determine which files are in which state is the ***git status*** command.  
If you run this command directly after a clone, you should see something like this:  
*$ git status  
On branch master  
Your branch is up-to-date with 'origin/master'.  
nothing to commit, working directory clean*  

Let’s say you add a new file to your project, a simple **README** file. If the file didn’t exist before, and you run git status, you see your untracked file like so:  
*$ echo 'My Project' > README  
$ git status  
On branch master  
Your branch is up-to-date with 'origin/master'.  
Untracked files:  
(use "git add \<file>..." to include in what will be committed)*    

*README*

*nothing added to commit but untracked files present (use "git add" to track)*
#### Tracking New Files
