# GitHub
GitHub is the single largest host for Git repositories, and is the central point of collaboration for millions of developers and projects. A large percentage of all Git repositories are hosted on GitHub, and many open-source projects use it for Git hosting, issue tracking, code review, and other things. So while it’s not a direct part of the Git open source project, there’s a good chance that you’ll want or need to interact with GitHub at some point while using Git professionally.
## Account Setup and Configuration
### SSH Access
With SSH keys, you can connect to GitHub **without supplying your username or password** at each visit.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/ssh.PNG)  
From there, click the "Add an SSH key" button, give your key a name, paste the contents of your **~/.ssh/id_rsa.pub** (or whatever you named it) public-key file into the text area, and click “Add key”.  
### Your Email Address
The way that GitHub maps your Git commits to your user is by email address. If you use multiple email addresses in your commits and you want GitHub to link them up properly, you need to add all the email addresses you have used to the Emails section of the admin section.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/email_address.PNG)  
The top address is verified and set as the primary address, meaning that is where you’ll get any notifications and receipts. The second address is verified and so can be set as the primary if you wish to switch them. The final address is unverified, meaning that you can’t make it your primary address. If GitHub sees any of these in commit messages in any repository on the site, it will be linked to your user
now.
### Two Factor Authentication
Two-factor Authentication is an authentication mechanism that is becoming more and more popular recently to mitigate the risk of your account being compromised if your password is stolen somehow.  
You can find the Two-factor Authentication setup under the Security tab of your Account settings.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/2FA.PNG)  
If you click on the “Set up two-factor authentication” button, it will take you to a configuration page where you can choose to use a phone app to generate your secondary code (a “time based one-time password”), or you can have GitHub send you a code via SMS each time you need to log in.  
## Contributing to a Project
### Forking Projects
If you want to contribute to an existing project to which you don’t have push access, you can **“fork”** the project. When you “fork” a project, GitHub will make a copy of the project that is entirely yours; it lives in your namespace, and you can push to it.  
People can fork a project, push to it, and contribute their changes back to the original repository by creating what’s called a **Pull Request,** and the owner and the contributor can then communicate, about the change until the owner is happy with it, at which point the owner can merge it in.  
### The GitHub Flow
Here’s how it generally works:  
1. Fork the project
2. Create a topic branch from master.
3. Make some commits to improve the project.
4. Push this branch to your GitHub project.
5. Open a Pull Request on GitHub.
6. Discuss, and optionally continue committing.
7. The project owner merges or closes the Pull Request.
8. Sync the updated master back to your fork.  

Instead of using email to communicate and review changes, teams use GitHub’s web based tools.  
### Advanced Pull Requests
#### Pull Requests as Patches
It’s important to understand that many projects don’t really think of Pull Requests as queues of perfect patches that should apply cleanly in order, as most mailing list-based projects think of **patch series contributions**.  
For instance, if you go back and look again at Pull Request final, you’ll notice that the contributor did not rebase his commit and send another Pull Request. Instead they added new commits and pushed them to the existing branch. This way if you go back and look at this Pull Request in the future, you can easily find all of the context of why decisions were made.  
#### Keeping up with Upstream
If your Pull Request becomes out of date or otherwise doesn’t merge cleanly, you will want to fix it so the maintainer can easily merge it.  
If you see something like Pull Request does not merge cleanly, you’ll want to fix your branch so that it turns green and the maintainer doesn’t have to do extra work. You can either rebase your branch on top of whatever the target branch is (normally the master branch of the repository you forked), or you can merge the target branch into your branch.  
Most developers on GitHub will choose to do the **latter**, since what matters is the **history** and the **final merge**, so rebasing isn’t getting you much other than a slightly cleaner history and in return is far more difficult and error prone.  
Here is the basic procedures:  
1. Add the original repository as a remote named “upstream”
2. Fetch the newest work from that remote
3. Merge the main branch of that repository into your topic branch
4. Fix the conflict that occurred
5. Push back up to the same topic branch  

#### References
If you absolutely wish to rebase the branch to clean it up, you can certainly do so, but it is highly encouraged to not force push over the branch that the Pull Request is already opened on. Instead, push the rebased branch to a new branch on GitHub and open a brand new Pull Request referencing the old one, then close the original.  

All Pull Requests and Issues are assigned numbers and they are unique within the project. If you want to reference any Pull Request or Issue from any other one, you can simply put **#\<num>** in any comment or description. You can also be more specific if the Issue or Pull request lives somewhere else; write **username#\<num>**.  

