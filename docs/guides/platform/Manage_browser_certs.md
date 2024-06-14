# Manage browser certificates

## Configure certificate for your browser
Modernization Engine for Lifecycle Integration (Merlin) supports Hyper Text Transfer Protocol over SecureSocket Layer (HTTPS) to protect the confidential data sent between client and server. Before logging into Merlin, you need to let the web browser trust the Merlin CA. This document describes how to trust the Merlin CA for Chrome and Firefox web browser.

### How to trust the Merlin CA for Chrome
When entering the Merlin web address with chrome for the first time, Chrome might show a blank page with the text `Loading...`.

As Chrome uses the system trust store to verify the CA, the way to add the CA to the system store between Windows and Mac are different. Both Mac and Windows users should follow the `Download Merlin CA` steps to obtain the Merlin CA, but follow different steps: `Add the CA to trust store with Windows system` or `Add the CA to trust store with Mac system` to trust CA.

1. Download the Merlin CA
    * Click the `Not Secure` button, and then select the Certificate is not valid item.
    * Click the `Certification Path` tab, then click the first CA, and click the `View Certificate` button. 
    * Click the `Details` tab in the CA certificate and then click the `Copy to File` button.
    * In the `Certificate Export Wizard`, click the `Next` button.
    * Select the DER encoded binary X.509(.CER) and then select `Next`.
    * Enter the path and name which you want to save the certificate. Then click `Next`.
    * Click the `Finish` button.

2. Add the CA to trust store with Windows system
    * If the client is a Windows system, open the folder where the certificate is saved. Double click the certificate.
    * In Certificate import Wizard, click the `Next` button.
    * Select ‘place all certificate in the following store’ and click `browse` button.
    * Select the `Trusted Root Certification Authorities` item, and then click the `ok` button. In the `Certificate Import Wizard`, click `Next` button.
    * Click the `Finish` button.
    * Select `Yes` in the security warning window.
    * Restart the Chrome browser. Enter the Merlin address and the Merlin GUI will load successfully.

3. Add the CA to trust store with Mac system
    * If client is a Mac system, after saved the certificate, paste it to the `Keychain Access` which is a Mac application. 
    * Double click the certificate 
    * Change all items to always trust. 
    * Restart the Chrome browser. Enter the Merlin address and the Merlin GUI will load successfully.  

### How to trust the Merlin CA for Firefox

Firefox uses its own trust store to verify the CA, so please follow the steps below to obtain the Merlin CA and trust it.

* Click `Not Secure` button and `Connection not Secure` item.
* Click `more info` item.
* Click `View Certificate` button.
* The Certificate will show in a new tab. Select the right side one. 
* Scoll down the page, and in the `Miscellaneous and Download` part click `PEM` to download the certificate.
* Click in the right top and click the settings item.
* In the `Settings` page, select `Privacy & Security`.
* Scoll down this page, select `View Certificate` in the Certificate part.
* In the `Certificate Manager`, in the `Authorities tab`, click the `import` button. 
* Select the download PEM file, click open to go on.
* Trust all and click `ok` button.
* Click `ok` to close the `Certificate Manager`.
* Restart Firefox. Enter the Merlin address and the Merlin GUI will load successfully.
