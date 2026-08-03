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
     - examples
       - VMWare vSphere(v-center), Linux KVM, Microsoft Hyper-v, zen, etc.,
  2. Type 2 - a.k.a Hosted Hypervisor
     - used in Laptops/Desktops/Workstation where already there is Host OS pre-installed(Windows, Mac OS-X or Linux )
     - examples
       - VMWare Workstation, VMware Fusion, Oracle VirtualBox, Parallel, etc.,
- each VM represents one Operating System with its own OS Kernel
</pre>

## Info - High Level Hypervisor Architecture
![hypervisor](HypervisorHighLevelArchitecture.png)

## Info - Containerization
<pre>
- is an application virtualization technology
- it is light-weight technology as all containers running on the same Host OS or Guest OS shares the Hardware
  resources on the underlying Host/Guest OS
- container represents an application not an OS
- container doesn't have its own OS Kernel, hence it depends on Host/Guest OS Kernel
- in some ways container and virtual machines behave similar
  - just like a VM has its own virtual network card(s), containers also has its own virtual network card(s)
  - just like a VM gets an IP address, containers also get its own IP address
  - just like a VM has its own software defined network stack, containers also has its own software defined network stack
  - network stack ( 7 OSI Layers )
  - Just like VMs, containers also has a file system ( files & folders )
- but technically comparing a Virtual Machine Guest OS with container is wrong, as Guest OS is a fully functional OS
  with its own Kernel, while container is just an application not a OS, it doesn't have its own OS Kernel
- one container represents one application
- containers will never able to replace OS or Virtualization
- in real world, containers runs inside VMs, VMs runs inside Physical Servers
</pre>

## Info - High Level Docker Architecute
![docker](DockerHighLevelArchitecture.png)
