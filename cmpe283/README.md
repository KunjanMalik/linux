# VMXAssignment1
## Team: Shivam Shrivastav and Kunjan Malik
Question 1's Answer:  
We worked together in a team of two and gone through the assignment video toegether. There were couple of initial common doubts which we discussed together and clarfied them. We then divided parallely installed the VMWareFusion and Ubuntu, did the hypervisor setup toegether. After all the initial setup, we ran the below mentioned commands step by step with the sample cmpe283.c and MakeFile in each of our VM. We checked the output and understood the sample code. Based on this code and the requirement for the below mentioned MSRs, we then divided load by sharing 2 MSRs each and built the code. The each of the details found from the SDM in added with reference in the code comments. After that we merged our code, checked the output using dmesg and finshed our parts collectively. So, 2 MSRs were done by both of us and shared the other codebase setup.  

## Steps to be followed
Question 2's answer:  
1. Install VMFusion for MAC system
2. Install Ubuntu 20.04
3. Need to enable "Enable hypervisor applications in this virtual machine" for VM(ubuntu)
4. run below command to clone the repo:  
    git clone https://github.com/torvalds/linux
5. change directory to linux and create sub-directory: cmpe283 and copy MakeFile to this sub-directory
6. Copy the smaple cmpe283.c file and update this file with our code for below MSRs:  
IA32_VMX_PINBASED_CTLS  
IA32_VMX_PROCBASED_CTLS  
IA32_VMX_PROCBASED_CTLS2  
IA32_VMX_EXIT_CTLS  
IA32_VMX_ENTRY_CTLS  
7.Build the module using the below commands inside this sub-directory:
  make all   
Below files will be created by this module  
cmpe283.ko, cmpe283.mod, cmpe283.mod.c, cmpe283.mod.o, cmpe283.o, Module.sysvers and modules.order  

8. Load and unload the module using the below commands:  
sudo insmod ./cmpe283.ko    
sudo rmmod ./cmpe283.ko

Insmod command in Linux systems is used to insert modules into the kernel. With this command, the module_init macro will be invoked first, which will then call the function init_module.   
In the same way, on Linux operating systems, the rmmod command removes a module from the Linux kernel. With this command, the module_exit macro will be invoked first, which will then call the cleanup_module.

8. The VMX features our above file will now logged in the Kernel lod and could be checked with the below command:  
dmesg  
  
Output:  
command: sudo insmod ./cmpe283.ko  
dmesg  
  
