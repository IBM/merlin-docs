
## Review Administrator Preparation

These set-up steps have already been run for you.  They are documented here for your awareness and education and will be useful when you want to set up Merlin in your own environment.

1. Initialize vault.  Store the secrets and token.
2. Create `ideproj` and `cicdproj` projects in Merlin
3. Install IBM i Developer tool into `ideproj` project.  Accept license. Specify `openVSXURL` of https://open-vsx.org
4. Install CI/CD tool into `cicdproj` project.  Accept license.
5. Create connection information (Inventory/Credential/Template) for IBM i with admin id
6. Run actions on template to configure IBM i 
   1. Enable ansible environment
   2. Validate dependent PTFs
   3. Add certificate
   4. Enable IBM i developer environment
   5. Enable debug service
   6. Enable Arcad environment
7. Configure CICD tool
   1. Start CICD tool
   2. Initialize jenkins server (use internal jenkins server)
   3. Add jenkins credential for Arcad
   4. Enable Arcad integration for jenkins
8. Create IBM i user ids with IFS home directory specified, bash as default shell, and `/QOpenSys/pkgs/bin` in `PATH`.  Grant authority to commands for debugging.
9. Create Merlin users
10. Create group and add users as members
11. In Authorization, add permissions for group to `ideproj` and `cicdproj` projects with `VIEW`
12. In CICD tool's jenkins configuration>manage permissions, add each user
13. For each Merlin user, log into Merlin and create connection information (Inventory/Credential/Template)
14. Setup git on IBM i
    1. Configure git repository for Source Configuration Management  
    2. Load ARCAD-EXAMPLE source into git repository (in general, if necessary migrate source from physical files -> IFS -> git repository)
    3. Generate SSH key pair for use from Merlin IDE, git repository, and IBM i userid
    4. Configure webhook for Arcad builder
15. Verify servers
    1. gitbucket http://<ibmi_ipaddress>:7450/ 
    2. Arcad builder http://<ibmi_ipaddress>:5252/about/ 
    3. Arcad builder web console https://<ibmi_ipaddress>:2012/builder/
    4. Elias https://<ibmi_ipaddress>:2012/elias/arcad/v1/ping/
16. Configure ARCAD-EXAMPLE project for Arcad in Merlin

### Configuring of Source Control Management
Modern application development presupposes good source control that can enable best practices.
Git is the most popular source code which can be used to drive your CI/CD pipeline using web-hooks that can trigger builds when certain events happen in git. There are many different web interfaces to git that help automate the development process.  These include Github, Gitlab and Gitbucket.  If this is hosted outside of Merlin than the web-hook needs some firewall configuration to access the builders running in Merlin.  The sandbox environment is configured with Gitbucket running on the IBM i.  This means that no new server is required and everything runs within the same secure network.  The source for the demo the repository already contains the sample application source and the web-hook is configured.  Arcad products clone source from the git repository when doing builds on the IBM i and the user profile launching this needs to have the ssh key set up to access the git repository.  This also taken care of the sandbox environment.

### IBM i configuration for Merlin

<!-- panels:start -->

<!-- div:left-panel -->

6 actions on IBM i performed sequentially from Merlin by the Administrator: 

1. Enable Ansible environment (_this is the automation framework used for the subsequent steps_)
2. Validate the dependent PTFs (_ensures that the IBM i is at the prerequisite software level_)
3. Install certificates (_required to have encrypted HTTPS communications_)
4. Enable IBM i Developer (_configures IBM i backend for Merlin_)
5. Enable IBM i Debug Service (_configures IBM i backend for the debugger_)
6. Enable ARCAD environment (_install Arcad solutions on the target IBM i_)

<!-- div:right-panel -->

![](01/Picture1.png)

<!-- panels:end -->

---

<!--### Creation of an Arcad Application
-->
<!-- panels:start -->

<!-- div:left-panel -->

<!--The Arcad application has already been created for you.  
When doing this for your own application this can easily be done from with the IDE as an action on the IBM i project or alternatively it can be done through Arcad green screen commands. 
-->
<!-- div:right-panel -->

<!--![](01/from_merlin.png)
-->
<!-- panels:end -->