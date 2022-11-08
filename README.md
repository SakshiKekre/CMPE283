This repository is a part Assignment 1: Discovering VMX Features for CMPE-283
Performed by: Sakshi Kekre (sakshisanjay.kekre@sjsu.edu)

****************************************************************************************************************************

Follow these steps to discover the VMX features present in your processor:

#Step 0: PREREQUISITES

  To Create a Virtual Machine on GCP:
    I. In your browser, log in to Google Cloud console
    II. Create a new project 
    III. Go to 'Compute Instance' and create a new VM from UI bu clicking on 'Create Instance'. In the create form
         - Set the Instance name and region fields as suitable
         - Set the machine configuration options [Series: n2 and Machine Type: n2-standard-2]
         - In 'Boot Disk' section, click on 'change image', select 'Public Images' tab, select 'Ubuntu' in the Operating System dropdown, click 'Select'
         - Click on 'Create' button at the bottom
        A new instance will be created 

  To Update the Virtual Machine for enabling Virtualization
    I. On your local machine, download the installer for gcloud cli from https://cloud.google.com/sdk/docs/install-sdk
    II. Unzip the installer and execute the install.sh file in your local terminal
    III. In a new terminal, execute below command to initialize the gcloud cli 
            $ gcloud init
         When prompted, select the appropriate project, region and zone in the same command
    IV. Export the configuration of the gcloud instance in a yaml file with below command
          $ gcloud compute instances export instance-1  --destination=export.yaml   --zone=<YourZone>
    V. Modify yaml file to enable nested virtualization by appending following string at the end of file:
            advancedMachineFeatures:
             enableNestedVirtualization: true

  Your cloud instance (virtual machine) is now created and enabled for nested virtualization.
  
****************************************************************************************************************************


#STEP 1: Connect to the gcloud instance using command
            $ gcloud compute ssh instance-1
        It will generate the required key pair for secure shell

****************************************************************************************************************************
  
#STEP2: Create a new directory and clone this GitHub repository in your directory. The readMSR.C file has code to 
       - read MSR Controls of your VM processor 
       - write these controls to the kernal log
       Copy files to repository

****************************************************************************************************************************

#STEP 3:Install gcc, make and linux headers using below commands

Compiler
$ sudo apt install gcc 

Build automation tool
$ sudo apt install make

Packages for linux kernal headers
$ sudo apt-get install linux-headers-$(uname -r)

****************************************************************************************************************************

#STEP 4: Make the kernal object and insert it in kernal using below commands

$make

$sudo insmod ./readMSR.ko

****************************************************************************************************************************

#STEP 5: As soon as your module is inserted in the kernal, it will print messages to the kernal logs. These logs can be seen using below command.
          $sudo dmesg
  
        An excerpt of these logs from my execution are shown below >
  