[  808.523540] cmpe283_1: loading out-of-tree module taints kernel.  
[  808.523544] cmpe283_1: module license 'unspecified' taints kernel.    
[  808.523545] Disabling lock debugging due to kernel taint.   
[  808.523593] cmpe283_1: module verification failed: signature and/or required key missing - tainting kernel.  
[  808.525613] CMPE 283 Assignment 1 Module Start.  
[  808.525617] Pinbased Controls MSR: 0x3f00000016.  
[  808.525619]   External Interrupt Exiting: Can set=Yes, Can clear=Yes.  
[  808.525619]   NMI Exiting: Can set=Yes, Can clear=Yes.  
[  808.525620]   Virtual NMIs: Can set=Yes, Can clear=Yes.  
[  808.525621]   Activate VMX Preemption Timer: Can set=No, Can clear=Yes.  
[  808.525622]   Process Posted Interrupts: Can set=No, Can clear=Yes.  
[  808.525623] Procbased Controls MSR: 0xfff9fffe0401e172.  
[  808.525624]    Interrupt-window exiting: Can set=Yes, Can clear=Yes.  
[  808.525624]   Use TSC offsetting : Can set=Yes, Can clear=Yes.  
[  808.525625]   HLT exiting : Can set=Yes, Can clear=Yes.  
[  808.525626]   INVLPG exiting : Can set=Yes, Can clear=Yes.   
[  808.525626]   MWAIT exiting: Can set=Yes, Can clear=Yes.   
[  808.525627]   RDPMC exiting: Can set=Yes, Can clear=Yes.   
[  808.525627]   RDTSC exiting: Can set=Yes, Can clear=Yes.   
[  808.525628]   CR3-load exiting: Can set=Yes, Can clear=No.   
[  808.525629]   CR3-store exiting: Can set=Yes, Can clear=No.   
[  808.525629]   CR8-load exiting: Can set=Yes, Can clear=Yes.   
[  808.525630]   CR8-store exiting: Can set=Yes, Can clear=Yes.   
[  808.525631]   Use TPR shadow : Can set=Yes, Can clear=Yes.  
[  808.525631]   NMI-window exiting: Can set=Yes, Can clear=Yes.  
[  808.525632]   MOV-DR exiting: Can set=Yes, Can clear=Yes.  
[  808.525633]   Unconditional I/O exiting: Can set=Yes, Can clear=Yes  
[  808.525633]   Use I/O bitmaps : Can set=Yes, Can clear=Yes  
[  808.525634]   Monitor trap flag : Can set=Yes, Can clear=Yes  
[  808.525634]   Use MSR bitmaps: Can set=Yes, Can clear=Yes   
[  808.525635]   MONITOR exiting: Can set=Yes, Can clear=Yes   
[  808.525636]   PAUSE exiting: Can set=Yes, Can clear=Yes.  
[  808.525636]   Activate secondary controls: Can set=Yes, Can clear=Yes.  
[  808.525637] Secondary procbased controls are available.  
[  808.525639] Secondary procbased Controls MSR: 0x53cfe00000000  
[  808.525639]    Virtualize APIC accesses: Can set=No, Can clear=Yes.  
[  808.525640]   Enable EPT : Can set=Yes, Can clear=Yes.  
[  808.525641]   Descriptor-table exiting : Can set=Yes, Can clear=Yes.  
[  808.525641]   Enable RDTSCP : Can set=Yes, Can clear=Yes.  
[  808.525642]   Virtualize x2APIC mode: Can set=Yes, Can clear=Yes.  
[  808.525642]   Enable VPID: Can set=Yes, Can clear=Yes.  
[  808.525643]   WBINVD exiting: Can set=Yes, Can clear=Yes.  
[  808.525644]   Unrestricted guest: Can set=Yes, Can clear=Yes.  
[  808.525644]   APIC-register virtualization: Can set=No, Can clear=Yes.  
[  808.525645]   Virtual-interrupt delivery: Can set=No, Can clear=Yes.  
[  808.525646]   PAUSE-loop exiting: Can set=Yes, Can clear=Yes.  
[  808.525646]   RDRAND exiting : Can set=Yes, Can clear=Yes.  
[  808.525647]   Enable INVPCID: Can set=Yes, Can clear=Yes.  
[  808.525647]   Enable VM functions: Can set=Yes, Can clear=Yes.  
[  808.525648]   VMCS shadowing: Can set=No, Can clear=Yes.  
[  808.525649]   Enable ENCLS exiting : Can set=No, Can clear=Yes.  
[  808.525649]   RDSEED exiting : Can set=Yes, Can clear=Yes.  
[  808.525650]   Enable PML: Can set=No, Can clear=Yes.  
[  808.525651]   EPT-violation #VE: Can set=Yes, Can clear=Yes.  
[  808.525651]   Conceal VMX non-root operation from Intel PT: Can set=No, Can clear=Yes.  
[  808.525652]   Enable XSAVES/XRSTORS: Can set=No, Can clear=Yes.  
[  808.525653]   Mode-based execution control for EPT: Can set=No, Can clear=Yes.  
[  808.525653]   Use TSC scaling: Can set=No, Can clear=Yes.  
[  808.525655] Entry Controls MSR: 0xf3ff000011f  
[  808.525655]   Load debug controls: Can set=Yes, Can clear=No.  
[  808.525656]   IA-32e mode guest: Can set=Yes, Can clear=Yes.  
[  808.525657]   Entry to SMM: Can set=No, Can clear=Yes.  
[  808.525657]   Deactivate dual-monitor treatment : Can set=No, Can clear=Yes.  
[  808.525658]   Load IA32_PERF_GLOBAL_CTRL: Can set=Yes, Can clear=Yes.  
[  808.525659]   Load IA32_PAT: Can set=Yes, Can clear=Yes.  
[  808.525659]   Load IA32_EFER: Can set=Yes, Can clear=Yes.  
[  808.525660]   Load IA32_BNDCFGS: Can set=No, Can clear=Yes.  
[  808.525661]   Conceal VM entries from intel PT: Can set=No, Can clear=Yes.  
[  808.525662] Exit Controls MSR: 0x3fffff00036dff  
[  808.525662]   Save debug controls: Can set=Yes, Can clear=No. 
[  808.525663]   Host address-space size: Can set=Yes, Can clear=Yes.  
[  808.525664]   Load IA32_PERF_GLOB AL_CTRL: Can set=Yes, Can clear=Yes.  
[  808.525664]   Acknowledge interrupt on exit : Can set=Yes, Can clear=Yes.  
[  808.525665]   Save IA32_PAT: Can set=Yes, Can clear=Yes.  
[  808.525665]   Load IA32_PAT: Can set=Yes, Can clear=Yes.  
[  808.525666]   Save IA32_EFER: Can set=Yes, Can clear=Yes.  
  
  
  

