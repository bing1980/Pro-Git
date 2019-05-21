# Distributed Git
## Distributed Workflows
In contrast with Centralized Version Control Systems **(CVCSs)**, the distributed nature of Git allows you to be far more flexible in how developers collaborate on projects. In centralized systems, every developer is a node working more or less equally with a central hub. In Git, however, every developer is potentially both a node and a hub.  
### Centralized Workflow
One central hub, or repository, can accept code, and everyone synchronizes their work with it. A number of developers are nodes — consumers of that hub — and synchronize with that centralized location.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/centralized_workflow.PNG)  
This means that if two developers clone from the hub and both make changes, the first developer to push their changes back up can do so with no problems. The second developer must merge in the first one’s work before pushing changes up, so as not to overwrite the first developer’s changes.  
### Integration-Manager Workflow
This scenario often includes a canonical repository that represents the “official” project. To contribute to that project, you create your own public clone of the project and push your changes to it. Then, you can send a request to the maintainer of the main project to pull in your changes. The maintainer can then add your repository as a remote, test your changes locally, merge them into their branch, and push back to their repository. The process works as follows:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/IM_workflow.PNG)  
This is a very common workflow with hub-based tools like **GitHub** or **GitLab**, where it’s easy to fork a project and push your changes into your fork for everyone to see.  
### Dictator and Lieutenants Workflow
This is a variant of a multiple-repository workflow. It’s generally used by huge projects with hundreds of collaborators. The process works like this:  
1.  Regular developers work on their topic branch and rebase their work on top of master. The master branch is that of the reference repository to which the dictator pushes.  
2. Lieutenants merge the developers' topic branches into their master branch.  
3. The dictator merges the lieutenants' master branches into the dictator’s master branch.  
4. Finally, the dictator pushes that master branch to the reference repository so the other developers can rebase on it.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/DL_workflow.PNG)

## Contributing to a Project
The main difficulty with describing how to contribute to a project are the numerous variations on how to do that.  
* The first variable is **active contributor count** — how many users are actively contributing code to this project, and how often?  
* The second variable is the **workflow** in use for the project.  
* The next variable is your **commit access**.  
### Commit Guidelines
The Git project provides a document that lays out a number of good tips for creating commits from which to submit patches — you can read it in the Git source code in the **Documentation/SubmittingPatches** file.  
First, your submissions should not contain any **whitespace errors**. Git provides an easy way to check for this — before you commit, run **git diff --check**  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/gitdiff.PNG)  
Next, try to make each commit a logically separate changeset.  
The last thing to keep in mind is the commit message. Getting in the habit of creating quality commit messages makes using and collaborating with Git a lot easier.  
### Private Small Team
Let’s see what it might look like when two developers start to work together with a shared repository.  
The first developer, John, clones the repository, makes a change, and commits locally:  
> $ git clone john@githost:simplegit.git  
Cloning into 'simplegit'...  
...  
$ cd simplegit/  
$ vim lib/simplegit.rb  
$ git commit -am 'remove invalid default value'  
[master 738ee87] remove invalid default value  
1 files changed, 1 insertions(+), 1 deletions(-)  

The second developer, Jessica, does the same thing — clones the repository and commits a change:  
> $ git clone jessica@githost:simplegit.git  
Cloning into 'simplegit'...  
...  
$ cd simplegit/  
$ vim TODO  
$ git commit -am 'add reset task'  
[master fbff5bc] add reset task  
1 files changed, 1 insertions(+), 0 deletions(-)  

Now, Jessica pushes her work to the server, which works just fine:  
> $ git push origin master  
...  
To jessica@githost:simplegit.git  
1edee6b..fbff5bc master -> master  

Continuing with this example, shortly afterwards, John makes some changes, commits them to his local repository, and tries to push them to the same server:  
