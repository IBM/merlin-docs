# IBM i Projects

IBM i projects are designed to self-describe how they will build themselves as much as possible. These projects leverage the use of a project level JSON file (`iproj.json`) to store project metadata information such as a description, Git repository, version, license, and several IBM i related attributes (target object library, library list, initial CL commands, and include directories). Optional directory level JSON files (`.ibmi.json`) can be used to override the target library and CCSID for that directory. For more information on the project metadata definition of IBM i projects, see the [Overview](https://ibm.github.io/vscode-ibmi-projectexplorer/#/pages/ibm-i-projects/overview) page about IBM i projects.

The **IBM i Project Explorer** view is what you will use to manage a project's library list, variables, object libraries, include paths, and much more. The **Job Log** view will also be used to easily visualize the contents of your job logs after having run a build or compile.

## Getting Started

### New Project

To get started on a new IBM i project, create a folder and open it in the workspace. For a workspace folder to be treated as an IBM i project, it must contain an `iproj.json` file. This can be done from the **Project Explorer** view using the **Create iproj.json** action. This will prompt you to enter a description for the project and then create the file. Upon creating this file, you can now get started with development. To get started with working on source code locally in your project from source physical files in QSYS, check out the documentation on how to [migrate source](https://ibm.github.io/vscode-ibmi-projectexplorer/#/pages/projectExplorer/migrate-source).

![Create iproj.json](../../images/guides/projectExplorerNew.png ':size=500')

In the scenario you ever find that your project's iproj.json file is corrupt, hover on the project in the Project Explorer view to see if there are any errors present. If there are errors, use the Open iproj.json action and refer to the Problems view to see how you can resolve them.

![Resolve iproj.json errors](../../images/guides/projectExplorerErrors.png ':size=500')

### Existing Project in Git

To get started with working on an existing IBM i project that lives in Git, use the **Git Clone** command in the command palette to load the source from a Git repository. For more information, see [Git Integration](./guides/ide/gitintegration.md). 

![Git Clone](../../images/guides/ideGitClone.png ':size=800')

## IBM i Connections

To connect to a remote IBM i, expand any project and select the **Open Connection Browser** inline action. You can also manually navigate to the **Connections** view which is in the **Explorer** view container by default. From here you can connect to a connection which you have defined as a template in Merlin. For more information on how to define a template in Merlin, see the [Manage Templates](./guides/platform/ManageTemplates.md) page.

![Open Connection Browser](../../images/guides/projectExplorerConnect.png ':size=500')

Once you have connected, you can see the active connection in the status bar. You can also hover over the template name in order to access common features such as **Settings**, **Actions**, and **Terminal**. To disconnect, click the **Disconnect from system** button located next to the template name in the status bar.

![Disconnect](../../images/guides/projectExplorerDisconnect.png ':size=850')

## Source

The **Source** heading is what will be used to work with the source files that exist locally in your workspaces. From here you will be able to visualize, compare, and deploy your local source files to your deploy location in the IFS which is where you will be running your builds and compiles out of.

### Set Deploy Location

To set the project's deploy location, use the **Set Deploy Location** action on the **Source** heading. You can modify this location later on as well using the **Edit Deploy Location** action. The suggested deploy location is `/home/<user>/builds/<project_name>`.

![Set Deploy Location](../../images/guides/projectSetDeployLocation.png ':size=850')

If you need to browse to the exact deploy location which you would like to set, this can be done from the **IFS Browser** by creating an IFS shortcut and using the right-click **Set Deploy Workspace Location** action from any directory.

![Set Deploy Workspace Location](../../images/guides/projectSetDeployWorkspaceLocation.png ':size=550')

### Set the Deployment Method

Once the deploy location is set, you can use the **Set Deployment Method** action to set the deployment method to be used when deploying the project.

![Set Deployment Method](../../images/guides/projectExplorerSetDeploymentMethod.png ':size=900')

There are five options for deployment:

1. `Compare`: This method will perform a MD5 hash comparison and upload those file which are different. Any files that are in the deploy location that are not in the local workspace will be deleted.  Consequently, this is the only method that guarantees the deploy location and project will have identical contents after deployment.
2. `Changes`: This method will upload all files which have been changed since the last upload.  Note that if the workspace is closed and reopened, this method will lose track of files changed in the previous session.
3. `Working Changes`: This method only works if the project is associated with a Git repository as it will upload files that have been been changed since the last commit and are not yet staged.
4. `Staged Changes`: The same as the `Working Changes` method, but only uploads staged and indexed files.
5. `All`: This method will upload all files in the local project.

Note that in all methods files that are listed in the `.gitignore` file will not be deployed.

In addition to affecting the deployment process, the deployment method for the project will also impact the content rendered under the **Source** heading. The source files and directories visualized here are a direct reflection of what will be deployed based on the chosen deployment method.

![Source Files and Directories](../../images/guides/projectExplorerSourceFilesAndDirectories.png ':size=600')

For files that have been deployed, you have the ability to compare the local copy versus the remote using the **Compare with Remote** action. 

![Compare with Remote](../../images/guides/projectExplorerCompareWithRemote.png ':size=600')

### Deploy the Project

To start the deployment process, you can use the **Deploy Project** action on the **Source** heading. This will upload the project's local files to the deploy location using the current deployment method. To view the output of this deployment process, navigate to the **Output** view and select the **IBM i Deployment** channel.  Note that when invoking the [build or compile](./guides/ide/developerbuild.md), the source is automatically deployed before running the compile commands so no explicit deploy action is required.

![Deploy Project](../../images/guides/projectExplorerDeployProject.png ':size=650')

## IBM i

The **IBM i** heading is where you will be leveraging the IBM i connection defined on a project. Once connected, you will be able to work with your desired libraries, objects, members, and IFS files.

### Variables

The project metadata for IBM i projects support the use of variables in the following fields: `objlib`, `curlib`, `preUsrlibl`, `postUsrlibl`, `setIBMiEnvCmd`, `buildCommand`, `compileCommand`, and `includePath`. These variables are always prefaced with an `&`. By levering the use of variables, the same project definition can be used to target a different build library from one developer to another. The **Variables** heading is where you will be able to visualize these variables.

#### Create Environment File

To get started with viewing and defining project variables, there must exists a root level `.env` file which will be used for storing the value of these variables. This can be done by using the **Create .env** action. To avoid accidentally pushing your `.env` file to your Git repository, make sure that you add it as an entry into your `.gitignore` file.

![Create .env](../../images/guides/projectExplorerCreateEnv.png ':size=650')

#### Edit Variable

Listed under the **Variables** heading are all variables used in the root level `iproj.json` or in any `.ibmi.json` within the project. When there are variables which do not have a value assigned, the heading itself will have a red decoration to indicate the number of unresolved variables. To assign a value to a variable, use the **Edit Variable** action. This will be stored in the project's `.env` file.

![Edit Variable](../../images/guides/projectExplorerEditVariable.png ':size=900')

Instead of manually inputting the value of a variable, you also have the ability to assign a library names or directory to a variable using the **Assign to Variable** action. This can be done from a library in the **Project Explorer** view or the **Object Browser** as well as directories in the **IFS Browser**. For libraries or include paths which are hardcoded in the project's `iproj.json` file, they can be converted to variables using the **Configure as Variable** action. This will substitute the hardcoded value for a variable which you will provide and set the value of this variable to be the hardcoded value. For more information on variables, see the [Work with Variables](https://ibm.github.io/vscode-ibmi-projectexplorer/#/pages/projectExplorer/work-with-variables) page.

### Library List

The **Library List** heading is where you will be able to view your project's library list. The library list is initially set according to your user profile, but can be modified based on the fields set in the project's `iproj.json` file.

Refer to the following guide to distinguish the types of libraries in the library list based on color:

- System libraries: Blue
- Current library: Green
- User libraries: Yellow

#### Set the Current Library

To set the project's current library, use the **Set Current Library** action. If the `curlib` field in the project's `iproj.json` file is either a hardcoded library or not specified, this action will set it to be the `&CURLIB` variable with the value set in the `.env` file as the library's name. However, if the `curlib` field is already set to a variable, the variable will be kept and the value will be simply updated.

![Set Current Library](../../images/guides/projectExplorerSetCurrentLibrary.png ':size=550')

In the case you would first like to browse for the library to set as the current library, you can leverage the **Object Browser** view. Here you can query for the library and use the **Set as Current Library** action.

#### Add to the Library List

Similar to setting the current library, you can add to the set of user libraries by using the **Add Library List Entry** action. After inputting the library name, you will also be prompted to select where to position the library. Adding to the beginning of the library list will add to the `preUsrlibl` field in the `iproj.json` whereas adding to the end of the library list will add to the `postUsrlibl` field.

![Add Library List Entry](../../images/guides/projectExplorerAddLibraryListEntry.png ':size=850')

You also have the freedom to query for libraries in the **Object Browser** view and then use the **Add to Library List** action for when you would like to first browse for libraries before adding them.

Libraries defined in the `preUsrlibl` or `postUsrlibl` field in the project's `iproj.json` file can be reordered directly in the **Project Explorer** view. Note that you will only be able to reorder the libraries within each group itself. This can be done using **Move Up** or **Move Down** actions accordingly. In addition, libraries defined in the project's `iproj.json` are the only libraries which can be removed from the library list. This can be done using the **Remove from Library List** action. Note that removing any library which corresponds to a variable will only remove the value from the `.env` file and not the variable itself from the `iproj.json` file. For more information on the library list, see the [Manage the Library List](https://ibm.github.io/vscode-ibmi-projectexplorer/#/pages/projectExplorer/manage-the-library-list) page.

### Object Libraries

The **Object Libraries** heading is the location to browse for libraries defined in the `curlib`, `objlib`, `preUsrlibl`, and `postUsrlibl` of the project's `iproj.json`.
Typically these are the libaries of interest while doing application development as it includes any libraries that the target objects are built into. For more information on object libraries, see the [Browse Object Libraries](https://ibm.github.io/vscode-ibmi-projectexplorer/#/pages/projectExplorer/browse-object-libraries) page.

![Object Libraries](../../images/guides/projectExplorerObjectLibraries.png ':size=900')

### Include Paths

The `includePath` field in the project's `iproj.json` file specifies directories to be searched for includes or copy files. These set of directories can be managed using the **Include Paths** heading. Include paths which can be resolved locally are not expandable, but can be clicked on to be taken to the directory in the **File Explorer**. However, include paths which are resolved to remote locations in the IFS can be expanded.

#### Add Include Paths

Adding to the set of include paths is as simple as using the **Add to Include Paths** action to be prompted for the path to add. Note that any absolute path will be resolved based on the local path to the workspace or the project's deploy location.

![Add to Include Paths](../../images/guides/projectExplorerAddToIncludePaths.png ':size=850')

To add an include path to a directory in the local project, leverage the **Add to Include Paths** action on any directory in VS Code **File Explorer**.

![Add to Include Paths from the File Explorer](../../images/guides/projectExplorerIncludePathsLocal.png ':size=400')

Similarly, to add an include path to a directory in the IFS, leverage the same action in the **IFS Browser**.

![Add to Include Paths from the IFS Browser](../../images/guides/projectExplorerIncludePathsIFS.png ':size=450')

To reorder include paths, you can use the **Move Up** and **Move Down** actions in the **Project Explorer**. To remove an include path, you can use the **Remove from Include Paths** action. Note that removing any include path which corresponds to a variable will only remove the value from the `.env` file and not the variable itself from the `iproj.json` file. For more information on include paths, see the [Update Include Paths](https://ibm.github.io/vscode-ibmi-projectexplorer/#/pages/projectExplorer/update-include-paths) page.

## PASE and 5250 Terminals

You can launch a `PASE` or `5250` terminal right from your workspace. To do this, use the **IBM i: Launch Terminal Picker** command or hover over the template name in the status bar and click on **Terminals**.

![Terminal Actions](../../images/guides/projectExplorer5250.png ':size=850')

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin) 