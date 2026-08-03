# Day 1

## Info - Bootloader - Multibooting/Dual Booting
<pre>
- Boot loader is a tiny system utility that gets installed in Boot sector of your hard disk
- When the system is booted, the BIOS POST (Power On Self Test completes) and BIOS firmware
  instructs the CPU to run the bootloader at Sector 0, Byte 0 (Boot Sector )
- Once the CPU starts the boot loader system utility, it will scan your system looking for hard disks ( SSD ),
  detecting for all the Operating Systems installed on your machine
- In case, it detects more than one OS, it presents a menu for you to choose between the OS
- Only one OS can be active at any point of time
- Let's say, your laptop has got Windows 11 and Ubuntu 24.04, and you booted your laptop into Windows 11, in order
  to use Ubuntu 24.04, you need to shutdown the Windows 11 OS and boot in Ubuntu 24.04 and vice versa
- examples
  - Boot Camp ( commercial product used in Macbooks to install Windows )
  - LILO 
  - GRUB ( all latest Linux distributions use this )
</pre>
  
## Info - Hypervisor Overview
<pre>
- it is virtualization technology
- with virtualization, we can run multiple OS on the same machine side by side
- i.e more than one OS can actively run in the same laptop/desktop/workstation/server
- there are 2 types of Hypervisor
  1. Type 1 - a.k.a Bare Metal Hypervisor
     - used in Servers & Workstations
  2. Type 2 - a.k.a Hosted Hypervisor
     - used in Laptops/Desktops/Workstation where already there is Host OS pre-installed(Windows, Mac OS-X or Linux )
</pre>
