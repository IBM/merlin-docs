#  Debugging

The IBM i Debugger for Merlin consists of a host component called **IBM i Debug Service**, and a client **IBM i Debug** extension included in the IDE. IBM i Debug Service is delivered as host [PTFs](./guides/platform/ManageIBMiServer.md#running-actions-on-the-ibm-i-server).  A Java 11 JRE is required to run IBM i Debug Service on the host.

- Run **Validate the dependent PTFs** action on a template from **Connections** to verify the required PTFs are applied.
- Run **Enable IBM i debug service** action on a template from **Connections** to start the debug service on the IBM i.

The IBM i user profile needs to have the following authorities:
- `*USE` authority to the Start Debug (`STRDBG`) command.
- `*USE` authority to the End Debug (`ENDDBG`) command.
- `*USE` authority to the Start Service Job (`STRSRVJOB`) command.
- `*USE` authority to the End Service Job (`ENDSRVJOB`) command.

For a demonstration, see the Debugging video in the [Merlin Getting Started for Users](https://www.youtube.com/playlist?list=PLPELYviDwCnY6L5r5ZnmCneqhakLcB7ko) playlist.

## Supported Features

Supported language types:

 - RPG (including RPGLE and OPM RPG)
 - COBOL
 - C/C++
 - CL

Supported batch debug features:

 - Debugging of batch programs
 - Passing parameters to a batch program when starting a debug session
 - Setting line breakpoints
 - Saving breakpoints across debug sessions
 - Stepping (step into/over/return)
 - Viewing variable values in the Locals view
 - Monitoring expressions in the Watch view
 - Displaying variable values in debug editor hover
 - Debugging with the \*SOURCE and \*LIST views
 - Debugging with the **Update Production Files** option


Supported service entry point debug features:

 - Setting a service entry point on a program, service program, module or procedure
 - Enabling, disabling and removing a service entry point 
 - Setting a condition on a service entry point
 - Modifying the user profile of a service entry point
 - Logging service entry point activities in an output channel
 - Saving service entry points across multiple IDE sessions


## Limitations

The following features are not supported in the current release:
-	Code coverage


## Debugging from IBM i Project Explorer

With a project in **IBM i Project Explorer**, connect to the IBM i.  Once you are connected to an IBM i, navigate to the program you would like to debug using the **Library List** or **Object Libraries**. Two debug actions will be available from the Debug popup menu:
 - **As Batch**: Prompt for the command to start a batch debug session. The user can enter program parameters in the prompt. This action is only available on program objects (`*PGM`).
 - **Set Service Entry Point**: Set a Service Entry Point (SEP) on the selected target. The default Service Entry Point location is presented in the prompt. The user can modify the value to set a SEP on a module or a procedure. This action is available on program (`*PGM`) and service program (`*SRVPGM`) objects.

![Debug Action](../../images/guides/debugActions.png ':size=650')

When a batch debug session starts, the debug panel will open on the left and the source will open in the editor.  From the debug panel, the following actions are available: **Continue**, **Step Over**, **Step into**, **Step out**, **Restart**, and **Stop**.  In the editor, breakpoints can be set on lines in the left margin.

![Debug](../../images/guides/debugButtons.png ':size=750')

While debugging, the **Variables** view will show the variable values.  In the editor, hovering over a variable will show its value.

![Debug Hover](../../images/guides/debugHover.png ':size=850')

## Managing Service Entry Points

Service Entry Point (SEP) can be set using the **Set Service Entry Point** popup menu action from the **Project Explorer** and **Object Browser**. SEP can also be set using the **Create Service Entry Point** toolbar action from the **Service Entry Points** view in the **Run and Debug** side bar. The action will prompt for the Service Entry Point location. The Service Entry Point location can be in a short format which includes the library name and program name (e.g. `MY_LIB/MY_PGM`), or in a long format which also includes the program type, module name and procedure name (e.g. `MY_LIB/MY_PGM *PGM/MY_MOD/MY_PROC`). The supported program types are `*PGM` and `*SRVPGM`.

![Debug Service Entry Point](../../images/guides/debugSEPView.png ':size=600')

A checkbox at a SEP indicates that the SEP is enabled. You can clear the checkbox to disable the SEP.

You can use the **Edit Condition…** inline action to set a condition on a SEP. The condition is an expression that is evaluated to a boolean value at the given context.

You can use the **Remove Service Entry Point** inline action to remove the selected SEP, or the **Remove All Service Entry Points** toolbar action to remove all SEPs from the view.

You can use the **Modify User Profile...** context menu action to modify the owner of the job where the service entry point is hit. The default owner user profile is the currently logged on user.

Service Entry Point related messages appear in the **IBM i Service Entry Points** output channel.

Service Entry Points are saved in the debug service job on the host. When a new debug client connects to a running debug service, it will restore the saved service entry points for the current user.

## Settings

The following settings are available from the **Debugger** tab of the **IBM i: Connection Settings** page. The page can be accessed from the Command Palette.

- **Debug port**: The secure debug port.

- **SEP debug port**: The Service Entry Point daemon port.

- **Update production files**: Enable updating of production files during debugging.

- **Debug trace**: Enable tracing for **Debug Adapter Protocol**.

The debug port and SEP debug port are specified in the DebugService.env file on the host. 

## Frequently Asked Questions

**Question**: Can I pass parameters to the batch program being debugged?  
**Answer**: Use the **Debug As Batch** action, change **Command used to start debugging** to specify program parameters.

**Question**: Can I debug with Update Production Files set to true?  
**Answer**: You can turn on Update Production Files by checking the **Update production files** setting from the Debugger tab of Connection Settings. The Connection Settings page can be opened from **View > Command Palette… > IBM i: Connection Settings**.

**Question**: I am not able to set a line breakpoint when debugging a LISTING source.  
**Answer**: Please check the following settings in the Settings page:

	Features > Debug > Allow Breakpoints Everywhere > Allow setting breakpoints in any file

**Question**: What is the JRE requirement for running the debug service?  
**Answer**: A Java 11 JRE is required to run IBM i Debug Service v2.0. You can set the **JAVA_HOME** environment variable to the root path of a Java 11 JRE. If **JAVA_HOME** is not set, we will use the Java 11 JVM under the following path:

    /QOpenSys/QIBM/ProdData/JavaVM/jdk11/64bit

**Question**: How can I stop IBM i Debug Service?  
**Answer**: You can run the following command to stop IBM i Debug Service:

    QSH CMD('/QIBM/ProdData/IBMiDebugService/bin/stopDebugService.sh')

**Question**: What port numbers are used by the debug service?  
**Answer**: IBM i Debug Service v2.0 uses three port numbers: the debug daemon port (default is 8001), the secure debug port (default is 8005) and the service entry point daemon port (default is 8008). The secure debug port is used by the secure communication between the debug service and the debug client. The debug daemon port is only used to stop the debug service. The service entry point daemon port is used for service entry point communication.

**Question**: How can I change the port numbers for the debug service?  
**Answer**: For the debug daemon port, you can change the following value in file /QIBM/ProdData/IBMiDebugService/bin/DebugService.env to specify a different port number:

    DBGSRV_PORT=8001

For the secure debug port, you can change the following value in /QIBM/ProdData/IBMiDebugService/bin/DebugService.env to specify a different port number:

    DBGSRV_SECURED_PORT=8005


For the service entry point daemon port, you can change the following value in /QIBM/ProdData/IBMiDebugService/bin/DebugService.env to specify a different port number:


    DBGSRV_SEP_DAEMON_PORT=8008
    
You need to restart the debug service after changing a port number.

**Question**: How can I see the output of the debug service?  
**Answer**: You can see the output of IBM i Debug Service v2.0 from the log file under the following path on the host machine: 

    /QIBM/UserData/IBMIDEBUGSERVICE/DebugService_log.txt

**Question**: Can I get trace information for the debug service?  
**Answer**: You can turn on tracing by checking the **Debug trace** setting from the Debugger tab of Connection Settings. The Connection Settings page can be opened from **View > Command Palette… > IBM i: Connection Settings**. Debug trace will appear in the **Debug Console**.

**Question**: I am getting a message “EQAVS1007E myHost on port 8005 could not be connected” when I start a debug session.  
**Answer**: IBM i Debug Service is not started yet. Please start it from the playbook first.

**Question**: The start of the debug service fails and a log file is generated under the path /QIBM/UserData/IBMIDEBUGSERVICE/QDBGSRV/.eclipse.   
**Answer**: You can remove the whole Eclipse cache file directory under /QIBM/UserData/IBMIDEBUGSERVICE/QDBGSRV/.eclipse and restart the debug service.

**Question**: I am seeing a message “IBM i Debug Server has not been started yet. Please run the STRDBGSVR command from a user profile with enough authority.”.  
**Answer**: Just as what the message says, you need to run STRDBGSVR from a terminal session to start RDi Debug Server (QB5ROUTER) first. IBM i Debug Service requires a running RDi Debug Server. 

**Question**: I am seeing a message “Your debug server version is not up to date. IBM i Debug Service requires a host PTF update.”.  
**Answer**: IBM i Debug Service depends on the following RDi debug host PTFs for strong password encryption. You need to load one of the PTFs below or a superseded PTF.
- V7R3 PTF  SI82198 
- V7R4 PTF  SI82335 
- V7R5 PTF  SI82343

**Question**: How can I set a Service Entry Point on a procedure?  
**Answer**: Select the **Set Service Entry Point** action on the target program or service program. In the service entry point location prompt, change the last part of the entry field value “/*ALL/*ALL” to specify the module name and procedure name.

**Question**: What happens to the Service Entry Points after I disconnect from the current connection?  
**Answer**: The Service Entry Points are removed from the debug client after the connection is terminated. However, the SEPs are still saved in the running host debug service job. The SEPs will be restored in the client if you connect to the same host again.

**Question**: Service entry point without a condition works for me but conditional service entry point does not work. The program appears to be hung from a 5250 session when a conditional service entry point is hit.  
**Answer**: It may be an authority issue with your debug user profile. Your debug user profile need to have *USE authority to the profile that owns the QB5ROUTER job and also its job description. Suppose the owner of the QB5ROUTER job is USR1 and your debug user profile is USR2, and the job description of the USR1 profile is QGPL/USR1, you can use the following commands from a terminal session to grant additional authority to your debug user profile:  

    GRTOBJAUT OBJ(QGPL/USR1) OBJTYPE(*ALL) USER(USR2) AUT(*USE)  
    GRTOBJAUT OBJ(USR1) OBJTYPE(*USRPRF) USER(USR2) AUT(*USE)  

You need to restart the debug service and the debug router (QB5ROUTER) after these changes.

**Question**: I started the second debug session, but it does not come up.  
**Answer**: The debug client can support multiple debug sessions, but the IDE has a limitation that the new debug session does not automatically take focus, if you already have an existing debug session. You can select the new debug session from the drop down list of the Debug toolbar, or from the Call Stack view. The source of the new debug session will appear in a debug editor after the debug session is selected.

**Question**: I am seeing a **Password Request** dialog with message “The extension IBM: ibmidebug is requesting connection data.”    
**Answer**: When the **Service Entry Points** view is populated, it connects to the running debug service to retrieve the current set of SEPs. This operation requires the user password. If you select **Allow**, the **IBM i Developer** extension will allow the **IBM i Debug** extension to access the user password in its secret store.  

**Question**: What diagnostic information should I collect to report a debug problem?  
**Answer**: Please collect the following diagnostic information:
- The content of the DebugService_log.txt file under /QIBM/UserData/IBMIDEBUGSERVICE.
- The content of the debug service workspace log in /QIBM/UserData/IBMIDEBUGSERVICE/startDebugService_workspace/${User}/.metadata/.log, where ${USER} is the user profile that starts the debug service.
- Check the **Debug trace** option from the Debugger tab of the Connection Settings page. Save the trace text in the **Debug Console** to a file. Edit the file to remove the user password that appears under the trace statement for the launch request.

For information on using ARCAD tools, see [How to use ARCAD integration with IBM i Modernization Engine for Lifecycle Integration](https://supportcontent.ibm.com/support/pages/how-use-arcad-integration-ibm-i-modernization-engine-lifecycle-integration-merlin)


