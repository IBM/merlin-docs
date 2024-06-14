# Overview
  
## Dashboard
The IBM i Developer Dashboard allows the user to create, start, manage, and delete workspaces. Here you should see the workspaces available. The sidebar shows frequently used shortcuts and can be toggled with the top left menu icon.

![Dashboard](../../images/guides/ideDashboard.png ':size=800')

To create a new `IBM i Developer Workspace`, click on the **IBM i Developer** or **IBM i Developer and demo application** tool stack from the **Create Workspace** page. Other tool stacks are also available. If you would like to create a new `IBM i Developer Workspace` workspace with a Git repository already imported, use the `Import from Git` feature by pasting your Git repo URL and clicking the **Create and Open** button.

**Note:** If you would like to use an SSH URL when importing the Git repository, you must first create an SSH key in the [Credentials](./guides/platform/ManageCredential.md) page in Merlin.

![Create Workspace](../../images/guides/ideDashAddWorkspace.png ':size=800')

To start an existing workspace, go to **Workspaces** and click on the workspace you want to open.
* You may also start a workspace in **Recent Workspaces** in the sidebar.
* Note: You can only have **ONE** workspace running at a time.

![Open Workspace](../../images/guides/ideDashStartWorkspace.png ':size=800')

## Workspace
Once a Workspace is started, you should see the IBM i Developer Integrated Development Environment (IDE) displayed on your browser. This is where the user can develop, build, and modernize their applications. 

![Workspace](../../images/guides/ideWorkspace.png ':size=850')

If this is your first time accessing this workspace, you should refer to the walkthroughs from the welcome page which go through key aspects of VS Code and several extensions (**IBM i Developer**, **Code for IBM i**, and **IBM i Project Explorer**).
* You may access all available walkthroughs by clicking **More...** under the listed walkthroughs on the welcome page or by using the **Welcome: Open Walkthrough...** command in the command palette.
* If you close the welcome page and would like to view it again, it can be brought back using the **Help: Welcome** command in the command palette.

![Walkthroughs](../../images/guides/ideWalkthroughs.png ':size=500')

The VS Code user interface is divided into five main areas:
* **Editor** - The main area to edit your files. You can open as many editors as you like and can arrange them vertically and horizontally.
* **Primary Side Bar** - Holds the different views contained in the selected view container from the **Activity Bar**.
* **Activity Bar** - Holds different view containers such as the **Explorer**, **Search**, **Source Control**, **Run and Debug**, and **Extensions** views.
* **Panel** - An additional space to hold views below the editor. This is where you can find the **Problems**, **Output**, **Debug Console**, **Terminal**, **Job Log** views.
* **Status Bar** - Contains status information about the current workspace and file(s) you have open.

### Editor

The editor is in the center of the workspace. For information on the editing features, go to the [Edit](./guides/ide/edit.md) page.

![Editor](../../images/guides/ideWorkspaceEditor.png ':size=800')

### Explorer

The **Explorer** is the first view container in the activity bar which contains several built-in and IBM i related views which you will be using most often. For information on the IBM i related views, go to the [IBM i Projects](./guides/ide/ibmiprojectexplorer.md) page.

* **File Explorer** - Files and folders in your current workspace.
* **Outline** - Symbol tree of the currently active editor.
* **Timeline** - Time-series events such as Git commits of the currently active editor.
* **Project Explorer** - Work with IBM i projects in your current workspace.
* **Connections** - Connect to an IBM i connection.

Note: Right click on the **Explorer** heading to toggle views.

![Explorer](../../images/guides/ideWorkspaceExplorer.png ':size=300')

### Source Control

The IDE has integrated source control management (SCM) and includes built-in **Git** support. For information on Git features, go to the [Git Integration](./guides/ide/gitintegration.md) page.

![Source Control](../../images/guides/ideSourceControl.png ':size=900')

### Debugging

You may debug RPG (including RPGLE and OPM RPG), COBOL, C/C++, and CL programs using the debug views in VS Code. The **IBM i Debug** extension supports batch debugging and service entry points. For information on debug features, go to the [Debugging](./guides/ide/debug.md) page.

![Debugging](../../images/guides/debugging.png ':size=800')

### Extensions

When a new workspace is created, several IBM i related extensions are installed to help you with development such as the **IBM i Developer**, **Code for IBM i**, **IBM i Project Explorer**, and much more. For information on all installed extensions, go to the [Extensions](./guides/ide/extensions.md) page.

![Extensions View](../../images/guides/extensionsView.png ':size=500')

### Panel

The panel is used to hold additional views below the editor. You may re-arrange views between different view containers by dragging the view's heading.

* **Problems** - View informational, warning, and error diagnostics.
* **Output** - View output channels for logging of built in VS Code features and various extension.
* **Debug Console** - Shows debugging output.
* **Terminal** - Integrated terminal that starts at the root of the workspace.
* **Job Log** - Visuzlize IBM i job logs after running a build or compile.

![Panel](../../images/guides/ideWorkspacePanel.png ':size=900')

Note: To create a new terminal, select **Terminal > New Terminal**

![Terminal](../../images/guides/ideWorkspaceTerminal.png ':size=500')

### Command Palette

To run a command, navigate to the command palette at the top of the page and select the **Show and Run Commnads** option. Selecting this option will prefix the search with an `>` which will then show all commands which can be run. This can also be brought up using **View > Command Palette...**.

![Commands](../../images/guides/ideWorkspaceCommand.png ':size=700')

### Settings

Settings can be used to manage extension settings, keyboard shortcuts, themes (colors, file icons, and product icosn), and much more. To change any of these settings, click the **Manage** gear icon at the bottom left and select the appriopriate option. For information on settings, go to the [Settings](./guides/ide/settings.md) page.

![Settings](../../images/guides/ideWorkspaceSettings.png ':size=350')

To return to the dashboard, click the white **IBM i Developer Workspaces Web** button in the bottom left status bar and use the **IBM i Developer Workspaces: Open Dashboard** command.

![Return to Dashboard](../../images/guides/ideWorkspaceReturn.png ':size=800')

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin)