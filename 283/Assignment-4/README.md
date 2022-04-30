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


<h1>Assignment-04</h1>
	
<h3>Question 2: Include a sample of your print of exit count output from dmesg from “with ept” and “without ept”.</h3>

<h4> Output when EPT=0 (Shadow Paging)</h4>
	
[11636.050289] virbr0: port 2(vnet0) entered learning state
[11638.066133] virbr0: port 2(vnet0) entered forwarding state
[11638.066161] virbr0: topology change detected, propagating
[11751.330698] CPUID(0x4ffffffd), exit number=0 exits=857331
[11751.332459] CPUID(0x4ffffffd), exit number=0 exits=857357
[11751.405178] CPUID(0x4ffffffd), exit number=1 exits=0
[11751.406299] CPUID(0x4ffffffd), exit number=1 exits=0
[11751.464722] CPUID(0x4ffffffd), exit number=2 exits=0
[11751.466825] CPUID(0x4ffffffd), exit number=2 exits=0
[11751.524842] CPUID(0x4ffffffd), exit number=3 exits=0
[11751.527192] CPUID(0x4ffffffd), exit number=3 exits=0
[11751.567062] CPUID(0x4ffffffd), exit number=4 exits=0
[11751.569433] CPUID(0x4ffffffd), exit number=4 exits=0
[11751.613196] exit reason number =5 not enabled in KVM
[11751.618585] exit reason number =5 not enabled in KVM
[11751.665479] exit reason number =6 not enabled in KVM
[11751.668092] exit reason number =6 not enabled in KVM
[11751.712311] CPUID(0x4ffffffd), exit number=7 exits=0
[11751.714931] CPUID(0x4ffffffd), exit number=7 exits=0
[11751.760569] CPUID(0x4ffffffd), exit number=8 exits=0
[11751.762946] CPUID(0x4ffffffd), exit number=8 exits=0
[11751.809424] CPUID(0x4ffffffd), exit number=9 exits=0
[11751.817488] CPUID(0x4ffffffd), exit number=9 exits=0
[11751.884645] CPUID(0x4ffffffd), exit number=10 exits=280938
[11751.886977] CPUID(0x4ffffffd), exit number=10 exits=280939
[11751.946971] exit reason number =11 not enabled in KVM
[11751.951597] exit reason number =11 not enabled in KVM
[11752.013195] CPUID(0x4ffffffd), exit number=12 exits=0
[11752.018874] CPUID(0x4ffffffd), exit number=12 exits=0
[11752.068463] CPUID(0x4ffffffd), exit number=13 exits=0
[11752.070994] CPUID(0x4ffffffd), exit number=13 exits=0
[11752.126975] CPUID(0x4ffffffd), exit number=14 exits=101493
[11752.129880] CPUID(0x4ffffffd), exit number=14 exits=101493
[11752.179212] CPUID(0x4ffffffd), exit number=15 exits=0
[11752.181913] CPUID(0x4ffffffd), exit number=15 exits=0
[11752.230499] CPUID(0x4ffffffd), exit number=16 exits=0
[11752.233053] CPUID(0x4ffffffd), exit number=16 exits=0
[11752.280940] exit reason number =17 not enabled in KVM
[11752.283313] exit reason number =17 not enabled in KVM
[11752.353174] CPUID(0x4ffffffd), exit number=18 exits=0
[11752.360194] CPUID(0x4ffffffd), exit number=18 exits=0
[11752.414210] CPUID(0x4ffffffd), exit number=19 exits=0
[11752.416669] CPUID(0x4ffffffd), exit number=19 exits=0
[11752.467828] CPUID(0x4ffffffd), exit number=20 exits=0
[11752.470594] CPUID(0x4ffffffd), exit number=20 exits=0
[11752.523694] CPUID(0x4ffffffd), exit number=21 exits=0
[11752.526809] CPUID(0x4ffffffd), exit number=21 exits=0
[11752.574251] CPUID(0x4ffffffd), exit number=22 exits=0
[11752.576721] CPUID(0x4ffffffd), exit number=22 exits=0
[11752.626263] CPUID(0x4ffffffd), exit number=23 exits=0
[11752.628724] CPUID(0x4ffffffd), exit number=23 exits=0
[11752.676719] CPUID(0x4ffffffd), exit number=24 exits=0
[11752.679598] CPUID(0x4ffffffd), exit number=24 exits=0
[11752.732735] CPUID(0x4ffffffd), exit number=25 exits=0
[11752.735638] CPUID(0x4ffffffd), exit number=25 exits=0
[11752.800048] CPUID(0x4ffffffd), exit number=26 exits=0
[11752.802676] CPUID(0x4ffffffd), exit number=26 exits=0
[11752.855390] CPUID(0x4ffffffd), exit number=27 exits=0
[11752.858905] CPUID(0x4ffffffd), exit number=27 exits=0
[11752.916962] CPUID(0x4ffffffd), exit number=28 exits=5732292
[11752.919611] CPUID(0x4ffffffd), exit number=28 exits=5732330
[11752.975480] CPUID(0x4ffffffd), exit number=29 exits=4
[11752.978331] CPUID(0x4ffffffd), exit number=29 exits=4
[11753.026597] CPUID(0x4ffffffd), exit number=30 exits=295253
[11753.028973] CPUID(0x4ffffffd), exit number=30 exits=295253
[11753.084651] CPUID(0x4ffffffd), exit number=31 exits=3306
[11753.087285] CPUID(0x4ffffffd), exit number=31 exits=3306
[11753.136833] CPUID(0x4ffffffd), exit number=32 exits=0
[11753.139317] CPUID(0x4ffffffd), exit number=32 exits=0
[11753.188618] CPUID(0x4ffffffd), exit number=33 exits=0
[11753.191451] CPUID(0x4ffffffd), exit number=33 exits=0
[11753.244826] CPUID(0x4ffffffd), exit number=34 exits=0
[11753.250713] CPUID(0x4ffffffd), exit number=34 exits=0
[11753.309651] exit reason number = 35 not defined by SDM
[11753.312683] exit reason number = 35 not defined by SDM
[11753.368740] CPUID(0x4ffffffd), exit number=36 exits=0
[11753.371238] CPUID(0x4ffffffd), exit number=36 exits=0
[11753.426294] CPUID(0x4ffffffd), exit number=37 exits=0
[11753.428755] CPUID(0x4ffffffd), exit number=37 exits=0
[11753.489082] exit reason number = 38 not defined by SDM
[11753.491648] exit reason number = 38 not defined by SDM
[11753.541038] CPUID(0x4ffffffd), exit number=39 exits=0
[11753.546747] CPUID(0x4ffffffd), exit number=39 exits=0
[11753.592714] CPUID(0x4ffffffd), exit number=40 exits=0
[11753.595767] CPUID(0x4ffffffd), exit number=40 exits=0
[11753.649244] CPUID(0x4ffffffd), exit number=41 exits=0
[11753.654759] CPUID(0x4ffffffd), exit number=41 exits=0
[11753.713043] exit reason number = 42 not defined by SDM
[11753.715842] exit reason number = 42 not defined by SDM
[11753.758275] CPUID(0x4ffffffd), exit number=43 exits=0
[11753.760922] CPUID(0x4ffffffd), exit number=43 exits=0
[11753.808958] CPUID(0x4ffffffd), exit number=44 exits=0
[11753.811876] CPUID(0x4ffffffd), exit number=44 exits=0
[11753.862609] CPUID(0x4ffffffd), exit number=45 exits=0
[11753.865242] CPUID(0x4ffffffd), exit number=45 exits=0
[11753.925208] CPUID(0x4ffffffd), exit number=46 exits=12
[11753.927955] CPUID(0x4ffffffd), exit number=46 exits=12
[11753.982791] CPUID(0x4ffffffd), exit number=47 exits=4
[11753.985547] CPUID(0x4ffffffd), exit number=47 exits=4
[11754.036773] CPUID(0x4ffffffd), exit number=48 exits=383853
[11754.039340] CPUID(0x4ffffffd), exit number=48 exits=383853
[11754.089224] CPUID(0x4ffffffd), exit number=49 exits=0
[11754.092178] CPUID(0x4ffffffd), exit number=49 exits=0
[11754.143197] CPUID(0x4ffffffd), exit number=50 exits=0
[11754.146243] CPUID(0x4ffffffd), exit number=50 exits=0
[11754.195636] CPUID(0x4ffffffd), exit number=51 exits=0
[11754.197892] CPUID(0x4ffffffd), exit number=51 exits=0
[11754.253058] CPUID(0x4ffffffd), exit number=52 exits=0
[11754.255494] CPUID(0x4ffffffd), exit number=52 exits=0
[11754.321857] CPUID(0x4ffffffd), exit number=53 exits=0
[11754.327709] CPUID(0x4ffffffd), exit number=53 exits=0
[11754.428558] CPUID(0x4ffffffd), exit number=54 exits=7
[11754.431221] CPUID(0x4ffffffd), exit number=54 exits=7
[11754.485829] CPUID(0x4ffffffd), exit number=55 exits=6
[11754.494270] CPUID(0x4ffffffd), exit number=55 exits=6
[11754.556811] CPUID(0x4ffffffd), exit number=56 exits=0
[11754.562771] CPUID(0x4ffffffd), exit number=56 exits=0
[11754.617215] CPUID(0x4ffffffd), exit number=57 exits=0
[11754.622970] CPUID(0x4ffffffd), exit number=57 exits=0
[11754.677307] CPUID(0x4ffffffd), exit number=58 exits=65707
[11754.680163] CPUID(0x4ffffffd), exit number=58 exits=65707
[11754.729595] CPUID(0x4ffffffd), exit number=59 exits=0
[11754.734958] CPUID(0x4ffffffd), exit number=59 exits=0
[11754.783854] CPUID(0x4ffffffd), exit number=60 exits=0
[11754.786558] CPUID(0x4ffffffd), exit number=60 exits=0
[11754.837116] CPUID(0x4ffffffd), exit number=61 exits=0
[11754.842477] CPUID(0x4ffffffd), exit number=61 exits=0
[11754.896569] CPUID(0x4ffffffd), exit number=62 exits=0
[11754.899312] CPUID(0x4ffffffd), exit number=62 exits=0
[11754.945960] CPUID(0x4ffffffd), exit number=63 exits=0
[11754.951421] CPUID(0x4ffffffd), exit number=63 exits=0
[11754.996498] CPUID(0x4ffffffd), exit number=64 exits=0
[11754.998945] CPUID(0x4ffffffd), exit number=64 exits=0
[11755.055576] exit reason number = 65 not defined by SDM
[11755.058256] exit reason number = 65 not defined by SDM
[11755.112997] exit reason number =66 not enabled in KVM
[11755.118547] exit reason number =66 not enabled in KVM
[11755.168758] CPUID(0x4ffffffd), exit number=67 exits=0
[11755.171976] CPUID(0x4ffffffd), exit number=67 exits=0
[11755.220835] CPUID(0x4ffffffd), exit number=68 exits=0
[11755.223332] CPUID(0x4ffffffd), exit number=68 exits=0

