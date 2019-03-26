# sd-wan-lte - Juniper SRX SD-WAN configuration with LTE backup

This repo can be used to generate Juniper SRX SD-WAN configuration. 
Platforms supported: SRX300, SRX320, SRX340, SRX345 and SRX550M

This repo includes 2 options;

- 1 WAN link with LTE as a backup option 
- 2 Active/Active WAN links (MPLS and Internet) with optional LTE as backup 

Different options are categorized using different Jinja2 template. Select the appropriate Jinja2 template for each of the above mentioned options.

##Option1: (sd-wan-lte.j2) - 1 WAN link with LTE backup

Following features are supported; 
  - All outbound traffic goes via 1 primary WAN link.
  - LTE is configured to be on backup, when the primary WAN links goes down or is not available. 
  - On the LTE backup connection, only critical applications (user defined) are prioritized and rest of the traffic is rate limited (user defined rate limit) 
  - LAN interfaces belong to 1 subnet (user defined) with an IRB interface serving as a DHCP server. 

![](/images/sd-wan-lte-topo.png)

### Pre-Requisite 

Please gather following information before executing the playbook:

  - DNS Servers
  - WAN interface - interface to which your ISP is connected
  - LAN interface(s) - list of interfaces to provide LAN connectivity [Note: SRX will act as a DHCP server]
  - DHCP Server info
  - MPIM Slot and SIM slot - please use the slots that you are connecting the mpim and SIM card to. 
  - Dial String - Dial string information info. If not know, leave it as-is
  - APN Name - get access point name from your SIM carrier 
  - List of Critical Application to provide business continiuty 

### Instructions:
Step 1: Clone this git repo:

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

	ansible-playbook sd-wan-lte-playbook.yml -e "template=template/sd-wan-lte.j2"

	Provides credentials to access SRX

### Notes:

Logs are dumped in ansible.logs file. Configuration template is present in template/ folder. This can be expaneded to configure SRX devices to do most type of configuration. 

### Additional Documentation:

Please vist https://www.juniper.net/documentation/en_US/junos/topics/task/configuration/lte-mpim-configuration-overview.html for additional documentation. 

##Option2: (sd-wan-2wan-lte.j2) - 1 WAN link with LTE backup

Following features are supported; 
  - 2 WAN links
    + 1 (MPLS or Internet)- WAN1
    + 2 (Internet) - WAN2
  - 1 LTE backup link 
  - IPSec configuration option available for WAN2 and LTE links
  - Pre-defined set of Applications: 
    + Office365
    + Saleforce 
    + Dropbox
    + Slack
    + Skype
    + ZoomMeeting
    + GotoMeeting
    + WebEx
    + Youtube 
    + [Additional apps can be added to the template sd-wan-2wan-lte.j2 as needed]

![](/images/sd-wan-lte-topo2.png)

### Pre-Requisite 
Please gather following information before executing the playbook:

  - DNS Servers
  - WAN 1 and 2 interfaces - both physical WAN links interfaces (ge-0/0/x)
  - Gateway IP for both WAN 1 and 2*
  - LAN interface(s) - list of interfaces to provide LAN connectivity [Note: SRX will act as a DHCP server]
  - DHCP Server info
  - MPIM Slot and SIM slot - please use the slots that you are connecting the mpim and SIM card to. 
  - Dial String - Dial string information info. If not know, leave it as-is
  - APN Name - get access point name from your SIM carrier 
  - IPSec PSK and peer IKE gateway info (if need ipsec on WAN2 and LTE links)

### Instructions:
Step 1: Clone this git repo:

  git clone https://github.com/rahulr-github/sd-wan-lte.git


Step 2: Add SRX hostname/IP to 'inventory' file:

  Edit 'inventory' file to add the SRX hostname and IP. Format as below:
  Format: <hostname>    ansible_host=<host IP>
  Example: srx-test1    ansible_host=10.10.10.10

Step 3: Copy sample variables file to <hostname>.yml 

  cp hostname_vars.yml <hostname>.yml 
  Example: cp hostname_vars.yml srx-test1.yml 

Step 4: Modify variables file to have the right parameters. 

Step 5: Execute using 'ansible-playbook'

  ansible-playbook sd-wan-lte-playbook.yml -e "template=template/sd-wan-2wan-lte.j2"

  Provides credentials to access SRX
