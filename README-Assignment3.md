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

## Create a test script file(test_script.c) as below inside inner VM (once inner VM is up).

For the script we took reference from stackoverflow: https://stackoverflow.com/questions/21422433/how-does-the-linux-kernel-get-info-about-the-processors-and-the-cores  and build our test script with recurrsive function to print all exit reason's count as below.  
![test script](https://user-images.githubusercontent.com/24988178/102020963-8af15480-3da2-11eb-82d3-034a1e3cdb3c.png) 


We have checked the output of number of VM exit per reason and over inner VM as below by running our test script:  
gcc test_script.c -o test_script  
./test_script  

![test script output no ept](https://user-images.githubusercontent.com/24988178/102020723-081bca00-3da1-11eb-8798-bb5317e7c936.png)

  


We checked at Host machine with “dmesg” command 

![no ept dmesg screen shot](https://user-images.githubusercontent.com/24988178/102020634-61cfc480-3da0-11eb-8c66-dde1208826c3.png)

# Questions


1.	For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched.  
Answer: We worked in a team of 2.  
Shivam Shrivastav and Kunjan Malik.  
We both did the other assignments together and researched together about this assignment as well.  
Shivam researched about all the exits reasons NOT enabled/handled in SDM.  
Kunjan researched about all the exits reasons NOT enabled/handled in KVM.  
The results were noted and we handled each of the “if” conditions in the requirement in our code base (cupid.c and vmx.c).   
  
First we built the solution separately without the atomic read to check if we are at least able to achieve the main requirement.
We shared the load, where Kunjan worked on the total exits per reason increment part in vmx.c and if conditions for KVM NOT enabled exits in cpuid. Shivam worked on building a new function to increment the total exit per reason and handled if condition of the exit reasons not enabled in SDM in cupid.c. We then integrated our parts and worked together to build a complete solution.  
After the successful results, we worked on handling concurrent exits in multi-VCPU VM . This was achieved using atomic read and atomic increment functionality. With this, we built the complete solution and tested our output using the above mentioned test script.  


2.	Describe in detail the steps you used to complete the assignment. Consider your reader to be someone skilled in software development but otherwise unfamiliar with the assignment. Good answers to this question will be recipes that someone can follow to reproduce your development steps.  
Answer: The basic steps and pre-requisite is same as assignment 2 (https://github.com/KunjanMalik/linux/blob/master/README.md ).  
For our code changes needed for assignment 3, you can refer the following files:  
~linux/arch/x86/kvm/cupid.c   
~linux/arch/x86/kvm/vmx/vmx.c  
To run the entre setup, please follow above steps assignment 2 readme and next steps as mentioned above. After this execute the below test script code mentioned in test script section and follow the all the required steps.   

3.	Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?  
Answer: Exits seems to increase but it does not seem to be increasing at a stable rate. When we run a variety of VM instructions in our inner VM, say unauthorized file access, playing video on the VM or internet or running cupid in guest VM or other VM operations, more number of VM exits occur due to Page Fault, exceptions etc accordingly.
In our setup, approximately, 1323281 exits occured after VM boot.   


4. Of the exit types defined in the SDM, which are the most frequent? Least?

Answer: There are many exits increasing where 48 is the most frequent exit reason. Even 10 and 30 are the second most frequent exit. Many of the exits have count as 0 like: Exit Reason 2, 3, 63, 64 and 67 etc, so theyare the ones with least number. 






