# Linux Security - Name Space

---

Table of Content :

- Concept

- Various name spaces

- HowTo

- Limitations

Run task on a dedicated domain with the same performance as native OS or how to share in home made isolation.

### Recent but not that new

- Plan 9 from AT&T
  
  - Started in mid 80's
  
  - Commercial launch in '95
  
  - Targeting embedded / small systems

- Solaris zone
  
  - Commercial launch circa 2000
  
  - targeting mass web server hosting

- Linux name space
  
  - 2002

### Namespace concepts

- behave like parallel worlds

- share the same kernel

- kernel keeps each world isolated

- subsystem as layers

- used to create container services

- equivalent but different from virtualisation

- negligible overhead to the system

### Various name spaces

```
- Cgroup : CLONE_NEWCGROUP - Cgroup root directory
- IPC : CLONE_NEWIPC - System V IPC, POSIX queues
- Network : CLONE_NEWNET - Network Devices, stacks, ports, etc..
- Mount : CLONE_NEWNS - Mount Points
- PID : CLONE_NEWPID - Process IDs
- User : CLONE_NEWUSER - User and group IDs
- UTS : CLONE_NEWUTS - Hostname and NIS domain name
```

### Network NS

- Create virtual ethernet interfaces
  
  - Associated with new namespace
  
  - link to parent by virtual bridge

- Link parent and child name spaces
  
  - create a virtual cable
  
  - each end has dedicated name
  
  - child does not 'see' parent or children

- Software bridge
  
  - parent creates a software bridge
  
  - act as virtual internal switch

### User & Group NS

- Cloning root user
  
  - UID 0 has special privilege in its own name space

- Mapping
  
  - UID 0 maps as a normal user in parent name space
  
  - each name hast its own UID 0
  
  - Parent name space see all child UID
  
  - No cross visibility between child name spaces

### Process ID NS

- Cloning PID
  
  - Init as always PID 1
  
  - Run with all privileges

- Mapping
  
  - PID 1 maps as a normal PID in parent name space
  
  - Each name space has its own PID 1
  
  - Parent name space see all child PID
  
  - No cross visibility between children name space

### Mount NS

Cloning mount point

- Similar to chroot

- DAC are mapped with local UID/GID

Mapping

- Virtual drive are mapped on directory used as mount point in parent name space

- Mount can be
  
  - Requested by the child name space
  
  - Imposed by the parent name space

- Multiple mode available
  
  - Shared / private / slave / unbindable
  
  - Help : https://www.kernel.org/doc/Documentation/filesystems/sharedsubtree.txt
  
  - Warning : propagation rules require detailed attention
  
  - A **shared** mount can be replicated to as many mountpoints and all the
    replicas continue to be exactly same.
    
    A **slave** mount is like a shared mount except that mount and umount events only propagate towards it.
    All slave mounts have a master mount which is a shared.
    
    A **private** mount does not forward or receive propagation.
    This is the mount we are familiar with. Its the default type.
    
    A **unbindable** mount is a unbindable private mount

### Cgroup and MAC NS

### Other NS

### How to use it

### Examples

### Summary

### Homework

- Container
  
  - Install LXC
  
  - Create a container for apache
  
  - Use iptable to route inbound port 80 traffic to apache

- By hand
  
  - Remount /etc for a given shellpoint new etc to and empty dir
  
  - Remount /tmp for a given shellin a clean ramfs
  
  - Report finding from child and parent name space perspective

