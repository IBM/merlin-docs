# Git Integration

**Git** is integrated within the IDE, which means you can run commands like `git clone`, `git pull`, `git commit`, and so on, to help you with source control management. 

In order to run a Git command, go to **View > Command Palette...**, then type in `Git` to find the Git command you wish to run. If you are more familiar with Git on the command line, you may also run the Git commands from an opened terminal. To open a terminal, do **Terminal > new Terminal**.

The **Source Control** view is helpful to see the status of your git branch. Here you can see the changes you have made since the last commit and perform various git actions.

## Example Workflow

Below is an example using Git within the IDE showing the steps from cloning a project to committing your changes to a newly created branch.

1. Use the **Git: Clone** command to load source from a Git repository. This can also be done using the **Clone Repository** button in the **File Explorer**.

  ![Git Clone](../../images/guides/ideGitClone.png ':size=800')

2. Create a new branch for development with **Git: Create Branch**.

  ![Git Create Branch](../../images/guides/ideGitBranch.png ':size=800')

3. After making some changes, you will see the list of your changes in the **Source Control** view. You may see the *diff* between the original files and the changed files when you click on a file.   

  ![Git Source control View](../../images/guides/ideGitDiff.png ':size=800')

4. Now to stage the changes with **Git: Stage All Changes**.

  ![Git Stage](../../images/guides/ideGitStage.png ':size=800')

5. Use **Git: Commit** to commit your changes.

  ![Git Commit](../../images/guides/ideGitCommit.png ':size=800')

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin) 