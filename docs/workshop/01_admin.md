
## One-Time (Administrator) Preparation

These set-up steps have already been run for you.  They are documented here for your awareness and education and will be useful when you want to set up Merlin in your own environment.

After your Infrastructure has been installed/configured, but before you can actually start using Merlin:

1. Configure git for Source Configuration Management  
2. (If needed) migrate source from physical files -> IFS -> git repo
3. IBM i Integration in Merlin
4. Create Application definition - source and object libraries

### Configuring of Source Control Management
Modern application development presupposes good source control that can enable best practices.
Git is the most popular source code which can be used to drived your CI/CD pipeline suing web-hooks that can trigger builds when certain events happen in git. There are many different web interfaces to git that help automate the development process.  These include Github, Gitlab and Gitbucket.  If this is hosted outside of Merlin than the git hook needs some firewall configuration to access the builders running in Merlin.  The sandobx environment is configured with Gitbucket running on the IBM i.  This means that no new server is required and everything runs within the same secure network.  The source for the demo the repository already contains the sample application source and the git hook is configured.  Arcad products clone source from the git repository when doing builds on the IBM i and the user profile launching this needs to have the ssh key set up to access the git repository.  This also taken care of the sandbox environmetn.

### IBM i integration in Merlin

<!-- panels:start -->

<!-- div:left-panel -->

6 actions on IBM i performed sequentially from Merlin by the Administrator: 

1. Enable ansible (_this is the automation framework used for the subsequent steps_)
2. Validate PTF level (_ensures that the IBM i is a the prequisite software level_)
3. Install certificates (_are required to have encrypted HTTPS communications_)
4. Enable IBM i Developer (_configures IBM i backend for Merlin_)
5. Enable remote debugger (_configures IBM i backend for the debugger_)
6. Enable Arcad (_install Arcad solutions on the target IBM i_)

<!-- div:right-panel -->

![](01/Picture1.png)

<!-- panels:end -->

---

### Creation of an Arcad Application

<!-- panels:start -->

<!-- div:left-panel -->

The Arcad application has already been created for you.  
When doing this for your own application this can easily be done from with the IDE as an action on the IBM i project or alternatively it can be done through Arcad green screen commands. 
<!-- div:right-panel -->

![](01/from_merlin.png)

<!-- panels:end -->