# Mandatory Access Control

-----

Helping links : https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/security-enhanced_linux/sect-security-enhanced_linux-introduction-selinux_architecture



## LSM - Linux Secure Module

**Linux Security Modules** (LSM) is a framework that allows the Linux kernel to support a variety of computer security models while avoiding favoritism toward any single security implementation.

- Started in 2005 by the NSA

- Included from 2.6.29 Kernel (2006)

- Allows third party policy modules
  
  - SELinux
  
  - AppArmor
  
  - Smack
  
  - etc..

- SE Linux : Redhat, Centos

- AppArmor : Ubuntu, OpenSUSE

- Smack : Tizen, AGL

-----

## SE Linux

**SELinux**, is a Linux security module, which allows to define a policy of mandatory access control (MAC) to the elements of a Linux-based system. Its architecture separates the application from the access policy and its definition.

SELinux fundamentally answers the question: "May <subject> do 
<action> to <object>", for example: "May a web server access
 files in users' home directories?".

- Started in 2005 by NSA

- Administrative defined security

- Policy Control Subjects and Objects

- Subjects
  
  - Users
  
  - Processes
  
  - Flows

- Objects
  
  - Files
  
  - I/O

## Benefits of running SELinux

- All processes and files are labeled with a type. A type defines a 
  domain for processes, and a type for files. Processes are separated from
   each other by running in their own domains, and SELinux policy rules 
  define how processes interact with files, as well as how processes 
  interact with each other. Access is only allowed if an SELinux policy 
  rule exists that specifically allows it.

- Fine-grained access control. Stepping beyond traditional UNIX 
  permissions that are controlled at user discretion and based on Linux 
  user and group IDs, SELinux access decisions are based on all available 
  information, such as an SELinux user, role, type, and, optionally, a 
  level.

- SELinux policy is administratively-defined, enforced system-wide, and is not set at user discretion.

- Reduced vulnerability to privilege escalation attacks. One example:
   since processes run in domains, and are therefore separated from each 
  other, and because SELinux policy rules define how processes access 
  files and other processes, if a process is compromised, the attacker 
  only has access to the normal functions of that process, and to files 
  the process has been configured to have access to. For example, if the 
  Apache HTTP Server is compromised, an attacker cannot use that process 
  to read files in user home directories, unless a specific SELinux policy
   rule was added or configured to allow such access.

- SELinux can be used to enforce data confidentiality and integrity, as well as protecting processes from untrusted inputs.

---

## SELinux Architecture

![Image of Yaktocat](/Users/pulsz/dev/ProjetPerso/personnal-guides/Linux/Lesson 3 - MAC/architecture.png)

SELinux is driven by loadable policy rules. 

A process attempts to open a file:

- the operation is intercepted in the kernel by SELinux. If an SELinux policy rule allows the operation, it continues, otherwise, the operation is blocked and the process receives an error.

SELinux decisions, such as allowing or disallowing access, are cached. This cache is known as the Access Vector Cache (AVC). When using these cached decisions, SELinux policy rules need to be checked less, which increases performance. Remember that SELinux policy rules have no effect if DAC rules deny access first.

---

## SEL Multi-Level Security (MLS)

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/mls

Under **MLS**, users and processes are called *subjects*, and files, devices, and other passive components of the system are called *objects*.
Both subjects and objects are labeled with a security level, which entails a subject's clearance or an object's classification. Each security level is composed of a *sensitivity* and a *category*, for example, an internal release schedule is filed under the internal documents category with a confidential sensitivity.

Relating to our internal schedule example above, only users that have gained the confidential clearance are allowed to view documents in the confidential category. However, users who only have the confidential clearance are not allowed to view documents that require higher levels or clearance; they are allowed read access only to documents with lower levels of clearance, and write access to documents with higher levels of clearance.

![security-intro-to-mls.png](/Users/pulsz/dev/ProjetPerso/personnal-guides/Linux/Lesson 3 - MAC/security-intro-to-mls.png)



The figure below shows all allowed data flows between a subject running under the 
"Secret" security level and various objects with different security levels. In simple terms, the Bell-LaPadula model enforces two properties: *no read up* and *no write down*.

![security-mls-data-flow.png](/Users/pulsz/dev/ProjetPerso/personnal-guides/Linux/Lesson 3 - MAC/security-mls-data-flow.png)

---

## SELinux Summary

- Powerful system

- Almost impossible to master

- Must trust ready made rules

- Outside packages
  
  - Can be included in SELinux
  
  - By default out of control

- Host replication must be scripted to be reliable

---

## Other access control : Seccomp-BPF

- System call filtering
  
  - Since 2.6 kernel
  
  - Used by browser and Docker
  
  - Filters expressed in BPF

- Large rules impact perfs

- Only applicable by patching app code

- Restriction are not inherited by child process / not threads

----

## Conclusion

- Linux MAC
  
  - Powerful but complex
  
  - SELinux and AppArmor smack on embedded

- Does not replace regular update

- Package from valid source remains

- No visibility on attack prior to boot

- **By default custom added Apps are not supervised at all.**
