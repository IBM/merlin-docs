# Getting Started

**Note:** For additional getting started information and videos, visit the [Merlin Overview](https://www.ibm.com/support/pages/node/6574839) support page.

Steps for the Merlin administrator:
* The adminstrator creates a project and installs the **IBM i Developer** tool into the project.  
* The administrator runs the [actions](./guides/platform/ManageIBMiServer.md#running-actions-on-the-ibm-i-server) on the IBM i from **Templates** 
* Merlin user is created by the adminstrator and the user is given access to the Merlin project with the **IBM i Developer** tool

Steps for the user:
* Define Credentials in platform **Connections**
    * Define IBM i hostname in **Inventory**
       * **Note:** verify all prerequisites are installed on the IBM i and that configuration has been completed.
    * Define IBM i user profile and password in **Credentials**
    * Define **Templates** with inventory and credential
* Start IBM i Developer
    * Under **Tools>Deployed Tools**, launch **IBM i Developer** to launch the  Dashboard.  Dashboard is where application developers create, start, stop, and manage their development workspaces all from a web browser.
* Start workspace
    * From **Create Workspace** page:
        * Select **IBM i Developer** tools stack to create a clean workspace with all the IBM i developer tools 
        * Select **IBM i Developer and demo application** tools stack to create a workspace populated with an IBM i application loaded from git with all the IBM i developer tools.


**Note:** IBM i Developer is built on top of Red Hat OpenShift Dev Spaces. For more information on what else can be done with workspaces, see the [Red Hat documentation](https://access.redhat.com/documentation/en-us/red_hat_openshift_dev_spaces/3.12/html/user_guide/getting-started-with-devspaces).

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin) 