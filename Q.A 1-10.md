---
sidebar_position: 1
title: AZ-800 Q/A (1-20) 
---

** Q1. How do you identify if a server is a PDC emulator? **

````yaml
A: Steps to take on domain controller:

1. Active directory users and computers
2. (Right Click) Domain 
3. (Click) Operations Master
4. (Click) PDC Tab

Alternatively: 

(cmd) netdom.exe query fmso
````

** Q2. On-Prem AD domain that syncs with Azure AD tenant. You want to implement SSPR. You need to ensure users who reset using SSPR can use new password resources in AD DS domain. 
What should you do? **
````yaml
A: Enable microsoft entra self-service password reset write-back:

E: Steps to take: 
1. Sign in to the Microsoft Entra admin center as Global Administrator.
2. Browse to Protection > Password reset, then choose On-premises integration.
3. Check the option for Write back passwords to your on-premises directory .
4. Check the option for Allow users to unlock accounts without resetting their password to Yes.
5. Save

````
** Q3. You have an Azure AD. You need to provide an admin with privilege to manage GPO's. What admin group should you add the admin into? **
````yaml
A: For Azure AD, it will be the AAD DC administrators group. 

E: Settings for users and computers inside Azure AD are managed by GPO's. Members of the Azure AD DC admins group can create custom GPO's and 
organisational units (OU's)
````
** Q4. Which actions should you perform in sequence to Deploy a new virtual machine and Azure AD to ensure the VM's can join the AD. **

````yaml
A: Steps to take: 
1. Inside a new Azure subscription 
2. Create your virtual machines 
3. Create a new virtual network 
4. Create an Azure AD instance 
5. Modify the settings of the azure virtual network

E: https://learn.microsoft.com/en-us/entra/identity/domain-services/tutorial-create-instance
````
** Q5. You have an Azure AD domain. You create a new user 'admin1'. You need them to deploy custom GPO's to all domain computers. Which solutions offer least privilege? **

````
A: Steps:
1. Add 'admin1' into the AAD DC administrators group (Entra Group). 
2. Instruct 'admin1' to apply custom settings via modification of Azure AD DC computers GPO.

E: https://learn.microsoft.com/en-us/entra/identity/domain-services/manage-group-policy 
````
** Q6. You have a single AD Domain forest. The fores contains a single AD site. You want to deploy a read-only domain controller to a new datacenter
on server 'server1'. User1 is a local admin on 'server1'. **
````yaml
Q. Recommend a deployment plan that: 
1. allows user1 to perform rodc installation on server1
2. ensures you can control AD DS replication schedule on server1
3. Ensures server1 is a new site named remote-site1
4. uses principle of least privilege

A: We need to first: 
1. Create a site and a subnet
2. Pre-create an RODC account 
3. instruct user1 to run AD DS installation on server1

E: We first need to create a site and subnet for the remote site. The new site will be added to the default IP site link. To configure the replication schedule, we use the new site 

When we pre-create an RODC account, we can specify who is allowed to attach the server to pre staged account. This means user1 doesnt need to be inside the domain admins group 

User1 can then connect the rodc to the pre-staged acc by running AD DS installation wizard. 

E: https://mehic.se/2018/01/02/how-to-install-and-configure-read-only-domain-controller-rodc-2016/

````
** Q7. Your network contains 20 domain controllers, 100 servers and 100 client computers. You have a GPO named GPO1 that has policy preferences and you plan to link it to the domain. **

Q. You need to ensure the preference in gp01 apply only to domain member server and NOT controllers or client PC's. Out of Domain, Operating System, Security group or Environment variable, which item level should you target?

A: Operating system. 

E: You can use item-level targeting to change the scope of individual preference items, so they apply only to selected users or computers. Within a single Group Policy object (GPO), 
you can include multiple preference items, each customized for selected users or computers and each targeted to apply settings only to the relevant users or computers.

An Operating System targeting item allows a preference item to be applied to 
computers or users only if the processing computer's operating system's product name, release, edition, or computer role matches those specified in the targeting item.

E: https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn789189(v=ws.11)#introduction

** Q8. You deploy a new AD DS forest. The domain contains three controllers called DC1,DC2,DC3. **

Q. You plan to move all domains to datacentres in different locations and need to configure replication to meet following requirements: 
1. Each domain controller must reside in its own AD site
2. Replication schedule between each site must be controlled independently 
3. Interruptions to replication must be minimised. 

What actions should you take?

A: Steps as follows: 
1. Create sites for all 3 domain controllers. i.e site1, site2, site3
2. create connection objects between dc1 - dc2 and dc1 - dc3
3. Remove any default-first-site-links if there was one previously. 

E: https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/creating-a-site-link-design

** Q9. You have an AD DS forest. The root domain contains domain controllers 1-5, each with a different fsmo. **

dc1 = domain naming master
dc2 = RID master
dc3 = PDC emulator 
dc4 = schema master 
dc5 = infrastructure master

A failure of which domain controller prevents you from creating application partitions?

A: dc1 = domain naming master

E: The domain naming master FSMO role holder is the DC responsible for making changes to the forest-wide domain name space of the directory, 
that is, the Partitions\Configuration naming context or LDAP://CN=Partitions, CN=Configuration, DC=domain. 
This DC is the only one that can add or remove a domain from the directory. It can also add or remove cross references to domains in external directories

E: https://learn.microsoft.com/en-us/troubleshoot/windows-server/identity/fsmo-roles

** Q10. You have an on-premise ad. it contains a user, computer, 1 domain local security group, 1 universal security group. You want to sync your AD with Azure AD by using AD Connect. **

Q. You need to ensure that all objects can be used in conditional access policies. What should you do?

A: Hybrid Azure AD join needs to be configured to enable Computer1 to be used in Conditional Access Policies. 
Synchronized users, universal groups and domain local groups can be used in Conditional Access Policies. 

E: https://learn.microsoft.com/en-us/azure/active-directory/conditional-access/howto-conditional-access-policy-compliant-device

** Q11. Your network contains a multi-site AD DS forest. Each AD site is connected by manually configured site links and auto generated connections. **

Q. You want to minimise the convergence time for changes to AD. What should you do? 

A: For each site link, you need to modify the options attribute. 

E: When you configure site link replication manually, the lowest you can get it to a cycle replication is 15min. If you want lower, you need to specifically modify 
the site link option attribute to use notifications. 

E: https://learn.microsoft.com/en-us/archive/blogs/canberrapfe/active-directory-replication-change-notification-you

** Q12. You have a single AD forest named twitter.com. You deploy 5 servers to domain. You add the servers to a groups name group1. **

Q. You plan to configure a Network Load Balancer named nlb1 that will contain the 5 servers. You also need to ensure nlb1 can use a gMSA to authenticate. How do you do so? 

A: In order: 
1. Add a kds root key on a domain-controller
````commandline
Add-KdsRootKey 
````
2. Add an AD service account on domain-controller 
````commandline
Add-ADServiceAccount
````
3. Install service account on server where gMSA account will be used
````commandline
Install-ADServiceAccount
````

E: Domain Controllers (DC) require a root key to begin generating gMSA passwords. 
The domain controllers will wait up to 10 hours from time of creation to allow all domain controllers to converge their AD replication before allowing the creation of a gMSA.

E1: https://learn.microsoft.com/en-us/windows-server/security/group-managed-service-accounts/create-the-key-distribution-services-kds-root-key

The Set-ADServiceAccount cmdlet modifies the properties of an Active Directory managed service account (MSA). 

````commandline
Set-ADServiceAccount [-Identity] ITFarm1 -PrincipalsAllowedToRetrieveManagedPassword Host1$,Host2$,Host3$
````
E2: https://learn.microsoft.com/en-us/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts

** Q13. You have an on-premise AD DS domain that synchs with an Azure AD tenant. You have several Win 10 devices that are Azure AD hybrid joined. **

Q. You need to ensure that when users sign in to the devices, they can use Win hello for business. Which opt feature should you select in Azure AD connect?

A: Device write-back is an optional feature in Azure AD Connect that allows the on-premises AD DS domain to receive information about the Azure AD joined devices,
including the device registration state. This feature is required for Win Hello to function correctly. 

E: The goal of Windows Hello for Business is to move organizations away from passwords by providing them a with strong credential that provides easy two-factor authentication.

E: * "Additional infrastructure needed for certificate-trust deployments includes a certificate registration authority. 
In a federated environment, you need to activate the Device Writeback option in Microsoft Entra Connect." * 

E: https://learn.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-planning-guide#device-registration

** Q14. Your network contains an AD DS forest named twitter.com. The forest also contains a child domain called uk.twitter.com. **

In the twitter.com domain, you create users admin1 and admin2. You need to ensure the users can perform: 

admin1 - create and manage ad sites 
admin2 - can deploy domain controllers to the uk.twitter.com domain 

what solution of least privilege should you employ. 

A: for admin1: Add to domain admins group for twitter.com. 
A: for admin2: Add to uk.twitter.com domain admins group

E. Adding admin1 or admin2 to enterprise groups of twitter.com domain would work but also be overkill. Principle of least privilege states that domain administrators
gives required permission. 

For admin2, the privilege only needs to extend to the the uk.twitter.com domain so again, this is the answer. 

** Q16. Network contains AD forest. This forest has three ad sites named site1, site2, and site3.  **

Q: Each site contains two domain controllers and the sites are connected using defaultipsitelink. 

If you open an new branch office with only client computers and want to ensure that the computers in the new office are primarily 
authenticated by domain controllers in site1, how would you achieve this. 

A: You could apply, the Try Next closest Site settings but make it difficult or more expensive,
for site two and three, so it defaults to site 1. 

https://learn.microsoft.com/en-us/windows-server/remote/remote-access/ras/multisite/configure/step-2-configure-the-multisite-infrastructure

** Q20. Network contains a single domain ad ds forest named contoso.com. The forest contains the servers dc1 and server1. **

You plan to install business apps on server1. The application will install a custom windows service. New policies however, state that all 
custom windows services must run under the context of a group managed service account. 

You deploy a root key. You need to create, configure, and install gMSA that will be used for the new app. 

What actions should you take? 

A: On dc1, run the ```  New-ADServiceAccount``` cmdlet and then run `` Install-ADServiceAccount`` cmdlet on server1