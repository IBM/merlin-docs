
<!--## Preparation: Developer Authentication (SSH)

Result: Source files migrated to git

![](02/repo_a.png)

![](02/repo_b.png)
-->
## Connection to Merlin & Platform overview 

<!-- panels:start -->

<!-- div:left-panel -->

* URL to Merlin landing page will be provided to you.
* Merlin userid and password will be provided to you.

<!-- div:right-panel -->

![](02/connect.png)

<!-- panels:end -->

---

<!-- panels:start -->

<!-- div:left-panel -->

### Landing page

Here, users have access to projects "cicd" and "ide" which have provisioned services: IBM i CI/CD and IBM i Developer (as configured by Merlin admin).

Isolation can be done by projects, with different authorizations & roles on resources.

<!-- div:right-panel -->

![](02/landing.png)

<!-- panels:end -->

---

<!-- panels:start -->

<!-- div:left-panel -->

### Platform overview

**Inventory**: 

- IBM i managed by Merlin (ex: Dev/Test/Build IBM i, production)

- PowerVC/PowerVS Merlin can provision on private or public clouds

- Jenkins Merlin can trigger CI/CD pipelines

**Provisioning**:

Merlin can provision IBM i VM for a project (dev environment etc). PowerVC / PowerVS


<!-- div:right-panel -->

![](02/overview.png)

<!-- panels:end -->

---

<!-- panels:start -->

<!-- div:left-panel -->

**Starting Point**: Under **Connections**, Merlin admin already created an Inventory entry with your IBM i hostname and authorized you to use it. 

<!-- div:right-panel -->

![](02/starting_point.png)

<!-- panels:end -->

---

<!-- panels:start -->

<!-- div:left-panel -->

Merlin admin already created a Credential entry with your IBM i userid and password.

<!-- div:right-panel -->

![](02/add_cred.png)

<!-- panels:end -->

---

<!-- panels:start -->

<!-- div:left-panel -->

Merlin admin already created a template which associates the defined inventory and credential together.  This Merlin template will be used by IBM i Developer so you can interact with your IBM i and services (compiler, debugger, arcad tools, etc.).

<!-- div:right-panel -->

![](02/template.png)

<!-- panels:end -->

---

<!-- panels:start -->

<!-- div:left-panel -->

<!--
### Launch IDE

Go to Tools > Deployed Tools

* Project: merlin-tools

Right click on IBM i Developer, Launch the Application (you may have to Open the workspace if it doesn’t automatically)
-->

<!-- div:right-panel -->

<!--
![](02/launch.png)
-->
<!-- panels:end -->