# Dell Inspiron Gaming 7577 & macOS 

Prebuilt EFI folders for Dell Inspiron 7577 to make it bootable with macOS through OpenCore boot loader.

I can install, update and upgrade macOS on my laptop with this folder yet it is not guaranteed that it will work for you as well. Even though they are same models, two computers can have differences. Proceed all processes at your own risk.

Check releases page for downloadable EFI zip files.

<b> Do not use prebuilt EFI folders before you read this page otherwise it will end up with a failure. </b>

# Supported macOS Versions

| OS Version 		| Known Problems 			| Tested 	|
| ----------- 		| ---------- 				| ------	|
| macOS Monterey  	| Universal + Bluetooth          		| Yes             	|
| macOS Big Sur  	| None except universal          	| No            	|
| macOS Catalina  	| None except universal          	| No              	|
| macOS Mojave	| None except universal          	| No              	|

# Laptop Components 

| Device Type		| Component 	|
|-------------	| --------- 	|			
| CPU			| Intel i7-7700HQ |
| Graphics		| Intel HD Graphics 630 / Nvidia GTX 1050 Ti |
| Memory		| 16GB 2400MHz DDR4 RAM |
| Network		| Intel Dual Band WiFi 8265 & Bluetooth |
| Screen		| 15.6” 1080p IPS Display |
| Storage		| 128GB Samsung M.2 SSD (SATA) / 256GB Samsung 860 Evo SSD |

# Universal Problems

* Nvidia 1050ti graphic card does not work ( Optimus technology is not supported )

* SDCard Reader ( Maybe it works but I have no idea. I have never tried it. )

* 2.1 Audio ( It is possible to enable subwoofer with a different ID but I do not prefer it since provided ID in config.plist has best compatibility with headphones. Detailed information can be obtained over AppleALC manual page. The laptop has ALC256 codec. )

# BIOS Version and Settings

> BIOS Version: 1.14.0

* Disable Secure Boot

* Change SATA Operation to AHCI ( make a google search for detailed information about changing SATA Operation models before proceed this step. )

* Disable Virtualization

<details>
<summary>Advanced BIOS Settings for Enthusiastic People after successful installation </summary>

  
<b> IMPORTANT NOTE1: All changes done through this screen will be back to default when you perform a BIOS upgrade or re-flash the same version. It also defaults itself if you reset CMOS physically. </b>

<b>IMPORTANT NOTE2: Even though steps descried here are not required for a successful boot process, it is strongly advised. Steps here are my preferred methods but excluded from config in order to prevent new comers’ mistakes. </b>

To enable advanced BIOS options;

1- Set Misc>Boot>HideAuxiliary to YES

2- Execute AdvancedBiosSettings at Opencore Picker screen

Enter given commands below for each settings. When it is done, type “exit” without quotes to exit this command shell screen.





| Command			| Explanation 		|
|---------		| ----------- 		|			
| setup_var 0x4DE 0x00	| Disables CFG Lock 	|

> IMPORTANT NOTE for CFG LOCK: After execute this command, you must disable Kernel>Quirks>AppleXcpmCfgLock in config.plist after you boot into macOS. It is not recommended to use both at the same time for a long period. 

  </details>  
  
# Config Settings

  * Config file does not include SMBIOS parameters ( MLB, ROM, SystemProductName, SystemSerialNumber and SystemUUID ) which is a must. One needs to provide own values. MacSerial by Acidanthera is a good way to obtain proper serial and motherboard serial numbers. UUID can be generated with terminal command “uuidgen”. Builtin ethernet, wifi or thunderbolt device MAC address can be used as ROM value. For working iMessage and FaceTime all should be set in a sensible way and make sure that they are not used by someone else either hackintosh or real Mac. When you change a value ( SN, MLB, UUID or ROM ) you should change all other values to prevent apple servers being suspicious about your account. Tested SystemProductName models by me and returned zero errors as follow: Macbookpro14,1 ; Macbookpro14,2 ; Macbookpro 14,3.

> You should provide values for PlatformInfo>Generic> MLB, ROM, SystemProductName, SystemSerialNumber and SystemUUID

  * To enable boot chime, UEFI>Audio>PlayChime should be set to YES.

  * If you do not want to see Opencore GUI and boot directly into macos after installation, set Misc>Boot>ShowPicker to NO and set Misc>Security>ScanPolicy to 65795.

  * To enable Virtualization in BIOS, Booter>Quirks>DevirtualiseMmio should be set to YES.

# Dual Booting macOS and Windows with Opencore

<b> No, don’t do that. </b> I strictly do not recommend booting windows through Opencore. Best way is making switch with F12 key when DELL logo appears. 

# Credits

[Team Acidanthera](https://github.com/acidanthera) for OpenCore boot loader itself and AppleALC, BlueToolFixup, Brightness Keys, Lilu, RealtekRTL8111, VirtualSMC and its plugins, VoodooPS2Controller and Whatevergreen kexts. 

[Team VoodooI2C](https://github.com/VoodooI2C/VoodooI2C) for VoodooI2C and VoodooI2CHID kexts.

[Team OpenIntelWireless](https://github.com/OpenIntelWireless) for Airportitlwm, IntelBluetooth firmware and  Injector kexts.

[Daliansky](https://github.com/Daliansky) for prebuilt cosmetic SSDTs ( DMAC, HRT, MCHC, MEM2, PMCR and SBUS )

[zhen-zen](https://github.com/zhen-zen) for ThermalSolution kext

[uzairblaoch](https://github.com/uzairblaoch) for providing information about enabling HDMI

[datastone](https://github.com/datasone) for grub-mode-setup_var

> Thanks everyone who helped me with patience and developers for maintaing kexts, drivers, scripts and patches. 

# Disclaimer

I am not responsible for anything happened to you or your laptop. Blame someone else for your suckin' life.

<strong> This whole process is made because of fucking educational purposes. </strong>
