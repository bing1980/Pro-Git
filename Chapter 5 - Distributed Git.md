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

She’s ready to push her work, but gets an email from Josie that a branch with some initial “featureB” work on it was already pushed to the server as the featureBee branch. Jessica needs to merge those changes with her own before she can push her work to the server. Jessica first fetches Josie’s changes with **git fetch**:  
> $ git fetch origin  
...  
From jessica@githost:simplegit  
\* [new branch] featureBee -> origin/featureBee  

Assuming Jessica is still on her checked-out featureB branch, she can now merge Josie’s work into that branch with git merge:  
> $ git merge origin/featureBee  
Auto-merging lib/simplegit.rb  
Merge made by the 'recursive' strategy.  
lib/simplegit.rb | 4 ++++  
1 files changed, 4 insertions(+), 0 deletions(-)  

At this point, Jessica wants to push all of this merged “featureB” work back to the server, but she doesn’t want to simply push her own featureB branch. Rather, since Josie has already started an upstream featureBee branch, Jessica wants to push to that branch, which she does with:  
> $ git push -u origin featureB:featureBee  
...  
To jessica@githost:simplegit.git  
fba9af8..cd685d1 featureB -> featureBee  

This is called a refspec.  
Suddenly, Jessica gets email from John, who tells her he’s pushed some changes to the featureA branch on which they are collaborating, and he asks Jessica to take a look at them. Again, Jessica runs a simple git fetch to fetch all new content from the server, including (of course) John’s latest work:  
> $ git fetch origin  
...  
From jessica@githost:simplegit  
3300904..aad881d featureA -> origin/featureA  

Jessica can display the log of John’s new work by comparing the content of the newly-fetched featureA branch with her local copy of the same branch:  
**$ git log featureA..origin/featureA**  
If Jessica likes what she sees, she can merge John’s new work into her local featureA branch with:  
> $ git checkout featureA  
Switched to branch 'featureA'  
$ git merge origin/featureA  
Updating 3300904..aad881d  
Fast forward  
lib/simplegit.rb | 10 +++++++++-  
1 files changed, 9 insertions(+), 1 deletions(-)  

Finally, Jessica might want to make a couple minor changes to all that merged content, so she is free to make those changes, commit them to her local featureA branch, and push the end result back to the server.  
> $ git commit -am 'small tweak'  
[featureA 774b3ed] small tweak  
1 files changed, 1 insertions(+), 1 deletions(-)  
$ git push  
...  
To jessica@githost:simplegit.git  
3300904..774b3ed featureA -> featureA  

Jessica’s commit history now looks something like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/PMT2.PNG)  
After the integrators merge these branches into the mainline, a fetch will bring down the new merge commit, making the history look like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/PMT3.PNG)  
The sequence for the workflow you saw here is something like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/PMT4.PNG)  

### Forked Public Project
First, you’ll probably want to clone the main repository, create a topic branch for the patch or patch series you’re planning to contribute, and do your work there. The sequence looks basically like this:  
> $ git clone <url>  
$ cd project  
$ git checkout -b featureA  
... work ...  
$ git commit  
... work ...  
$ git commit  

When your branch work is finished and you’re ready to contribute it back to the maintainers, go to the original project page and click the “Fork” button, creating your own writable fork of the project:
**$ git remote add myfork \<url>**  

You then need to push your new work to this repository. It’s easiest to push the topic branch you’re working on to your forked repository, rather than merging that work into your master branch and pushing that.
In any event, you can push your work with:  
**$ git push -u myfork featureA**  

Once your work has been pushed to your fork of the repository, you need to notify the maintainers of the original project that you have work you’d like them to merge. This is often called a **pull request**.  
The **git request-pull** command takes the base branch into which you want your topic branch pulled and the Git repository URL you want them to pull from, and produces a summary of all the changes you’re asking to be pulled.  
**$ git request-pull origin/master myfork**  
This output can be sent to the maintainer — it tells them where the work was branched from, summarizes the commits, and identifies from where the new work is to be pulled.  

If you want to submit a second topic of work to the project, don’t continue working on the topic branch you just pushed up — start over from the main repository’s master branch:  
> $ git checkout -b featureB origin/master  
... work ...  
$ git commit  
$ git push myfork featureB  
$ git request-pull origin/master myfork  
... email generated request pull to maintainer ...  
$ git fetch origin  

