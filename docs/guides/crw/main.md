# Using Merlin IDE (IBM i Developer)

This section covers Merlin IDE, IBM i Developer and developing applications using it. Before you use IBM i Developer, make sure [you have a template setup](./guides/configuration.md) and [IBM i Developer installed](./guides/appinstall.md) into a project.

Merlin IDE / IBM i Developer is a cloud-hosted version of VS Code with Merlin & IBM i specific extensions. IBM i Developer connects to your IBM i based on credentials you have specified in Merlin. Repositories get cloned into your Workspace and then deployed to your IBM i.

## Creating Your First Workspace

When you launch into IBM i Developer for the first time, you may not have any Workspaces. For this, we're going to start from a sample. On the Create Workspace menu, you will be able to select from samples. You should select 'IBM i Developer'.  Other sample tools stacks might useful for developing with other programming languages and frameworks.  The 'Custom Workspace' tab can be explored for those who want to understand the yaml source behind these tool stacks and even customize them.  It is also possible load a customize workspace directly from a URL that your provide.  This can help on-board new developers very quickly.

![](../../images/ide/ide-1.png)

Selecting it starts to spin up a Workspace for you.  Under the covers a Linux container is being spun up with all of the client tooling required to do develop classic IBM i applications.  This can take a moment, and  automatically launchs into the IDE when it's done.

![](../../images/ide/ide-2.png)

## Connecting to Git Repositories

