# Editing RPG

Apart from the common language features mentioned in [Edit](./guides/ide/edit.md), ILE RPG source editing also has the following capabilities:

- **Outline** view - Provides a structured view of the source code for navigation and supports filtering.

  Select `outline` in **View > Open View..** to open the outline view.  

  ![RPG outline view](../../images/guides/iderpgoutlineview-1.png ':size=850')  

  Click a field or a variable in the Outline view to position the editor to its definition and highlight it.  

  ![RPG outline view Click](../../images/guides/iderpgoutlineview-2.png ':size=800')  

  Outline view supports filtering the items by name. Click on an item in the outline view and press `Ctrl+F` (`Cmd+F` on macOS). A popup will will appear at the top right corner of the outline view where you can now type the string that you would like to filter items by. There are also buttons next to the search box to `Filter` items or enable `Fuzzy Match`.  

  ![RPG outline view Filter](../../images/guides/iderpgoutlineview-3.png ':size=850')  

- Hover - Show information for item under cursor.

  Put the mouse over the variable and the hover will appear and show the definition of the variable.  

  ![RPG hover](../../images/guides/iderpghover.png ':size=800')  

- References - Show where a particular code element is referenced throughout the workspace.

  After right clicking on a specific variable, users can select either **Go to References** or **Peek > Peek References** to show all references embedded inline. You can navigate between different references in the peeked editor and make quick edits right there.

  ![RPG Go to References](../../images/guides/iderpggotoreferences.png ':size=800')

  Users can also select **Find All References** to reveal the references in the **References** view.  

  ![RPG Find all References](../../images/guides/iderpgfindallreferences.png ':size=800')

- Definitions - Show source of particular code element.

  After right clicking on a specific variable, users can select **Go to Definition** to go to the definition of the variable or **Peek > Peek Definition** to show the definition embedded inline.  

  ![RPG Definition](../../images/guides/iderpgdefinition.png ':size=650')

- Formatting - Format the whole document or a selected region.

  Users can select **Format Document** to format the whole document or **Format Selection** to format the selected region after right clicking in the source file.  

  ![RPG Formatting](../../images/guides/iderpgformatting.png ':size=650')

  Formatting options can be found on the [Preferences](./guides/ide/settings.md) page.

  ![RPG Formatting Options](../../images/guides/iderpgformattingoptions.png ':size=750')

- Content Assist - Code and variable completion.

  Content assist can be triggered in active editor by typing **Ctrl+Space**.  

  ![RPG Content Assist](../../images/guides/iderpgcontentassist.png ':size=700')

- Rename - Intelligently rename all references to variables.

  Select **Rename Symbol** and then type the new desired name and press Enter. All usages of the symbol will be renamed, across files.  
  
  ![RPG Rename](../../images/guides/iderpgrename.png ':size=650')

- Extract Constant - Extract a literal value into a constant.
  
  Click on a literal value, and then click the light bulb icon that appears to the left. Two options will be available: 
  **Extract constant to enclosing scope** and **Extract constant to global scope**. Clicking one of these options will create
  a new constant in the selected scope, with the extracted literal value. The literal value will be replaced
  in your code with the new constant.

  ![RPG Extract Constant](../../images/guides/iderpgextractconstant.png ':size=750')

- Source Error Reporting - Errors are shown beside the text in the editor and in the **Problems** view.
  
  In the editor, if the cursor hovers over the problem, it will show the details of the error. In the **Problems** view, clicking a specific problem will position the editor to the location of the error. Use **View > Problems** to open the **Problems** view.   
  
  ![RPG Problems](../../images/guides/iderpgproblems.png ':size=750')

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin)