Now, each of your topics is contained within a silo — similar to a patch queue — that you can rewrite, rebase, and modify without the topics interfering or interdepending on each other, like so:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/FPP1.PNG)  
Let’s say the project maintainer has pulled in a bunch of other patches and tried your first branch, but it no longer cleanly merges. In this case, you can try to rebase that branch on top of origin/master, resolve the conflicts for the maintainer, and then resubmit your changes:  
> $ git checkout featureA  
$ git rebase origin/master  
$ git push -f myfork featureA  

This rewrites your history to now look like:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/FPP2.PNG)  
Because you rebased the branch, you have to specify the -f to your push command in order to be able to replace the featureA branch on the server with a commit that isn’t a descendant of it. An alternative would be to push this new work to a different branch on the server (perhaps called **featureAv2**).  

You start a new branch based off the current origin/master branch, squash the featureB changes there, resolve any conflicts, make the implementation change, and then push that as a new branch:  
> $ git checkout -b featureBv2 origin/master  
$ git merge --squash featureB  
... change implementation ...  
$ git commit  
$ git push myfork featureBv2  

At this point, you can notify the maintainer that you’ve made the requested changes, and that they can find those changes in your featureBv2 branch:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/FPP3.PNG)  

### Public Project over Email
The workflow is similar to the previous use case — you create topic branches for each patch series you work on. The difference is how you submit them to the project. Instead of forking the project and pushing to your own writable version, you generate email versions of each commit series and email them to the developer mailing list:  
> $ git checkout -b topicA  
... work ...  
$ git commit  
... work ...  
$ git commit  

You use git format-patch to generate the mbox-formatted files that you can email to the list — it turns each commit into an email message with the first line of the commit message as the subject and the rest of the message plus the patch that the commit introduces as the body.  
> $ git format-patch -M origin/master  
0001-add-limit-to-log-function.patch  
0002-changed-log-output-to-30-from-25.patch  

The format-patch command prints out the names of the patch files it creates. The -M switch tells Git to look for renames. The files end up looking like this:  
> $ cat 0001-add-limit-to-log-function.patch  
From 330090432754092d704da8e76ca5c05c198e71a8 Mon Sep 17 00:00:00 2001  
From: Jessica Smith <jessica@example.com>  
Date: Sun, 6 Apr 2008 10:17:23 -0700  
Subject: [PATCH 1/2] add limit to log function  
Limit log functionality to the first 20   
lib/simplegit.rb | 2 +-  
1 files changed, 1 insertions(+), 1 deletions(-)  
diff --git a/lib/simplegit.rb b/lib/simplegit.rb  
index 76f47bc..f9815f1 100644  
--- a/lib/simplegit.rb  
\+++ b/lib/simplegit.rb  
@@ -14,7 +14,7 @@ class SimpleGit  
end  
def log(treeish = 'master')  
\- command("git log #{treeish}")  
\+ command("git log -n 20 #{treeish}")  
end  
def ls_tree(treeish = 'master')   
2.1.0  

To email this to a mailing list, you can either paste the file into your email program or send it via a command-line program.  
Git provides a tool to help you send properly formatted patches via IMAP, which may be easier for you.  
First, you need to set up the imap section in your **~/.gitconfig** file:  
> \[imap]  
folder = "[Gmail]/Drafts"  
host = imaps://imap.gmail.com  
user = user@gmail.com  
pass = YX]8g76G_2^sFbd  
port = 993  
sslverify = false  

When that is set up, you can use git imap-send to place the patch series in the Drafts folder of the specified IMAP server:  
> $ cat \*.patch |git imap-send  
Resolving imap.gmail.com... ok  
Connecting to [74.125.142.109]:993... ok  
Logging in...  
sending 2 messages  
100% (2/2) done  

You can also send the patches through an SMTP server:  
> \[sendemail]  
smtpencryption = tls  
smtpserver = smtp.gmail.com  
smtpuser = user@gmail.com  
smtpserverport = 587  

