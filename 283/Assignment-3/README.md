<h1>Assignment-03</h1>
    
    
<h3>Question 2 : Steps followed:</h3>
    
1. Run the assignment-2 environment. <br>
    
2. Navigate to ~/linux/arch/x86/kvm/cpuid.c and edit the code block. Put another if..else condition for when eax = 0x4ffffffd. <br>
	  
	  	else if(eax == 0x4ffffffd) 
        {

		//reasons not in SDM
		if(ecx==35 || ecx==38 || ecx==42 || ecx==65 || ecx>68 || ecx<0){
			printk(KERN_INFO "exit reason number = %u not defined by SDM",ecx);
			eax=0;
			ebx=0;
			ecx=0;
			edx=0xffffffff;
		}
		else if( ecx==5 || ecx==6 || ecx==11 || ecx==17 ||  ecx==35 || ecx==38 || ecx==42 || ecx==66){
				printk(KERN_INFO"exit reason number =%u not enabled in KVM",ecx);
				eax=ebx=ecx=edx=0;
			}
		else{
				printk(KERN_INFO "CPUID(0x4ffffffd), exit number=%u exits=%d\n",ecx,arch_atomic_read(&exitsPerReason[ecx]));
				eax=atomic_read(&exitsPerReason[(int)ecx]);
				ebx=ecx=edx=0;
			}
		} 
     
3. Make the necessary changes in vmx.c as well (variable declarations)<br>
4. Make changes for code block and write if..else condition for when eax = 0x4ffffffc. <br>
	  
    		else if(eax == 0x4ffffffc)
        {

		if(ecx==35 || ecx==38 || ecx==42 || ecx==65 || ecx>68 || ecx<0){
                        printk(KERN_INFO "exit reason number = %u not defined by SDM",ecx);
                        eax=0;
                        ebx=0;
                        ecx=0;
                        edx=0xffffffff;
                }
                else if( ecx==5 || ecx==6 || ecx==11 || ecx==17 ||  ecx==35 || ecx==38 || ecx==42 || ecx==66){
                                printk(KERN_INFO"exit reason number =%u not enabled in KVM",ecx);
                                eax=ebx=ecx=edx=0;
		}
                                                                     
5. Save the changes and run the below commands in order as mentioned.<br>
    ``` sudo make -j 16 modules ``` <br>
    ``` sudo make -j 16 ```                                                             
    ``` sudo make INSTALL_MOD_STRIP=1 modules_install ```                                                          
    ``` rmmod kvm_intel ```<br>
    ``` rmmod kvm ```<br>
    ``` modprobe kvm_intel ``` <br>
    ``` modprobe kvm ``` <br>
                                                                     
6. Now run the nested VM and run test script or ``` cpuid -l 0x4ffffffd -s <exit reason> ``` to verify output for different exit reasons.<br>   
    
7. Run dmesg in the host VM to get output. <br>  
	 
<h3>Answer to Questions:</h3>
	  <h4>Question-3</h4>
	  
We noticed that the count increased at a stable rate. Below are the screenshots for exit reason=0. The exit count is currently 12035. <br>
		  
We did another reboot to see the increase in count of exits. The exit count raised to 24070 which is exactly the double of what it was previously. <br>
	  
We did one more reboot to find tha rate of increase. The number of exits is now 36105. Which is three times of the first output. Which conculdes that the number of exits is increasing at a stable rate.<br>
	  
For exit reason=0, the number of exits increase by approximately 12k on each boot.<br>	  
                           
<h4>Question-4</h4>
	  
The most frequent exits were noticed for exit reason =48.<br>
	  
There were many exit reasons with 0 exits (least frequent). The full dmesg output is in the test3.txt /CMPE-283-Assignment-3 folder.	  <br>
