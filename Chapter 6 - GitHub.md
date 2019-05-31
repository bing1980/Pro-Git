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
