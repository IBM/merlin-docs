Below are specific steps to working with Azure DevOps. It is assume an organisation and project repository already exist.

# Simple builds and development

For non-ARCAD builds, HTTPS can be used in IDE and CICD.

## Fetching the HTTPS login

Head to the project in Azure DevOps and open the repository tab. Click on the 'Clone' button.

![](../images/azure/azure-1.png)

A new window appears. The HTTPS clone URL is used later. Select 'Generate Git Crentials'

![](../images/azure/azure-2.png)

This shows a username and password. Keep those for later as they are needed during the clone and CICD profile setup.

![](../images/azure/azure-3.png)

## Cloning into Merlin IDE

Launch into IBM i Developer from Merlin.

![](../images/azure/azure-4.png)

Find 'Git: Clone' from the command palette (F1):

![](../images/azure/azure-5.png)

Paste the HTTPS clone URL into the input box shown and press enter to clone.

![](../images/azure/azure-6.png)

Theia will prompt the user on where the repository should be cloned. Select the 'projects' folder and 'Select Repository Location'

![](../images/azure/azure-7.png)

Theia will prompt for a password. This will be the password that Azure has generated.

![](../images/azure/azure-8.png)

After the clone is successful, the directory that was created from the clone will appear in the Workspace.

![](../images/azure/azure-9.png)

## HTTPS in CICD

Launch into CICD.

![](../images/azure/azure-10.png)

When in the CICD UI, launch into 'Manage Jenkins Credential' and Add a new entry:

![](../images/azure/azure-11.png)

In the window that appears, provide the Azure username and password clone credentials. The id and description should be specific to the Azure account used.

![](../images/azure/azure-12.png)

Next, create a new profile with a unique name.

![](../images/azure/azure-13.png)

When cloning the git repository, you can specify the repository URL and the newly created credential.

![](../images/azure/azure-14.png)

# ARCAD Builder

To make use of ARCAD Builder in both IDE and CICD, SSH key pairs must be used.

## Connecting Azure to the Workspace

When generating a key for a particular host, it must be the domain and all subdomains. If the SSH clone URL is:

`git@ssh.dev.azure.com:v3/your-cool-company/company_system/company_system` 

then the host is:

`ssh.dev.azure.com`

Launch into IDE, open the command palette and search for 'Generate Key for Particular Host'. 

![](../images/azure/azure-15.png)

Enter the full host name `ssh.dev.azure.com`:

![](../images/azure/azure-16.png)

When prompted to display the public key, it opens in a new tab.

![](../images/azure/azure-17.png)

In Azure DevOps, press the user settings icon and then select 'SSH public keys'

![](../images/azure/azure-18.png)

Give this key a unique name (unique to the current Theia Workspace) and paste the key generated in the Theia Workspace. Clicking 'Add' will add and save the public key to your account.

![](../images/azure/azure-19.png)

Head back to the repository that will be cloned and press the 'Clone' button. A new window will appear. Ensure SSH is selected and copy the shown SSH URL.

![](../images/azure/azure-20.png)

Head back to the Theia Workspace, open the command palette and search for 'Git: Clone'. When the input box is shown, paste the SSH URL from the Azure DevOps repository and press enter.

![](../images/azure/azure-21.png)

The repository is now cloned into your Workspace and the ARCAD tools (Builder, Observer, etc) can be used. See [Using Merlin IDE](./guides/crw/main.md) for a guide on building from the Workspace.

## Connecting Azure to ARCAD Builder

Before continuing:

1. correctly configure the repository as an ARCAD project before creating a profile. See 'ARCAD Project' in [Using Merlin IDE](./guides/crw/main.md) to learn how to configure a project.
2. ensure that the `repository` property in the repository's `iproj.json` is the SSH clone URL pointing to the Azure DevOps repository.

The user profile on IBM i that is running the build needs to have a seperate SSH key pair setup. See 'ARCAD Builder clone process' in the [CICD guide](./guides/cicd/main.md) to learn how to generate an SSH key pair for an IBM i user profile.

The generated public key must be added to a Azure account that has access to the repository being used.