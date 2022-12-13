# Setup MERLIN from an IBM i application
This is a summary of the steps to push source members from source files into a Git repository.
More information available in documents available here: https://info.arcadsoftware.com/merlin-home

## Change type of RPGLE copies
The first thing to is to change the RPGLE copies type to have the new RPGLEINC value.

![image](https://user-images.githubusercontent.com/32166038/207340529-d3e6593e-d5ab-43c1-a4a5-ef001879fa30.png)

![image](https://user-images.githubusercontent.com/32166038/207339908-96d7e2e1-84f0-48af-99ee-5dee34048fdd.png)

## Declare an ARCAD application
The second step is to declare a new ARCAD application.
To do that, use the ADCLAPP command and fill the application ID (if your application ID is longer than three characters, fill also the library name prefix), add a brief description, the application Manager and the operational libraries (i.e. not only the libraries that contain the source members that will be pushed to Git but also the libraries that contain the objects and files of your application).

![image](https://user-images.githubusercontent.com/32166038/207340313-56b5791e-00b2-478b-b11d-b9b35a47ff8f.png)

## Generate Sources for SCM
Before loading the ARCAD repository, the third step is to generate a source for components that normally usually do not need it, such as *DTAARA, *MSGF, *SRVPGM, etc.
To do that, use the GENSCMSRC macro-command.
 
![image](https://user-images.githubusercontent.com/32166038/207344720-9f74f6b7-393b-4f8b-9bed-df7437547778.png)

Warning: for the parameter "Source file library" use one of the libraries indicated in the previous step.

## Load the ARCAD repository
To do that, use the DECLARE3 macro-command.

![image](https://user-images.githubusercontent.com/32166038/207347201-29c66464-64d5-4f86-b8eb-db9ceea55bcd.png)

## Create development environment
A development environment in mandatory to create a version in ARCAD.
To do that, use the ADCLENV command.

![image](https://user-images.githubusercontent.com/32166038/207347604-cefc3afa-d877-4c99-91fd-1f9d53f8d896.png)

## Create version types
Finally, you need to create a version type for your application.
To do that, use the AWRKVERTYP and create three version types (*RELEASE, *FEATURE and *SANDBOX) with the parameter Check-out from reference parameter set to *SCM and the parameter Transfer to reference to *NO execpt for *RELEASE (*YES in that case). 
 
![image](https://user-images.githubusercontent.com/32166038/207348145-ae4cc879-2cce-4fa0-9c47-b760ec45d154.png)

![image](https://user-images.githubusercontent.com/32166038/207349539-649fb8af-d402-4b63-bc22-dc07cf73036b.png)

## Create a remote Git repository
The next step of setup is to create a Git Repository.

![image](https://user-images.githubusercontent.com/32166038/207349997-1e69e0af-a7e7-4e9a-a55a-7ebfa0d759a1.png)
 
## Update application operational attributes
To do that, use the AEDTAPPATR, then
Set the Source member header update to *NONE.

![image](https://user-images.githubusercontent.com/32166038/207350243-5ba52c0d-8315-4be0-81f6-75fa44380721.png)

Set the Mandatory attachment of document/component to N (No) because we will not use the ARCAD maintenance report in this getting started.

![image](https://user-images.githubusercontent.com/32166038/207350457-e4bdf5c3-f208-48b1-9a22-44af683deeb9.png)
 
Set the Name of the External SCM product parameter to GIT and paste the SSH URL of your Git repository in the Repository URI parameter and press ENTER.

![image](https://user-images.githubusercontent.com/32166038/207350719-7c0f0015-d47f-49cc-a9fa-902efb4bf11f.png)

Your application is updated. Press F15=GIT config to finish the Git Configuration.
Set the MERLIN Project parameter to *YES. Set a Generation IFS directory and press F3 to go back to screen of the operational attributes.

![image](https://user-images.githubusercontent.com/32166038/207351277-5aba8f9c-f123-4188-8ec4-61fc9fb32de5.png)

## Loading the remote Git repository
To send all the source members to the Git repository use the ARCAD macro-command LODSCMREP.

![image](https://user-images.githubusercontent.com/32166038/207352838-a4403c41-86ad-4354-a356-dcc0735e33f3.png)
 
When the macro-command is finished, all the source members are visible in Git.

![image](https://user-images.githubusercontent.com/32166038/207353189-c766296d-cfbb-4823-8919-48bf1c7cbf34.png)
 
## Clone the Git repository in your workspace
Go to your IBM i Developer workspace to clone the Git repository

## Configure ARCAD builder
The last thing to do before using the IBM i Developer workspace is to configure ARCAD Builder for your application and add builder webhooks to your Git repository.