After this is done, you can use git send-email to send your patches:  
> $ git send-email \*.patch  
0001-added-limit-to-log-function.patch  
0002-changed-log-output-to-30-from-25.patch  
Who should the emails appear to be from? \[Jessica Smith <jessica@example.com>]  
Emails will be sent from: Jessica Smith \<jessica@example.com>  
Who should the emails be sent to? jessica@example.com  
Message-ID to be used as In-Reply-To for the first email? y  

## Maintaining a Project
In addition to knowing how to contribute effectively to a project, you’ll likely need to know how to maintain one. This can consist of accepting and applying patches generated via format-patch and emailed to you, or integrating changes in remote branches for repositories you’ve added as remotes to your project.  
### Working in Topic Branches
When you’re thinking of integrating new work, it’s generally a good idea to try it out in a topic branch — a temporary branch specifically made to try out that new work.
The maintainer of the Git project tends to namespace these branches as well — such as **sc/ruby_client**, where **sc** is short for the person who contributed the work. As you’ll remember, you can create the branch based off your master branch like this:  
**$ git branch sc/ruby_client master**  
Or, if you want to also switch to it immediately, you can use the checkout **-b**option:  
**$ git checkout -b sc/ruby_client master**  

### Applying Patches from Email
If you receive a patch over email that you need to integrate into your project, you need to apply the patch in your topic branch to evaluate it. There are two ways to apply an emailed patch: with **git apply** or with **git am**.  
#### Applying a Patch with apply
If you received the patch from someone who generated it with **git diff**, you can apply it with the **git apply** command.
Assuming you saved the patch at /tmp/patch-ruby-client.patch, you can apply the patch like this:  
**$ git apply /tmp/patch-ruby-client.patch**  
**git apply** is overall much more conservative than patch. It won’t create a commit for you — after running it, you must stage and commit the changes introduced manually.  
You can also use git apply to see if a patch applies cleanly before you try actually applying it — you can run git apply --check with the patch:  
> $ git apply --check 0001-seeing-if-this-helps-the-gem.patch  
error: patch failed: ticgit.gemspec:1  
error: ticgit.gemspec: patch does not apply  

#### Applying a Patch with am
If the contributor is a Git user and was good enough to use the format-patch command to generate their patch, then your job is easier because the patch contains author information and a commit message for you. If you can, encourage your contributors to use format-patch instead of diff to generate patches for you. You should only have to use git apply for legacy patches and things like that.  
To apply a patch generated by format-patch, you use git am. Technically, git am is built to read an mbox file, which is a simple, plain-text format for storing one or more email messages in one text file.   
If someone has emailed you the patch properly using git send-email, and you download that into an mbox format, then you can point git
am to that mbox file, and it will start applying all the patches it sees.  
However, if someone uploaded a patch file generated via git format-patch to a ticketing system or something similar, you can save the file locally and then pass that file saved on your disk to git am to apply it:  
**$ git am 0001-limit-log-function.patch  
Applying: add limit to log function**  
For example, if this patch was applied from the mbox example above, the commit generated would look something like this:  
> $ git log --pretty=fuller -1  
commit 6c5e70b984a60b3cecd395edd5b48a7575bf58e0  
Author: Jessica Smith <jessica@example.com>  
AuthorDate: Sun Apr 6 10:17:23 2008 -0700  
Commit: Scott Chacon <schacon@gmail.com>  
CommitDate: Thu Apr 9 09:19:06 2009 -0700  
add limit to log function  
Limit log functionality to the first 20  

The Commit information indicates the person who applied the patch and the time it was applied. The Author information is the individual who originally created the patch and when it was originally created.  
But it’s possible that the patch won’t apply cleanly, in that case, the git am process will fail and ask you what you want to do:  
> $ git am 0001-seeing-if-this-helps-the-gem.patch  
Applying: seeing if this helps the gem  
error: patch failed: ticgit.gemspec:1  
error: ticgit.gemspec: patch does not apply  
Patch failed at 0001.  
When you have resolved this problem run "git am --resolved".  
If you would prefer to skip this patch, instead run "git am --skip".  
To restore the original branch and stop patching run "git am --abort".  

You solve this issue much the same way — edit the file to **resolve the conflict, stage the new file, and then run git am --resolved** to continue to the next patch:  
**$ (fix the file)  
$ git add ticgit.gemspec  
$ git am --resolved  
Applying: seeing if this helps the gem**  

