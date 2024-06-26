# Windows Server Setup

The very first step in this project is to create a virtual machine in VirtualBox that will contain Windows Server 2022
-  I downloaded the ISO from	https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022
    - Registration may be necessary
 
-  Within Virtual Box I selected the **New** option, created a name for the virtual machine, ensured the *Type* was Microsoft Windows, chose **Windows 2022 (64-bit)** for the *version*, and selected the **Skip unattended installation** option.
- For **Hardware** I chose **2** processors and **8 GB (8192 MB)** of memory
- I left the default options for all the other options and finally chose **Install Now**

After the virtual machine had been created, I enabled two network interface cards for the Windows server, so it could receive internet for the external NIC and connect to the domain for the internal one.
- To do this, I selected my Windows Server VM within *VirtualBox Manager* and then chose the **Settings** option, I then selected **Network** from the left-hand menu.

  - For *Adapter 1*, I confirmed it was attached to **NAT**, and for **Adapter 2**, I selected the option of enabling the network adapter. Afterward, I attached it to an **internal Network** and chose a name for the internal network 

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/29e23af0-1b7a-446a-849c-add0e7d498d9)


## Windows Server OS Configuration

- When Booting up the Windows Server, I selected the **Standard Evaluation (Desktop Experience)** as it provides a graphic user interface (GUI)
- Once the Windows Server OS is finished installing and configured, select the **Devices** option of the VirtualBox window > then select **Insert Guest Additions CD Image...**
- Once that is selected, within the Windows Server 2022 OS, head into File Explorer and select the drop-down menu for **This PC** > Then select the drop-down menu for **Local Disk (C:)**
- You will then see the *D Drive* for **VirtualBox Guest Additions**, select the *D Drive* and then execute the **VBoxWindowsAdditions-amd64**

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/6c2a75b5-552f-4b09-b097-505a0b39a01c)

  - By executing this application, it enables copy-and-paste and Drag-and-Drop features for a smoother experience between the host and virtual machine. 
  - Go through the installation wizard for Virtual Box Guest Additions with all default options, afterwards you will be prompted to restart the virtual machine for changes to take effect.
 
## Internal and External Adapter Configuration for Windows Server

Within The Windows Server OS, use the search bar to find and select the **Control Panel** > **Network and Internet** > **Network and Sharing Center** > **Change adapter settings** 
-  You will see two enabled ethernet adapters, to differentiate whether they are related to the internal and external network, right-click either one > select **Status**

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/a44c01d6-80f3-42e8-b65f-f9914e81cdfb)

  - The external adapter attached to NAT will have the *IPv4 Connectivity* status of **Internet**, while the status of the internal NIC will show **No network access**
    - To confirm the internal NIC has no internet connectivity, right-click the adapter > select **Details...** > select **Status**
    -  will see the Network Details show the internal IP address has an IPv4 APIPA address of **169.254...**
        - An **APIPA** address indicates the NIC failed to retrieve an IP address from the DHCP server
     
![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/a33adb3f-8f9e-46bd-b6c6-d887666335f1)
          
- Lastly, I renamed the two adapters to better differentiate between the two adapters when I set up the RAS/NAT feature later on.
  - Right-click on the adapter > Select "Rename"
 
  ## Internal Adapter Configuration

  I will update the Internal Adapter's TCP/IPv4 properties, as they are used during the configuration of the Domain Controller, DHCP server feature,

  - This is done by going into the internal adapter settings, like in the previous step
    - Control Panel > Network and Internet > Network and Sharing Center > Change adapter settings
    - Right-click on the internal adapter > Select **Properties** > select **Internet Protocol Version 4 (TCP/IPv4) and then select properties
      - I input the static IP configurations of (172.16.5.1) for the *IP address:*
        -  You can choose any Class A, Class B, or Class C IP address depending on whether you want to replicate an enterprise, medium, or small company respectively. 
      - The *subnet* will be **255.255.255.0**
      - I did not input a *Default Gateway* as the Windows server was set up as a DHCP server later on during the setup of the DHCP service using *Server Manager*. 
      - For the *Preferred DNS Server* setup as of the internal adapter, I chose the address of **127.0.0.1**
      - Ensure you click on **OK** afterwards to save your configurations.     

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/a313568b-a22e-4eb1-ac05-0e6e0fbf21d0)
