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

<h2>List of Prerequisites</h2>

- Microsoft Azure tenant and subscription
- Item 2
- Item 3
- Item 4
- Item 5

<h2>Installation Steps</h2>

<p>
1.) Create a resource group within Azure. This will serve as a container that holds the virtual machine that will be created in step 2. When naming the resource group, be sure to choose a name that will make the resource group easy to identify and locate within your Azure subscription. 
</p>
<p>
<img src="https://i.imgur.com/pALmWhj.png" height="65%" width="65%" alt="Resource Group Creation"/>
</p>
<p>--------------------------------------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
2.) Create a Windows virtual machine within the resource group created in step 1. This virtual machine will serve as the host machine that runs the osTicket software. Be sure to select the inbound port RDP 3389, as this port will be used to connect to and control the virtual machine. Also make note of both the username and password because they will be used to log into the virtual machine after it is created. Select "Review + create" to begin deploying the virtual machine.
</p>
<p>
<img src="https://i.imgur.com/ESu8MRo.png" height="65%" width="65%" alt="Virtual Machine Creation"/>
</p>
<p>--------------------------------------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
3.) After the virtual machine deployment is complete, copy the public IP address of the virtual machine. This can be found by navigating to your resource group -> your virtual machine -> properties (default), and it will be listed under "Networking". This public IP address will be used to Remote Desktop into your virtual machine.
</p>
<p>
<img src="https://i.imgur.com/DSWvhtp.png" height="50%" width="50%" alt="Copying The Public IP Address"/>
</p>
<p>--------------------------------------------------------------------------------------------------------------------------------------------------------------</p>
<br />

<p>
4.) Open Windows Remote Desktop. Enter the Public IP address from the virtual machine that was created. Another prompt will apear asking for the username and password of the virtual machine - use the username and password that were used when creating the virtual machine (step 2). If the Public IP address, username, and password are all correct, Windows Remote Desktop will create a session from which you can control the virtual machine.
</p>
<p>
<img src="https://i.imgur.com/fdCwwuL.png" height="40%" width="40%" alt="Remote Desktop Credentials"/>
</p>
<p>--------------------------------------------------------------------------------------------------------------------------------------------------------------</p>
<br />

