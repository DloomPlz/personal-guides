# Lesson 4 - Firewall and Iptable

---

`Control what gets in and out your system``

### Definition

**A Firewall ?**

- Isolation between network interfaces

- Restricted routing and forewarding

- Use to protect services from external network traffic

- External can be:
  
  - Open Internet
  
  - Another Internal Network

- Highly configurable and secure

### Network domain

- Internal

- External

- DMS (DeMilitarized Zone)

- Unconfigure

### Network Interfaces

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

- Set of rules

- Applicable to any IP source

- Many filtering option
  
  - Input Streams (ip@ src/dst)
  
  - Output streams (ip@ src/dst)
  
  - Output application & MAC tag

- Port redirection

- Throttling

` Nftable are replacing iptable but the iptable is sticky`

![nftable-concepts.png](/Users/pulsz/dev/ProjetPerso/personnal-guides/Linux/Lesson 4 - Firewall and iptable/nftable-concepts.png)

##### Iptable In

- INPUT
  
  - Define acceptable connection
  
  - Allow existing connection
  
  - Drop on configured

##### Iptable Out

- OUTPUT
  
  - Define open src/dest
    
    - ip
    
    - port
    
    - new conn
  
  - Allow existing conn
    
    - conn may be allowed by an INPUT
  
  - NAT or Forward
    
    - NAT hides the real src address
    
    - forward ack as a router
    
    - Mangling allow to change the packet content

##### Iptable nftable Throttlin

- Throttling
  
  - Allow restrict any service but limit it to fair usage
  
  - Simple protection against DOS
  
  - Safe minimal bandwidth for emergency ssh conn

### Firewall configuration

- Iptable are too complex 

- Nftable are easier but still not obvious
  
  - High risk of error
  
  - Unmaintainable
  
  - Many good free tools

- ```textile
  TOOLS:
  - Firewalld (default CENTOS7)
  - Easy Firewall (web based)
  - Shorewall (compiles meta data)
  ```

### Troubleshooting

- Traffic Generation
  
  - Telnet & ping can change port
  
  - Nmap
    
    - Scanner
    
    - Zenmap (gui version)

- Traffic monitoring
  
  - TCPDump
  
  - Wireshark
  
  - iptop

#### Dedicated firewall distro

- OpenWRT
  
  - Run on PC
  
  - Integrated software update
  
  - All config as meta data

- Ipfire

- Special hardware
  
  - Cisco

### Summary

- Iptable / nftable
  
  - Base for packet manipulation
  
  - Dangerous to config by hand

- Firewall tool (safer)
  
  - Describe firewall as meta data
  
  - Create correct iptable

- Isolation
  
  - Obvious at Internet entry point
  
  - Valuable on any connected device (includ VM / container)
  
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
