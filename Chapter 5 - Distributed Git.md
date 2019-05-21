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
> $ git push origin master  
To john@githost:simplegit.git  
! [rejected] master -> master (non-fast forward)  
error: failed to push some refs to 'john@githost:simplegit.git'  

In this case, John’s push fails because of Jessica’s earlier push of her changes. John must first fetch Jessica’s upstream changes and merge them into his local repository before he will be allowed to push.  
As a first step, John fetches Jessica’s work:    
> $ git fetch origin  
...  
From john@githost:simplegit  
\+ 049d078...fbff5bc master -> origin/master  

At this point, John’s local repository looks something like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/PST1.PNG)  
Now John can merge Jessica’s work that he fetched into his own local work:  
> $ git merge origin/master  
Merge made by the 'recursive' strategy.  
TODO | 1 \+  
1 files changed, 1 insertions(+), 0 deletions(-)  

![image](https://github.com/bing1980/Pro-Git/blob/master/img/PST2.PNG)   
After testing Jessica's work locally, he would like to push the new merged work up to the server:  
> $ git push origin master  
...  
To john@githost:simplegit.git  
fbff5bc..72bbc59 master -> master  

![image](https://github.com/bing1980/Pro-Git/blob/master/img/PST3.PNG)  

In the meantime, Jessica has created a new topic branch called issue54, and made three commits to that branch.  
She hasn’t fetched John’s changes yet, so her commit history looks like this:     
![image](https://github.com/bing1980/Pro-Git/blob/master/img/PST4.PNG)  
Suddenly, Jessica learns that John has pushed some new work to the server and she wants to take a look at it, so she can fetch all new content from the server that she does not yet have with:  
> $ git fetch origin  
...  
From jessica@githost:simplegit  
fbff5bc..72bbc59 master -> origin/master  

Jessica’s history now looks like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/PST5.PNG)  
Jessica thinks her topic branch is ready, but she wants to know what part of John’s fetched work she has to merge into her work so that she can push. She runs **git log --no-merges issue54..origin/master** to find out.  
Now, Jessica can merge her topic work into her master branch, merge John’s work (origin/master) into her master branch, and then push back to the server again.  
First, Jessica switches back to her master branch in preparation for integrating all this work:  
> $ git checkout master  
Switched to branch 'master'  
Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded.  

Jessica can merge either origin/master or issue54 first,the order doesn’t matter.  
She chooses to merge the issue54 branch first:  
> $ git merge issue54  
Updating fbff5bc..4af4298  
Fast forward  
README | 1 +  
lib/simplegit.rb | 6 +++++-  
2 files changed, 6 insertions(+), 1 deletions(-)  

Jessica now completes the local merging process by merging John’s earlier fetched work that is sitting in the origin/master branch:  
> $ git merge origin/master  
Auto-merging lib/simplegit.rb  
Merge made by the 'recursive' strategy.  
lib/simplegit.rb | 2 +-  
1 files changed, 1 insertions(+), 1 deletions(-)  

Jessica’s history now looks like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/PST6.PNG)  
Now origin/master is reachable from Jessica’s master branch, so she should be able to successfully push:  
> $ git push origin master  
...  
To jessica@githost:simplegit.git  
72bbc59..8059c15 master -> master  

![image](https://github.com/bing1980/Pro-Git/blob/master/img/PST7.PNG)  

The general sequence is something like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/PST8.PNG)  

### Private Managed Team
In this next scenario, you’ll look at contributor roles in a larger private group.  
Let’s say that John and Jessica are working together on one feature (call this “featureA”), while Jessica and a third developer, Josie, are working on a second (say, “featureB”). In this case, the company is using a type of integration-manager workflow where the work of the individual groups is integrated only by certain engineers, and the master branch of the main repo can be updated only by those engineers.  

Assuming she already has her repository cloned, she decides to work on featureA first. She creates a new branch for the feature and does some work on it there:  
> $ git checkout -b featureA  
Switched to a new branch 'featureA'  
$ vim lib/simplegit.rb  
$ git commit -am 'add limit to log function'  

At this point, she needs to share her work with John, so she pushes her featureA branch commits up to the server. Jessica doesn’t have push access to the master branch — only the integrators do — so she has to push to another branch in order to collaborate with John:  
> $ git push -u origin featureA  
...  
To jessica@githost:simplegit.git  
\* [new branch] featureA -> featureA  

While she waits for feedback from John, Jessica decides to start working on featureB with Josie. To begin, she starts a new feature branch, basing it off the server’s master branch:  
> $ git fetch origin  
$ git checkout -b featureB origin/master  
Switched to a new branch 'featureB'  

Now, Jessica makes a couple of commits on the featureB branch:  
> $ vim lib/simplegit.rb  
$ git commit -am 'made the ls-tree function recursive'  
[featureB e5b0fdc] made the ls-tree function recursive  
1 files changed, 1 insertions(+), 1 deletions(-)  
$ vim lib/simplegit.rb  
$ git commit -am 'add ls-files'  
[featureB 8512791] add ls-files  
1 files changed, 5 insertions(+), 0 deletions(-)  

Jessica’s repository now looks like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/PMT1.PNG)  




