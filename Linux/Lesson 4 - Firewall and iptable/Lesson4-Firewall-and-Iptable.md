# Lesson 4 - Firewall and Iptable

---

Helping links :

`Control what gets in and out your system`

### Definition

**A Firewall** is a network security system that monitors and controls incoming and outgoing network traffic based on predetermined security rules.

A firewall typically establishes a barrier between a trusted internal network and untrusted external network, such as the Internet.

- Isolation between network interfaces

- Restricted routing and forewarding

- Use to protect services from external network traffic

- Highly configurable and secure

### Network domain

A **network domain** is an administrative grouping of multiple private computer networks or hosts within the same infrastructure.

Domains can be identified using a domain name; domains which need to be accessible from the public Internet can be assigned a globally unique name within the Domain Name System (DNS). 

<u>Could be :</u>

- Internal

- External

- DMS (DeMilitarized Zone)

- Unconfigured

### Network Interfaces

A **network interface** is a software or hardware interface between two pieces of equipment or protocol layers in a computer network.

A network interface will usually have some form of network address. This may consist of a node identifier and a port number or may be a unique node ID in its own right. 

<u>Could be :</u>

- Ethernet Type
  
  - Wired
  
  - Wifi / Bluetooth
  
  - Secondary interfaces

- Virtual
  
  - Bridge
  
  - VPN
  
  - TAP & TUN
  
  - Serial & USB

### IPtable

##### nftable concepts

**nftables** is a subsystem of the Linux kernel providing filtering and classification of network packets/datagrams/frames (available since Linux kernel 3.13 / released January 2014).

nftables is supposed to replace certain parts of `netfilter`, while keeping and reusing most of it. 

Advantages over netfilter :

- less code duplication

- more throughput. 

nftables is configured via the user-space utility `nft` while `netfilter` is configured via the utilities `iptables`, `ip6tables`, `arptables` and `ebtables` frameworks.

nftables utilizes the building blocks of the Netfilter infrastructure, such as the existing hooks into the networking stack, connection tracking system, userspace queueing component, and logging subsystem. 

It is composed of :

- Set of rules

- Applicable to any IP source

- Many filtering option
  
  - Input Streams (ip@ src/dst)
  
  - Output streams (ip@ src/dst)
  
  - Output application & MAC tag

- Port redirection

- Throttling

` Nftable are replacing iptable but the iptable is sticky`

##### Iptable Input and Output

INPUT : 

- Define acceptable connection

- Allow existing connection

- Drop on configured

OUTPUT :

- Define open source/destination
  
  - ip
  
  - port
  
  - new connection

- Allow existing connection
  
  - connection may be allowed by an INPUT

- NAT or Forward
  
  - NAT hides the real source address
  
  - forward ack as a router
  
  - Mangling allow to change the packet content

##### Iptable / nftable Throttling

**Throttling**

- Allow or restrict any service but limit it to fair usage

- Simple protection against DOS

- Safe minimal bandwidth for emergency ssh connection

### Firewall configuration

Iptable are too complex 

Nftable are easier but still not obvious

- High risk of error

- Unmaintainable

- Many good free tools

**firewalld** is a firewall management tool for Linux operating systems. 

It provides firewall features by acting as a front-end for the Linux kernel's netfilter framework via the nftables userspace utility acting as an alternative to the `nft` command line program.

TOOLS:

- Firewalld (default CENTOS7)
- Easy Firewall (web based)
- Shorewall (compiles meta data)

### Troubleshooting

Traffic Generation tools :

- `Telnet` & `ping` 

- `Nmap`
  
  - Scanner
  
  - `Zenmap` (gui version)

Traffic monitoring tools :

- `TCPDump`

- `Wireshark`

- `iptop`

#### Dedicated firewall distribution

`OpenWRT`

- Integrated software update

- All configuration as meta data

`Ipfire`

- Special hardware for Cisco

### Summary

**Iptable** / **nftable**

- Base for packet manipulation

- Dangerous to config by hand

Firewall tool (safer)

- Describe firewall as meta data

- Create correct iptable

Isolation

- Obvious at Internet entry point

- Valuable on any connected device (including VMs / containers)

- White listing strategy shall prevail

### Quizz

**What is the difference between a MAC and a Discritionnary acces control ?**

- MAC -> User can change permission at boot time, some does before starting the process but anyway you can't change the process rights when it's running.

- DAC -> Easier to change the right as every owner of an object as a right to change the access on object it owns, they change it at their discretion as the name say.. 

**What is the nearest concetp of MAC on Windows ?**

- The NTFS file format is quite close to the concept of DAC extended.

- Active directory is somewhat close to the concept of MAC since it allows to enforce policy that the users won't be able to change

**What are the 3 supported MAC in linux Kernel? What are the main differences between them ?**

- **SE Linux** : *Redhat centos,* very complex (initially made by NSA, you either gotta trust them or read & understand what they made). Stored in extended attribute

- **Linux APParmor** : *Ubunutu, suze*. Stored on file system. 

- **Smack** : *agl,embedded.* Lighter and less complex, might disappear soon when the support will stop as there is less of a need for lighter MAC since embedded system capacity goes higher and higher. 

**In SELinux, what is the default strategy in the distro ? What does it mean ?**

The default strategy is targeted, only a few processes are protected. 

Strategy : How you apply your rules

Policy : Rules that you apply. In SE Linux they are usually pretty complex (as in thousands of line), they are stored in ?

### Homework

- Firewalld
  
  - Install and activate
    
    - sudo fireall-cmd --state 
  
  - Limit ping to 1 every 10s 
    
    - sudo firewall-cmd --add-rich-rule='rule family=ipv4 icmp-type name=echo-request drop limit value=6/m' --permanent
  
  - Stop any incoming traffic except dhcp, dns and ssh
    
    - sudo firewall-cmd --add-service dns --permanent
    
    - sudo firewall-cmd --add-service dhcp --permanent
    
    - sudo firewall-cmd --add-service ssh --permanent
    
    - sudo firewall-cmd --reload
  
  - Allow on https as outgoing traffic 
    
    - sudo firewall-cmd --add-service https --permanent
  
  - Reroute all outgoing http toward an address where a mini server will return and error 
    
    - sudo yum install iptables-services
    
    - sudo systemctl start iptables
    
    - sudo systemctl enable iptables
    
    - sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination <dest-ip>:<dest_port>

- By hand 
  
  - Print the iptable/nftable created
    
    - check if rules are set : `sudo iptables -t nat -L -n`
  
  - Understand them
  
  - Check log when rejecting traffic.
