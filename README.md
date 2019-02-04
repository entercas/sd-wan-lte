
# Juniper SRX SD-WAN configuration with LTE backup

This project can be used generate configuration and apply them on Juniper SRX devices. 
  This is the phase 1 of this project which inlcudes the following features;
  - All outbound traffic goes via 1 primary WAN link.
  - LTE is configured to be on backup, when the primary WAN links goes down or is not available. 
  - On the LTE backup connection, only critical applications (user defined) are prioritized and rest of the traffic is rate limited (user defined rate limit) 
  - LAN interfaces belong to 1 subnet (user defined) with an IRB interface serving as a DHCP server. 
  
