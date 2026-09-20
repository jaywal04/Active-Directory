Active Directory (AD) is a centralized identity and directory service for Windows environments. It manages who can access the network (authentication) and what they are permitted to do (authorization).

> [!info] Terminology
> Full glossary of every AD term used below: [[AD Terms]].

They key components are:
1. Domain Controller (DC): the Window Server that host AD database, process authentication requests and run directory services
2. Objects: users, security groups, device, shared folders, etc.
	1. Attributes: each object has a attribute describing their characteristics (name, description, email, etc.)
3. Organizational Units (OUs): Folder in domain to organize objects
4. Domain: logical boundary of computers and users sharing a common database and security policy


## VMware Workstation Pro Setup (Avoiding Product Key Requirement on Trial Image):
1. Chose Typical wizard setup
2. Select last option: "I will install the OS later"
3. Pick a location to install and edit the Server
4. 20GB disk size is probably enough
	1. Pick "Store virtual disk as single file" for easier management
5. Before starting the machine, in settings for the machine:
	1. Go to CD/DVD (SATA), select the ISO image file that was downloaded
6. Start the server
	1. Spam keys to initialize

## Initial Server OS Setup
1. Select "Window Server 2022 Standard Evaluation (Desktop Experience)" for GUI instead of CML
2. Select "Custom: Install..."
3. Press install to finish setup
4. Once finishing install, it should ask for password for the system under the temp username  "Administrator"
5. Once added, enter password in startup screen to enter Server GUI

## Setup Active Directory
1. In "Server Manage", on top right corner, click on Manage -> "Add Roles and Features"
2. Follow these sequence for each sections:
	1. Before You Begin: Click on next
	2. Installation Type: "Role-base or feature installation"
	3. Server Selection: click next
	4. Server Roles: enable "Active Directory Domain Services" (AD DS) for now
	5. Features: "Group Policy Management" should already enabled if AD DS was checked. If so, click on next
	6. AD DS: click on next
	7. Confirmation: Click on install
	8. Once install, in Results, click on "Promote this Server to a domain controller"
		1. In the new window, select "Add new forest": A **forest** is a container that contain multiple domains namespaces **trees** that trust each other.
			1. Name the domain and add `.local` at the end and press next
				1. Ex. `domain.local`
			2. Domain Controller Option: keep the default options
				1. Create the **DSRM password** (Directory Services Restore Mode)
					1. This is a break-glass recovery password for booting the DC into repair mode. It is **not** a domain-wide password and is not used for normal logons.
				2. Press Next
			3. Domain Options: Click next
			4. Additional Options: The NetBIOS name should match the <u>domainname</u>.local you picked at the beginning in caps. If so, click Next
			5. Paths: Click next
			6. Review Option: click next
			7. Prerequisites Check: Click on install once the button is available. 
			8. Once installed, will prompt to restart

## Navigating Active Directories
Once AD installed, navigate to "Active Directory Users and Computers"

### Creating OUs (College User Types)
- Right click on `domainname`.local -> New ->  Organizational Units -> Name the OUs
	1. Students, Guests, Faculty & Staff and Administrators 
	2. You add sub OUs within main OUs
		1. For Students OUs they could be based on any categories
			1. Ex: Undergraduate, Graduate, Ph.D, etc

### Adding Groups in OUs
There are two different groups types (Security vs Distribution) and three types of Groups scopes.

#### Group Scopes
1. Universal: Can contain members from any domain in the forest, and can be granted permission to resources anywhere in the forest
2. Global: Can only contain members from the same domain it was created in, but can be granted permission to resources in any domain in the forest
	1. Any OUs assigned as global can be access anywhere in `domainname`.local
3. Domain Local: Can contain members from any domain in the forest, but can only be granted permission to resources within its own domain

#### Group Types
1. Security: Used to assign permission to shared resources based on level of access and users - allow IT Staff in Administrator OU to gain file/folder access from Faculty and Staff OU.
	1. Can be built in or custom
2. Distribution: Only used for email communication (ie. Exchange mailing lists). Don't give access/permission
	1. Is a list (DL) of different user's email within a OUs
	2. <u>Example:</u> Instead of typing 5,000 separate email addresses to Undergraduate students message, Just  send one email to `marketing@yourcompany.com` and the server delivers that email to all 5,000 inboxes.  
	3. The email list could be department based, role-based, or everyone

