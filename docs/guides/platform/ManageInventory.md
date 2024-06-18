# Manage Inventory
The Inventory manages the systems information used for Merlin. It will save the general system information including hostname, type, description and some other information. The detailed inventory info will be changed according to the inventory type. Merlin supports four types of hosts: `IBM i`, `IBM PowerVC`, `IBM Cloud`, and `Jenkins`.  

Go to Merlin GUI and select **Connections -> Inventory** to go to the Inventory page.    

![Inventory Page](../../images/guides/Addinventory0.png ':size=800')

All the inventories which the user created or has permission to will be shown. It will display the `Name`, `Hostname`, `Type`, `Description`, `Owner`, `Provision`, `Created Time`, and `Updated Time`. The `Provision` item is a tag that specifies the IBM i server deployed by the Power VC template.  

- **Add Inventory** - Click the `+ Add` button and enter the `Name`, `Type` and `Hostname`.  After the type is selected, the required fields will updated. If one of requirements are not meet,  the `+ Add` button will stay disabled. Specify all the necessary fields and click the `+ Add` button so then the new inventory record will be imported to the Inventory table.

  ![Add Inventory](../../images/guides/Addinventory.png ':size=600')

  For secure connections, Merlin supports the `Test TLS` function which allows customers to accept the remote CA such as for a Power VC Inventory. After specifying the `Hostname` and `Port`, the `Test TLS` button will be enabled and it will catch the Power VC CA. In the detailed certificate windows, click the `Accept` button. Then this CA be added to the Merlin trust store.

  ![Add Power VC Inventory](../../images/guides/Pvcinventory.png ':size=600')

  **Note:** For the `Name` field, please avoid the use of the special characters .,:;/\"*?<>|=+&%'\`@". The `Hostname` field supports `IP`, `Hostname`, and `URL`.  

- **Delete Inventory** - The `Delete` button is disabled by default. Selecting items in the inventory table will highlight them and also enable the `Delete` button. Click the `Delete` button and click `Yes` to the confirmation to delete all selected items.

- **Edit Inventory**  - The `Edit` button is disabled by default. Selecting one item in the inventory table will highlight it and also enable the `Edit` button. Click the `Edit` button to see the inventory info. All fields may edited except the `owner`.

- **Refresh Inventory** - Click the `Refresh` button for the Merlin GUI to retrieve the latest inventory info.