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
- They must change their password when first logging in.
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

**Before and after executing the script, the AuditOU container was created, and four objects were within.**
![image](https://github.com/user-attachments/assets/39aa3218-bdb8-4522-be53-9e4816d5ba81)
![image](https://github.com/user-attachments/assets/ecb55303-7177-4e05-8073-2131ad2484d8)

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

Clicking on the Accounts tab, and looking at the account options, it's important that the 1st box is checked so that the user is forced to change the account's default password, and that the 3rd box is unchecked so that the password expires at some point.

![image](https://github.com/user-attachments/assets/6499b7da-43f5-4121-ae9e-b46ea72107cc)

Also, checking the logon hours, I'll make sure that the account is unaccessible on the weekends. And checking logon workstations, I'll make sure the user can only access the audit-alpha computer.

![image](https://github.com/user-attachments/assets/077d2a61-3ee2-4b5a-b9b9-a89b4e8e19d9)
![image](https://github.com/user-attachments/assets/bec071fd-790f-493a-960a-005c1861bd1b)

Now on the Member Of tab, I see that Anthony Stevens is a member of two groups Domain Admins and Domain Users, and it needs to be in the sec-glo-audit group rather than the Domain Admins group. 

![image](https://github.com/user-attachments/assets/6f8d58cf-1358-4319-9efd-fc342c9aab06)

To fix the problem I removed the account from the Domain Admins security group and added it to the sec-glo-audit group.
![image](https://github.com/user-attachments/assets/7d2536fc-d596-4a09-b0cd-19ef05b06c00)

#
<h3>Remediate IAM Account Issues</h3>

**Now I'll remediate the issues with all the other accounts/objects within AuditOU**

**Catherine Ruiz:** The only problem with the configuration was that the 'change password on next logon' box was not selected. The logon hours were correctly set and the account is only able to log on to audit-beta as required. Lastly, the account i only part of two security groups, Domain Users and sec-glo-audit, which is correct.

![image](https://github.com/user-attachments/assets/4a0fd52d-4c37-4cf1-b0fc-ff89d013a07a)
![image](https://github.com/user-attachments/assets/a3398b8a-bda8-41d4-a631-a30e5a7b6853)

**Douglas Price:** The only problem with this account was the computer that it was supposed to be allowed to access. Instead of audit-gamma, it was allowed to log on to audit-delta. I removed audit-delta and added gamma.

![image](https://github.com/user-attachments/assets/68b115a6-33cb-43bd-8b4d-ded8a7383bde)
![image](https://github.com/user-attachments/assets/a833788c-24b8-4de9-b9a5-740b6ccba728)

**Irene Taylor:** The only problem with this account was the computer that it was supposed to be allowed to access. Instead of audit-delta, it was allowed to log on to audit-gamma. I removed audit-gamma and added delta.

![image](https://github.com/user-attachments/assets/9a7a448f-ffcc-405a-a83a-cae6834ef226)
![image](https://github.com/user-attachments/assets/0fa6cc21-f251-42eb-beab-19c66e239222)

**Luke Packard:** This account has two issues with its configuration:
- The logon hours were set to all days of the week, so I removed access on the weekends
- The account was allowed to access any computer on the network, instead of just audit-epsilon. So I just added audit-epsilon.

![image](https://github.com/user-attachments/assets/be583a68-1ade-4fed-aa89-0a31696d987b)
![image](https://github.com/user-attachments/assets/3f967b4c-a3be-4378-ae79-d35a8beb812a)

![image](https://github.com/user-attachments/assets/946aa147-9d5c-41b0-809b-10c1b01368b5)
![image](https://github.com/user-attachments/assets/6641c912-1dd9-43ca-a094-a1a8d6c0014b)
#
<h3>Analyze and Remediate Other IAM Issues</h3>

Now that the audit accounts have been properly configured, I'll make sure that the rest of the accounts on the domain are properly configured, following the IAM policies.

![image](https://github.com/user-attachments/assets/d3c317d7-197c-467a-a954-49a3a4e940dc)
![image](https://github.com/user-attachments/assets/b728f3ea-a91e-4fb3-ad33-6e7cf9316b05)
![image](https://github.com/user-attachments/assets/24b474bd-b4ba-4566-8c32-b9cfe8930eba)

The first thing I'll do is disable the guest account, as it's a security risk to have a guest account, that's accessible by anyone.

![image](https://github.com/user-attachments/assets/5ffded28-5b14-413a-8ec8-1edc1c1ac67e)
![image](https://github.com/user-attachments/assets/8f9d20f1-a0a6-454d-be31-f0afa0df78ed)

I also disabled the administrator account as any admins, like Bobby for example, should have their own account. 

![image](https://github.com/user-attachments/assets/c5ff2831-712a-460c-a104-f1689ddab683)
![image](https://github.com/user-attachments/assets/f1e9dc7c-d6cc-4077-8619-cfd0a75101bf)

There are three other accounts here: Bobby, Sam, and Viral. I've made sure that all of them, are not able to access their accounts on weekends and that passwords can expire as that is more secure.

![image](https://github.com/user-attachments/assets/cd2fcfe3-3013-4211-b750-50899ff46834)
#
**Summary: Identity and Access Management is the act of provisioning and moderating access to the domain, based on security policies and the principle of least privilege. In this case, five new accounts or objects were created incorrectly but incorrectly configured, and before the employees were able to be given access, I needed to administer the configuration changes based on the security policies.**

**[Part 2 - Identity and Access Management: Configuring and Analyzing Share Permissions](https://github.com/LuisMateo1/IAM-Configuring-and-Analyzing-Share-Permissions/)**

