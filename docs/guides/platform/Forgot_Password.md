# How to set up Email configurations and enable Forgot Password on Keycloak
This document simply describes how to set up the Email configurations and enable the forgot password function on Keycloak GUI.  
**Requirements:**  
- An available SMTP server that allows to relay mails
- Administration user  
 
Step 1, Go to the project which installed Merlin  
Go to the Openshift GUI, **Home** -> **Projects** -> click specific project  

<img src="../../images/guides/forgotp1.png" width = "800" height = "600" alt="Projects" align=center />  

Step 2, Get the credential of keycloak  
**Workloads** -> **Secrets** -> click **merlin-credential-secret** in the table  

<img src="../../images/guides/forgotp2.png" width = "800" height = "600" alt="Secrets" align=center />  

Get **ADMIN_USERNAME** and **ADMIN_PASSWORD**  

<img src="../../images/guides/forgotp3.png" width = "800" height = "600" alt="admin_password" align=center />    

Step 3, Find the route of keycloak  
**Networking** -> **Routes** -> Click the Location link in the keykloak item. It will open the Keycloak GUI.  

<img src="../../images/guides/forgotp4.png" width = "800" height = "600" alt="Route" align=center />  

Step 4, Login  
Click the **Administration Console**.  

<img src="../../images/guides/forgotp5.png" width = "800" height = "600" alt="Console" align=center />  

Login with **ADMIN_USERNAME** and **ADMIN_PASSWORD**  

<img src="../../images/guides/forgotp6.png" width = "800" height = "600" alt="login_1" align=center />  

Step 5, Set up Email  
**Realm Settings** -> Click **Email** tab -> enter the necessary info -> Click Save button.  

<img src="../../images/guides/forgotp7.png" width = "800" height = "600" alt="login_2" align=center />  

Step 6, Enable Forgot Password   
**Realm Settings** -> Click **Login** tab -> enable the Forgot Password -> click Save button.  

<img src="../../images/guides/forgotp8.png" width = "800" height = "600" alt="login_3" align=center />  

Step 7, Verify the Forgot Password was enabled  
Go to the Merlin login page -> click **Forgot Password**  

<img src="../../images/guides/forgotp9.png" width = "800" height = "600" alt="login_4" align=center />  

Enter the username or email address -> click Submit.  

<img src="../../images/guides/forgotp10.png" width = "800" height = "600" alt="username" align=center />

Note: please make sure this account has set up a valid email address in profile, or this account could not receive emails.  
Received a Reset Password Email. 

<img src="../../images/guides/forgotp11.png" width = "800" height = "600" alt="reset" align=center /> 

Step 8, Reset password  
Click the link to reset credentials in the Email -> enter the new password -> click submit, then the password will be reset.

<img src="../../images/guides/forgotp12.png" width = "800" height = "600" alt="submit" align=center />  
