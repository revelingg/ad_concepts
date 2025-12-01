

## The Goal: 
	The goal for this is to learn the basic concepts relating to AD ( users, group policys, software deployment, file sharing, and so on). To understand these concepts, they will be split into 5 parts and a mini project/task will be assigned in each segment. The first project will be building a company OU structure ( creating departments, and organizing users by department, create security groups and so on). Next, I will inact strict password policies and apply it at the domain level, then afterwards deploy software for the users on the network via the GPO. Similarly, I'll practice learning file structure working with inheritance and such by creating a shared folder for HR department in my company and setting rules regarding that. Lastly, I will create a helpdesk group with certain password controls and restrictions to be able to "help" users in the domain. 


## Phase 1:  Building a company OU structure 
	- Created the sample OU's 
		- HR
		- IT
		- Sales
		- Finance
		- Interns
	- Populated the OU with users enforcing proper naming convention, descriptions, and password schemantics 
	- Added user login rules ( Only allowed M-F 8-5pm)
	- Respective groups for each OU were then created with **Security** & **Global** as the types & the users added to it by department. 
	- Next, a new OU to contian the various groups was created. I created an OU called Groups with sub OU's for fileshare, printer and gpo permissions. 
	- Within Fileshare, i created domain local groups for hr read&write, hr read, IT read&write, and sales read
	- Lastly i added the global groups into the domain local to finalize the structure, ensuring to follow **AGDLP rules**
	- In the group policy management, I blocked inheritance on the IT department OU since they would need less strict rules. 

	### AGDLP: Best practice model when it comes to assinging permissions in AD. Goes by accounts, groups, domain locals, and them permissions. Through this chain, users are able to properly inherit permissions. It is managable and easily scalable, ensuring everything is standardized. 

<img width="922" height="538" alt="image" src="https://github.com/user-attachments/assets/f70240dd-7c1f-474b-8630-046521c68172" />


<img width="719" height="629" alt="image" src="https://github.com/user-attachments/assets/e71f58c3-9a9f-4747-892d-c8a7771a8acf" />

<img width="767" height="677" alt="image" src="https://github.com/user-attachments/assets/1a2d56e5-f9d7-4d25-a8b6-4fb88b8de7d1" />



## Phase 2: Group Policy security & hardening
	- First goal is going to be configruing password settings. This can be accessed throuh the settings tap on the general domain policy. After accessing the account policy I then configured the password policies & other security policies needed thatll then apply to everything in the domain. 
	- To configure specific password policies, you create **Fine grain  policies** accessed in the AD tools center. Through there, you select your domain, system then password container. At this point you should see the previous rules if any, and can then move forward to creating new rules
	- To make it stick to the specific user, make it only apply to the user or group needed. 
	- I then created a PSO for my interns enforcing strict password rules], along with a specifc user on my network. 
	- Next USB settings are configured to disable USB access to HR computers. This can be found in the GPM, edit the general domain policy, navigate to computer config, policies, admin templates, system and then removable storage
	- The next step is to configure login banners for users. A simple step located in the windows security policy section under local policies. Afterwards, a GPO is linked to the Users OU and RDP is disabled for them. 
	- Deploying files was sort of confusing. Firstly, the files have to be MSI files for the GPO to read it, secondly to configure where it gets deployed make specific GPO links in the policy manager, then on the domnain controlelr itself not only do you have to configre regular share permissions on the folder, you also have to configure NFTS share permissions and give it access to the proper chanels 
	- Then I configured firewall rules ( blocking RDP, powershell remoting and such) by blocking the specifc ports used for them ( tcp 3389,5985) and all the programs that would use said ports since other apps we need wouldnt be using those ports anyway
	- Lastly, I configured audit events to ensure that when users login/off, access the file storage via computer or a removable device and so on my network tracks it and a report is generated. 


<img width="466" height="540" alt="image" src="https://github.com/user-attachments/assets/795d0e63-4215-4f06-90ab-c07c818d58f2" />




## Phase 3: Creating helpdesk team & delegating permissions
	- The goal of this section would be to creat a team of helpdesk memebers and give them certain administravie priviledges to be able to do the jobs they need ie(reset passwords, unlock accounts, ensure least priviledge and so on)
	- I used the IT OU, but moved certain members in the OU into the HelpDesk group I created. Afterwards, I opened the control wizard and begun delegating permissions to the specifc group needed. 
	- Next, I installed and configured RAST so the workstations could have access to server tools in order to manage users. Subsequently, I verfied full functionality and made sure all the permissions I previosly set were in place and only accessible to the users group. 
	- After verification, my lab was concluded. 


<img width="769" height="579" alt="image" src="https://github.com/user-attachments/assets/45db6ae4-40cd-4d2f-8815-52f89fc906d2" />


	

## Things I've learnt:
	- Passwords restrictions cant fully be set in the OU part, they have ot be done in the security group the user is in 
	- Domain level passwords are the default for everyone, but u can use Fine-Grained password policies to do per user settings and so on 
	- Ou's are like folders for organizing the different roles and then contianig GPO's also, the GPO's are the users/role/department and domain local groups contain what roles they can access/configure permissions and such. 
	- To delete something configured with "prevent accidental deletion", enable advanced features, go to object tab in properties and then disable the protection to allow deletion. 
	- Blocking inhertiance allows them to have their own rules rather than just following server wide rules. These allow for testing new GPOs and segmenting stuff so othoer GPOs dont apply when not needed.
	- WMI filtering is for devices OS hardware etc and Security filtering is for specifc users and Loopback works for specifci devices 
	- Admin templates are things that arent part of windows "core" settings, so things like passwords or stuff built into windows is found in the windows tempaltes, but others such as hardware access USB and such for exmaple are found in admin templates. In short Windows settings contain only native built policies
	- Windows security templates is like who gets into what, and the Admin tempaltes are the rules once in. Admin templates are for admin accounts also control their actions on other devices
	- Computer/User. User is for user cases and such computer is per devices. 
	- Note: Windows will use the most restrictive of the 2 options. Always ensure they match when needed or have them properly configured so its still functional
	- Workstations need RAST to be able to access admin tools 


## Summary: 
	Throughout this lab, I developed a strong foundational understanding of core Active Directory concepts by breaking them into structured phases and completing hands-on tasks for each one. I learned how to properly design an organization’s structure using OUs, create and manage users and groups, and follow best-practice permission models like AGDLP. I then built on that foundation by applying real security controls through Group Policy, configuring password rules, blocking USB access, deploying software, enforcing login banners, restricting RDP, and creating firewall and audit policies to secure the environment. Finally, I practiced real administrative delegation by creating a Helpdesk team with limited privileges and verifying that they could perform only the tasks assigned to them. Altogether, this lab helped me understand how identity, security, and permissions interact in a Windows domain, and gave me hands-on experience with designing, hardening, and managing a functional Active Directory environment.