command:  sudo rmmod ./cmpe283.k  
dmesg  

[  808.525613] CMPE 283 Assignment 1 Module Start.  
[  808.525617] Pinbased Controls MSR: 0x3f00000016.  
[  808.525619]   External Interrupt Exiting: Can set=Yes, Can clear=Yes.  
[  808.525619]   NMI Exiting: Can set=Yes, Can clear=Yes.  
[  808.525620]   Virtual NMIs: Can set=Yes, Can clear=Yes.  
[  808.525621]   Activate VMX Preemption Timer: Can set=No, Can clear=Yes. 
[  808.525622]   Process Posted Interrupts: Can set=No, Can clear=Yes.  
[  808.525623] Procbased Controls MSR: 0xfff9fffe0401e172.  
[  808.525624]    Interrupt-window exiting: Can set=Yes, Can clear=Yes.  
[  808.525624]   Use TSC offsetting : Can set=Yes, Can clear=Yes.  
[  808.525625]   HLT exiting : Can set=Yes, Can clear=Yes.  
[  808.525626]   INVLPG exiting : Can set=Yes, Can clear=Yes.  
[  808.525626]   MWAIT exiting: Can set=Yes, Can clear=Yes.  
[  808.525627]   RDPMC exiting: Can set=Yes, Can clear=Yes.  
[  808.525627]   RDTSC exiting: Can set=Yes, Can clear=Yes.  
[  808.525628]   CR3-load exiting: Can set=Yes, Can clear=No.  
[  808.525629]   CR3-store exiting: Can set=Yes, Can clear=No.  
[  808.525629]   CR8-load exiting: Can set=Yes, Can clear=Yes.  
[  808.525630]   CR8-store exiting: Can set=Yes, Can clear=Yes.  
[  808.525631]   Use TPR shadow : Can set=Yes, Can clear=Yes.  
[  808.525631]   NMI-window exiting: Can set=Yes, Can clear=Yes.  
[  808.525632]   MOV-DR exiting: Can set=Yes, Can clear=Yes.  
[  808.525633]   Unconditional I/O exiting: Can set=Yes, Can clear=Yes.  
[  808.525633]   Use I/O bitmaps : Can set=Yes, Can clear=Yes.  
[  808.525634]   Monitor trap flag : Can set=Yes, Can clear=Yes.  
[  808.525634]   Use MSR bitmaps: Can set=Yes, Can clear=Yes.  
[  808.525635]   MONITOR exiting: Can set=Yes, Can clear=Yes.  
[  808.525636]   PAUSE exiting: Can set=Yes, Can clear=Yes.  
[  808.525636]   Activate secondary controls: Can set=Yes, Can clear=Yes.  
[  808.525637] Secondary procbased controls are available.   
[  808.525639] Secondary procbased Controls MSR: 0x53cfe00000000.  
[  808.525639]    Virtualize APIC accesses: Can set=No, Can clear=Yes.  
[  808.525640]   Enable EPT : Can set=Yes, Can clear=Yes.  
[  808.525641]   Descriptor-table exiting : Can set=Yes, Can clear=Yes.  
[  808.525641]   Enable RDTSCP : Can set=Yes, Can clear=Yes.  
[  808.525642]   Virtualize x2APIC mode: Can set=Yes, Can clear=Yes.  
[  808.525642]   Enable VPID: Can set=Yes, Can clear=Yes.  
[  808.525643]   WBINVD exiting: Can set=Yes, Can clear=Yes.  
[  808.525644]   Unrestricted guest: Can set=Yes, Can clear=Yes.  
[  808.525644]   APIC-register virtualization: Can set=No, Can clear=Yes.  
[  808.525645]   Virtual-interrupt delivery: Can set=No, Can clear=Yes.  
[  808.525646]   PAUSE-loop exiting: Can set=Yes, Can clear=Yes.  
[  808.525646]   RDRAND exiting : Can set=Yes, Can clear=Yes.  
[  808.525647]   Enable INVPCID: Can set=Yes, Can clear=Yes.  
[  808.525647]   Enable VM functions: Can set=Yes, Can clear=Yes.  
[  808.525648]   VMCS shadowing: Can set=No, Can clear=Yes.  
[  808.525649]   Enable ENCLS exiting : Can set=No, Can clear=Yes.  
[  808.525649]   RDSEED exiting : Can set=Yes, Can clear=Yes.  
[  808.525650]   Enable PML: Can set=No, Can clear=Yes.  
[  808.525651]   EPT-violation #VE: Can set=Yes, Can clear=Yes.  
[  808.525651]   Conceal VMX non-root operation from Intel PT: Can set=No, Can clear=Yes.  
[  808.525652]   Enable XSAVES/XRSTORS: Can set=No, Can clear=Yes.  
[  808.525653]   Mode-based execution control for EPT: Can set=No, Can clear=Yes.  
[  808.525653]   Use TSC scaling: Can set=No, Can clear=Yes.  
[  808.525655] Entry Controls MSR: 0xf3ff000011ff.  
[  808.525655]   Load debug controls: Can set=Yes, Can clear=No.  
[  808.525656]   IA-32e mode guest: Can set=Yes, Can clear=Yes.  
[  808.525657]   Entry to SMM: Can set=No, Can clear=Yes.  
[  808.525657]   Deactivate dual-monitor treatment : Can set=No, Can clear=Yes.  
[  808.525658]   Load IA32_PERF_GLOBAL_CTRL: Can set=Yes, Can clear=Yes.  
[  808.525659]   Load IA32_PAT: Can set=Yes, Can clear=Yes.  
[  808.525659]   Load IA32_EFER: Can set=Yes, Can clear=Yes.  
[  808.525660]   Load IA32_BNDCFGS: Can set=No, Can clear=Yes.  
[  808.525661]   Conceal VM entries from intel PT: Can set=No, Can clear=Yes.  
[  808.525662] Exit Controls MSR: 0x3fffff00036dff.  
[  808.525662]   Save debug controls: Can set=Yes, Can clear=No.  
[  808.525663]   Host address-space size: Can set=Yes, Can clear=Yes.  
[  808.525664]   Load IA32_PERF_GLOB AL_CTRL: Can set=Yes, Can clear=Yes.  
[  808.525664]   Acknowledge interrupt on exit : Can set=Yes, Can clear=Yes.  
[  808.525665]   Save IA32_PAT: Can set=Yes, Can clear=Yes.  
[  808.525665]   Load IA32_PAT: Can set=Yes, Can clear=Yes.  
[  808.525666]   Save IA32_EFER: Can set=Yes, Can clear=Yes.  
[ 1093.267522] CMPE 283 Assignment 1 Module Exits.  
