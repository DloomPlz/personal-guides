### Linux Security : Lesson 1 : Around the boot

#### BIOS / UEFI

- Everything installed under the operating system is not visible by it.

- BIOS is mostly bases on ARM.
  
  - It intialize the hardware and provides a CPI table (descriptions of hardware)
    
    - It allows the user to run the operating system with unknown hardware

- UEFI
  
  - Allows the user to boot very fast (initially created for Macintosh to boot on an Intel chipset)
  
  - It is basically a **small operating system** which is required for secure booting.

- Intel Hardware
  
  - GRUB / Systemdboot
  
  - Level 2 Boot has to be signed by authority with valid certficate in UEFI BIOS.
    
    - Grub is the secondary boot loader
      
      - It can automatically select snapshots, or go backwards if an error occurs
      
      - Grub2 is signed by default.
      
      - Plugins are available
    
    - Systemdboot is the third level of booting.
      
      - Is less flexible (no snapshots / no going backwards)
      
      - Can be good in very hardened needed cases.

#### ARM

- UBOOT
  
  - Basic initialization of kernel booting

- Kernel
  
  - Contains a **device tree** : data structure describing the hardware components of a particular computer so that the operating systems kernel can use and manage those components, including the CPU / Memory / chips / peripherals

- Initramfs
  
  - initramfs is to mount the root file system. The initramfs is a complete set of directories that you can find in a normal root file system. It is grouped in a single cpio archive and compressed with one of the many compression algorithms.
    
    At boot time, the boot loader loads the kernel and initramfs image into memory and boots the kernel. The kernel checks for the presence of an initramfs and, if it finds it, mounts it on / and starts /init. The init program is typically a shell script. Note that the boot process is longer, even significantly longer, if an initramfs is used. 

- Systemd
  
  - Replace initrd nowadays
    
    - Initrd basically configure the boot by a bash  script
    
    - dirty and hard to use
    
    - it launches 'manually' all the processes in the correct order
    
    - logs gestion in a local file (logs rotation)
    
    - setup user space / environment variable
    
    - **The process management quality depends on the quality of the bash init script**
  
  - PID 1
    
    - Fist process after boot
      
      - will launch other processes
      
      - Will manage the demons (stop / start)
      
      - Starts processes in the right order
      
      - Monitoring: supervises the processes and relaunches them in case of problems
      
      - Scheduling tasks
  
  - Systemd configure instead of script
    
    - The goal is to explain to systemd how you want your software to be used / configure
    
    - It has dependencies management / logs / user / backups basically provided
    
    - Specific syntax
  
  - It uses systemctl
    
    - Can start / reload / stop process
  
  - Useful commands
    
    - Unit
      
      - ressources (apps/ services / deamon)
      
      - with dependencies
      
      - can be stopped / launched under certain conditions
    
    - Target
      
      - Unit group
    
    - Socket / path
      
      - starts a serice when specific socket / path is used
    
    - Timer
      
      - Start service periodically
    
    - Device / mount / automount / swap
      
      - file system mounting
    
    - Snapshot
    
    - Slice
      
      - Use `cgroup` for resource restrictions
    
    - Systemd spawn
      
      - spawn a low resource container
    
    - systemd run
      
      - launch an ephemeral process

- Login Manager
  
  - A display manager, or login manager, is typically a graphical user interface that is displayed at the end of the boot process in place of the default shell.

#### Kernel Command Line

- Initrd
  
  - **`initrd`** is a scheme for loading a temporary root file system into memory which may be used as part of the Linux startup process. `initrd` and `initramfs` refer to two different methods of achieving this. Both are commonly used to make preparations before the real root file system can be mounted.
  
  - It contains modules / drivers / UI / utilities
  
  - Is very dangerous because it is very low in the action chain
    
    - If malicious code is integrated in the file, it will by almost unnoticable.

#### Display / Session Manager

- Started by systemd
  
  - Start the user session
    
    - means that it also starts user services in the background
      
      - The services could have been modified by user ( or external threat )

#### UID / GUID ACL

- UID (username)

- GUID (groups)
  
  - 1 UID can have multiple GUID
  
  - GUID used to give rights (write / read / exec ) on files

#### Conclusion

- Linux boot is complex

- Signature are rarely end to end

- UEFI and GRUB run on FAT32 partitions

- care about embedded devices (camera)

- Once kernel is booted
  
  - Set of tools (extended ACL ..)
  
  - Systemd is very dynamic so very hazardous and dangerous