If you want Git to try a bit more intelligently to resolve the conflict, you can pass a **-3** option to it, which makes Git attempt a three-way merge:  
> $ git am -3 0001-seeing-if-this-helps-the-gem.patch  
Applying: seeing if this helps the gem  
error: patch failed: ticgit.gemspec:1  
error: ticgit.gemspec: patch does not apply  
Using index info to reconstruct a base tree...  
Falling back to patching base and 3-way merge...  
No changes -- Patch already applied.  

In this case, without the -3 option the patch would have been considered as a conflict. Since the -3 option was used the patch applied cleanly.  
If you’re applying a number of patches from an mbox, you can also run the am command in interactive mode, which stops at each patch it finds and asks if you want to apply it:  
**$ git am -3 -i mbox**  

### Checking Out Remote Branches
If your contribution came from a Git user who set up their own repository, pushed a number of changes into it, and then sent you the URL to the repository and the name of the remote branch the changes are in, you can add them as a remote and do merges locally.  

For instance, if Jessica sends you an email saying that she has a great new feature in the ruby-client branch of her repository, you can test it by adding the remote and checking out that branch locally:  
**$ git remote add jessica git://github.com/jessica/myproject.git  
$ git fetch jessica  
$ git checkout -b rubyclient jessica/ruby-client**  
If she emails you again later with another branch containing another great feature, you could directly fetch and checkout because you already have the remote setup.  

If you aren’t working with a person consistently but still want to pull from them in this way, you can provide the URL of the remote repository to the git pull command. This does a one-time pull and doesn’t save the URL as a remote reference:  
**$ git pull https://github.com/onetimeguy/project**  
> From https://github.com/onetimeguy/project  
\* branch HEAD -> FETCH_HEAD  
Merge made by the 'recursive' strategy.  

### Determining What Is Introduced
You can exclude commits in the master branch by adding the **--not** option before the branch name.  
For example, if your contributor sends you two patches and you create a branch called contrib and applied those patches there, you can run this:  
**$ git log contrib --not master**  
> commit 5b6235bd297351589efc4d73316f0a68d484f118  
Author: Scott Chacon <schacon@gmail.com>  
Date: Fri Oct 24 09:53:59 2008 -0700  

> seeing if this helps the gem  

> commit 7482e0d16d04bea79d0dba8988cc78df655f16a0  
Author: Scott Chacon <schacon@gmail.com>  
Date: Mon Oct 22 19:38:36 2008 -0700  

> updated the gemspec to hopefully work better

To see a full diff of what would happen if you were to merge this topic branch with another branch, you may think to run this:  
**$ git diff master**  
If master is a direct ancestor of your topic branch, this isn’t a problem; but if the two histories have diverged, the diff will look like you’re adding all the new stuff in your topic branch and removing everything unique to the master branch.  
What you really want to see are the changes added to the topic branch — the work you’ll introduce if you merge this branch with master. You can do that by explicitly figuring out the common ancestor and then running your diff on it:  
**$ git merge-base contrib master  
36c7dba2c95e6bbb78dfa822519ecfec6e1ca649  
$ git diff 36c7db**  
or, more concisely:  
**$ git diff $(git merge-base contrib master)**  
Git provides another shorthand for doing the same thing: the triple-dot syntax:  
**$ git diff master...contrib**  
This command shows you only the work your current topic branch has introduced since its common ancestor with master.  

### Integrating Contributed Work
When all the work in your topic branch is ready to be integrated into a more mainline branch, the question is how to do it.  
You have a number of choices, so we’ll cover a few of them.  
#### Merging Workflows
One basic workflow is to simply merge all that work directly into your master branch.  
For instance, if we have a repository with work in two branches named ruby_client and php_client that looks like History with several topic branches., and we merge ruby_client followed by php_client, your history will end up looking like After a topic branch merge:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/IntegratingCW1.PNG)  
If you have a more important project, you might want to use a two-phase merge cycle. In this scenario, you have two long-running branches, master and develop, in which you determine that master is updated only when a very stable release is cut and all new code is integrated into the develop branch.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/IntegratingCW2.PNG)  
#### Large-Merging Workflows  
The Git project has four long-running branches: master, next, and pu (proposed updates) for new work, and maint for maintenance backports.  



