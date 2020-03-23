# Dell-7577-macOS-hackintosh-EFI-Files

EFI folder to make this laptop bootable with macOS Catalina operating system

<b>#*Check releases tab for downloadable zip files*#</b> 

![](ss200214.png)


<b>Specs</b>

* Intel i7-7700HQ CPU
* Intel HD Graphics 630 / nVidia GTX 1050 Ti
* 16GB 2400MHz DDR4 RAM
* 15.6” 1080p IPS Display
* 128GB Samsung M.2 SSD (SATA)/ 256GB Samsung 860 Evo SSD 
* Intel Dual Band WiFi - 8265

<b>Known Problems</b>

* Stock WiFi doesn't work ( there is a hope to make it work in the future but right now even though it can work for up to half an hour it results with a crash and reboot at the end. one can change it with a compatible wifi card but that is not my choice for now. )
* SDCard Reader ( I have no idea about it. I have never tried to make it work nor I have a plan to do so in the future. But it can be fixed probably if it is a must for someone. )
* 2.1 audio ( there is an id which enables subwoofer for this but I don't like having two output devices in system settings and do not use it because of that )
* Thunderbolt ( I also have no idea about it because I don't have a device to check whether it is working or not. Mine is disabled in bios settings. )
* HDMI ( it doesn't work because it is connected to nvidia card which is disabled with a SSDT. Optimus technology is not supported in macos environment. ) 

<b>BIOS Options</b>
* Disable Secure Boot
* Change SATA operation to AHCI ( google it to learn more before you proceed this action if you use windows already)

<b> Dual Booting </b>

I have two seperate ssd drives listed above. Windows is installed to 256gb and Macos is installed to 128GB. I do not boot Windows10 through Opencore. Both ssd drives are partitioned GUID partition schema type and both use their own bootloader. You can make switch with F12 key when you see DELL logo on starts.

<b>Explanation of files in EFI folders</b>

							###ACPI###
	
	SSDT-BRTK			--- Fixing F11 and F12 brightness keys	
	SSDT-DGPU 			--- Disabling Nvidia Card
	SSDT-HPET 			--- Resolving IRQ Conflicts and fixes RTC
	SSDT-I2C			--- Initializing touchpad
	SSDT-PLUG 			--- Plug-in Type=1 (CPU )
	SSDT-PNLF 			--- Enabling backlight control
	SSDT-PRW 			--- Proper power management
	SSDT-USBX			--- Power Management based on Macbook14,3 for USB ports
	SSDT-XOSI			--- Operating system calls
							
							###Drivers###
	
	ApfsDriverLoader.efi		--- Apfs file system driver
	OpenRuntime.efi			--- Memory mapping ( replacing Aptiomemoryfix.efi )
	HFSPlus.efi			--- HFS+ file system driver
	AudioDxe.efi			—- Audio driver for bootchime and VoiceOver support
				
							###Kexts###
	
	AppleALC			--- Audio kext for ALC256 codec
	CPUFriend			--- Handling CPU frequencies
	CPUFriendDataProvider		--- Data for CPUFriend
	IntelBluetoothFirmware		—-- Bluetooth support
	IntelBluetoothInjector		—-- Bluetooth support
	Lilu				--- Base for other kexts
	RealtekRTL8111			--- Ethernet kext
	SATA100				--- 100 series chipset support
	SMCBatteryManager		--- Battery support
	USBInjectAll			—-- Enabling usb ports
	VirtualSMC			--- SMC emulator
	VoodooI2C			--- Touchpad support
	VoodooI2CHID			--- Touchpad support
	VoodooPS2Controller		--- PS2 Keyboard support
	Whatevergreen			-—- Display support 


<b> To Do List and Things to Consider </b>

* Config file does not include SMBIOS parameters which is a must. One needs to provide own values. There are guides here and there. Your friend is google as always. For ROM adress you can use your builtin ethernet card MAC adress. MacSerial is a good way to obtain proper serial and motherboard serial numbers. UUID can be generated with terminal command uuidgen. Make it produced at least five times to be sure it is unique enough. For working imessage and facetime all should be set in a sensible way and make sure that they are not used by someone else either hackintosh or real mac.

* USBInjectAll.kext tries to enable all possible usb ports ( usb2 and usb3 ports ) and avoid usb        port limit for macos. It is not advised to use for long term. You should map your usb ports. There are applications for this process. One is Hackintool and it has a GUI and easy to figure out. There are scripts on the other hand. One is USBMap by Corpnewt. You should google usb mapping and increase your hacking XP  before you proceed. When you are done with usb mapping, you should disable quirk Kernel>Quirks>xhciportlimit. 

* CPUFriendDataProvider.kext is set to 800 mhz and 0x80 balance power.

It is possible to use this EFI for installing OS X with OC as it is. There is only a problem which is touchpad issue. VoodooI2C does not work on installation phase and after os upgrades. You have to make kext cache with terminal command ( sudo kextcache -i / ) If you have a usb mouse like me, you are good to go as it is. Just plug your usb mouse until you install and make kext cache and reboot. If you do not have a usb mouse, I recommend you to use ApplePS2smarttouchpad.kext Remove or disable entries for voodoops2 and voodooi2c then add entries for appleps2smarttouchpad.kext Be careful you have to add base kext first then its plugins. Voodoops2controller or voodooi2c kexts can be an example for this. Oc wants them in order by default. It does not inject applealc, whatevergreen or virtualsmc if they come before lilu. Because lilu is a base for them. 

For boot chime or voice over support you have to download audio files from Acidanthera GitHub ( https://github.com/acidanthera/OcBinaryData/tree/master/Resources ) and place it in OC / Resources/Audio. You have to enable quirk UEFI>Audio>PlayChime for bootchime. 

Misc>Security>Scan policy is set to “0” for displaying every possible drivers. You can change it back to OC suggested ( 983299 ) when you are done with your efi folder to scan safer.

If you dual boot like me, you can disable quirk Misc>Boot>ShowPicker. In this way, it will directly start booting macOS for you as a normal Mac after Dell logo.

<b> To make a bootable usb stick follow these steps. It is easy but requires a Mac or hackintosh which can connect to app store. </b>

download macOS Catalina install app from app store.

2)Plug your  at least 16 gb usb2 stick to your Mac or Mac like pc. Open disk utility and format your usb stick ( be careful to not format any other drive ) format it with GUID partition schema and macOS journaled. Give it name oowyeah.

3)When install app is downloaded open your terminal and copy and paste this command and hit enter


sudo "/Applications/Install macOS Catalina.app/Contents/Resources/createinstallmedia" --volume  /Volumes/oowyeah --nointeraction

4)When process is done, you should copy efi folder to usb stick efi partition. Mounting EFI partitions is explained below.


to mount efi partitions use an app or terminal commands ( I suggest using terminal way ) but be careful.

diskutil list ( this will list all of your connected drives including external )
for example I have two internal drives: disk0 for 128GB and disk1 for 256GB and one external drive [ usb stick ] disk3 for 16GB

 sudo diskutil mount diskXsY ( x for drive Y for section )
for example usb stick is listed as disk3 and diskutil list shows EFI identifier as disk3s1 so I should type command as “ sudo diskutil mount disk3s1 ”

Thanks to everyone in OSX86 environment who helped me with patience and developers for maintaing kexts, drivers, scripts and patches.

My language can be a little bit weird because I am not native and have difficulty to explain things. Feel free to correct my mistakes if you find any.

This whole process is made because of fucking educational purposes.


