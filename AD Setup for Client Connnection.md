
# Setup Static Address for AD Server
1. In network setting for Ethernet connection, change IPv4 from dynamic to static
2. Assign IP, subnet, Gateway and DNS
	- IP: Any available IP address - `192.168.x.z`
		- `x` is the prefix for the VM assigned by host
		- `z` can be any value from `3-254`
		- `192.168.48.10`
	- Subnet: `255.255.255.0`
	- Gateway: Can left blank unless need to reach the internet or a VLAN
		- If so, it would be `192.168.x.1` for VMware or `192.168.x.2` for VirtualBox 
		- `192.168.48.2` or `192.168.48.1`
	- DNS: `172.0.0.1`
3. Ping `{domainname}.local` to see if connected
	1. If not, might need to flush and register DNS
		1. `ipconfig /flushdns` → `ipconfig \registerdns`
	2. `ipconfig` should also show the IP address, subnet mask and DNS set with gateway empty or set to the VM gateway assigned by the host to access the internet

# Connect Client/User Device to AD Server
1. In network setting for Ethernet connection, change IPv4 from dynamic to static
2. Assign IP, subnet, Gateway and DNS
	- IP: Any available IP address in the same range as AD server
		- `192.168.48.x` → `192.168.48.11`
	- Subnet: `255.255.255.0`
	- Gateway: Can left blank unless need to reach the internet or a VLAN
		- If so, it would be `192.168.x.1` for VMware or `192.168.x.2` for VirtualBox 
	- DNS: The same IPv4 address as the AD Server - `192.168.48.10`
3. Ping `jay.local` to see if client vm can communicate to AD server
4. If so, press `win+R` and type `sysdm.cpl` to open "System Properties"
5. On the Computer Name tab, click "Change"
6. Under Member of, select Domain and type `{domainnam}.local` - `jay.local` and press okay
7. Might ask for `{domainname}/Administrator` credential, if so, enter them
8. Enter and it should reset computer
9. In the sign-in screen, click on "Other"
10. Enter the User credential set in AD OUs
11. If successful, it should display the full user's name: "Welcome Patrick Star"
12. In terminal, typing `whoami` should print `{domainname}\{username}` - `jay\patricks`