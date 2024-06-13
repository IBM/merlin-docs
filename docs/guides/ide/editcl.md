# Editing CL

Apart from those common language features mentioned in [Edit](edit.md), CL source editing also has the following capabilities:

- Outline view - Provides a structured view of the source code for navigation and supports filtering.

  Select `outline` in **View > Open View..** to open the outline view.  
  
  ![CL Outline View](../../images/guides/idecloutlineview-1.png ':size=750')  

  Click a field or a variable in the Outline view to position the editor to its definition and highlight it.  
  
  ![CL Outline View Click](../../images/guides/idecloutlineview-2.png ':size=750')  

  Outline view supports filtering the items by name. Click on an item in the outline view and press `Ctrl+F` (`Cmd+F` on macOS). A popup will will appear at the top right corner of the outline view where you can now type the string that you would like to filter items by. There are also buttons next to the search box to `Filter` items or enable `Fuzzy Match`.  

  ![CL Outline view Filter](../../images/guides/idecloutlineview-3.png ':size=800')  

- References - Show where a particular code element is referenced throughout the codebase.

  After right clicking on a specific variable, users can select either **Go to References** or **Peek > Peek References** to show all references embedded inline. You can navigate between different references in the peeked editor and make quick edits right there.

  ![CL Go to References](../../images/guides/ideclreferences.png ':size=800')  
  
  Users can also select **Find All References** to reveal the references in the **References** view.  

  ![CL Find all References](../../images/guides/ideclfindallreferences.png ':size=800') 

- Definition - Show source of particular code element.
  
  After right clicking on a specific variable, users can select **Go to Definition** to go to the definition of the variable or **Peek > Peek Definition** to show the definition embedded inline.  

  ![CL Definition](../../images/guides/idecldefinition.png ':size=750')  

- Content Assist - Code and variable completion. Content assist can be triggered in the active editor by typing **Ctrl+Space**.  

  ![CL Content Assist](../../images/guides/idclcontentassist.png ':size=750')  

- Source Error Reporting - Errors are shown beside the text in the editor and in the **Problems** view.
  
  In the editor, if the cursor hovers over the problem, it will show the details of the error. In the **Problems** view, clicking a specific problem will position the editor to the location of the error. Use **View > Problems** to open the **Problems** view.  

  ![CL Source Error Reporting](../../images/guides/ideclproblems.png ':size=750')  

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin)
