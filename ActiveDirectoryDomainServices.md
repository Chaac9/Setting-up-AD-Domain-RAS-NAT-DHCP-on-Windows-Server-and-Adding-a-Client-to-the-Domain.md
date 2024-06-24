# Description of Roles and Features 

Active Directory Domain Services (AD DS): 

Domain Controller (DC) role:

Dynamic Host Configuration Protocol (DHCP) service:

Remote Access Service / Network Access Control (RAS / NAT):


## Installing Active Directory Domain Services (AD DS) on Windows Server
In this part, I installed AD DS to enable the Domain Controller role and create a domain from which clients could join.

My first step was to rename the Windows Server to "DC-01", to easily identify it and to ensure a smooth transition when enabling the domain controller role and the configuration of additional services.
-  Right-click the Windows 'Start menu' on the bottom left corner and select 'System' > The 'About' page should open, but if another page, such as 'Display' opens, then scroll down the left-hand 'System' tab and select the 'About' option > Within the 'Device Specifications' category, select the 'Rename this PC' option to rename the Windows Server

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/fe9f9e04-6183-4a87-9246-773413dbc92c)

I then enabled the **AD DS** feature by having gone into the **Server Manager** app. To open up the app you can use the Windows search bar and then type in *Server Manager*. 
-  Once in Server Manager, select the **ADD Roles and Features** option, and then the installation wizard should open.
-  I chose all default options, up until the **Server Selection** category, from which I saw my server named **DC-01** in the **Select a server from the server pool**  selection and continued my configuration.
-  In the **Server Roles** category I selected the **Active Directory Domain Services** option and continued with the default options for the rest of the selections until the *Install* option was finally given in the last category tab. 

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/13dda556-5bfe-4d99-99f6-b5df980ed674)

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/bf5291a7-8915-456f-990f-32ac42fd87b3)

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/2ced3068-bd43-4574-b46c-fb15f112ffb9)


## Promoting the Windows Server as a Domain Controller (DC)
Once *AD DS* completed installation, I was given the option to promote the server as a domain controller within the Server Manager app dashboard.
-  In the Server Manager dashboard select the flag icon with a warning symbol. You will see that the *AD DS* feature is successfully installed and the option to **Promote this server to a domain controller**, select this domain controller option.
-  Once the **Active Directory Domain Services Configuration Wizard** opened up, I chose the **Add a new forest** option and typed in a root domain name of **myrootdomain.com**, but you can type anything else, as long as it ends with a top-level domain of **.com** or **.local**.
    - A **.com** top-level domain indicates an external domain, while a **.local** can be used for an internal domain.
 -  In the **Domain Controller Options** tab, input a password for the **Directory Services Restore Mode** category.
 -  For all other tabs, I selected the default options and completed the installation.
 -  Once completed, the Windows server *DC-01* restarted and joined to the **myrootdomain.com** domain, which I had input as the name of my root domain. 

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/91a40355-06be-42bb-a0b3-73b38e1a9736)

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/29320a5f-5a4d-4049-b7e4-fdd19aebdf90)

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/e27b03b9-70bb-433f-bf37-0c6cec7d07bd)

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/a2993941-df20-448b-9b65-63aff76ce722)

## Creating a built-in Admin Account within the Domain using Active Directory Users and Computers (ADUC)

I created a built-in administrator account within **Active Directory Users and Computers** as an account with full privileges allows for easier manageability of the domain, as well as for disaster recovery planning should the primary admin account become inaccessible or be compromised. This new admin account had also been created for a smoother transition when adding services or clients to the domain, which would require authorized personnel credentials to proceed with due to access control restrictions. 

-  In the *Windows search bar* type in **Active Directory Users and Computers** and select the application
    - You could also open up the **Windows Search Menu** and selecting **Windows Administrative Tools** > Then selecting **ACtive Directory Users and Computers**
-    Once the *ADUC* application opens up, right-click the domain name and select **NEW** and then selecting **Organizational Unit**.
-    I named this new organizational unit *Administrators*

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/d5856400-7851-4a6b-b054-71cc44bfa206)

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/d0d42129-f3d7-4fd2-bbef-4cc88235b4b5)

The new admin account will be created within the newly created **Administrator* Organizational Unit (OU) 
-  Right-click the **Administrators** OU > Select **New** > Select **Users**
-  Input the information of the user that will be used to log onto the domain
    - For the *User logon name:* the naming convention I chose was to use the initial of the first name along with the last name.
-  Once the user is created, I made it a member of the **Domain Admins** group
    - Right-click the user > Select **Properties** > Select **Member Of** > Select **Add...** > Type **Domain Admins** > Select **Check Names**
    - Once **Domain Admins** is selected, select **Ok**, and once back in the User's properties select **apply** and then **ok**
 -  Once the new *domain admin* user is created, restart the computer, and then select the **Other user** option on the bottom left of the login page to log in as the new domain admin user
     -   When logging in only input the user part without the domain part
     -   For example: user@domainname.com
         - Local part: *User*
         - Domain part: *domainname.com*   

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/d2bcb357-04a3-4523-99e1-ef24e499da4b)

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/cd94cdb3-4108-44a6-bcea-dbe705e94375)

![image](https://github.com/Chaac9/Setting-up-AD-Domain-RAS-NAT-DHCP-on-Windows-Server-and-Adding-a-Client-to-the-Domain.md/assets/98796264/1c59c873-8dae-4ada-91ea-c313e201cbcb)


