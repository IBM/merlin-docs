# Extensions

## Installed Extensions

The **Extensions** view is where all VS Code extensions will be managed including several extensions which are automatically installed when creating a new workspace. These extensions add languages, debuggers, and tools to support your development workflow.

![Extensions View](../../images/guides/extensionsView.png ':size=500')

The following extensions will be automatically installed:

* **IBM i Developer** - Provides content assist, code completion, hover information, and more for IBM i programming languages such as RPG and SQL.
* **Code for IBM i** - Connect to an IBM i, edit/compile all ILE languages, view errors inline, and much more.
* **IBM i Project Explorer** - Develop IBM i applications using buildable local projects.
* **IBM i Debug** - Provides a Debug Adapter Protocol (DAP) client for IBM i Debugger.
* **Db2 for IBM i** - Db2 for IBM i tools.
* **RPGLE** - Language tools and linter for RPGLE.
* **CL** - Language tools for CLLE.
* **IBM i Languages** - Provides syntax highlighting for IBM i languages such as RPG, CL, DDS, MI, and RPGLE fixed/free.
* **vscode-sourceorbit** - Object dependency management.
* **Code for IBM i Walkthroughs** - Walkthroughs relevant to **Code for IBM i**.
* **ARCAD-Elias** - ARCAD tools (**ARCAD-Skipper**, **ARCAD-Observer**, and **ARCAD-Transformer**).

## Install an Extension

The [Visual Studio Code Extension Marketplace](https://marketplace.visualstudio.com/VSCode) is where extension authors publish extensions that extend the features in VS Code. You may browse for any of these extensions directly in the **Extensions** view. To narrow down your search, use filters such as `@popular` or `@category:"programming languages"`.

![Browse for Extensions](../../images/guides/extensionsMarketplace.png ':size=600')

After you have found an extension you would like to install, select it to see the Extension details page. Here you can find the extension ID, publisher, number of downloads, ratings, general overview of the extension. After selecting the **Install** button, VS Code will download and install the extension from the Marketplace. Once the installation is complete, the **Install** button will be replaced by a **Disable** and **Uninstall** button.

![Install Extension](../../images/guides/extensionsInstall.png ':size=750')

Extensions may also be installed from the [Open VSX Registry](https://open-vsx.org/), by downloading the `VSIX` file of an extension you would like to install and then using the **Install from VSIX...** action.

![Install from VSIX](../../images/guides/extensionsInstallFromVSIX.png ':size=600')

## Manage Extensions

### Update Extension

You may want to update an extension to pick up bug fixes or new features. To do this, navigate to the extension in the **Extensions** view and select the **Update** button. This button will only be visible if the extension is not at its latest version. To ensure that your extensions are always up-to-date, enable the **Auto Update** checkbox. In the case you would like to downgrade or install a specific version of an extension, click the dropdown beside the **Uninstall** button and select **Install Another Version...**.

![Update Extension](../../images/guides/extensionsUpdate.png ':size=800')


### Disable an Extension

You may want to disable an extension if you would like to temporarily disable its features without uninstalling it. To do this, navigate to the extension in the **Extensions** view and select the **Disable** button.

![Disable Extension](../../images/guides/extensionsDisable.png ':size=800')

### Uninstall an Extension

To uninstall an extension, navigate to to it in the **Extensions** view and select the **Uninstall** button. Once the uninstall is complete, the **Install** button will be visible again.

![Uninstall Extension](../../images/guides/extensionsUninstall.png ':size=800')