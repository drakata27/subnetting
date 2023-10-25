# Topology Overview
This is an overview of the topology that is distributed across five networks using VLSM
![image](https://github.com/drakata27/subnetting/assets/108131465/2480ed95-086e-4d0a-a8f6-05dd85756e56)

### UK Subnet
<img src="https://github.com/drakata27/subnetting/assets/108131465/ecd38691-9a6c-4086-a810-dfc98f81405f" alt="UK subnet" width="700" height="500">

### India Subnet
<img src="https://github.com/drakata27/subnetting/assets/108131465/00164805-71ad-46fb-a648-09229494c9d6" alt="India subnet" width="750" height="500">

### US Subnet
<img src="https://github.com/drakata27/subnetting/assets/108131465/2bda25a2-031e-4332-9e62-c082629e62c7" alt="US subnet" width="700" height="500">

## Assigning IP Addresses to Router Interfaces via CLI
To configure the IP addresses for the routers’ interfaces we need to use the command line interface. 
The CLI is used by connecting the router to a PC. In this case the routers are connected via console cable to a PC. The following 
are steps on how this is done and as an example is used the UK router and GigabitEthernet0/0 interface.

```
UK>en
UK#conf
UK#configure terminal
UK(config)#interface gigabitEthernet 0/0
UK(config-if)#ip address 172.16.0.1 255.255.254.0
UK(config-if)#no shutdown
```

## Assigning VLANs to Switch Ports via CLI
VLANs were created on each switch for the departments IT, HR, and Finance.

### Creating the VLANs
```
Switch>en
Switch#configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name IT 
Switch(config-vlan)# vlan 20
Switch(config-vlan)# name HR 
Switch(config-vlan) # vlan 30
Switch(config-vlan) # name Finance 
```

### Assigning VLANs to Ports
Example for VLAN 10
```
Switch>en
Switch#configure terminal
Switch(config)# interface fastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```

## OSPF Configuration
The communication between the different offices is possible thanks to the routing protocol that was used. 
I have decided to use OSPF since RIP does not support VLSM. 
Additionally, OSPF provide other advantages over RIP such as having no hop limit and enabling 
the calculation of routes by routers based on incoming requests. 

#### Example for UK network
While in global configuration mode, type “router ospf ” followed by the process ID which is 1 in this case.
After that type “network” followed by the network address of the network you want to connect to,
the wild card of the network, and area ID, which is 0 in this case. This example shows the UK router, 
so it is connected to the India router via the network 172.16.3.128,
and the UK subnet via the network 172.16.0.0.
```
UK>en
UK#conf
UK#configure terminal
UK(config)#router ospf 1
UK(config-router)#network 172.16.3.128 0.0.0.3 area 0
UK(config-router)#network 172.16.0.0 0.0.1.255 area 0
```

#### Verify OSPF setup
```
UK#show ip ospf neighbor
```

## Best Practices
### Banner
A good practice is to set up a banner that will display a warning when someone tries to 
log in unauthorised on the router. A banner can be set by the command “banner motd” 
followed by the warning message. Below is an example of a banner in the UK router. 
Next time someone tries to use the router, the message will display.

```
UK>en
UK# conf t
UK(config)#banner motd "Authorised Access Only"
```
### Hostname
All the routers in this topology have been given appropriate names so it can be easy to distinguish between them.
A hostname is given to a router by going to the terminal in the configuration mode and typing “hostname”
followed by the name you want to give. Below is an example of the UK router.

```
Router1#conf t
Router1(config)#hostname UK
```

### Users
Another good practice is to create local user accounts on the routers.
This will keep an extra layer of security to the network. Account can be created by going to the configuration mode and typing “username” followed by a username and “secret” followed by a password. 
Next time you want to use the router you will be asked for a password.

```
UK(config)#username userUK secret @password_Example@
UK(config)#exit
```


