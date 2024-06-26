# Enabling Remote Access Server(RAS) / Network Address Translation (NAT), and Dynamic Host Configuration Protocol (DHCP)

## Enabling RAS / NAT

By enabling RAS / NAT, clients within the domain will be able to access the private virtual network and access the internet through the Domain Controler (Windows server)

-  In the Server Manager application on the domain controller go to **Add Roles and Features** > Select defaults for all tab categories until you reach the **Server Roles** tab and select **Remote Access**


 ![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/9447e9e5-af4d-4532-acb3-6a6d06b4ee3c)
  
-  I selected the *next* button and chose all default options for all other categories until the **Role Services** tab and chose **Routing** within that selection.
-  I chose defaults for the remaining tabs and began the installation
-  Once installation is completed, in *Server Manager* go to **Tools** > **Routing and Remoting Access** > right-click the server, mine was *DC-01 (local)* and select **Configure and Enable Routing And Remote Access**

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/171ef244-2be2-4a65-937e-9a5118740371)
  
-  Then, select the **NAT** option which allows internal clients to connect to the internet using a public IP address

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/fe59edb7-a58e-4e72-9cf5-42aad6251a7f)

-  On the **NAT Internet Connection** tab, select your internet configuration that connects to the outside network.
    - Should the **Use this public interface to connect to the Internet:**  option be disabled, then cancel the installation and reattempt to install the service again.
        - An example of the option being disabled is shown in the image below:
          ![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/5639c5b0-54e4-4e3f-9bb3-28abf175f2fa)

-  Within the **Routing and Remote Access** page, you should then see the server icon color change from red to green

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/24e88ea2-bf41-4150-b9d3-f77387cf6380)


## Enabling DHCP

The next step was to enable the DHCP service on the Windows server to allow clients to automatically receive an IP address to allow them to access and connect to the internet even when connected to the internal domain network.

- To setup DHCP go to the **Server Manager** app > Select **Add Roles and Features** > select defaults until in server roles and select **DHCP Server** > I selected all default options until prompted to begin the installation

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/77945a00-697e-4618-8aaf-f8111feb632f)

- Once DHCP has finished installation, on Server Manager go to > **Tools** > **DHCP**
- In the DHCP window select the drop-down icon for the server > The **IPv4** option should now be viewable > Right-click on **IPv4** and select **New Scope...**
- Once the *New scope wizard* is open, select next
  - For the name category, I input the DHCP lease scope (172.16.1.5-253) I would use for clients, excluding used and possible static IPs, and added a description.
    
![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/ba18bbc3-9d35-4bc0-976c-bfcb2286600e)
  - In the **IP Address Range** I input the starting IP and ending IP address to be used for my range of IP addresses to be leased for clients
    - For the subnet mask, I chose *24* as it allows for 254 clients, which is within the range of my scope of 249 clients.
      
![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/5950fddc-1967-46dc-87b8-3f0801c74447)
  -  On the **Add Exclusions and Delay** category I added no options, as my DHCP scope already excluded potential static IPs within the range of 172.16.1.1-4
    - This category is to exclude IP addresses that have the potential to be static within the DHCP scope
      
![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/b0c17a12-eb94-4396-a7d5-0a9126f2eb57)
  - For the **Lease Category** tab, the lease duration involves the amount of time a client can hold an IP before it can be used by another client
    - I configured the lease duration to hold an IP address to connect to the network for a day.
        - Shorter lease durations can be configured for open networks in which a client may connect only for a few minutes or hours, such as coffee shops or restaurants.
          
![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/1c19ea64-b2ca-4cba-a473-ae148cec7136)
-  In the **Configure DHCP Options** I selected *yes* to configure the DNS and gateway to provide internet access to clients
-  On the **Router (Default Gateway** tab, I added the IP address of the Windows server (172.16.1.1) and then selected the **Add** option.

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/fc4e5189-3703-4a4f-9c27-92593afe4aef)
-  On the **Domain Names and DNS Servers** tab, the domain should populate the area for the *Parent Domain:* selection, as long as it has already been configured
  - The Domain Controller IP address should automatically populate as well, but if not then clients will not be able to join the domain.
    
![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/ef4b0df0-b820-4d66-a981-1aa77909a63a)

- I did not configure a WINS server for the next tab
- Select the option to activate the scope and select *Next* afterward
- Select *Finish* to complete the DHCP configuration for the server         

***
Back in the *DHCP* page, you can see the IPv$ and IPv6 setup are still down (red)
-  To enable them, right-click the server acting as your DHCP server, mine being *dc-01.myrootdomain.com'

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/22ea7330-2e53-48e1-9020-a2e88c1ccec6)

-  Right-click the server > select **Authorize** > select **Refresh**

  ![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/1c0b0a52-b17a-481f-9f75-0428fd763e18)

-  Should see a green check mark afterward instead of the red downward arrow to indicate the DHCP server has been enabled 

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/390f61c0-85d8-4c46-a1ba-88abb8fecc99)


Within the **IPv4** drop-down menu in **Scope** > **Address Leases** you will see all clients who have a leased IP address     

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/28f5b68e-7106-4bbe-bec9-440a8e180949)
