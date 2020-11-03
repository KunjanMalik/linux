## Setup Steps
We have followed the below steps to setup the kernel and get the system ready for Assignment 2:

Compile the new kernel code with below commands:  

We are using Ubuntu as our virtual machine which is mounted over VMware fusion.  
We forked and cloned the git repository https://github.com/torvalds/linux over OS.  
This OS(ubuntu) has KVM capabilities enabled (verified with KVM-ok command as well) and VMX\SVM has exposed. 
We built the Linux kernel source code(assignment requirement) with below commands under the Linux Folder:  
sudo bash  
apt-get install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev  
uname -a   
cp /boot/config-4.15.0-112-generic ./.config   
make oldconfig   
make && make modules && make install && make modules-install   
Reboot  
After Reboot, we faced below error and unable to see the upgraded kernel version (5.10)  
![s1](https://user-images.githubusercontent.com/24988178/98042859-493bc880-1e4a-11eb-82df-c10cf0e00110.png)


To fix the above error, we ran below commands and were able to build the kernel to 5.10 version.  

Reboot -f   
We selected the Generic code version from the available list, which was before reboot with the generic one select and hit enter.   
![s2](https://user-images.githubusercontent.com/24988178/98042869-4d67e600-1e4a-11eb-8f74-3edc50180f36.png)

We went linux source folder and followed below commands.  
Sudo bash. 
sudo make -j 2 && sudo make modules_install -j 2 && sudo make install -j 2 (nproc command provided the CPU number output 2).  
update-grub   
Make  
Reboot  
Verified the version with command “uname -r” and Kernel is ready.  
![s3](https://user-images.githubusercontent.com/24988178/98042870-4f31a980-1e4a-11eb-8a33-f7c989e057b3.png)


## Now install KVM QEMU

As Kernel(5.10) is ready, now we installed the KVM(hypervisor) over host OS and referred below link:  
https://www.tecmint.com/install-kvm-on-ubuntu/  
$ egrep -c '(vmx|svm)' /proc/cpuinfo (will verify virtualization support if value is more then 0). 
$ sudo kvm-ok  
$ sudo apt install cpu-checker  
$ sudo apt install -y qemu qemu-kvm libvirt-daemon libvirt-clients bridge-utils virt-manager   
$ sudo systemctl status libvirtd   
$ sudo systemctl enable --now libvirtd   
$ lsmod | grep -i kvm   
While, we were executing above commands we got error “opencl-amdgpu-pro-orca” to fix this we referred below link:   
https://unix.stackexchange.com/questions/546572/virt-manager-kvm-is-not-available   

Now hypervisor is installed and we are now able to launch virt-manager with command (sudo virt-manager) or can be opened directly from the application section.   

We need to install the Linux(ubuntu) OS over the hypervisor through virt manager.   
Upload the iso image file under new VM.  
Install the ubuntu and reboot.   
We are able to login to our inner VM (ubuntu) as below:   
![s4](https://user-images.githubusercontent.com/24988178/98042873-522c9a00-1e4a-11eb-93a4-202399c5dc1e.png)



## Requirment
Our assignment requirement is to modify the CPUID emulation code in KVM to report back additional information when a special CPUID “leaf function” is called.  


Modify the kernel code into two files over host(Ubuntu) machine at below location:  
          ~linux/arch/x86/kvm/cupid.c   
          ~linux/arch/x86/kvm/vmx/vmx.c  
We ran the following command in host machine under Linux directory:  
           -sudo rmmod kvm_intel 
           -sudo rmmod kvm  
           -sudo make SUBDIRS= arch/x86/kvm/  
           -sudo insmod arch/x86/kvm/kvm.ko  
           -sudo insmod arch/x86/kvm/kvm-intel.ko  

We have start the guest or inner VM from virt manager  
### Create a shell test script file(test_script.sh) as below inside inner VM (once inner VM is up).   

#!/bin/bash  

cpuid="0x4FFFFFFF"   
output=$(cpuid -l $cpuid-1)   

eax=$(echo $output | grep 0x4fffffff | awk '{print toupper($4)}'| cut -c7-14)  
ebx=$(echo $output | grep 0x4fffffff | awk '{print toupper($5)}'| cut -c7-14)   
ecx=$(echo $output | grep 0x4fffffff | awk '{print toupper($6)}'| cut -c7-14)   

exits=$(echo "ibase=16; $eax" | bc)   
cycles=$(echo "ibase=16; $ebx$ecx" | bc)   

echo "CPUID($cpuid), exits=$exits, cycles spent in exit=$cycles"   



We have checked the output of number of VM exit and CPU cycles over inner VM as below:   

kunjan@kunjan-Standard-PC-Q35-ICH9-2009:~/Documents$ ./test_script.sh CPUID(0x4FFFFFFF), exits=2325981, cycles spent in exit=5452092626    


We checked at Host machine with “dmesg” command   

30282.107579] Exit Count : 2325981   
[30282.107581] EBX : 12    
[30282.107581] ECX : 2981318754    
[30282.107894] Exit Count : 2332268    
[30282.107896] EBX : 12  
[30282.107896] ECX : 3021617040 
  
  

## Questions
### 1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched. (You may skip this question if you are doing the lab by yourself).  

We worked in a team of 2.    
Shivam Shrivastav and Kunjan Malik    
We both have researched together about how to install and run installed VMFusion, Ubuntu Host VM and Inner VM in both of our systems.   
First we built the solution separately without the atomic read to check if we are at least able to achieve the main requirement.    
We shared the load, where Kunjan worked on the total exits part in vmx.c and cpuid. Shivam worked on total cycles in cpuid.c and vmx.c. We then integrated our parts and worked together to build a complete solution.   
After the successful results, we worked on handling concurrent exits in multi-VCPU VM . This was achieved using atomic read and atomic increment functionality.    With this, we built the complete solution and tested our output using the above mentioned test script.   
  
### 2. Steps described in the readme.md file.  
For our code changes, you can refer the following files: 
 ~linux/arch/x86/kvm/cupid.c   
 ~linux/arch/x86/kvm/vmx/vmx.c   
To run the entre setup, please follow above steps and execute the test script code mentioned above test scriot section and follow the all the above steps.       

### 3. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?      

It does not seem to be increasing at a stable rate. When we run a variety of VM instructions in our inner VM, say unauthorized file access, playing video on the VM or internet or other VM operations., more number of VM exits occur due to Page Fault, exceptions etc accordingly.   
In our setup, approximately, 2325981 exits occur within 54520926306 cpu cycles after VM boot.   



             
   

