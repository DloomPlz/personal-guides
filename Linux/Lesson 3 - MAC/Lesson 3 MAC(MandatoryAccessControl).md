# Mandatory Access Control

-----

### Table of Content

- [LSM](#LSM)

- [Various MAC](#MAC)

- [General Principle](#General)

- [Centos and SE Linux](#Centos)

- [Workshop](#Workshop)

----

### LSM - Linux Secure Module

**Linux Security Modules (LSM) is a framework that allows the Linux kernel to support a variety of computer security models while avoiding favoritism toward any single security implementation.**

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

### SE Linux

**SELinux, is a Linux security module, which allows to define a policy of mandatory access control to the elements of a Linux-based system. Its architecture separates the application from the access policy and its definition.**

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

---

### SELinux Architecture

![Image of Yaktocat](/Users/pulsz/dev/ProjetPerso/personnal-guides/Linux/Lesson 3 - MAC/architecture.png)

We execute a new program, what's happening :

- At the kernel level
  
  - Books : manage by the kernel
    
    - They tag every system call in a security context
  
  - Security context : identity in LSM that is unchangeable
    
    - Create a namespace and a set of cgroup

- Books are passed into Security LSM
  
  - LSM wil apply security
    
    - Permissions / credentials 
    
    - track in a very secure manner

- SID is given to new process

----

### SEL Concepts

![Image of Yaktocat](/Users/pulsz/dev/ProjetPerso/personnal-guides/Linux/Lesson 3 - MAC/concepts.png)

- Process is a programm and a programm is on a file

- When process is executed it will have another security context

----

### SEL Control

- Sestatus -> print SEL status
  
  - Enable ?
  
  - Permissive ?
  
  - Policy Name

- Policy
  
  - Targetted (default) -> control predefine list of services
  
  - /etc/selinux/policy_name

- Best documentation published by `redhat`

- You need to recover when hacked
  
  - Backup
  
  - Monitor

---

### SEL Object and Subjects

- Subjects can affect Objects
  
  - Domain are for Subjects

- Objects are manipulated by Subjects
  
  - Types are for Objects (e.g files)

---

### SEL User Domain by Redhat

- Unconfined by default

- Other type available

---

### SEL Multi-Level Security (MLS)

- Not used by default

- Quickly very complex enforces the Bell-La Padula MAC

- Only used when needed

- Not compatible with X and many desktop Apps.

---

### SEL Summary

- A powerful system

- Almost impossible to master

- Must trust ready made rules

- Outside packages
  
  - Can be included in SEL
  
  - By default out of control

- Host replication must be scripted to be reliable

---

### Other access control : Seccomp-BPF

- System call filtering
  
  - Since 2.6 kernel
  
  - Used by browser and Docker
  
  - Filters expressed in BPF

- Large rules impact perfs

- Only applicable by patching app code

- Restriction are not inherited by child process / not threads

----

### Conclusion

- Linux MAC
  
  - Powerful but complex
  
  - SELinux and AppArmor smack on embedded

- Does not replace regular update

- Package from valid source remains

- No visibility on attack prior to boot

- **By default custom added Apps ar not supervised at all.**
