# Developer build

IBM i Developer allows the user to run the project-based developer builds. IBM i Developer **does not** restrict users from using any specific building technology. For example, users may use Better Object Builder (BOB) or ARCAD tooling for the builds. Alternatively, users may even write their scripts and invoke them as long as those build tools respect the [IBM i Projects Standard](https://ibm.github.io/ibmi-bob/#/prepare-the-project/project-metadata).

## Prepare the Workspace and the Project

Before getting started, please make sure you have set up your [IBM i Projects](./guides/ide/ibmiprojectexplorer.md).

- Specify an IBM i Connection
- Set the deploy location
- Set the values for any variables defined in the project

The build or compiles of a project are based on the `buildCommand` and `compileCommand` fields in the project's iproj.json. Similarly, another solution is to use an `actions.json` file to store configurations for different technologies.

## Run a Build

Before running a build, the project's build command must be set. This can be done using the **Set Build Command** action on the project. Here you can leverage any build tool such as `elias` in ARCAD Builder or `makei` in `ibmi-bob`. The build command and compile command (described below) both support the substitution of variables. For more information about variable substitution, see the [buildCommand](https://ibm.github.io/vscode-ibmi-projectexplorer/#/pages/ibm-i-projects/iproj-json?id=buildcommand) section of the `iproj.json` documentation.

![Set Build Command](../../images/guides/devBuildSetBuildCommand.png ':size=500')

To then launch a build, use the **Run Build** action on the project heading or the `ctrl+shift+b` shortcut (`cmd+shift+b` on Mac). This will first deploy the project and then invoke the build command. To view the output of this build process, navigate to the **Output** view and select the **IBM i Output** channel.

![Run Build](../../images/guides/devBuildRunBuild.png ':size=700')

## Run a Compile

Similar to the build command, the project's compile command must be first set using the **Set Compile Compile** action on the project.

![Set Compile Command](../../images/guides/devBuildSetCompileCommand.png ':size=500')

When compiling with Arcad Builder, special comments at the top of the file can customize the compile with compilation attributes or to execute commands.  Default compilation attributes can be set using `AWRKAPP appname + Option 9`.
   - %ATTR instruction encodes the attributes to pass to the compilation command.  e.g. `* %ATTR DEVTYPE(*SCS)`
   - %EXEC pre-compilation command
   - %EXECM pre-compilation command with error message checking
   - %EXECA post-compilation command
   - %EXECAM post-compilation command with error message checking

To compile a directory or file, this can be achieved from several locations:

- To compile the active text editor in which you are working in, using the **Run Compile** action located at the top right of the editor, in the VS Code command palette, or by simply using the `ctrl+shift+c` shortcut (`cmd+shift+c` on Mac)
- To compile an entire directory or a specific file, use the right-click **Run Compile** action on any file or directory in the **File Explorer** or under the **Source** heading

![Run Compile](../../images/guides/devBuildRunCompile.png ':size=750')

## Run an Action

An alternative to using the build or compile command is to leverage the use of Code for IBM i workspace actions. These actions are stored in `.vscode/actions.json` at the root of the project and should be pushed to the Git repository. To generate an initial `actions.json` file, use the **Launch Action Setup** action on the project. Here you will be prompted to select from a set of pre-defined actions.

![Launch Action Setup](../../images/guides/devBuildLaunchActionSetup.png ':size=500')

Refer to the example below of having setup a project to use `ibmi-bob`:

```bash
[
  {
    "extensions": [
      "GLOBAL"
    ],
    "name": "Build all",
    "command": "OPT=*EVENTF BUILDLIB=&CURLIB /QOpenSys/pkgs/bin/makei build",
    "environment": "pase",
    "deployFirst": true,
    "postDownload": [
      ".logs",
      ".evfevent"
    ]
  },
  {
    "extensions": [
      "GLOBAL"
    ],
    "name": "Build current",
    "command": "OPT=*EVENTF BUILDLIB=&CURLIB /QOpenSys/pkgs/bin/makei compile -f {filename}",
    "environment": "pase",
    "deployFirst": true,
    "postDownload": [
      ".logs",
      ".evfevent"
    ]
  }
]
```

After having setup the actions, you can invoke them using the **Run Action** action on the project. This can also be done from the top right of the active text editor or by simply using the `ctrl+e` shortcut (`cmd+e` on Mac).

![Run Action](../../images/guides/devBuildRunAction.png ':size=700')

## Getting Feedback

### Output from Build/Compile Command

When running a build or compile, a terminal will automatically open which will display the standard output for the running build/compile command.

![IBM i Build Output Channel](../../images/guides/devBuildBuildOutput.png ':size=1100')

### Compiler Feedback

IBM i Developer will populate the problem view (View->Problems, or Ctrl/Cmd + Shift + M) with compiler feedbacks. Those feedbacks come from the event files under `.evfevent` directory.By default, the build tools will create those event files, and IBM i Develop will then pull them back in the Build/Compile process.

![Problem View](../../images/guides/devBuildProblemView.png ':size=1000')

You can click on any line to jump to the source directly.

### Job Log Results

**IBM i Project Explorer** provides a view in the **IBM i Job Log** panel to display all of the information you would get in an IBM i job log and spool files after a build.  This will be very helpful when diagnosing why a build failed.

![Job Log panel](../../images/guides/devBuildjobLog.png ':size=1000')

## Job log Tree Contents

### Project Level
The top level of this tree is the project name with the project description as its details. This allows the job logs of multiple projects to be shown in this view. Only the latest build is persisted in the project, but the other jobs are kept in memory until the workspace is restarted or the **Clear Previous Job Logs** action on the project is run.

![Clear Previous Job Logs](../../images/guides/devBuildClearPrevious.png ':size=750')

The **Show Job Log** action, which is also available inline, will load the joblog.json file with the raw data into the editor.  This might be useful for searching, etc.

![Show Job Log](../../images/guides/devBuildShowJobLog.png ':size=750')

### Job Log Level
The next level is the timestamp when the build or compile was run and represents all the command run for that particular build.
The commands can be toggled via the inline action to show all executed commands or only those that failed.

![Show Failed Jobs](../../images/guides/devBuildShowFailedJobs.png ':size=800')

If the action above is selected, all successful commands are filtered out so that you can focus on the failing ones.  This can be very useful when lots of objects are being built.

![Show All Jobs](../../images/guides/devBuildShowAllJobs.png ':size=800')

To help zero in on the problem, there is also an action to filter the messages, so that you can set the minimum severity of messages you would like to see. In the example below, the message severity was set to 10 so that all of the information messages were filtered out.

![Filter Message Severity](../../images/guides/devBuildFilterMessageSeverity.png ':size=750')

### Object Level
The children of a job log are all the objects built.  The description is the source in the IFS that is being compiled into that object.

![Copy Command](../../images/guides/devBuildObjectLevelActions.png ':size=750')

The actions available on the object, both in the context menu and the inline menu include **Copy** and **Show Object Build Output**.  The **Copy** action copies the command into the clipboard.  This allows you to paste it into the 5250 or PASE where you can run the command in isolation. The **Show Object Build Output** will show the equivalent of spool file output including the compile listing and any standard error and output from PASE commands.  Note that the compile errors will also be shown in the `Problems` view but sometimes there are other reasons for a command to fail and they can be seen here.

### Messages Level

On the next level the first item is unique. It is the compile command being run in QSYS to build the target.   This command can be copied using the **Copy** action.

The remaining items are the messages that were produced in the job log when executing this command.  While there are no actions on messages, the hover shows all of the message details including second level help.

![Second Level Message](../../images/guides/devBuildSecondLevelMessage.png ':size=750')

## Where Does The Data Come From
Build commands such as BOB (https://github.com/IBM/ibmi-bob) and ARCAD generate a file named `.logs/joblog.json`. They also generate text files in the same directory with names like `target_object.splf` that contain the PASE standard out or spool file equivalent for each command, including the compile listing.  These files are copied to the `.logs` directory in the project root.

![Job Log File](../../images/guides/devBuildJobLogFile.png ':size=850')

This copy process is automatic when invoking build or compile action for **IBM i Project Explorer** projects. For **Code for IBM i** actions the downloading of these files can be configured by the `postDownload` attribute in the `actions.json`  file.  The `.evfevent` directory specified here is necessary to get the compiler errors in the `Problems` view.

![Configure download of .logs directory](../../images/guides/devBuildPostDownload.png ':size=850')

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin)    

