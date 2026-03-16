# Day 1

## Info - Hypervisor Overview
<pre>
- is Virtualization technology
- this helps us run multiple OS on the same Laptop/Desktop/Workstation/Server
- i.e more than one OS can be actively running side by side
- let's we have a laptop with Quad Core CPUs, 8GB RAM and 500GB HDD/SDD, 
  how many maximum Virtual Machines this laptop would support ?
  - the deciding/limiting factor is the Processor supports how many CPU Cores?
  - 4 x 2 = 8 virtual cores you have, which mean total OS that can run on this laptop is limited to 8 ( 1 Host OS +  7 Virtual Machines)
  - with the help of HyperThreading feature, each Physical CPU Core supports running 2 parallel threads, hence they are seen/considered as two virtual/logical Core by the Hypervisor software
  - We could install any Operating System within a Virtual Machine and they are called as Guest OS
- there are 2 types of Hypervsisorts
  1. Type 1 ( a.k.a Bare-Metal Hypervisors )
  2. Type 2 ( a.k.a Hosted Hypervsiors
- Type 1
  - is meant to be used in Workstations and Servers
  - this type of Hypervisor comes with a minimal OS, hence we don't need to install Host OS, instead we can directly install the Hypervisor
  - examples
    - VMWare vsphere(vCenter)
    - Microsoft Hyper-V
    - Linux KVM
- Type 2
  - can be installed on top a Host OS ( Linux, Windows or Mac OS )
  - is meant to be used in Laptops, Desktops and Workstations
  - examples
    - VMware Workstation ( supported in Linux and Windows )
    - VMWare Fusion ( supported in Mac OS-X )
    - Parallels ( supports in Mac OS-X )
    - Oracle VirtualBox ( supported in Windows, Linux and Mac OS-X )
</pre>
