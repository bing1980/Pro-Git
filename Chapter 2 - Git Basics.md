## Recording Changes to the Repository
### Checking the Status of Your Files
> **$ git status**    
> On branch master  
> Your branch is up-to-date with 'origin/master'.  
> nothing to commit, working directory clean  
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
**$ git commit -a -m 'added new benchmarks'**  
> [master 83e38c7] added new benchmarks  
> 1 file changed, 5 insertions(+), 0 deletions(-)  

Notice how you don’t have to run git add on the CONTRIBUTING.md file in this case before you commit.  
That’s because the -a flag includes all changed files.
### Removing Files