> This section assumes you have a GitHub account. A GitHub account for this guide is not required but will prove useful as will this will run through setting up keypairs, which is a standard process in most git ecosystems. [Click here to sign up](https://github.com/signup).

Before we can write any code, we have to clone a repository. This means we have to connect to whichever git hosting service you use. For this service, we are going to use GitHub since that is where our examples are hosted.  In order to maintain source (commit, push, etc) in GitHub we need to have a private key in the local repository that corresponds to the public key configure in your GitHub account.

If you have already have a public key set up in your Github account, you can simply run the 'SSH: Upload Private Key' command to upload the corresponding private key into this workspace.

Alternatively, we can create an SSH private/public key pair and have the private configured in this workspace and add the public key to your GitHub configuration.  THe following instructions illustrate this last alternative.

> A keypair is only needed when working with private repositories. The 'bob-recursive-example' repository is a public repository on GitHub and can be cloned via HTTPS without needing a GitHub account or setting up a public/private keypair.

### Generating a SSH Public/Private Key Pair

First, open the command palette (F1 or Control / Command + Shift + F1) and search for 'SSH: Generate Key'.

![](../../images/ide/ide-3.png)

This generates a key pair for you and when it's done you are asked if you'd like to view the public key.

![](../../images/ide/ide-4.png)

Selecting 'View' opens a new tab with the public key inside of it. Keep this tab open as it is going to needed.  Note that you can always get this public key back by running the 'SSH: View Public Key...' command.

![](../../images/ide/ide-5.png)

### Adding Your Key to GitHub

All the major services (BitBucket, GitLab, GitBucket, etc) have similar setups. This guide should work the same for them.

Open up GitHub, go to the user settings, [go to the 'SSH and GPG keys'](https://github.com/settings/keys) page and select 'New SSH key'.

![](../../images/ide/ide-6.png)

Copy and paste the public key that was generated earlier in IBM i Developer and paste it into the Key input field. Give it a name that identify it with the Workspace you created.

![](../../images/ide/ide-7.png)

## Cloning a Repository

Now that we have setup our key pair, we can clone from GitHub. I went ahead and [made a fork of bob-recursive-example](https://github.com/edmundreinhardt/bob-recursive-example) for me to work on and I am going to clone that into my Workspace.

First, open the command palette (F1 or Control / Command + Shift + F1) and search for 'Git: Clone'. When the input box appears, paste the **SSH clone URL** for the git repository.

![](../../images/ide/ide-8.png)

> The HTTPS clone URL can also be used if the repository being cloned is public on GitHub.

The cloning process is going to ask you were to clone the repository. By default it should use a `projects` folder. You can optionally set it somewhere else, though it is recommend to use the default. When you've made your selection, hit 'Select Repository Location' to continue the clone.

![](../../images/ide/ide-9.png)

You may see a message with the following text. **This is not an error**, but a warning you will get the first time you clone from a new location. You can clone and 

> Git: Warning: Permanently added 'github.com,140.82.114.3' (ECDSA) to the list of known hosts.

After the clone is successful, you should see the directory automatically appear in the Explorer view.

![](../../images/ide/ide-10.png)

## IBM i Developer features

IBM i Developers come with lots of features that enable developers to be more productive.

* **green**: a search that scans the entire workspace for the provided string. It also supports searching with a regex string.
* **orange**: peek is a feature that shows either all references or the definitions inline without having to navigate to another view. You can use peek, as well as many other reference tools, by right clicking on any definition in the source code.
* **red**: the outline view displays all defined variables, structs and files in active editor

![](../../images/ide/ide-21.png)

## Connecting to a Remote System

Since we're writing RPGLE code, that means the build must happen on a remote IBM i. Previously, [we setup a Template](./guides/configuration.md) which points to a developer IBM i. We're going to connect that Template to our Workspace.

Open up the IBM i Project Explorer view. Inside of it, you should see the project/repository you cloned. If the project is expanded, you will see that it is not connected to any IBM i currently. Right click and select 'Specify IBM i Connection' or simply click on the + action icon.

![](../../images/ide/ide-11.png)

It may or may not prompt you for your Merlin password. This is so it can fetch Templates from the Merlin vault for your user. This shows a list of available templates, and you can select the connection you want to use for this project.

![](../../images/ide/ide-12.png)

Following that, you are able to select a build directory. It is recommended that you leave it as the default. This is the IFS directory that the source gets uploaded to before they are compiled on the server. The errors seen are not important right now.

![](../../images/ide/ide-13.png)

Since this is the first time using this Template from this Workspace, you may be prompted with a warning about authenticity. You can click 'Always' to not see this message about this Template again.

![](../../images/ide/ide-14.png)

## IBM i Project Explorer

There are two main parts of the project explorer with our basic project:

* **Source** which allows you to browse files in the specified build directory
* **IBM i** where you manage the connection to the remote system, as well as any variables
   * **Variables** for configuring environment variables needed for the project build
   * **Library List** to manage the library list for the Workspace
   * **Object Libraries** for browsing libraries related to the project
   * **My Queries** to browse objects in the QSYS file system and the IFS

If you expand IBM i and then expand Variables under that, you should see `&lib1`. This variable is in the list here because it is used inside of the `iproj.json` file in the repository. This variable is used to determine which library to build objects in.

> When this variable changes, it is assumed that the library provided exists. If it does not exist, a library can be created by right clicking on the IBM i connection and selecting 'Create Library'.

![](../../images/ide/ide-15.png)

Clicking the pencil icon (edit) will open a user input box at the top of the window where you can enter a value to assign to the variable. When you are finished, the value appears next the the `&lib1` label.

## Configuring the Build

This example uses a simple git repo that uses ibmi-bob. You can use any repository you want, but the build tool must be invoked through pase.

For example, ARCAD Builder uses `elias`, bob-recursive-example uses the `makei` command from `ibmi-bob` it could also use `gmake` (GNU Make), or any other PASE command. Even though IBM i Developer is being used, the build happens on the IBM i. Those tools (ibmi-bob, GNU Make, etc) **need to be installed on the IBM i** used for the build.

> This guide is using ibmi-bob (`makei`) to launch the build. Assuming the Inventory was [setup in full](./guides/configuration.md) (and Actions were used to verify the Inventory), [check out the ibmi-bob installation step](https://ibm.github.io/ibmi-bob/#/getting-started/installation).

Once you have the `&lib1` variable setup, it's time to build our project. IBM i Developer has two types of 'builds':

* **Compile** which is used to compile a single source file (and maybe anything it depends on)
* **Build** to compile the entire project

The [ibmi-bob documentation](https://ibm.github.io/ibmi-bob/#/prepare-the-project/iproj-json?id=buildcommand) states what the commands should be for when using ibmi-bob.

In IBM i Developer, you are able to set the default commands in the Theia preferences. If you open the preferences (Control / Command + , ) and find the IBM i Developer settings, you will see the defaults for compile and build.

![](../../images/ide/ide-17.png)

You are also able to specify them at a project level inside the `iproj.json`. This is recommend because then all developers are using the same command and if the commands have to change in the future every developer recieves the change as `iproj.json` is checked into git.

![](../../images/ide/ide-18.png)

If you wanted to, you could also specify another build tool here. The `buildCommand` and `compileCommand` properties are just commands that are execute in the remote IBM i within the PASE environment, once the local source files are uploaded to the remote IFS directory.  You could use `make`, `elias` (ARCAD), or even your own existing system.

After running a build or a compile, you should get error feedback right away within IBM i Developer.

## Running the Build

To run a build (of the entire project):

1. Open the command palette (F1 or Control / Command + Shift + P)
2. Search for 'build project' and select 'IBM i Developer: Build Project'
3. An info message appears to let you know it has started
4. The output window appears with the output from your build tool

![](../../images/ide/ide-19.png)

## Running a Compile

To compile (a single source and items it depends on)

1. Open the source you want to compile
2. Search for 'compile' and select 'IBM i Developer: Compile'
3. An info message appears to let you know it has started
4. The output window appears with the output from your build tool

![](../../images/ide/ide-20.png)