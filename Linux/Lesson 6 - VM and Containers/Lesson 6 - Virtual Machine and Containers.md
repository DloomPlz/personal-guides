# Lesson 6 - Virtual Machine and Containers

---

## Definitions :

**Virtualisation** is a process of running a virtual instance of a computer system in a layer abstracted from the actual hardware.

**Full Virtualisation** completely simulates the underlying hardware. 

- In this type of environment, any software capable of execution on the physical hardware can be run in the VM, and any OS supported by the underlying hardware can be run in each individual VM. 

- Users can run multiple different guest OSes simultaneously. In full virtualization, the VM simulates enough hardware to allow an unmodified guest OS to be run in isolation.

**ParaVirtualisation** is a technique that presents a software interface to the virtual machines which is similar, yet not identical to the underlying hardware–software interface.

- The intent of the modified interface is to reduce the portion of the guest's execution time spent performing operations which are substantially more difficult to run in a virtual environment compared to a non-virtualized environment. 

- The paravirtualization provides specially defined 'hooks' to allow the guest(s) and host to request and acknowledge these tasks, which would otherwise be executed in the virtual domain (where execution performance is worse).

**Virtual Machine** : emulation of a computer system. Virtual machines are based on computer architectures and provide functionality of a physical computer.

**MMU** (memory management unit), sometimes called paged memory management unit (PMMU), is a computer hardware unit having all memory references passed through itself, primarily performing the translation of virtual memory addresses to physical addresses.

- An MMU effectively performs virtual memory management, handling at the same time memory protection, cache control, bus arbitration and, in simpler computer architectures (especially 8-bit systems), bank switching. 

**Docker** is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers.

- Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels.

- All containers are run by a single operating-system kernel and are thus more lightweight than virtual machines.

**Container** is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. 

- A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

## The concept

- OS virtualisation started by IBM in the 60’
  
  - First Lab release CP-40
  
  - Commercial launch in 1968 as CP/CMS
  
  - First stable release 1972

- Application virtualisation
  
  - Really made available with Java in 1995

- Mass adoption
  
  - OS VM → VmWare 1998
  
  - Container → Docker 2013

---

## Concept of virtualisation

- Run several OS on the same hardware
  
  - Share the CPU(s)
  
  - Allocate RAM areas
  
  - Allocate/share I/O

- Enabling technologies
  
  - Interrupt for the CPU equivalent to a time share
  
  - Memory protection unit (MMU) for the RAM

- New evolutions
  
  - IO share, CPU core preferences
  
  - Virtual devices enabled by hardware.

---

## Different models and types of virtualisation

- Models
  
  - Application
  
  - Desktop
  
  - Hardware
  
  - Network
  
  - Storage

- Types
  
  - Paravirtualisation
  
  - Full virtualisation

---

## Benefit & Side effects

- Benefits
  
  - Strong isolation by Hardware 
  
  - Snapshots
  
  - Replications

- Side effects
  
  - Large increase of OS to maintain
  
  - Great way to keep out of support OS
  
  - Hypervisor become a great hacker targe

---

## Containers

- Light virtualisation

- Build above Name Spaces

- Fast enough to be trigger forsingle use (only run when needed)

- Can be stack on top of a VM

- Common in the cloud

## Containers security

- Isolation is good

- Content is often unknown

- Many unreliable repo (don't fetch unknown/untrusted image of container)

- Maintenance can quickly get out of control

---

## Security rules

- Use only known safe repo

- One Application per container

- No full OS in container
  
  - Minimal package set
  
  - No sharing of device
  
  - All traffic via network API

- Container are immutable
  
  - No data in container
  
  - No update

- Update by replacement
  
  - Requires Continuous integration
  
  - Layers are your friends

---

## Troubleshooting

- Name spaces
  
  - Containers use Name Spaces
  
  - Everything is visible from host OS

- Network traffic
  
  - Network monitoring tools
  
  - Firewall and redirection

- Extreme short time live issues
  
  - Redirection to host log always possible
  
  - systemd can be changed to freeze

---

## Limitations

- Shared kernel
  
  - Host kernel weaknesses propagate
  
  - Not 100% host independent

- No introspection
  
  - Host does not control container
  
  - Libraries and utilities may carry CVEs

- So easy to create a mess
  
  - Frankenstein is never far
  
  - Admin can get out of depth

---

## Summary

- VM provide great isolation by HW

- Container have same limitations as Name Spaces

- Complexity can increase by 10 to 100 very quickly

- Danger with container is to loose control of whatis running in the system (docker is dangerously simple)

- Docker can quickly be dangerous when use as light VM

- Keeping complexity under control is a must buteasier to say than to do.
