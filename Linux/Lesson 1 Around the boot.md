### Linux Security : Lesson 1 : Around the boot

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

#### 1. BIOS / UEFI

- Everything installed under the operating system is not visible by it.

- UEFI is a more modern solution, supporting larger hard drives, faster boot times, more security features, and—conveniently—graphics and mouse cursors.

- BIOS is a pre-installed firmware on a system/server’s Mother board. BIOS controls the computer hardware. BIOS is used to perform hardware initialization during the booting process. The main job assigned to BIOS is POST (Power On Self Test). It’s a hardware self test. POST tests are simple read/write tests of a few location.
  
  - It allows the user to run the operating system with unknown hardware

- It loads the boot loader

#### 2.  GRUB / Systemdboot

- Level 2 Boot has to be signed by authority with valid certficate in UEFI BIOS.

- Grub is the secondary boot loader : called the kernel loader. The main task at this stage is to load the Linux kernel.

- It can automatically select snapshots, or go backwards if an error occurs

- Grub2 is signed by default.

- Plugins are available

- Configurable by grub config file `/etc/grub2.cfg`

- Systemdboot is the third level of booting.
  
  - Is less flexible (no snapshots / no going backwards)
  
  - Can be good in very hardened needed cases.

#### 3. Kernel Command Line

- Initrd
  
  - **`initrd`** is used by kernel as temporary root file system until kernel is booted 
    and the real root file system is mounted. It also contains necessary 
    drivers compiled inside, which helps it to access the hard drive 
    partitions, and other hardware. `initrd` and `initramfs` refer to two different methods of achieving this. Both are commonly used to make preparations before the real root file system can be mounted.
  
  - It contains modules / drivers / UI / utilities / microcode for CPU (INTEL)
  
  - Is very dangerous because it is very low in the action chain
    
    - If malicious code is integrated in the file, it will by almost unnoticable.

- Here the kernel stage begins. Kernel is in 
  compressed format. We can select the kernel from the GRUB menu. If not 
  selected, the GRUB automatically load the default one in its 
  configuration. We can change the default kernel details from the GRUB 
  configuration.
  
  The kernel you selected is now loaded into 
  the memory. An image file containing the basic root file system with all
   kernel modules are then loaded into the memory. This image file is 
  located under /boot and it’s known as **initramfs**.
  
  **Initramfs**, abbreviated from “initial RAM file system”, is the successor of **initrd** “initial ramdisk”. This image file contains the initial file system. 
  The GRUB starts the kernel and tells the memory address of this image 
  file. The kernel then mount this image file as a starter memory based 
  root file system.
  
  The kernel then starts to detect the system’s hardware. The root file system on disk takes over from the one in memory. The boot process then starts INIT (SYSTEMD) and the software daemons according to the Sys Admin’s settings. 

#### 4.  Init

- The kernel, once it is loaded in step 4, it finds **init** in sbin (**/sbin/init**) and executes it. [**In RHEL/CentOS 7 /sbin/init is linked to ../lib/systemd/systemd**].
  
  When init starts, it become the first or parent process on your Linux machine/server (PID 1).
  
  The first thing **init** does is reading the initialization file, **/etc/inittab**. This instructs **init** to
   read an initial configuration script for the environment, which sets 
  the path, starts swapping, checks the file systems, and so on. From the **/etc/inittab** system will find the run level selected and start services by looking in the appropriate rc directory for that run level.

- Systemd
  
  - Replace init nowadays
    
    - Initrd basically configure the boot by a bash script
      
      - dirty and hard to use
      
      - it launches 'manually' all the processes in the correct order
      
      - logs gestion in a local file (logs rotation)
      
      - setup user space / environment variable
      
      - **The process management quality depends on the quality of the bash init script**
  
  - As PID 1
    
    - First process after boot
      
      - will launch other processes
      
      - Will manage the deamons (stop / start)
      
      - Starts processes in the right order
      
      - Monitoring: supervises the processes and relaunches them in case of problems
      
      - Scheduling tasks
  
  - Systemd configure instead of script
    
    - The goal is to explain to systemd how you want your software to be used / configure
    
    - It has dependencies management / logs / user / backups basically provided
    
    - Specific syntax
    
    - Dynamic -> hazardous
    
    - Security
  
  - It uses systemctl
    
    - Can start / reload / stop process
  
  - Useful resources / commands
    
    - Unit
      
      - ressources (apps/ services / deamon)
      
      - with dependencies
      
      - can be stopped / launched under certain conditions
    
    - Target : Unit group
    
    - Socket / path : starts a serice when specific socket / path is used
    
    - Timer : Start service periodically
    
    - Device / mount / automount / swap : file system mounting
    
    - Snapshot
    
    - Slice : Use `cgroup` for resource restrictions
    
    - Systemd spawn : spawn a low resource container
    
    - systemd run : launch an ephemeral process

#### 5.  Display / Session Manager

- Started by systemd
  
  - A display manager, or login manager, is typically a graphical user interface that is displayed at the end of the boot process in place of the default shell.
    
    - means that it also starts user services in the background
      
      - The services could have been modified by user ( or external threat )

---



#### UID / GUID ACL

- UID (username)

- GUID (groups)
  
  - 1 UID can have multiple GUID
  
  - GUID used to give rights (write / read / exec ) on files

#### ARM

- UBOOT
  
  - Basic initialization of kernel booting

- Kernel
  
  - Contains a **device tree** : data structure describing the hardware components of a particular computer so that the operating systems kernel can use and manage those components, including the CPU / Memory / chips / peripherals

- Initramfs
  
  - initramfs is to mount the root file system. The initramfs is a complete set of directories that you can find in a normal root file system. It is grouped in a single cpio archive and compressed with one of the many compression algorithms.
    
    At boot time, the boot loader loads the kernel and initramfs image into memory and boots the kernel. The kernel checks for the presence of an initramfs and, if it finds it, mounts it on / and starts /init. The init program is typically a shell script. Note that the boot process is longer, even significantly longer, if an initramfs is used.

#### Conclusion

- Linux boot is complex

- Signature are rarely end to end

- UEFI and GRUB run on FAT32 partitions

- care about embedded devices (camera)

- Once kernel is booted
  
  - Set of tools (extended ACL ..)
  
  - Systemd is very dynamic so very hazardous and dangerous
