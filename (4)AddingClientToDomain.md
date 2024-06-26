# Creating a Windows 10 Pro client and Adding it to the Domain

The last thing done for this home lab was to create a Windows 10 Pro Virtual Machine and connect to the "myrootdomain.com" domain. 

## Virtual Machine Creation 

Ensure you have the Windows iso that will be used for the initial installation of the Windows 10 Pro virtual machine 

When creating a new VirtualBox virtual machine select the *New* option to begin
-  I named the device and ensured the *Type* was of **Microsoft Windows** and the version was **Windows 10 (64bit)**. 
-  For the **Hardware** tab I configured the device with 4 GB of RAM and 1 CPU, I chose the default options for all other configuration options. 
-  Once configured I selected *Settings* > selected the *Network* tab > and for **Adapter 1** I selected the option to Attach to an *Internal Network*, I then chose my internal network from which my server was already connected to
-  In the *General* tab in the **Advanced** section I chose to enable Bidirectional accessibility for both the *Shared Clipboard* and *Drag'n'Drop* options

Once you start up the Windows virtual machine, select **Install Now** and then select **I don't have a product key**
-  Within *Windows setup* I selected **Windows 10 pro**
-  I then selected **Setup for personal use** within the **How would you like to set up** page
-  On the *Let's add your account page* I chose the **Offline Account** option, as it doesn't require to setup a Microsoft Account > Then I chose **Limited Experience**
-  In the *Connect to a Network* part I chose the limited setup option

The first thing I did once the *Windows 10 Pro* device started up, was to check my adapter settings through the command line 
-  You can enter the **command-line-interface (CLI)** by typing it into the *Windows search menu* 
-  Once in the CLI, input the command **ipconfig**, you will then see the adapter settings of the client
  - The first thing I noticed was that the IP address of the client was the first IP address within the DHCP scope range of the Domain 

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/b28c6780-74e4-402f-9787-6e2ddd01d267)

I then renamed the client PC to more easily distinguish it and added it to the domain **myrootdomain.com**
-  Right-click the **Windows Menu** icon and select **Settings** > In the **About** page go to the **Related Settings** tab and scroll down and select **Advanced system settings**

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/a324688e-bb74-4d5b-a5e3-7c3ccec4a8fe)

-  In **System Properties** select the **Change** option that has the description *To rename the computer or change the domain or workgroup, click Change.
- Once the **Computer Name/Domain Changes**  page opened, I changed both the *Computer name* and selected the *Domain* category under **Member of** and input my domain name of *myrootdomain.com*
  
![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/eb631b85-f9a0-430f-a4b5-a1f057a1072c)

Once both have been configured, I selected the **OK** option to save my changes, which prompted a new window to enter credentials to add the client to the domain
-  I input the username and password of the user I created within the **Active Directory Users and Computers** app and who I added as a member of the **Domain Admins** group.

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/a44e89b5-01f6-4633-8110-04a3c2abcfb1)

-  I had then prompted to restart the virtual machine client for changes to take effect 
![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/43da314f-5393-4c8b-a5e3-d489ac6eafae)

- On the Login page again, I entered the credentials of another user I created within the domain 
![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/a726d862-e969-451f-9edc-107f41e38126)

Once I had logged in, I confirmed my device name had changed and that the client had joined the domain by going into the Window's *settings* and have gone into the **About** page 

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/c9f98516-482a-4539-9fd1-594b208cf87d)

Another way I confirmed the client joined the domain was by going into the command line interface and typing in **ipconfig** to check the results.

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/36c73dbe-0e17-49fa-9a01-90c269d4ced3)

Back in the Domain Controller, within the DHCP app, I had checked the **Address Leases** within my *IPv4 Scope* to confirm the first IP address within the scope was used by the Windows 10 client 

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/f04f8038-4975-4457-978c-0739399f932c)
