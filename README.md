# Dell-7577-macOS-hackintosh-EFI-Files
EFI folder to make this laptop bootable with macOS Catalina operating system

<b>#*Check releases tab for downloading*#</b> 

![](ss200214.png)


<b>Specs</b>

* Intel i7-7700HQ CPU
* Intel HD Graphics 630 / nVidia GTX 1050 Ti
* 16GB 2400MHz DDR4 RAM
* 15.6” 1080p IPS Display
* 128GB Samsung M.2 SSD (SATA)
* Intel Dual Band WiFi - 8265

<b>Known Problems</b>

* Stock WiFi doesn't work ( there is a hope to make it work in the future but right now even though it can work for up to half an hour it results with a crash and reboot at the end. one can change it with a compatible wifi card but that is not my choice for now. )
* SDCard Reader ( I have no idea about it. I have never tried to make it work nor I have a plan to do so in the future. But it can be fixed probably if it is a must for someone. )
* 2.1 audio ( there is an id which enables subwoofer for this codec but it was consuming too much battery when last time I used it and as a result I changed it with a 2.0 id to make it more battery efficient. )
* Thunderbolt ( I also have no idea about it because I don't have a device to check whether it is working or not. )
* HDMI ( it doesn't work because it is connected to nvidia card which is disabled with a SSDT. Optimus technology is not supported in macos environment. ) 

*** VoodooPS2Controller & VoodooPS2Keyboard Issue ***
This kext is used for accessing keyboard. Be aware that it is not getting along well with VoodooI2C as of writing this guide. If you do not use "-v" boot argument like me, you have to push a random key to avoid early kernel panic when bootloader starts. ApplePS2Controller & ApplePS2Keyboard duo is not suffering from this issue but brightness keys are not working so voodoops2 is a go at the end. )

<b>BIOS Options</b>
* Disable Secure Boot
* Change SATA operation to AHCI ( google it to learn more before you proceed this action if you use windows already)
* Disable Legacy Boot option


<b>Explanation of files in EFI folders</b>

							###ACPI###
	
	SSDT-ALS0		--- Fake ambient light sensor device
	SSDT-BRTK		--- Fixing F11 and F12 brightness keys ( working with VoodooPS2Controller )
	SSDT-DGPU 		--- Disabling Nvidia Card
	SSDT-HPET 		--- Resolving IRQ Conflicts and fixes RTC
	SSDT-I2C		--- Initializing touchpad
	SSDT-PLUG 		--- Plug-in Type=1 (CPU )
	SSDT-PNLF 		--- Enabling backlight control
	SSDT-PRW 		--- Proper power management
	SSDT-SBUS-MHCH		--- PMC ACPI sample (???)
	SSDT-USB		--- Power Management based on Macbook14,3 for USB hubs (USBX)
	SSDT-XOSI		--- Operating system calls
							
							###Drivers###
	
	ApfsDriverLoader	--- Apfs file system driver
	FwRuntimeServices	--- Memory mapping ( replacing Aptiomemoryfix.efi )
	HFSPlus			--- HFS+ file system driver
	*OcQuirks		--- Enabling "FwRuntimeServices" for Clover bootloader
				
							###Kexts###
	
	AppleALC		--- Audio kext for ALC256 codec
	*ApplePS2Controller	--- Enabling PS2 devices
	*ApplePS2Keyboard	--- PS2 keyboard kext
	CPUFriend		--- Handling CPU frequencies
	CPUFriendDataProvider	--- Data for CPUFriend ( set to 800 mhz & balance power )
	Lilu			--- Base for other kexts
	RealtekRTL8111		--- Ethernet kext
	SATA100			--- Support for 100 series chipset
	SMCBatteryManager	--- Battery kext
	USBPorts		--- Configuring USB ports ( just google usb mapping for further information. )
	VerbStub		--- Fixing headphone crackling sound
	VirtualSMC		--- SMC emulator
	VoodooI2C		--- Touchpad controller
	VoodooI2CHID		--- Touchpad
	VoodooPS2Controller	--- Enabling PS2 devices
	VoodooPS2Controller	--- PS2 Keyboard kext


<b> To Do List </b>

* Config file does not include SMBIOS parameters which is a must for Apple Services. One need to provide own values. There are guides here and there. Your friend is google as always. For ROM adress you can use your builtin ethernet card MAC adress. MacSerial is a good way to obtain proper serial and motherboard serial numbers. UUID can be generated with terminal command uuidgen. Make it produced at least five times to be sure it is unique enough. For working imessage and facetime all should be set in a sensible way and make sure that they are not used by someone else either hackintosh or real mac.

* USBPort.kext is set for my own laptop. It can be different for your device for many reasons like different bios version. Best way is to map your own usb ports. Hackintool can do that process for you or there are other guides depending on USB-Inject-All kext. If your usb 2 and 3 ports are working then you have nothing to do because hey it works! ( This laptop has 3 usb ports which are both for USB2 and USB3 for user to attach usb devices. Also internal devices like webcam and bluetooth are counted as usb devices. )

* CPUFriendDataProvider.kext is set to 800 mhz and 0x80 balance power. CPUFriendFriend pyhton script by Corpnewt was used to create it. You can create your own with following that script commands.  

* SSDTTime by Corpnewt is another good script to create ssdts for resolving IRC conflicts. It makes the process done for you. If you have a problem with provided ones, create your owns.

* Unzip the file you downloaded, do necessary edits on config.plist and copy EFI folder to your drive's EFI partition. To see your EFI partition you can use an application or launch terminal type "diskutil list", find appropiate EFI partition, mount it with command "sudo diskutil mount diskXsY" ( X and Y should be numbers depending on diskutil list result. eg. disk0s1)



<b> These EFI folders are intended to use after a successful install of macos. For preparing usb and pre-install stages you can follow vanilla laptop guide by fewt. Also there is a guide by 1revenger1.  </b>


Thanks to everyone in OSX86 environment who helped me with patience and developers for maintaing kexts, drivers, and scripts.

This whole process is made because of f-king educational purposes.

