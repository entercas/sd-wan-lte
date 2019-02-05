# sd-wan-lte - Juniper SRX SD-WAN configuration with LTE backup

This project can be used generate configuration and apply them on Juniper SRX devices. 
  This is the phase 1 of this project which inlcudes the following features;
  - All outbound traffic goes via 1 primary WAN link.
  - LTE is configured to be on backup, when the primary WAN links goes down or is not available. 
  - On the LTE backup connection, only critical applications (user defined) are prioritized and rest of the traffic is rate limited (user defined rate limit) 
  - LAN interfaces belong to 1 subnet (user defined) with an IRB interface serving as a DHCP server. 

![](/images/sd-wan-lte-topo.png)

## Pre-Requisite 

Please gather following information before executing the playbook:

  - DNS Servers
  - WAN interface - interface to which your ISP is connected
  - LAN interface(s) - list of interfaces to provide LAN connectivity [Note: SRX will act as a DHCP server]
  - DHCP Server info
  - MPIM Slot and SIM slot - please use the slots that you are connecting the mpim and SIM card to. 
  - Dial String - Dial string information info. If not know, leave it as-is
  - APN Name - get access point name from your SIM carrier 
  - List of Critical Application to provide business continiuty 

## Instructions:
Step 1: Clone this git repo.

  git clone https://github.com/rahulr-github/sd-wan-lte.git

Step 2: Add SRX hostname/IP to 'inventory' file:

	Edit 'inventory' file to add the SRX hostname and IP. Format as below:
	Format: <hostname>    ansible_host=<host IP>
	Example: srx-test1	  ansible_host=10.10.10.10

Step 3: Copy sample variables file to <hostname>.yml 

	cp hostname_vars.yml <hostname>.yml 
	Example: cp hostname_vars.yml srx-test1.yml 

Step 4: Modify variables file to have the right parameters. 

Step 5: Execute using 'ansible-playbook'

	ansible-playbook sd-wan-lte-playbook.yml

	Provides credentials to access SRX

## Notes:

Logs are dumped in ansible.logs file. Configuration template is present in template/ folder. This can be expaneded to configure SRX devices to do most type of configuration. 

## Additional Documentation:

Please vist techpubs.juniper.net for additional documentation. 



