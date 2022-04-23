<h1> ASSIGNMENT - 1 <h1>

<h3>Steps followed:</h3>
1. Retrieve the starter .c file and Mekefile from canvas. <br>
2. Add the remaining code sections to the file.(struct, definitions and vm features)<br>


| MSR Name |	MSR Index |	Description |	References |
| :--- | :--- | :--- | :--- |
| IA32_VMX_PINBASED_CTLS |	0x481 |	This MSR is used for pinbased controls if no true controls capability |	SDM volume 3C, section 24.6.1 |
| IA32_VMX_PROCBASED_CTLS |	0x482 |	This MSR is used for primary procbased controls if no true controls capability |	SDM volume 3C, section 24.6.2 |
| IA32_VMX_PROCBASED_CTLS2 |	0x48B |	This MSR is used for secondary procbased controls if available |	SDM volume 3C, section 24.6.2 |
| IA32_VMX_EXIT_CTLS |	0x483 |	This MSR is used for exit controls if no true controls capability |	SDM volume 3C, section 24.7.1 |
| IA32_VMX_ENTRY_CTLS |	0x484 |	This MSR is used for entry controls if no true controls capability |	SDM volume 3C, section 24.8.1 |

<br>

3. Create a GitHub repo, fork the torvalds:master repo and create a folder CMPE 283 inside the linux repo.<br>
4. Upload the edits .c file and Makefile to the folder to get started with building the kernel.<br>
5. Create a GCP account and set up a VM compute instance.<br>
6. Create an N2 type- Ubuntu 20.04.3 virtual machine (15 GB RAM, 4 vcpus and 100 GB HDD) in Las Vegas region. Make sure the nested virtualization is enabled on the machine. <br>
7. Clone the github repo created in the VM instance.<br>
8. Run the ```make``` command.Running this command initially gave us a lot of package dependency errors.<br>
9. Install the packages by ```apt-get install <package name>```. To name a few, we had to install flex, bison, binutils, libelf and libssl.<br> 
10. Had to run ```make oldconfig``` and ```make prepare``` in order to build the entire kernel in the cloned source code repo.<br>
11. The ```make modules``` and ```make``` command then ran successfully. It took about 1.5 hours for it to complete.<br>
12. Copy all the modules built to the boot location. (strip debugging information)<br>
13. Run the ```make install``` command to install the kernel on the system.<br>
14. Reboot the VM so the the newly installed kernel boots up.<br>
15. Run the ```make``` command in the root directory. (the output screenshot is attached below). Include the MODULE_LICENSE line in the cmpe283-1.c file.<br> 
16. Run the ```sudo insmod cmpe283-1.ko``` command to insert modules in the kernel.<br>
17. Run the ``` dmesg ``` command to display the output printed on the system message buffer console.<br>
