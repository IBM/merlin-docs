# Manage Credentials  
The Credential works for managing the credential info used for Merlin. It will save the credential info, including username, password, ssh key, API token, etc. The detailed credential info will be changed according to the credential type. Now Merlin supports four types of credentials same as the inventory host types.  
Go to Merlin GUI, **Connections -> Credentials**, user will turn to the Credentials page.  

<img src="../../images/guides/Credentials.png" width = "800" height = "500" alt="Credential" align=center />

For the Admin side,  all credentials will be shown, for the user side, all the own Credentials will be shown, it includes Name, Type, Description, Owner, Created Time, and Updated Time.  
- **Add Credential**  
Click the Add button, Add the credential name, and select the credential type, the detailed credential info will be shown. According to the credential type, the credential info is difference.   
For credential of IBM i as an example, user must enter the username and password of IBM i server, or the +Add button will be disabled.  Optionally, ssh key information can be specified.  If specified, the ssh key will be used instead of the password to connect to the IBM i during the IDE developer build process.   
 Note: if using ssh key, additional configuration is performed during connection to the IBM i before the build starts.

<img src="../../images/guides/AddCredentials.png" width = "600" height = "300" alt="Add Credential" align=center />

- **Delete Credentials**  
The delete button is disabled by default. Select items in the credentials table, and the selected item will change to highlight and the delete button change to available status.  Click the delete button and click yes to confirm that will delete the selected items.
- **Edit Credential**  
The Edit button is disabled by default.  Select one item in the credential table, the selected item will change to highlight and the Edit button change to available status, click the Edit button, the credential info will be shown.  All fields can be edited except the owner.    
- **Refresh Credentials**  
Click the refresh button, the Merlin GUI will get a refresh so that to get the latest credentials information.
