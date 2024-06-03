# Settings

Access settings from **File > Settings**.  

## Preferences

### IBM i Debug

* See [Settings](debug.md) section of debug documentation

### IBM i Developer
 
- Build Command  
Command to invoke when building a project.  A project can override the command used by specifying a build command in the project's `iproj.json`.
- Compile Command  
Command to invoke when building the selected file.  A project can override the command used by specifying a compile command in the project's `iproj.json`.

#### Formatting Options RPGLE

- Maximum Line Width  
The maximum number of characters for each line.
- Preserve Relative Indent Of Continued Line  
Specify if continued lines preserve their relative indentation.
- Select Indent  
The number of spaces to indent after a `select` statement.
- Start Column  
Indentation begins at the column that you set.
- Tab Size  
The number of spaces to indent.

#### Formatting Options SQL

- Adjust Casing Only  
Specify if to only adjust uppercasing or lowercasing and to not change the indentation/line breaks.
- Identifiers  
Specify the uppercasing or lowercasing of SQL identifiers.
- Keywords And Built-in Functions  
Specify the uppercasing or lowercasing of SQL keywords and built-in functions.
- Indent  
Specify the indent level for SQL formatting (1-20).
- Maximum Line Length  
Specify the maximum number of characters for each line.
- New Line On And/Or  
Specify if to insert line break before or after AND or OR keyword, or just leave alone.
- New Line On Comma  
Specify if to insert line break before or after a comma, or just leave alone.
- Include File  
If include files outside the project are resolved when getting the compiler errors after build, it will affect the speed of showing the diagnostics in problem view.

#### Logging

- Language Server Logging Category  
Specify logging level for RPG or SQL language server.
- Level  
Specify logging level.
- Projects  
Edit project configuration in `settings.json`.
- SSH Verbosity Level  
Specify if the verbosity for SSH commands is increased.

#### Upload Command

- Delete Files Not In Source Dir  
Specify if remote files that do not exist locally be deleted when synchronizing local project files to the remote IBM i IFS directory. 

## Keyboard shortcuts

Keyboard shortcuts can be specified to invoke commands. To view and modify all shortcuts, navigate to the command palette and run the command **Open Keyboard Shortcuts** 

The following IBM i Developer commands have default shortcuts:

* Build Project
  * Windows: `Ctrl`+`Shift`+`b`
  * Mac: `Cmd`+`Shift`+`b`
* Compile
  * Windows: `Ctrl`+`Shift`+`c`
  * Mac: `Cmd`+`Shift`+`c`
* Upload to Build Directory on IBM i
  * Windows: `Ctrl`+`Shift`+`u`
  * Mac: `Cmd`+`Shift`+`u`

## Color theme

View and select a color theme by running the **Color Theme** command from the command palette.

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin) 