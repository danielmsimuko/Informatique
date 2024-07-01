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