<h4> Output when EPT = non zero (Nested Paging)</h4>
	
[10903.960907] CPUID(0x4ffffffd), exit number=0 exits=12035
[10903.962707] CPUID(0x4ffffffd), exit number=0 exits=12035
[10903.968307] CPUID(0x4ffffffd), exit number=1 exits=0
[10903.970254] CPUID(0x4ffffffd), exit number=1 exits=0
[10903.975591] CPUID(0x4ffffffd), exit number=2 exits=0
[10903.977348] CPUID(0x4ffffffd), exit number=2 exits=0
[10903.984672] CPUID(0x4ffffffd), exit number=3 exits=0
[10903.986171] CPUID(0x4ffffffd), exit number=3 exits=0
[10903.993376] CPUID(0x4ffffffd), exit number=4 exits=0
[10903.994771] CPUID(0x4ffffffd), exit number=4 exits=0
[10904.002180] exit reason number =5 not enabled in KVM
[10904.003582] exit reason number =5 not enabled in KVM
[10904.010992] exit reason number =6 not enabled in KVM
[10904.012503] exit reason number =6 not enabled in KVM
[10904.019835] CPUID(0x4ffffffd), exit number=7 exits=0
[10904.021762] CPUID(0x4ffffffd), exit number=7 exits=0
[10904.026890] CPUID(0x4ffffffd), exit number=8 exits=0
[10904.028543] CPUID(0x4ffffffd), exit number=8 exits=0
[10904.033050] CPUID(0x4ffffffd), exit number=9 exits=0
[10904.034628] CPUID(0x4ffffffd), exit number=9 exits=0
[10904.039164] CPUID(0x4ffffffd), exit number=10 exits=138268
[10904.047243] CPUID(0x4ffffffd), exit number=10 exits=138269
[10904.053293] exit reason number =11 not enabled in KVM
[10904.054854] exit reason number =11 not enabled in KVM
[10904.059591] CPUID(0x4ffffffd), exit number=12 exits=0
[10904.061673] CPUID(0x4ffffffd), exit number=12 exits=0
[10904.066806] CPUID(0x4ffffffd), exit number=13 exits=0
[10904.068410] CPUID(0x4ffffffd), exit number=13 exits=0
[10904.073297] CPUID(0x4ffffffd), exit number=14 exits=0
[10904.074808] CPUID(0x4ffffffd), exit number=14 exits=0
[10904.080230] CPUID(0x4ffffffd), exit number=15 exits=0
[10904.083012] CPUID(0x4ffffffd), exit number=15 exits=0
[10904.089034] CPUID(0x4ffffffd), exit number=16 exits=0
[10904.090568] CPUID(0x4ffffffd), exit number=16 exits=0
[10904.095130] exit reason number =17 not enabled in KVM
[10904.096800] exit reason number =17 not enabled in KVM
[10904.102042] CPUID(0x4ffffffd), exit number=18 exits=0
[10904.103680] CPUID(0x4ffffffd), exit number=18 exits=0
[10904.109893] CPUID(0x4ffffffd), exit number=19 exits=0
[10904.111440] CPUID(0x4ffffffd), exit number=19 exits=0
[10904.115948] CPUID(0x4ffffffd), exit number=20 exits=0
[10904.117653] CPUID(0x4ffffffd), exit number=20 exits=0
[10904.122749] CPUID(0x4ffffffd), exit number=21 exits=0
[10904.124412] CPUID(0x4ffffffd), exit number=21 exits=0
[10904.129099] CPUID(0x4ffffffd), exit number=22 exits=0
[10904.130648] CPUID(0x4ffffffd), exit number=22 exits=0
[10904.135262] CPUID(0x4ffffffd), exit number=23 exits=0
[10904.136899] CPUID(0x4ffffffd), exit number=23 exits=0
[10904.142302] CPUID(0x4ffffffd), exit number=24 exits=0
[10904.143904] CPUID(0x4ffffffd), exit number=24 exits=0
[10904.148401] CPUID(0x4ffffffd), exit number=25 exits=0
[10904.150069] CPUID(0x4ffffffd), exit number=25 exits=0
[10904.154616] CPUID(0x4ffffffd), exit number=26 exits=0
[10904.156227] CPUID(0x4ffffffd), exit number=26 exits=0
[10904.160929] CPUID(0x4ffffffd), exit number=27 exits=0
[10904.163339] CPUID(0x4ffffffd), exit number=27 exits=0
[10904.167945] CPUID(0x4ffffffd), exit number=28 exits=25697
[10904.169669] CPUID(0x4ffffffd), exit number=28 exits=25697
[10904.174173] CPUID(0x4ffffffd), exit number=29 exits=2
[10904.175729] CPUID(0x4ffffffd), exit number=29 exits=2
[10904.180458] CPUID(0x4ffffffd), exit number=30 exits=148345
[10904.182912] CPUID(0x4ffffffd), exit number=30 exits=148346
[10904.187544] CPUID(0x4ffffffd), exit number=31 exits=1531
[10904.189310] CPUID(0x4ffffffd), exit number=31 exits=1531
[10904.193874] CPUID(0x4ffffffd), exit number=32 exits=0
[10904.195431] CPUID(0x4ffffffd), exit number=32 exits=0
[10904.200036] CPUID(0x4ffffffd), exit number=33 exits=0
[10904.202111] CPUID(0x4ffffffd), exit number=33 exits=0
[10904.206880] CPUID(0x4ffffffd), exit number=34 exits=0
[10904.208612] CPUID(0x4ffffffd), exit number=34 exits=0
[10904.213251] exit reason number = 35 not defined by SDM
[10904.214788] exit reason number = 35 not defined by SDM
[10904.219522] CPUID(0x4ffffffd), exit number=36 exits=0
[10904.221619] CPUID(0x4ffffffd), exit number=36 exits=0
[10904.226349] CPUID(0x4ffffffd), exit number=37 exits=0
[10904.227968] CPUID(0x4ffffffd), exit number=37 exits=0
[10904.232649] exit reason number = 38 not defined by SDM
[10904.234277] exit reason number = 38 not defined by SDM
[10904.238853] CPUID(0x4ffffffd), exit number=39 exits=0
[10904.240686] CPUID(0x4ffffffd), exit number=39 exits=0
[10904.245858] CPUID(0x4ffffffd), exit number=40 exits=0
[10904.247502] CPUID(0x4ffffffd), exit number=40 exits=0
[10904.252126] CPUID(0x4ffffffd), exit number=41 exits=0
[10904.253733] CPUID(0x4ffffffd), exit number=41 exits=0
[10904.258199] exit reason number = 42 not defined by SDM
[10904.259752] exit reason number = 42 not defined by SDM
[10904.264645] CPUID(0x4ffffffd), exit number=43 exits=0
[10904.266263] CPUID(0x4ffffffd), exit number=43 exits=0
[10904.270795] CPUID(0x4ffffffd), exit number=44 exits=0
[10904.272393] CPUID(0x4ffffffd), exit number=44 exits=0
[10904.277010] CPUID(0x4ffffffd), exit number=45 exits=0
[10904.278544] CPUID(0x4ffffffd), exit number=45 exits=0
[10904.283432] CPUID(0x4ffffffd), exit number=46 exits=6
[10904.285168] CPUID(0x4ffffffd), exit number=46 exits=6
[10904.289609] CPUID(0x4ffffffd), exit number=47 exits=2
[10904.291152] CPUID(0x4ffffffd), exit number=47 exits=2
[10904.295612] CPUID(0x4ffffffd), exit number=48 exits=383853
[10904.297531] CPUID(0x4ffffffd), exit number=48 exits=383853
[10904.302782] CPUID(0x4ffffffd), exit number=49 exits=0
[10904.304413] CPUID(0x4ffffffd), exit number=49 exits=0
[10904.308946] CPUID(0x4ffffffd), exit number=50 exits=0
[10904.310484] CPUID(0x4ffffffd), exit number=50 exits=0
[10904.315213] CPUID(0x4ffffffd), exit number=51 exits=0
[10904.316917] CPUID(0x4ffffffd), exit number=51 exits=0
[10904.321923] CPUID(0x4ffffffd), exit number=52 exits=0
[10904.323488] CPUID(0x4ffffffd), exit number=52 exits=0
[10904.328008] CPUID(0x4ffffffd), exit number=53 exits=0
[10904.329718] CPUID(0x4ffffffd), exit number=53 exits=0
[10904.334328] CPUID(0x4ffffffd), exit number=54 exits=3
[10904.335877] CPUID(0x4ffffffd), exit number=54 exits=3
[10904.340565] CPUID(0x4ffffffd), exit number=55 exits=3
[10904.342468] CPUID(0x4ffffffd), exit number=55 exits=3
[10904.347119] CPUID(0x4ffffffd), exit number=56 exits=0
[10904.348884] CPUID(0x4ffffffd), exit number=56 exits=0
[10904.353509] CPUID(0x4ffffffd), exit number=57 exits=0
[10904.355085] CPUID(0x4ffffffd), exit number=57 exits=0
[10904.359920] CPUID(0x4ffffffd), exit number=58 exits=0
[10904.361948] CPUID(0x4ffffffd), exit number=58 exits=0
[10904.366627] CPUID(0x4ffffffd), exit number=59 exits=0
[10904.368265] CPUID(0x4ffffffd), exit number=59 exits=0
[10904.373166] CPUID(0x4ffffffd), exit number=60 exits=0
[10904.374705] CPUID(0x4ffffffd), exit number=60 exits=0
[10904.379303] CPUID(0x4ffffffd), exit number=61 exits=0
[10904.381147] CPUID(0x4ffffffd), exit number=61 exits=0
[10904.385819] CPUID(0x4ffffffd), exit number=62 exits=0
[10904.387445] CPUID(0x4ffffffd), exit number=62 exits=0
[10904.391988] CPUID(0x4ffffffd), exit number=63 exits=0
[10904.393721] CPUID(0x4ffffffd), exit number=63 exits=0
[10904.398204] CPUID(0x4ffffffd), exit number=64 exits=0
[10904.399751] CPUID(0x4ffffffd), exit number=64 exits=0
[10904.405133] exit reason number = 65 not defined by SDM
[10904.407222] exit reason number = 65 not defined by SDM
[10904.413891] exit reason number =66 not enabled in KVM
[10904.415453] exit reason number =66 not enabled in KVM
[10904.420321] CPUID(0x4ffffffd), exit number=67 exits=0
[10904.422592] CPUID(0x4ffffffd), exit number=67 exits=0
[10904.428457] CPUID(0x4ffffffd), exit number=68 exits=0
[10904.430113] CPUID(0x4ffffffd), exit number=68 exits=0
	
