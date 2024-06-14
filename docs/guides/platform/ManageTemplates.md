# Manage Templates
The Templates manage the template info used for Merlin. It will save the template info including `Name`, `Inventory`, `Credential`, `type`, `owner`, and `description`. The credential and type info will be changed according to the inventory type. Merlin supports four types of templates which are the same as the inventory host types.

Go to Merlin GUI and select **Connections -> Templates** to go to the Templates page.  

<img src="../../images/guides/Templates.png" width = "800" height = "500" alt="Templates" align=center />  

An admin will see all templates, but the `Run` button will only work on templates which they own. A user will only see templates which they own. It will display the `Name`, `Inventory`, `Credential`, `Type`, `Owner`, `Created Time`, and `Updated Time` by default. Users can also customize the columns by clicking the icon at the top right of the table.

- **Add Template** - Click the `+ Add` button and enter the `Name` and select an inventory. The `type` will be automatically populated to be the same as the inventory type. The listed credentials to select from will also be filtered to match the same type. After the type is set, the required fields will updated. For example, if `IBM PowerVC` is the set type, users must enter the vm info which is used in this template or the `+ Add` button will be disabled. Specify all the necessary fields and click the `+ Add` button so then the new template record will be imported to the Templates table.

  <img src="../../images/guides/PvcTemplate.png" width = "600" height = "400" alt="Add Templates" align=center />

- **Delete Templates** - The `Delete` button is disabled by default. Selecting items in the credential table will highlight them and also enable the `Delete` button. Click the `Delete` button and click `Yes` to the confirmation to delete all selected items.

- **Edit Template** - The `Edit` button is disabled by default. Selecting one item in the template table will highlight it and also enable the `Edit` button. Click the `Edit` button to see the template info. All fields may edited except the `owner`.

- **Run Template** - The `Run` button is disabled by default. Selecting one item in the template table will highlight it and also enable the `Run` button. According to the different templates type, the available actions will be different. For a Power VC template, Merlin supports the Provision actions which will help the user to deploy a new IBM VM. The user should make sure all the hardware configurations have been set up on the Power VC side.    

  <img src="../../images/guides/PvcAction.png" width = "600" height = "200" alt="Power VC Templates Action" align=center />  

  For the IBM i server template, Merlin supports `Enable Ansible environment`, `Validate the dependent PTFs`, `Install Certificate`, `Enable IBM i developer environment`, `Enable IBM i Debug Service`, and `Enable ARCAD environment`.   

  <img src="../../images/guides/IAction.png" width = "600" height = "200" alt="IBM i Templates Action" align=center />  

- **Refresh Templates** - Click the `Refresh` button for the Merlin GUI to retrieve the latest template info.