[23713.330919] readMSR: loading out-of-tree module taints kernel.
[23713.330957] readMSR: module verification failed: signature and/or required key missing - tainting kernel
[28002.104024] CMPE 283 Assignment 1 Module Start -- SSK 
[28002.104028] Pinbased Controls MSR: 0x3f00000016
[28002.104029]   External Interrupt Exiting: Can set=Yes, Can clear=Yes
[28002.104030]   NMI Exiting: Can set=Yes, Can clear=Yes
[28002.104030]   Virtual NMIs: Can set=Yes, Can clear=Yes
[28002.104031]   Activate VMX Preemption Timer: Can set=No, Can clear=Yes
[28002.104032]   Process Posted Interrupts: Can set=No, Can clear=Yes
[28002.104033] Primary Processor-Based Controls MSR: 0xf7b9fffe0401e172
[28002.104034]   nterrupt-window exiting: Can set=Yes, Can clear=Yes
[28002.104034]   Use TSC offsetting: Can set=Yes, Can clear=Yes
[28002.104035]   HLT exiting: Can set=Yes, Can clear=Yes
[28002.104035]   INVLPG exiting: Can set=Yes, Can clear=Yes
[28002.104036]   MWAIT Exiting: Can set=Yes, Can clear=Yes
[28002.104036]   RDPMC Exiting: Can set=Yes, Can clear=Yes
[28002.104037]   RDTSC exiting: Can set=Yes, Can clear=Yes
[28002.104037]   CR3-load exiting: Can set=Yes, Can clear=No
[28002.104038]   CR3-store exiting: Can set=Yes, Can clear=No
[28002.104038]   Activate tertiary controls: Can set=No, Can clear=Yes
[28002.104039]   CR8-load exiting: Can set=Yes, Can clear=Yes
[28002.104039]   CR8-store exiting: Can set=Yes, Can clear=Yes
[28002.104040]   Use TPR shadow: Can set=Yes, Can clear=Yes
[28002.104040]   NMI-window exiting: Can set=No, Can clear=Yes
[28002.104040]   MOV-DR exiting: Can set=Yes, Can clear=Yes
[28002.104041]   Unconditional I/O exiting: Can set=Yes, Can clear=Yes
[28002.104041]   Use I/O bitmaps: Can set=Yes, Can clear=Yes
[28002.104042]   Monitor trap flag: Can set=No, Can clear=Yes
[28002.104042]   Use MSR bitmaps: Can set=Yes, Can clear=Yes
[28002.104043]   MONITOR exiting: Can set=Yes, Can clear=Yes
[28002.104043]   PAUSE exiting: Can set=Yes, Can clear=Yes
[28002.104044]   Activate secondary controls: Can set=Yes, Can clear=Yes
[28002.104045] Secondary Processor-Based Controls MSR: 0x51ff00000000
[28002.104046]   Virtualize APIC accesses: Can set=Yes, Can clear=Yes
[28002.104046]   Enable EPT: Can set=Yes, Can clear=Yes
[28002.104047]   Descriptor-table exiting: Can set=Yes, Can clear=Yes
[28002.104047]   Enable RDTSCP: Can set=Yes, Can clear=Yes
[28002.104048]   Virtualize x2APIC mode: Can set=Yes, Can clear=Yes
[28002.104048]   Enable VPID: Can set=Yes, Can clear=Yes
[28002.104048]   WBINVD exiting: Can set=Yes, Can clear=Yes
[28002.104049]   Unrestricted guest: Can set=Yes, Can clear=Yes
[28002.104049]   APIC-register virtualization: Can set=Yes, Can clear=Yes
[28002.104050]   Virtual-interrupt delivery: Can set=No, Can clear=Yes
[28002.104050]   PAUSE-loop exiting: Can set=No, Can clear=Yes
[28002.104051]   RDRAND exiting: Can set=No, Can clear=Yes
[28002.104051]   Enable INVPCID: Can set=Yes, Can clear=Yes
[28002.104052]   Enable VM functions: Can set=No, Can clear=Yes
[28002.104052]   VMCS shadowing: Can set=Yes, Can clear=Yes
[28002.104053]   Enable ENCLS exiting: Can set=No, Can clear=Yes
[28002.104053]   RDSEED exiting: Can set=No, Can clear=Yes
[28002.104054]   Enable PML: Can set=No, Can clear=Yes
[28002.104054]   EPT-violation #VE: Can set=No, Can clear=Yes
[28002.104055]   Conceal VMX from PT: Can set=No, Can clear=Yes
[28002.104055]   Enable XSAVES/XRSTORS: Can set=No, Can clear=Yes
[28002.104056]   Mode-based execute control for EPT: Can set=No, Can clear=Yes
[28002.104056]   Sub-page write permissions for EPT: Can set=No, Can clear=Yes
[28002.104057]   Intel PT uses guest physical addresses: Can set=No, Can clear=Yes
[28002.104057]   Use TSC scaling: Can set=No, Can clear=Yes
[28002.104058]   Enable user wait and pause: Can set=No, Can clear=Yes
[28002.104058]   Enable PCONFIG: Can set=No, Can clear=Yes
[28002.104058]   Enable ENCLV exiting: Can set=No, Can clear=Yes
[28002.104059] Cannot Set Tertiary Processor-Based Controls
[28002.104060] Exit Controls MSR: 0x3fefff00036dff
[28002.104061]   Save debug controls: Can set=Yes, Can clear=No
[28002.104062]   Host address-space size: Can set=Yes, Can clear=Yes
[28002.104062]   Load IA32_PERF_GLOBAL_CTRL: Can set=No, Can clear=Yes
[28002.104063]   Acknowledge interrupt on exit: Can set=Yes, Can clear=Yes
[28002.104063]   Save IA32_PAT: Can set=Yes, Can clear=Yes
[28002.104064]   Load IA32_PAT: Can set=Yes, Can clear=Yes
[28002.104064]   Save IA32_EFER: Can set=Yes, Can clear=Yes
[28002.104065]   Load IA32_EFER: Can set=Yes, Can clear=Yes
[28002.104065]   Save VMXpreemption timer value: Can set=No, Can clear=Yes
[28002.104066]   Clear IA32_BNDCFGS: Can set=No, Can clear=Yes
[28002.104067]   Conceal VMX from PT: Can set=No, Can clear=Yes
[28002.104067]   Clear IA32_RTIT_CTL: Can set=No, Can clear=Yes
[28002.104068]   Clear IA32_LBR_CTL: Can set=No, Can clear=Yes
[28002.104068]   Load CET state: Can set=No, Can clear=Yes
[28002.104069]   Load PKRS: Can set=No, Can clear=Yes
[28002.104069]   Save IA32_PERF_GLOBAL_CTL: Can set=No, Can clear=Yes
[28002.104070]   Activate secondary controls: Can set=No, Can clear=Yes
[28002.104071] Entry Controls MSR: 0xd3ff000011ff
[28002.104072]   Load debug controls: Can set=Yes, Can clear=No
[28002.104072]   IA-32e mode guest: Can set=Yes, Can clear=Yes
[28002.104072]   Entry to SMM: Can set=No, Can clear=Yes
[28002.104073]   Deactivate dual-monitor treatment: Can set=No, Can clear=Yes
[28002.104073]   Load IA32_PERF_GLOBAL_CTRL: Can set=No, Can clear=Yes
[28002.104074]   Load IA32_PAT: Can set=Yes, Can clear=Yes
[28002.104074]   Load IA32_EFER: Can set=Yes, Can clear=Yes
[28002.104075]   Load IA32_BNDCFGS: Can set=No, Can clear=Yes
[28002.104075]   Conceal VMX from PT: Can set=No, Can clear=Yes
[28002.104076]   Load IA32_RTIT_CTL: Can set=No, Can clear=Yes
[28002.104076]   Load CET state: Can set=No, Can clear=Yes
[28002.104077]   Load guest IA32_LBR_CTL: Can set=No, Can clear=Yes
[28002.104077]   Load PKRS: Can set=No, Can clear=Yes