<h3>Question 3: What did you learn from the count of exits? Was the count what you expected? If not, why not?</h3>

Exit Code | Nested Paging | Shadow Paging
| :---: | :---: | :---:
0 | 12035 | 857331
10 | 138268 | 280938
14 | 0 | 101493
28 | 25697 | 5732292
29 | 2 | 4
30 | 148345 | 295253
31 | 1531 | 3306
46 | 6 | 12
47 | 2 | 4
48 | 383853 | 383853
54 | 3 | 7
55 | 3 | 6
58 | 0 | 65707

As per above observations, the number of exits change for both nested paging and shadow paging. With ept=0 which is shadow paging, the exit counts of exit reason 0, 10, 14, 28, 58 amplifies. This is because there are overheads associated when shadow paging is enabled. It is noticed that the exit code 28 occurs far more frequently in shadow paging mode as compared to nested paging.
	

<h3>Question 4: What changed between the two runs (ept vs no-ept)?</h3>

While comparing the values of total exit count with ept and without ept, we saw that when the parameter ept=0 is changed, the exit counts for few exit numbers significantly increases. The reason being that shadow paging uses a two-layer translation from guest physical address to host physical address and it involved more VMM involvement which causes additional VM exits. The shadow paging maintains a extra shadow page table which requires extra memory. On the other hand, for nested paging there are two page directories- one to guest virtual addr. to guest physical addr. and anothet to map guest physical addr. to host physical addr. This requires no intervention from VMM, and results in less VM Exits.
