#  E1. Exercise 4. Operating System structure

## Introduction

We can divide operating system in following parts:
- Kernel
- Memory manager
- Input/Output manager
- Filesystem manager

## Contents

Let's analyze operating system behaviour on each module. We will be using linux comands to see what's going on on a linux OS.

## Delivery

### Kernel

1. **Where does the kernel reside in disk?** Write down the command with the output and tell exactly where it is the kernel.

- In the /boot folder, and after use this command: `uname -r`
output: `4.18.7-100.fc27.x86_64`

2. **How can we show the actual kernel version that it is loaded on our system**? Please show command output
- `uname -r`
- `4.18.7-100.fc27.x86_64`

3. **How can we show the detected hardware done by the kernel?** Please show command output.
-  With this command: `lspci`
-  `00:00.0 Host bridge: Intel Corporation 4th Gen Core Processor DRAM Controller (rev 06)
00:02.0 VGA compatible controller: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor Integrated Graphics Controller (rev 06)
00:03.0 Audio device: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor HD Audio Controller (rev 06)
00:14.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB xHCI (rev 05)
00:16.0 Communication controller: Intel Corporation 8 Series/C220 Series Chipset Family MEI Controller #1 (rev 04)
00:1a.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #2 (rev 05)
00:1b.0 Audio device: Intel Corporation 8 Series/C220 Series Chipset High Definition Audio Controller (rev 05)
00:1c.0 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #1 (rev d5)
00:1c.2 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #3 (rev d5)
00:1c.3 PCI bridge: Intel Corporation 82801 PCI Bridge (rev d5)
00:1d.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #1 (rev 05)
00:1f.0 ISA bridge: Intel Corporation H81 Express LPC Controller (rev 05)
00:1f.2 SATA controller: Intel Corporation 8 Series/C220 Series Chipset Family 6-port SATA Controller 1 [AHCI mode] (rev 05)
00:1f.3 SMBus: Intel Corporation 8 Series/C220 Series Chipset Family SMBus Controller (rev 05)
02:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 06)
03:00.0 PCI bridge: Intel Corporation 82801 PCI Bridge (rev 41)
`

4. **How can we show the modules (drivers) actually in use?** Which module it is being used for video graphics card?
- We can show modules with this command: `lsmod`
- The module is `i915`


### Memory manager

1. **What is the 'cache' memory that shows the command 'free -m'?** Show also the command output
The cache is data that has been read from the disk and stored in RAM for later usage

|     | Total | Used | Free | Shared | Buff/Cache | Available |
| ------------- |:-------------:| -----:| -----:|-----:|-----:|-----:|
| Mem:  | 3831  | 1212 | 1222 | 112 | 1396 | 2232 |
| Swap: | 5116     |   0 | 5116 |  |  |  |

2. **What is the 'swap' memory that shows the command 'free -m'?**
- It is a partition in the disk that can be used as "slow ram" for when the computer runs out of memory 


3. **How does the memory manager handle an out-of-memory problem?**
- There's a program called OOM Killer that kills a process choosen according to a score assigned

4. **How can we show the actual 'out of memory score' of a Gimp instance that it is running?**
- `ps aux | grep gimp `

- ` ism4757+ 25092  1.3  1.7 673948 70216 ?        Sl   11:45   0:01  \_ gimp `
- `cat /proc/25092/oom_score`


### Input/Output manager

