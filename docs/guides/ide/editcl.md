# Editing CL

Apart from those common language features mentioned in [Edit](./guides/ide/edit.md), CL source editing also has these capabilities:

- Outline view - view of source that allow navigation to source, filtering the items

- References - show particular code element is referenced throughout the codebase  
  After right clicking on a specific variable, users can select either **Go to References** or **Peek References** to show all references embedded inline. You can navigate between different references in the peeked editor and make quick edits right there.

- Definition - show source of particular code element  
  After right clicking on a specific variable, users can select **Go to Definition** to go to the definition of the variable.  

- Content assist - code and variable completion  
  Content assist can be triggered in editor window by typing **⌃/CTRL+SPACE**.  

- Source error reporting - errors are shown beside the text in the editor and **Problems** view  
  Use **View > Problems** to open problems view.  
  The errors display in both editor and problems view. In the editor, if the cursor hovers over the problem, it will show the details of the error. In the problems view, clicking a specific problem will position the editor to the location of the error.  

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin)
