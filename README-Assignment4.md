# CMPE 283 Assignment 3
Team: Shivam Shrivastav and Kunajn Malik 

## Setup Steps
Setup steps are same as Assignment 3. Please refer the details here (https://github.com/KunjanMalik/linux/blob/master/README-Assignment3.md).  

To run the setup, we followed the below:  

Run above assignment 3 code and boot inner VM using this code.  
Once the inner VM has booted, we recorded total exit count information by running our test script in inner VM.
Output is shown below in questions
Remove the ‘kvm-intel’ module from running kernel:
rmmod kvm-intel.  

Reload the kvm-intel module with the parameter ept=0 (this will disable nested paging and force KVM to use shadow paging instead).  
insmod /lib/modules/5.8.9/kernel/arch/x86/kvm/kvm-intel.ko ept=0  

Boot the inner VM again, and captured the same output.
Output is shown below in questions.
  
  
# Questions

1. For each member in your team, provide 1 paragraph detailing what parts of the lab that member implemented / researched. (You may skip this question if you are doing the lab by yourself).  
Answer: Answer: We worked in a team of 2.  
Shivam Shrivastav and Kunjan Malik.  
We both did the other assignments together and researched together about this assignment as well.  
Since there was no code changes involved, we mostly did this assignment together over zoom call.
Output without ept (default ept=1 nested paging) was taken by Kunjan and output for ept=0(shadow paging) for taken by Shivam).  
  
  
2. Include a sample of your print of exit count output from dmesg from “with ept” and “without ept”.  
Output(no ept(ept=1): Nested Paging):    
![test script output no ept](https://user-images.githubusercontent.com/24988178/102020723-081bca00-3da1-11eb-8798-bb5317e7c936.png)  
  
    
Output(ept=0 : Shadow Paging)  
  
  
3. What did you learn from the count of exits? Was the count what you expected? If not, why not?  
Answer:   
Total Count of Exits without NO EPT (ept=1): 1323281  
![no ept dmesg](https://user-images.githubusercontent.com/24988178/102020507-a27b0e00-3d9f-11eb-95f0-346c27429d39.png)
Total Count of Exits with ept=0 : 1462763  
![ept 0 dmesg](https://user-images.githubusercontent.com/24988178/102021989-d65b3100-3da9-11eb-99ee-6735f4fe77f0.png)  

The count of exits during Nested paging(without ept) was less in comparision to the total exit count when we ran the command with ept=0(shadow paging).
The exit count was increase with ept=0 due to shadow paging. It was expected as per the class lecture and reasearch during final exam.


4. What changed between the two runs (ept vs no-ept)?  
Answer: When we run the command with ept=0(in shadow paging), there are more number of exits as compared to the running without ept in nested paging.
