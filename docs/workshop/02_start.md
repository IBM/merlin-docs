
## Preparation: Developer Authentication (SSH)

Result: Source files migrated to git

![](02/repo_a.png)

![](02/repo_b.png)

## Connection to Merlin & Platform overview 

<!-- panels:start -->

<!-- div:left-panel -->

* URL will be provided to you.
* `teamX` / `Passw0rd!` (X from 1 to 12)

<!-- div:right-panel -->

![](02/connect.png)

<!-- panels:end -->

---

<!-- panels:start -->

<!-- div:left-panel -->

### Landing page

Accessible services granted by Merlin administrators.

Here, teamX users (group "techxchange") have access to projects "cicd" and "merlin-tools". "cicd" and "merlin-tools" have provisioned services: IBM i CI/CD and and IBM i Developer, respectively.

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

**Starting Point**: Merlin admin already created an Inventory entry with your IBM i and authorized you to use it. System name with arcad-example application: MONZA, IP Address is *will be provided to you*.

<!-- div:right-panel -->

![](02/starting_point.png)

<!-- panels:end -->

---

<!-- panels:start -->

<!-- div:left-panel -->

Go to Credentials > Add new credential > fill in with your IBM i (MONZA) user profile

* Name: `teamX-profile-monza`
*	Username:  `teamX` (X from 1 to 12)
* Password:  `abc123`

<!-- div:right-panel -->

![](02/add_cred.png)

<!-- panels:end -->

---

<!-- panels:start -->

<!-- div:left-panel -->

### Creation of your template 

Go to Templates > add new template

* Name: `teamX-template-monza`
* Inventory: ibmi
* Credential: your credential created before

<!-- div:right-panel -->

![](02/template.png)

<!-- panels:end -->

> [!TIP]
> This Merlin template will be used by IBM i Developer so you can interact with your IBM i and services (compiler, debugger, arcad tools, etc.)

---

<!-- panels:start -->

<!-- div:left-panel -->

### Launch IDE

Go to Tools > Deployed Tools

* Project: merlin-tools

Right click on IBM i Developer, Launch the Application (you may have to Open the workspace if it doesn’t automatically)

<!-- div:right-panel -->

![](02/launch.png)

<!-- panels:end -->