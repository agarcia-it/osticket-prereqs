<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket. It will be installed on a virtual machine running Windows 10 via Microsoft Azure.<br />

<h2>Environments and Technologies Used</h2>

- Windows 10
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Protocol
- Internet Information Services (IIS)
- HeidiSQL
- osTicket

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
3.) After the virtual machine deployment is complete, copy the public IP address of the virtual machine. This can be found by navigating to your resource group -> your virtual machine -> properties (default), and it will be listed under "Networking". This public IP address will be used to Remote Desktop into your virtual machine (your public IP address will be different than the one pictured).
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
5.) <strong>*From this step on we will be working within the virtual machine via Remote Desktop Protocol*</strong> First we must install Windows IIS (Internet Information Services) on our Azure virtual machine. IIS allows our vm to serve up websites, which is necessary because osTicket runs out of a web browser. We will also Install CGI which allows us to install PHP manager which is used to run osTicket. To do so, navigate to Control Panel -> Programs -> Turn Windows features on or off -> enable Internet Information Services (expand) -> (expand) World Wide Web Services -> (expand) Application Development Features -> enable CGI. -> click OK.
</p>
<p>
<img src="https://i.imgur.com/GioUyjV.png" height="40%" width="40%" alt="Enable IIS and CGI"/>
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
6.) In this step, we will configure Windows and install the necessary files in order to run osTicket. These can be found at the link in the "Installation Files" section of this repository. These substeps should be completed in the following order:
<br /><br />
a. Download and install the file named "PHPManagerForIIS_V1.5.0.msi" with default settings.
<br />
b. Download and install the file named "rewrite_amd64_en-US.msi" with default settings.
<br />
c. Create the directory C:\PHP.
<br />
d. Download the file named "php-7.3.8-nts-Win32-VC15-x86.zip". Unzip the contents in the newly created C:\PHP directory.
<br />
e. Download and install "VC_redist.x86.exe" with default settings.
<br />
f. Download and install "mysql-5.5.62-win32.msi" with default settings. Upon clicking "Finish", MySql will open a new configuration prompt where you will create a password for the MySQL username "root". Choose "Standard Configuration" and make sure to remember the password you create because it will be needed later.
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
9.) <br />
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
10.) This step will be used to create the credentials for osTicket. <br />
Open IIS as an administrator and navigate to: Sites -> Default Website -> osTicket. On the right side, click the link titled "Browse *:80 (http)". If everything was done correctly up to this point, the link will open the osTicket web interface. Click "Continue" on the osTicket web interface. Enter credentials up to the "Database Settings" portion (we will return to fill out the rest in a later step) and do not close the osTicket web interface. (note that you do not have to use a real email address for this step. Additionally, remember the username and password as they will be used to log into osTicket as an admin).
</p>
<img src="https://i.imgur.com/HvJ0ngb.png" height="60%" width="60%" alt="osTicket Credential Page">
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
11.) <br/><br />
a. From the Installation Files link, download and install the file called "HeidiSQL_12.3.0.65.89_Setup.exe" using default settings. Launch the program when the installation is complete. HeidiSQL acts as a database client, which connects to and interacts with osTicket.
<br /><br />
b. In HeidiSQL, click "New" on the bottom-left. This step will require the login credentials that we created in step 6f of this tutorial (username "root", and the password you created). Click "Open" after entering credentials.
<br /><br />
<img src="https://i.imgur.com/IvPbBZH.png" height="60%" width="60%" alt="HeidiSQL Setup 1">
<br /><br />
c. In the left side of HeidiSQL: right-click -> hover over "New" -> select "Database". Name the database "osTicket" and click "OK".
<br /><br />
<img src="https://i.imgur.com/5eOS2ak.png" height="60%" width="60%" alt="HeidiSQL Setup 2">
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
12.) Return to the osTicket web interface and continue filling out the credentials for the "Database Settings" section. "MySQL Database" will be called "osTicket" (the one we created in Heidi in the previous step), the username is "root", and password is the password that was used in step 6f. <br /><br />
<img src="https://i.imgur.com/pLs5got.png" height="60%" width="60%" alt="osTicket Database Settings">
<br /><br />
If everything was installed successfully, your screen should look like this: <br /><br />
<img src="https://i.imgur.com/zjfSyPv.png" height="60%" width="60%" alt="osTicket Successful Installation">
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
13.) Now there is some cleanup required before the osTicket installation and configuration is complete.
<br /><br />
a. In the Windows File Explorer, navigate to: C:\inetpub\wwwroot\osTicket\ and <strong>delete</strong> the "setup" folder.
<br /><br />
b. Now we will change the permissions in the "ost-config.php" file back to Read Only. Navigate to the C:\inetpub\wwwroot\osTicket\include\ directory and find the "ost-config.php" file. Right-click -> Properties -> Security -> Advanced -> highlight Everyone -> Click edit. Uncheck the "Full Control", "Modify", and "Write" boxes so that only "Read" and "Read & execute" are checked. Click OK -> Apply -> OK -> OK.
<br /><br />
<img src="https://i.imgur.com/2aYU8Gn.png" height="60%" width="60%" alt="Reset ost-config.php Permissions to Read Only">
</p>
<p>---------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
14.) Use this link <a href="http://localhost/osTicket/scp/login.php">http://localhost/osTicket/scp/login.php</a> to log into the osTicket web client using the "Admin User" login credentials that were created in step 9. If your login was successful, your sreen will look like this:
<br /><br />
<img src="https://i.imgur.com/F2CLDQS.png" height="60%" width="60%" alt="Successful osTicket Login">
<br /><br />
Congratulations on a successful installation, configuration, and login to osTicket!
