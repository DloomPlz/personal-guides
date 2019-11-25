# Linux Security - Around the boot

---

### Sequence summary

1. After a processor is reset, it executes ROM startup code.

2. The ROM startup code initializes the CPU, memory 
   controller, and on-chip devices, and it configures the memory map. The 
   ROM startup code then executes a bootloader.

3. The bootloader decompresses the Linux kernel into RAM
    from Flash memory or a TFTP server transfer. It then executes a jump to
    the kernel's first instruction (PID 1). 

4. The kernel initializes its caches and various hardware devices.

5. The kernel mounts the root filesystem.

6. The kernel executes the init process.

7. The executing init process loads shared runtime libraries.

8. init reads its configuration file and executes scripts. Typically, init executes a startup script which configures and starts networking and other system services.

9. init enters a runlevel where system duties can be performed or the login process can start, allowing for user sessions.

---

## 1. BIOS / UEFI

Helping links : https://www.howtogeek.com/56958/htg-explains-how-uefi-will-replace-the-bios/

**BIOS** (basic input/output system) is the program a PC's microprocessor uses to get the computer system started after you turn it on. It also manages data flow between the computer's operating system and attached devices.

**UEFI** is a more modern solution, supporting larger hard drives, faster boot times, more security features, and—conveniently—graphics and mouse cursors.

*Why UEFI is more advanced than BIOS?*

- The BIOS must run in 16-bit processor mode, and only has 1 MB of space 
  to execute in. It has trouble initializing multiple hardware devices at 
  once, which leads to a slower boot process when initializing all the 
  hardware interfaces and devices on a modern PC.

- BIOS can only boot from drives of 2.1 TB or less. 3 TB drives are now 
  common, and a computer with a BIOS can’t boot from them. That limitation
   is due to the way the BIOS’s Master Boot Record system works.

- This new standard avoids the limitations of the BIOS. The UEFI firmware 
  can boot from drives of 2.2 TB or larger—in fact, the theoretical limit 
  is 9.4 zettabytes.

- UEFI can run in 32-bit or 64-bit mode and has more addressable address 
  space than BIOS, which means your boot process is faster. It also means 
  that UEFI setup screens can be slicker than BIOS settings screens, 
  including graphics and mouse cursor support.

Both UEFI and BIOS are here to perform POST (Power On Self Test) to load the boot loader.

## 2.  GRUB / Systemdboot

**Grub** is a bootloader. It provides a user the choice to boot one of multiple operating systems installed on a computer or select a specific kernel configuration available on a particular operating system's partitions. 

- Il will call the kernel loader.

- Grub2 can select snapshots, or go backwards if an error occurs

- Plugins can be installed

- Config file : `/etc/grub2.cfg`

**Systemdboot** is a simple UEFI boot manager which executes configured EFI images. 

- It is included with systemd
  
  - Is less flexible (no snapshots / no going backwards)
  
  - Can be good in very hardened needed cases.

## 3. Kernel Command Line

**initrd** is used by kernel as temporary root file system until kernel is booted 
and the real root file system is mounted. It also contains necessary 
drivers compiled inside, which helps it to access the hard drive 
partitions, and other hardware. `initrd` and `initramfs` refer to two different methods of achieving this. Both are commonly used to make preparations before the real root file system can be mounted.

- It contains modules / drivers / UI / utilities / microcode for CPU (INTEL)

- Is very dangerous because it is very low in the action chain
  
  - If malicious code is integrated in the file, it will by almost unnoticable.

Steps :

- Grub called the kernel loader.

- The user select the kernel from GRUB menu

- The kernel selected is now loaded into the memory. An image file containing the basic root file system with all kernel modules are then loaded into the memory. This image file is located under /boot and it’s known as **initramfs**.

**Initramfs**, abbreviated from “initial RAM file system”, is the successor of **initrd** “initial ramdisk”.

- It contains the initial file system. 

- GRUB starts the kernel and tells the memory address of this image 
  file. The kernel then mount this image file as a starter memory based 
  root file system.

- The kernel then starts to detect the system’s hardware. The root file system on disk takes over from the one in memory. The boot process then starts **INIT**(SYSTEMD) and the software daemons according to the Sys Admin’s settings. 

## 4.  Init

The kernel, once it is loaded, finds **init** in sbin (**/sbin/init**) and executes it.

When init starts, it become the first parent process on your Linux machine/server (PID 1).

The first thing **init** does is reading the initialization file, **/etc/inittab**. 

This instructs **init** to read an initial configuration script for the environment, which sets the path, starts swapping, checks the file systems...

Init will then find the run level selected and start services by looking in the appropriate directory for that run level.

But init is nowadays replaced by systemd.

**Systemd** is a project composed of three distinct parts :

- an initialization process, systemd, which manages the boot, the launch of the Linux kernel at the graphical interface, and the monitoring of the processes; (as PID 1)

- a set of tools that control the systemd process, in particular `systemctl`, and which allow, among other things, to monitor, restart and stop the various services of a machine;

- a set of tools that can be used as a basis for creating a complete operating system

- It Replace init nowadays because :
  
  - Initrd basically configure the boot by a bash script
    
    - dirty and hard to use
    
    - it launches 'manually' all the processes in the correct order
    
    - **The process management quality depends on the quality of the bash init script**

## 5.  Display / Session Manager

**Display manager**, or **login manager**, is typically a graphical user interface that is displayed at the end of the boot process in place of the default shell.

- It is started by systemd and it also starts user services in the background

- By now, everything is correctly configured for the user to login and use its computer.

---

## Linux processes : UID / GUID ACL

- UID (username)

- GUID (groups)
  
  - 1 UID can have multiple GUID
  
  - GUID used to give rights (write / read / exec ) on files

## ARM

**ARM** is a family of reduced instruction set computing (RISC) architectures for computer processors, configured for various environments.

## Conclusion

- Linux boot is complex

- Signature are rarely end to end

- UEFI and GRUB run on FAT32 partitions

- care about embedded devices (camera)

- Once kernel is booted
  
  - Set of tools (extended ACL ..)
  
  - Systemd is very dynamic so very hazardous and dangerous