Let’s look at an example. Say we rebased the branch in the previous example, created a new pull request for it, and now we want to reference the old pull request from the new one.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/reference1.PNG)  
When we submit this pull request, we’ll see all of that rendered like **Cross references rendered** in a Pull Request..  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/reference2.PNG)  
Now if Tony goes back and closes out the original Pull Request, we can see that by mentioning it in the new one, GitHub has automatically created a trackback event in the Pull Request timeline. This means that anyone who visits this Pull Request and sees that it is closed can easily link back to the one that superseded it.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/reference3.PNG)  
### GitHub Flavored Markdown
#### Task Lists
You can create a task list like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/tasklist1.PNG)  
We’ll see it rendered like this:    
![image](https://github.com/bing1980/Pro-Git/blob/master/img/tasklist2.PNG)  
#### Code Snippets
To add a snippet of code you have to “fence” it in backticks:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/code1.PNG)  
It would end up rendering like:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/code2.PNG)  
#### Emoji
Emojis take the form of :<name>: anywhere in the comment. For instance, you could write something like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/emoji1.PNG)  
When rendered, it would look something like this:  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/emoji2.PNG)  

### Keep your GitHub public repository up-to-date
Once you’ve forked a GitHub repository, your repository (your "fork") exists **independently** from the original. In particular, when the original repository has new commits, GitHub informs you by a message like:   
> This branch is 5 commits behind progit:master.  

You can update your local repository in two ways:  
For example, if you forked from https://github.com/progit/progit2.git, you can keep your master branch up-to-date like this:  
**$ git checkout master  
$ git pull https://github.com/progit/progit2.git     
$ git push origin master**  

This works, but it is a little tedious having to spell out the fetch URL every time. You can automate this work with a bit of configuration:  
**$ git remote add progit https://github.com/progit/progit2.git  
$ git branch --set-upstream-to=progit/master master  
$ git config --local remote.pushDefault origin**  
Once this done, the workflow becomes much simpler:  
**$ git checkout master  
$ git pull  
$ git push**  

## Maintaining a Project
### Creating a New Repository
All you really have to do here is provide a project name; the rest of the fields are completely optional. For now, just click the “Create Repository” button, and boom – you have a new repository on GitHub, named **\<user>/<project_name>**.  

Every project on GitHub is accessible over HTTPS as https://github.com/<user\>/\<project_name\>, and over SSH as git@github.com:<user>/<project_name>. Git can fetch from and push to both of these URLs, but they are access-controlled based on the credentials of the user connecting to them.  

### Adding Collaborators
If you’re working with other people who you want to give commit access to, you need to add them as “collaborators”.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/collaborators.PNG)  

### Managing Pull Requests
Pull Requests can either come from a branch in a fork of your repository or they can come from another branch in the same repository. The only difference is that the ones in a fork are often from people where you can’t push to their branch and they can’t push to yours, whereas with internal Pull Requests generally both parties can access the branch.  
#### Email Notifications
Someone comes along and makes a change to your code and sends you a Pull Request. You should get an email notifying you about the new Pull Request.  
There are a few things to notice about this email. It will give you a small diffstat — a list of files that have changed in the Pull Request and by how much. It gives you a link to the Pull Request on GitHub. It also gives you a few URLs that you can use from the command line. You could technically merge in the Pull Request work with something like this:   
**$ curl https://github.com/tonychacon/fade/pull/1.patch | git am**  

#### Collaborating on the Pull Request
You can now have a conversation with the person who opened the Pull Request. You can comment on specific lines of code, someone else comments on the Pull Request you will continue to get email notifications, so you know there is activity happening.  
Once the code is in a place you like and want to merge it in, you can either pull the code down and merge it locally, either with the **git pull \<url> \<branch>** syntax we saw earlier, or by adding the fork as a remote and fetching and merging.  
If the merge is trivial, you can also just hit the “Merge” button on the GitHub site. This will do a “non-fast-forward” merge, creating a merge commit even if a fast-forward merge was possible.  
![image](https://github.com/bing1980/Pro-Git/blob/master/img/merge_button.PNG)  
#### Pull Request Refs
If you’re dealing with a lot of Pull Requests and don’t want to add a bunch of remotes or do one time pulls every time, you can run with the command **ls-remote**. We will get a list of all the branches and tags and other references in the repository.  
**$ git ls-remote https://github.com/schacon/blink**  
> 10d539600d86723087810ec636870a504f4fee4d HEAD  
10d539600d86723087810ec636870a504f4fee4d refs/heads/master  
6a83107c62950be9453aac297bb0193fd743cd6e refs/pull/1/head  
afe83c2d1a70674c9505cc1d8b7d380d5e076ed3 refs/pull/1/merge  

Of course, if you’re in your repository and you run **git ls-remote origin** or whatever remote you want to check, it will show you something similar to this.  

There are two references per Pull Request - the one that ends in /head points to exactly the same commit as the last commit in the Pull Request branch. So if someone opens a Pull Request in our repository and their branch is named bug-fix and it points to commit a5a775, then in our repository we will not have a bug-fix branch (since that’s in their fork), but we will have pull/<pr#>/head that points to a5a775. This means that we can pretty easily pull down every Pull Request branch in one go without having to add a bunch of remotes.  
Now, you could do something like fetching the reference directly.  
**$ git fetch origin refs/pull/958/head**  
> From https://github.com/libgit2/libgit2
\* branch refs/pull/958/head -> FETCH_HEAD  

This tells Git, “Connect to the origin remote, and download the ref named refs/pull/958/head.”