#### How to Add Users to OUs
1. Right click on a OU
2. In the popup click on New → Users
3. Enter user details
4. Press Next once done
5. Enter a password for the user and set the account flags (must change at next logon, password never expires, etc.)
	1. Password policy is **not** set per user here. Complexity, length and age rules come from the **Default Domain Policy** and apply to the whole domain.
	2. To give a specific group different rules, use a **Fine-Grained Password Policy (PSO)** in ADUC's Password Settings Container.
6. Press Next → Finish
7. Normally, this is done via a script to a automation tool instead of manual input.

#### How to Create Groups and Adding Objects to a Group
Users (object) in a group allows easier to assign resources and permission instead of using one-by-one

>**Creating a Group**
1. Right on a OU
2. Click New → Group
3. The the Group, pick a scope and type and okay to save
---
> **Populating Group**
1. To add objects in a Group, in a OU with objects, select multiple users
2. Right click → click on "Add to a group"
3. Simply enter the Group name that already exist to be added into

#### Object Properties
![[Windows Active Directives 2016-1789835287645.webp|250x329]]
> **Access Object Properties**
1. Right click on a object
2. And select Properties

In the user object shown on the left, there are different options/tab to adjust the user's attributes, system behavior, groups they're in, etc.

>**Brief Summary of Each Tabs**

| Tab                             | What it does                                                                                                                                                                            |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| General                         | Holds the user's basic identity details — first/last name, display name, description, office, phone, e-mail and web page.                                                               |
| Address                         | Stores the user's physical mailing address (street, city, state, ZIP, country).                                                                                                         |
| Account                         | Controls the logon name (UPN and pre-Windows 2000), logon hours, allowed workstations, account options (must change password, never expires, disabled) and the account expiration date. |
| Profile                         | Sets the roaming profile path, logon script, and home folder / mapped home drive for the user.                                                                                          |
| Telephones                      | Records the user's additional contact numbers (home, pager, mobile, fax, IP phone) plus free-text notes.                                                                                |
| Organization                    | Records the user's job title, department, company, manager and direct reports for the org chart.                                                                                        |
| Member Of                       | Lists every group the user belongs to and is where you add or remove group memberships and set the primary group.                                                                       |
| Dial-in                         | Sets remote access (VPN / dial-up) permission, callback options and static IP/route assignment for the user.                                                                            |
| Environment                     | Configures the startup program and client device connections (drives, printers) for a Remote Desktop session.                                                                           |
| Sessions                        | Sets Remote Desktop session timeouts and what happens on disconnect or when the connection is broken.                                                                                   |
| Remote control                  | Controls whether admins may shadow (view or interact with) the user's Remote Desktop session and whether the user must consent.                                                         |
| Remote Desktop Services Profile | Defines the separate RDS profile path, home folder and whether the user is allowed to log on to Remote Desktop Session Hosts at all.                                                    |
| COM+                            | Assigns the user to a COM+ partition set for application-component access.                                                                                                              |



### NTDS Database
The Active Directory database is stored in `ntds.dit` located in `C:\Windows\NTDSC:\Windows\NTDSC:\Windows\NTDS`

Other files are log and check files. Used to track actions performed in Active Directory. Stored in memory first -> log/check file

##### Access Domain NTDS
1. In Server Manage, click on Tools → "Active Directory Sites and Service"
2. In "Default-First-Site-Name", navigate to "NTDS Settings" path
	1. There should be nothing in there

##### Creating a New Site: Inter-Site
1. Right click on "Site" folder
2. Name the site and pick the default Link Name → Okay
> In the new site, you can copy and past server in one site to another to provide redundancy


### Access Domain and Trust
1. In Server Manage, click on Tools → "Active Directory Domains and Trusts"
2. There should be only on domain that you created earlier when setting up AD. 
> In there, can you create child domains and relationships how other domain can communicate with each other

### Hidden Object
1. To show hidden objects in AD Users and Computer, click on View → Advance Features

### Enabling Recycle Bin
1. In Server Manage → click on Tools
2. Select "Active Directory Administrative Center"
3. Click in to local domain `domainname (local)`
4. On the right hand side under `domainname (local)`, click on "Enable Recycle Bin..."
5. For any deleted objects, it will be added in here so you can restore objects
6. 