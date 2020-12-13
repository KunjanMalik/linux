# CMPE 283 Assignment 3
Team: Shivam Shrivastav and Kunajn Malik 

## Setup Steps

The basic steps and pre-requisite is same as assignment 2 (https://github.com/KunjanMalik/linux/blob/master/README.md).

For our code changes needed for assignment 3, you can refer the following files:   \
~linux/arch/x86/kvm/cupid.c  
~linux/arch/x86/kvm/vmx/vmx.c   
To run the entre setup, please follow above steps assignment 2 readme and next steps as mentioned above. After this execute the below test script code mentioned in test script section and follow the all the required steps.  

We ran the following command in host machine under Linux directory:  
-sudo rmmod kvm_intel -sudo rmmod kvm 
-sudo make SUBDIRS= arch/x86/kvm/  
-sudo insmod arch/x86/kvm/kvm.ko  
-sudo insmod arch/x86/kvm/kvm-intel.ko  



We started the guest or inner VM from virt manager and checked the dmesg command in our outer VM.   

## Create a shell test script file(test.c) as below inside inner VM (once inner VM is up).

For the script we took reference from stackoverflow: https://stackoverflow.com/questions/21422433/how-does-the-linux-kernel-get-info-about-the-processors-and-the-cores  and build our test script as below.  
  
![Screenshot 2020-12-13 at 4 32 49 PM](https://user-images.githubusercontent.com/24988178/102010021-4e513900-3d61-11eb-94b2-d758be5f3587.png)

We have checked the output of number of VM exit per reason and over inner VM as below:

  


We checked at Host machine with “dmesg” command 