```bash
[root@localhost secu_linux]# yum -y install epel-release

[root@localhost secu_linux]# yum -y install lxc lxc-templates libcap-devel libcgroup busybox wget bridge-utils lxc-extra debootstrap

[root@localhost secu_linux]# lxc-checkconfig

[root@localhost secu_linux]# lxc-create -t download -n seculinux

[root@localhost secu_linux]# lxc-create -n centos_lxc -t centos

[root@localhost secu_linux]# cat /var/lib/lxc/centos_lxc/tmp_root_pass
Root-centos_lxc-hX5JVl

[root@localhost secu_linux]# lxc-start -n centos_lxc -d


[root@localhost secu_linux]# lxc-attach  --name centos_lxc

[root@centos_lxc /]# yum install httpd

[root@centos_lxc /]# systemctl start httpd

[root@centos_lxc /]# systemctl enable httpd

[root@centos_lxc /]# systemctl status httpd

iptables -A INPUT -i eth0 -p tcp --dport 80 -j ACCEPT
iptables -A PREROUTING -p tcp -m tcp -i eth0 --dport 80 -j DNAT --to-destination 10.175.162.124:80
iptables -A FORWARD -p tcp -d 10.175.162.124 --dport 80 -j ACCEPT



==============================


[root@localhost secu_linux]# unshare --mount

[root@localhost secu_linux]# mkdir /empty_dir

[root@localhost secu_linux]# mount --bind /etc /empty_dir

[root@localhost secu_linux]# ls -la /empty_dir/
total 1236
drwxr-xr-x. 86 root root     8192 25 nov.  01:19 .
dr-xr-xr-x. 19 root root      265 25 nov.  01:35 ..
-rw-r--r--.  1 root root       16 24 sept. 11:57 adjtime
-rw-r--r--.  1 root root     1518  7 juin   2013 aliases
-rw-r--r--.  1 root root    12288 24 sept. 12:02 aliases.db
drwxr-xr-x.  2 root root     4096 25 nov.  01:13 alternatives
-rw-------.  1 root root      541  9 août  01:07 anacrontab
...
```

---

### Q&A on NameSpaces

## How does one use namespaces  ?

- You can either write you own code, 

- or **use container soft**, for example : `lxc`, `docker`, `openstack`

- `systemd`

Openstack : 

- Create VMs
- Bridge them
- HDD LV
- Container
- Provides services like monitoring 

----

Pain to do, so we created Kubernecles, which is an orchestrator that organizes the jobs. It got an API. You can query it to do something, and Kubernecles will use `openstack` itself to create a VM, do the job and remove the VM.

`AWS` does exactly the same thing but is not open-source. It proposes an interface to run easily virtual machines and orchestrate the job. It is worldwide most used technology to do this job.

## Can you run different distributions on a namespace and what is the hard limit of namespaces?

You can run `centos` on top of a `debian` for example,  **but it will have the same kernel !** You can share RAM, etc, but it will be using the same kernel.

Fact : Kernel API is very stable.

## Give 3 example of namespaces and give their specifications

- `pid` provides isolation between processes. You should not be able to run twice the same piece of code, and this allows you to fake stuff so you can redo things you should not be able to do twice.
- `mnt` namespaces are purely based on isolation
- `network` 

Notes : MAC rules comes from the kernel, so running MAC rules on embedded is quite complex. You have to change the MAC rules on the embedded system. 

## What is the principle of a namespace and why was it invented?

Created for provide a light way of partition and isolate stuff. 

**The principle of namespace is to have different views of the same operating system.**

Is it extremely fast, and have a negligible overhead. The overhead doesn't increase if you add more and more namespaces.

BUT it is a software provided isolation. (!= VM that provides isolation trough hardware) 

# LXC

----

## `Apparté on bridge connections`

ETH0 is Internet

ETH1 is management

ETH1 ship handler:

```
            Changeable by BIOS
                  +
                  |
                  |
                  v
           +------+----+
+-----+    | MAC       |    +--------------------+
|     <----+ (Media    <----+ Protection against <---+ ETH1
| DMA |    | Access    |    | electric issues    |
|     |    | Control)  |    +--------------------+
+-----+    |           |
           +-----------+
```

The DMA is accessible trough `/dev/eth0` and you can use it to bridge connection using your host, connecting ETH0 to it.

ETH1 provides access to an interface to configure the microship MAC's addr and stuff (among other things). BIOS also provides this.

---

## Difference between namespace and a VM ? What are the consequences ?

- Virtual machines does not share kernel and is a hardware feature.
  
  - Good point : You don't have to boot when using namespace (same kernel)

- From namespace you can expose host

### Can i use a namespace to create a fool software as a VM ?What are the consequences ?

- Run the same Kernel

- KERNEL API is strong
