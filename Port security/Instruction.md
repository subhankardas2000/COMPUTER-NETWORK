Introduction
A growing challenge for network administrators is to be able to control who is allowed - and who isn't - to access the organization's internal network. This access control is mandatory for critical infrastructure protection in your network. It is not on public parts of the network where guest users should be able to connect.

Port security is a feature implemented in Cisco Catalyst switches which helps network engineers in implementing network security on network boundaries.

In its most basic form, the Port Security feature remembers the MAC address of the device connected to the switch edge port and allows only that MAC address to be active on that port. If any other MAC address is detected on that port, port security feature shutdown the switch port.

The switch can be configured to send a SNMP trap to a network monitoring solution to alert that a port is disabled for security reasons.

Lab instructions
This lab will test your ability to configure port security on CiscoTM 2960 switch interfaces.

1. Configure port security on interface Fa 0/1 of the switch with the following settings :

- Port security enabled

- Mode : restrict

- Allowed mac addresses : 3

- Dynamic mac address learning.

 

2. Configure port security on interface Fa 0/2 of the switch with the following settings :

- Port security enabled

- Mode : shutdown

- Allowed mac addresses : 3

- Dynamic mac address learning.

 

3. Configure port security on interface Fa 0/3 of the switch with the following settings :

- Port security enabled

- Mode : protect

- Static mac address entry : 00E0.A3CE.3236

 

4. From LAPTOP 1 :

Try to ping 192.168.1.2 and 192.168.1.3. It should work.

Try to ping 192.168.1.4 and 192.168.1.5. It should work.

 

5. Connect ROGUE laptop to the hub.

Try to ping 192.168.1.1. It should work.

Try to ping 192.168.1.4. It should fail.


Interface FastEthernet 0/1 configuration - Restrict mode
The port-security restrict mode drops packets with unknown source addresses until you remove a sufficient number of secure MAC addresses to drop below the maximum value and causes the SecurityViolation counter to increment.

Port security with sticky MAC addresses provides many of the same benefits as port security with static MAC addresses, but sticky MAC addresses can be learned dynamically. Port security with sticky MAC addresses retains dynamically learned MAC addresses during a link-down condition.

interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 3
 switchport port-security mac-address sticky 
 switchport port-security violation restrict
 
 When the rogue laptop is connected to the hub and tries to communicate with 192.168.1.4, the number of mac-addresses learned ont the fastethernet 0/1 interface exceeds 3. The interface drops traffic with the new mac-address (not learned by the switch because 3 mac addresses have already been registered on the fa0/1 interface) and increases the security viloation counter based on the 'restrict' port-security configuration of the interface.

Switch#show port-security 
Secure Port MaxSecureAddr CurrentAddr SecurityViolation Security Action
               (Count)       (Count)        (Count)
--------------------------------------------------------------------
        Fa0/1        3          3                 5         Restrict
        Fa0/2        3          1                 0         Shutdown
        Fa0/3        1          1                 0          Protect
----------------------------------------------------------------------


Interface FastEthernet 0/2 configuration - Shutdown mode (default)
The port-security shutdown mode puts the interface into the error-disabled state immediately and sends an SNMP trap notification.

interface FastEthernet0/2
 switchport mode access
 switchport voice vlan 20
 switchport port-security
 switchport port-security maximum 3
 switchport port-security mac-address sticky 

Interface FastEthernet 0/3 configuration - Protect mode
The port-security protect mode silently drops packets with unknown source addresses until you remove a sufficient number of secure MAC addresses todrop below the maximum value. No counter is incremented

interface FastEthernet0/3
 switchport mode access
 switchport port-security
 switchport port-security violation protect
 
