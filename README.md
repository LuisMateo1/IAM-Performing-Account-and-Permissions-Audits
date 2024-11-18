# Identity and Access Management: Performing Account and Permissions Audits
<h3>Objective</h3>

- Apply security solutions for infrastructure management.
#
**Scenario:**
515support is in the process of being audited. To comply with the audit process, it is necessary to create temporary user and computer accounts for the audit staff.
Each account exists as an object in the company’s Active Directory domain, where they are given certain attributes and permissions according to the requirements of the project.
Another technician has developed a script to provision the accounts automatically. You have been asked to execute the script and validate the results.

There are certain configurations that each member of the audit team must have for their Active Directory identity:
- The accounts must be placed in the AuditOU so that a privilege policy can be attached later.
- They must change their password at first login.
- Their password must expire at some point.
- They must not be allowed to access their account on weekends.
- They must be a part of the Domain Users and sec-glo-audit security groups, and no others.

Each member of the team is restricted to using only the assigned computer account:
- Anthony Stevens: audit-alpha
- Catherine Ruiz: audit-beta
- Douglas Price: audit-gamma
- Irene Taylor: audit-delta
- Luke Packard: audit-epsilon
#
<h3>Reviewing IAM Changes</h3>

- Just moments after executing the script, the technician who developed it contacts you to say that you’ve been sent the wrong version and that the script you’ve just run contains many serious identity and access management (IAM) policy deviations.
 While you could just examine the script more closely, you decide that you should audit the accounts manually and also validate the general state of accounts and permissions on the domain.

First I'll go to Tools > Active Directory Users and Computers, this allows me to view/interact with the corp.515support.com domain and its containers.

![image](https://github.com/user-attachments/assets/4a36b926-3fb9-4143-ac41-9a82500654a1)
![image](https://github.com/user-attachments/assets/76384a01-7c4f-4b07-8f48-dd3ea2c1fb58)

Clicking on the AuditOU container, we see that four of the five objects or accounts were created, and put into this container or Organizational Unit(OU) when I ran the script. However, these accounts are a network security risk because the script had policy deviations.

![image](https://github.com/user-attachments/assets/68d686d3-ca16-406e-a573-5273db8effec)

Since one of the accounts is missing, I'll search the domain for 'Anthony Stevens'. And it was found, which means the account was created just not added to the OU.

![image](https://github.com/user-attachments/assets/93565e9a-58a3-47ef-a2ed-5ac6c8843fb5)
![image](https://github.com/user-attachments/assets/e14a9d9f-43bf-4d1a-8d04-021e02e0e83d)

Since the account is created, I'll just move it to the AuditOU container. 
![image](https://github.com/user-attachments/assets/f0f54e71-712b-4119-98f8-76d98c2774ee)
![image](https://github.com/user-attachments/assets/125c8039-60c7-4270-be43-42eb3ab5356c)

After right-clicking > Refresh Anthony Steven's account is now in the AuditOU container.
![image](https://github.com/user-attachments/assets/995668db-d612-4b5b-9734-11f5cbed5c3d)

Now right-clicking > Properties on the 'anthony stevens' object, I'll be able to see more information on the configuration of the account.

![image](https://github.com/user-attachments/assets/7d0aab14-5bc5-45fb-9c56-3514e02ba6a5)

Clicking on the Accounts tab, and looking at the account options, its important for the 1st box is checked so that the user is forced to change the accoutns default password, and that the 3rd box is unchecked so that the password expires at some point.

![image](https://github.com/user-attachments/assets/6499b7da-43f5-4121-ae9e-b46ea72107cc)

Also, checking the logon hours, I'll make sure that the account is unaccessible on the weekends. And checking logon workstations, I'll make sure the user can only access the audit-alpha computer.

![image](https://github.com/user-attachments/assets/077d2a61-3ee2-4b5a-b9b9-a89b4e8e19d9)
![image](https://github.com/user-attachments/assets/bec071fd-790f-493a-960a-005c1861bd1b)

Now on the Member Of tab, I see that Anthony Stevens is a memeber of two groups Domain Admins and Domain Users, and it need to be in the sec-glo-audit group rather then then Domain Admins group. 
![image](https://github.com/user-attachments/assets/6f8d58cf-1358-4319-9efd-fc342c9aab06)

To fix the problem I went the the properties of the Domain Admins Group, and removed it from the Adminsitrators group. Then I added it to a new group called sec-glo-audit.

![image](https://github.com/user-attachments/assets/022ec3e5-def5-4cb3-88dd-6ef79fe48614)

#
<h3>Remediate IAM Account Issues</h3>

**Now I'll remediate the issues with all the other accounts/objects**
