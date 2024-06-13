# Editing SQL

Apart from the common language features mentioned in [Edit](edit.md), SQL source editing also has the following capabilities:

* Formatting - Format the whole document or a selected region.

  Users can select **Format Document** to format the whole document or **Format Selection** to format the selected region after right clicking in the source file. Formatting supports both **Embedded SQL formatting** and **Pure SQL formatting**.
  
  ![SQL Formatting](../../images/guides/idesqlformatting.png ':size=700')   
  
  Formatting options can be found on the [Preferences](./guides/ide/settings.md) page.  
  
  ![SQL Formatting Options](../../images/guides/idesqlformattingoptions.png ':size=700')  

* Content Assist - Provides both SQL keyword and database element completion. Content assist can be triggered in the active editor by typing **Ctrl+Space**.  

  * Content assist for SQL keywords  

  ![SQL Content Assist for Keywords](../../images/guides/idesqlcontentassist-1.png ':size=700')

  * Content assist for database element
    
  Note: Before using content assist for database element, please refer to the [documentation](https://ibm.github.io/ibmi-bob/#/prepare-the-project/iproj-json?id=sql) on how to set the SQL properties in `iproj.json` of the project.  

  ![SQL Content Assist for Database Element](../../images/guides/idesqlcontentassist-2.png ':size=700')

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin)