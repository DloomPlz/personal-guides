# Linux Security - DAC CGROUP

---

#### Default DAC

Access control under Linux is, by default, of the DAC type: `Discretionary Access Control`.

The rights to an object are **left to the discretion of the owner** of the object. If you own a file, you can set any rights to it. The main problem with this approach is that security policy evolves according to the users.

Is in a state that cannot be determined by the system administrator. In addition, the implementation of the DAC under Linux is essentially about placing permissions on files. 

Finally, as everything is a file, the spectrum is finally quite wide. In this model, the root user bypasses all access control.

#### Extended ACL (Access Control Lists)

**Access-list (ACL)** is a set of rules defined for controlling the network 
traffic and reducing network attack. ACLs are used to filter traffic 
based on the set of rules defined for the incoming or out going of the 
network.

**Extended ACL** is mostly used as it can distinguish IP traffic therefore the whole traffic will not be permitted or denied like in standard access-list.

- Practical to ban tools like `wget` or `curl`

- Limited for networked drive

- Access control list provides an additional, more flexible permission mechanism for file systems.

- It is designed to assist with UNIX file permissions. ACL allows you to 
  give permissions for any user or group to any disk resource.

### DISC Quota

- A **disk quota** is a limit set by a system administrator that restricts certain aspects of file system usage on modern operating systems. The function of using disk quotas is to allocate limited disk space in a reasonable way.

- `edquota` to configure a user quota

- Works by mount points : Physical partitions (*lvm*) / sub-volume or qgroupe (*btrfs*)

- There is a limit per uid or gid

- It is usable with any file system

**usrquota grpquota** is here to avoid system saturation and limit the space allocated to each user, Linux allows you to set limits on the use of disk resources.

- is a deamon / a service

- is often not installed by default

**btrfs** is a filesystem intended to address the lack of pooling, snapshots, checksums, and integral multi-device spanning in Linux file systems

With Quota, you can do **monitoring and reporting** : 

- Report per user : `repquota`

- usage for inode (nb of files)

- block (disk space)

### Stategy

Isolation Access restriction gives permission to users to only what they need which are : 

- monitoring (logs action of users)

- Analysis

### Cgroup

**cgroups** (abbreviated from control groups) is a Linux kernel feature that limits, accounts for, and isolates the resource usage (CPU, memory, disk I/O, network, etc.) of a collection of processes. 

A **control group** (abbreviated as cgroup) is a collection of processes that are bound by the same criteria and associated with a set of parameters or limits. These groups can be hierarchical, meaning that each group inherits limits from its parent group. The kernel provides access to multiple controllers (also called subsystems) through the cgroup interface; for example, the "memory" controller limits memory use, "cpuacct" accounts CPU usage, etc.

Control groups can be used in multiple ways:

- By accessing the cgroup virtual file system manually.

- By creating and managing groups on the fly using tools like cgcreate, cgexec, and cgclassify (from libcgroup).

- Through the "rules engine daemon" that can automatically move 
  processes of certain users, groups, or commands to cgroups as specified 
  in its configuration.

- Cgroup has a tree organisation
  
  - User defined
  
  - Sublevel inherit higher level control
  
  - Multiple trees can coexist
  
  - Cross tree branches is not allowed
  
  - Control via a pseudo file system

- Task and control allocation
  
  - Simple echo in pseudo file system
  
  - Subtask amd process inherit from parent

### seccomp

**seccomp** (short for secure computing mode) is a computer security facility in the Linux kernel.

- was originally intended as a means of safely running untrusted compute-bound programs by restricting system API.
  
  - Based on netfilter (freebsd)
  
  - used by most browser

- Limits
  
  - Can impact performance
  
  - Child don't inherit restriction
  
  - Valid only for well behaving applications

### Quizz

1. Difference between the extended mandatory access and the basic mandatory access ?

2. Share directory with John and Peter without giving access to Paul
   
   - Basic Access control (has to be root)  : John and Peter in the same group
   
   - ACL : Give access to specific users

3. What can you do with cgroup ? 2 examples
   
   - Restrain usage of process
   
   - Give access rights to limited memory space

4. If you are saying that you limit the CPU to 90% to process cgroup, what does that mean ?
   
   - Users will have 10% of CPU usage

5. What is the latest version of Linuc Kernel and when was it released ?
   
   - 5.3.11 (11 November 2019)

### Notes

- The group is allocated to a user at login (need to re-log)

- Extended ACL can remove/add rights, allows to change the default config too
  
  - It is stored in the extended attribute of filename of file
  
  - It loses rights when transferred to an archive not supporting extended ACL

- Once you allocate memory to a process, it stays the same (not dynamic)
  
  - Will kill proccesses with less value if memory is not enough
