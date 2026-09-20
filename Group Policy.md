
# Registry Editor
You can use Registry Editor to edit/add Group Policy but it's complicated and can lead to mistakes. 

**Basic Info:** 
All user settings is stored in `HKEY_CURRENT_USER` in Registry Editor
- `HKEY_LOCAL_MACHINE` is the computer settings
- `HKEY_USERS` contain all the users that logged on
	- If the folder end with 500, it was the administrator
	![[Group Policy-1789867550555.webp|503]]

# Group Policy (GP) Interface Basics
In Server Manager, under Tools, click on Group Policy Management
1. In Forest:{domain}.local\Domains\{domain}.local\Default Domain Policy, this affect every users and devices on the domain. 

## Creating GP Settings
1. In the same domain path ^, right click on a OU and select "Create a GPO..."
2. Once created, the OU should have a GP assigned.
3. To edit the GP, right click on the created GP and click **Edit** which a popup window should open
4. In the popup window, you can edit users and computers GP for that OU
	1. Policy: Hard-coded policy
	2. Preference: Optional policy where users can change
	![[Group Policy-1789867701877.webp|457]]
	- Policy include add pre-installed software, disable Desktop application, add a automation script when user signin, and so much more. 
5. Any changes made in the GP settings for the users in a OU does not automatically enforce the policy if features were enabled/disabled in the settings. 
	1. To enforce the changes, in cml, type `gpupdate \force`
		1. Make sure the OU enforcement is also set as Enforced shown in "{OU Name} GP Policy" when you click on the created GP
			![[Group Policy-1789867484403.webp|327x231]]
	2. The user must log off and signin for the changed policies to take effect


6. To enforce a policy set for a domain and apply to all OUs, right click on the Default Domain Policy and enable "Enforce" so that any password policy, account, scripts, and so on will be enforced for all OUs to all users and machines. 
	![[Group Policy-1789868551732.webp|301]]

