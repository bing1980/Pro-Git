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