1. **How can we list all the interrupts that it is aware our Operating System?** Show also command output
-  `cat /proc/interrupts`
          CPU0       CPU1       
  0:         14          0   IO-APIC   2-edge      timer
  5:          0          0   IO-APIC   5-edge      parport0
  8:          1          0   IO-APIC   8-edge      rtc0
  9:          0          4   IO-APIC   9-fasteoi   acpi
 16:          0         25   IO-APIC  16-fasteoi   ehci_hcd:usb1
 18:          0          0   IO-APIC  18-fasteoi   i801_smbus
 23:         29          0   IO-APIC  23-fasteoi   ehci_hcd:usb2
 24:      73059      32836   PCI-MSI 512000-edge      ahci[0000:00:1f.2]
 25:     121118      14628   PCI-MSI 327680-edge      xhci_hcd
 26:       7172     331603   PCI-MSI 1048576-edge      enp2s0
 27:      38741       8273   PCI-MSI 32768-edge      i915
 28:         13          0   PCI-MSI 360448-edge      mei_me
 29:          0        627   PCI-MSI 442368-edge      snd_hda_intel:card1
 30:        151          0   PCI-MSI 49152-edge      snd_hda_intel:card0
NMI:         99         98   Non-maskable interrupts
LOC:    2521774    2591051   Local timer interrupts
SPU:          0          0   Spurious interrupts
PMI:         99         98   Performance monitoring interrupts
IWI:          2          2   IRQ work interrupts
RTR:          0          0   APIC ICR read retries
RES:     381257     388838   Rescheduling interrupts
CAL:     896585     914480   Function call interrupts
TLB:     676014     692155   TLB shootdowns
TRM:          0          0   Thermal event interrupts
THR:          0          0   Threshold APIC interrupts
DFR:          0          0   Deferred Error APIC interrupts
MCE:          0          0   Machine check exceptions
MCP:         36         37   Machine check polls
HYP:          0          0   Hypervisor callback interrupts
HRE:          0          0   Hyper-V reenlightenment interrupts
HVS:          0          0   Hyper-V stimer0 interrupts
ERR:          0
MIS:          0
PIN:          0          0   Posted-interrupt notification event
NPI:          0          0   Nested posted-interrupt event
PIW:          0          0   Posted-interrupt wakeup event


2. **In multiprocessor systems, how does the operating system distribute interrupts by default?** How can we change the default behaviour?
- Each interrupt, by default is taken by each core, alternating.
- You can change priorities with `tuned-adm list`

3. **How does an operating system control a device that has no interrupts?** Explain the process 
- It controls the device by polling, the CPU asks constantly the device for new data, and the device provides it when it can


4. **What will happen if two applications want to send data through network device at the same time?**

- The Operating system will give priority to one or another

### Filesystem manager

1. **Which is the typical linux folder structure?** Describe what it is and the use of each folder.
- **bin**:  This folder contains base executables which are required for run level 1 
- **boot**:   This folder contains all the boot related files and folders such as grub.conf and the Kernel itself
- **dev**:  Linux treats hardware as files, and those files are in this folder
- **etc**:  This folder contains all the systems ºconfiguration files
- **home**: THis is the directory where all the users folder are created
- **lib**:  This directory contains kernel modules (Drivers) and those shared library images ("equivalent" to DLL's on Windows)
- **lib64**: Same as above but for 64 bitsº
- **lost+found**: This is  directory which stores files which are not properly closed due to poweroff for example
- **media**: Directory for mounting files systems on removable devices like CD-ROM drives
- **mnt**: Directory for temporarily mounted filesystems.
- **opt**: Optional software packages
- **proc**: Contains the information about the Linux System
- **root**: Home directory of the root user
- **run**: It is a folder for processes to store data in
- **sbin**: THis folder constains executables that are for changing system parameters
- **srv**: Contains data for services
- **sys**: Contains information about the device
- **tmp**: Temporary directory 
- **usr**: COntains subdirectories for programs
- **var**: Contains various system files

2. **How can we:?**
- Move a file that resides in /usr/local/src/file.md to the folder /opt:
    - `mv /usr/local/src/file.md /opt`

- Copy a file that resides in /usr/local/src/file.md to the folder /opt:
    - `cp /usr/local/src/file.md /opt`

- Move a file in /usr/file.md to /usr/local if we are currently in path /usr/local:
    - `mv /usr/local/src/file.md .`
- Create the file .gitignore using command 'touch' and then try to list it (ls). What happens?
    - Files starting with a dot in linux are hidden by default
