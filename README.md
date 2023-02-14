<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket. It will be installed on a virtual machine running Windows 10 via Microsoft Azure.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>Installation Files</h2>
- https://drive.google.com/drive/u/1/folders/1APMfNyfNzcxZC6EzdaNfdZsUwxWYChf6

<h2>List of Prerequisites</h2>

- Internet connection
- Microsoft Azure tenant and subscription
- Microsoft Remote Desktop
- Ability to download, install, and rename files & directories on a Windows machine

<h2>Installation Steps</h2>

<p>
1.) Create a resource group within Azure. This will serve as a container that holds the virtual machine that will be created in step 2. When naming the resource group, be sure to choose a name that will make the resource group easy to identify and locate within your Azure subscription. 
</p>
<p>
<img src="https://i.imgur.com/pALmWhj.png" height="65%" width="65%" alt="Resource Group Creation"/>
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
2.) Create a Windows virtual machine within the resource group created in step 1. This virtual machine will serve as the host machine that runs the osTicket software. Be sure to select the inbound port RDP 3389, as this port will be used to connect to and control the virtual machine. Also make note of both the username and password because they will be used to log into the virtual machine after it is created. Select "Review + create" to begin deploying the virtual machine.
</p>
<p>
<img src="https://i.imgur.com/ESu8MRo.png" height="65%" width="65%" alt="Virtual Machine Creation"/>
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
3.) After the virtual machine deployment is complete, copy the public IP address of the virtual machine. This can be found by navigating to your resource group -> your virtual machine -> properties (default), and it will be listed under "Networking". This public IP address will be used to Remote Desktop into your virtual machine.
</p>
<p>
<img src="https://i.imgur.com/DSWvhtp.png" height="50%" width="50%" alt="Copying The Public IP Address"/>
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
4.) Open Windows Remote Desktop. Enter the Public IP address from the virtual machine that was created. Another prompt will apear asking for the username and password of the virtual machine - use the username and password that were used when creating the virtual machine (step 2). If the Public IP address, username, and password are all correct, Windows Remote Desktop will create a session from which you can control the virtual machine.
</p>
<p>
<img src="https://i.imgur.com/fdCwwuL.png" height="40%" width="40%" alt="Remote Desktop Credentials"/>
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
5.) *From this step on we will be working within the virtual machine via Remote Desktop Protocol* First we must install Windows IIS (Internet Information Services) on our Azure virtual machine. IIS allows our vm to serve up websites, which is necessary because osTicket runs out of a web browser. We will also Install CGI which allows us to install PHP manager which is used to run osTicket. To do so, navigate to Control Panel -> Programs -> Turn Windows features on or off -> enable Internet Information Services (expand) -> (expand) World Wide Web Services -> (expand) Application Development Features -> enable CGI. -> click OK.
</p>
<p>
<img src="https://i.imgur.com/GioUyjV.png" height="40%" width="40%" alt="Enable IIS and CGI"/>
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
6.) In this step, we will configure Windows and install the necessary files in order to run osTicket. These can be found at the link in the "Installation Files" section of this repository. These substeps should be completed in the following order:
<br /><br />
a. Ddownload and install the file named "PHPManagerForIIS_V1.5.0.msi" with default settings.
<br />
b. Download and install the file named "rewrite_amd64_en-US.msi" with default settings.
<br />
c. Create the directory C:\PHP.
<br />
d. Download the file named "php-7.3.8-nts-Win32-VC15-x86.zip". Unzip the contents in the newly created C:\PHP directory.
<br />
e. Download and install "VC_redist.x86.exe" with default settings.
<br />
f. Download and install "mysql-5.5.62-win32.msi" with default settings. Upon clicking "Finish", MySql will open a new configuration prompt where you will create a password for the MySql root. Choose "Standar Configuration" and make sure to remember the password you choose for future use.
<br />
g. Open Internet Information Services (IIS) as an administrator. Double-click "PHP Manager" and select "Register new PHP version". A prompt will appear asking you to select the path to the executable file "php-cgi.exe". It should be found in under the C:\PHP directory that was created earlier. Select the "php-cgi.exe" file and press OK. Return back to the IIS main page and click "restart" to restart the web server.
<br /><br />
<img src="https://i.imgur.com/sIdtEUw.png" height="80%" width="80%" alt="Register New PHP Version"/>
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
7.) From the Installation Files link, download the file named "osTicket-v1.15.8.zip". Double-click the "osTicket-v1.12.8.zip" folder. Within that folder there will be two folders: "scripts" and "upload". Drag the "upload" folder into C:\inetpub\wwwroot. Rename "upload" to "osTicket". Open IIS and click "restart".
</p>
<p>
<img src="https://i.imgur.com/UBPTwjV.png" height="60%" width="60%" alt="Rename upload To osTicket"/>
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
8.) Return to IIS and navigate to: Sites -> Default -> osTicket. Double-click PHP manager and under "PHP Extensions" click on "Enable or disable an extension". From there, enable: "php_imap.dll", "php_intl.dll", and "php_opcache.dll". Refresh the osTicket site once again by clicking "restart" as in step 7.
</p>
<p>
<img src="https://i.imgur.com/syqwTIJ.png" height="60%" width="60%" alt="Enable PHP Extensions"/>
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
8.) <br /><br />
a. In File Explorer, navigate to C:\inetpub\wwwroot\osTicket\include\. Find the file named "ost-sampleconfig.php" and rename it to 
<br />"ost-config.php". 
</p>
<p>
<img src="https://i.imgur.com/h2QtKpc.png" height="60%" width="60%" alt="Change ost-sampleconfig.php File Name to ost-config.php"/>
</p>
<br />
b. Now we will set permissions on the file so that anyone using osTicket on this machine can make changes: Right-click the newly created "ost-config.php" -> Properties -> Security -> Advanced -> Disable Inheritance -> Remove all inherited permissions from this object -> Add -> Select a Principal -> type "everyone" in the text field and click "Check Names" (everyone should now be underlined) -> OK -> Full Control -> OK -> Apply -> OK.
<br /><br />
<p>
<img src="https://i.imgur.com/O2mJG59.png" height="60%" width="60%" alt="Permissions For ost-config.php"/>
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
9.) This step will be used to create the credentials for osTicket. <br />
Open IIS as an administrator and navigate to: Sites -> Default Website -> osTicket. On the right side, click the linke titled "Browse *:80 (http)". If everything has been done correctly up to this point, the link will open the osTicket web interface. Click "Continue" on the osTicket web interface. Enter credentials up to the "Database Settings" portion. (note that you do not have to use any real credentials for this step. However, it would be helpful to store this information in case it is needed at any point).
</p>
<img src="https://i.imgur.com/HvJ0ngb.png" height="60%" width="60%" alt="osTicket Credential Page">
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
10.) 
