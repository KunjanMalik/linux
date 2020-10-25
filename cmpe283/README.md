# VMXAssignment1
## Steps to be followed
1. Install VMFusion for MAC system
2. Install Ubuntu 20.04
3. Need to enable "Enable hypervisor applications in this virtual machine" for VM(ubuntu)
4. git clone https://github.com/torvalds/linux over VM
5. change to linux and create sub-directory(cmpe283) and makefile to this sub-directory
6. Need to update cmpe283.c file for below instructions:
IA32_VMX_PINBASED_CTLS
IA32_VMX_PROCBASED_CTLS
IA32_VMX_PROCBASED_CTLS2
IA32_VMX_EXIT_CTLS
IA32_VMX_ENTRY_CTLS
7.Build the module using this command instead this sub-directory: make all and below files will be created by this module
sudo insmod ./cmpe283.ko
sudo rmmod ./cmpe283.ko
8. The VMX features our above file will now logged in the Kernel lod and could be checked with the below command: dmesg

