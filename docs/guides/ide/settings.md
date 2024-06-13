# Settings

Settings can be used to manage extension settings, keyboard shortcuts, themes (colors, file icons, and product icosn), and much more. To change any of these settings, click the **Manage** gear icon at the bottom left and select the appriopriate option. 

![Settings](../../images/guides/ideWorkspaceSettings.png ':size=350')

## Preferences

### IBM i Developer

#### Formatting Options RPGLE

- **Maximum Line Width** - The maximum number of characters for each line.
- **Preserve Relative Indent Of Continued Line** - Specify if continued lines preserve their relative indentation.
- **Select Indent** - The number of spaces to indent after a `select` statement.
- **Start Column** - Indentation begins at the column that you set.
- **Tab Size** - The number of spaces to indent.

#### Formatting Options SQL

- **Adjust Casing Only** - Specify if to only adjust uppercasing or lowercasing and to not change the indentation/line breaks.
- **Identifiers** - Specify the uppercasing or lowercasing of SQL identifiers.
- **Keywords And Built-in Functions** - Specify the uppercasing or lowercasing of SQL keywords and built-in functions.
- **Indent** - Specify the indent level for SQL formatting (1-20).
- **Maximum Line Length** - Specify the maximum number of characters for each line.
- **New Line On And/Or** - Specify if to insert line break before or after AND or OR keyword, or just leave alone.
- **New Line On Comma** - Specify if to insert line break before or after a comma, or just leave alone.

#### Logging

- **Language Server Logging Category** - Specify logging level for RPG or SQL language server.
- **Level** - Specify logging level.

### IBM i Debug

* See [Settings](./guides/ide/debug.md) section of debug documentation

### IBM i Project Explorer

* **Disable User Library List View** - Specify if to disable the user library list view in **Code for IBM i**.

### Code for IBM i

* See [Global Settings](https://codefori.github.io/docs/settings/global/) page of **Code for IBM i** documentation.

### Db2 for IBM i

* See [Db2 for IBM i](https://codefori.github.io/docs/extensions/db2i/) documentation.

## Keyboard shortcuts

Keyboard shortcuts can be specified to invoke commands. To view and modify all shortcuts, navigate to the command palette and run the command **Preferences: Open Keyboard Shortcuts** 

The following **IBM i Project Explorer** and **Code for IBM i** commands have default shortcuts:

* **Run Build**
  * Windows: `Ctrl+Shift+b`
  * Mac: `Cmd+Shift+b`
* **Run Compile**
  * Windows: `Ctrl+Shift+c`
  * Mac: `Cmd+Shift+c`
* **Run Action**
  * Windows: `Ctrl+e`
  * Mac: `Cmd+e`
* **Launch Deploy**
  * Windows: `Ctrl+shift+e`
  * Mac: `Cmd+shift+e`
* **Go to File...**
  * Windows: `Ctrl+alt+p`
  * Mac: `Cmd+alt+p`
* **Go to File (Read Only)...**
  * Windows: `Ctrl+alt+o`
  * Mac: `Cmd+alt+o`
* **Connection Settings**
  * Windows: `Ctrl+alt+,`
  * Mac: `Cmd+alt+,`
* **Launch Terminal Picker**
  * Windows: `Ctrl+shift+j`
  * Mac: `Cmd+shift+j`

## Theme

Themes can be used to modify the look of your workspace. From the command palette, use the **Preferences: Color Theme** command to select a color theme, the **Preferences: File Icon Theme** to select a file icon theme, or the **Preferences: Product Icon Theme** to select a product icon theme.

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin) 