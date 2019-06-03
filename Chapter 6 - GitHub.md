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
