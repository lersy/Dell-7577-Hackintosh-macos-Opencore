Prebuilt EFI folders for Dell Inspiron 7577 to make it bootable with macOS through OpenCore boot loader.

I can install, update and upgrade macOS on my laptop with this folder yet it is not guaranteed that it will work for you as well. Even though they are same models, two computers can have differences. Proceed all processes at your own risk. 

I highly recommend you to read this page before using prebuilt EFI folders. <b>#*Check releases tab for downloadable zip files*#</b> 


<b>Specs</b>

* Intel i7-7700HQ CPU
* Intel HD Graphics 630 / Nvidia GTX 1050 Ti
* 16GB 2400MHz DDR4 RAM
* 15.6” 1080p IPS Display
* 128GB Samsung M.2 SSD (SATA) / 256GB Samsung 860 Evo SSD 
* Intel Dual Band WiFi - 8265 & Intel Bluetooth

| OS Version | EFI Version    | Known Problems related to version | EFI Sanity Check |
| ----------- | ------- | ---------- | ------------------- |
| macOS Big Sur  | December  | None except general          | Yes             |
| macOS Catalina  | December  | None except general          | No             |
| macOS Mojave  | December | None except general          | No              |
| macOS High Sierra   | December | None except general          | No              |

<b>Known Problems</b>

* SDCard Reader ( I have no idea about it. I have never tried to make it work nor I have a plan to do so in the future. If you find a solution, please let me know. )

* 2.1 audio ( there is an id which enables subwoofer but I don't use it because the id I use in my config.plist has best compatibility with headphone. Detailed explanations about each id can be found at AppleALC manual page. The laptop has ALC256 on it. )


<b>BIOS version and options</b>
* Current bios version is 1.11.0
* Disable Secure Boot
* Change SATA operation to AHCI ( google it to learn more before you proceed this action if you use windows already to not lose your existed data on windows partition )
* Disable Virtualization


<details>
<summary> See required advanced BIOS settings  </summary>

To enable advanced BIOS options, execute ModifiedGrubShell.efi at Opencore Picker Screen and enter given commands below for each settings. Be aware, values can be different for you so it is best to find your own values. To read how to find your own values click [here](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#turning-off-cfg-lock-manually).

| Command | Explanation    |
| ----------- | ------- |
| setup_var 0x4DE 0x00  | Disables CFG Lock	     |

This command disables CFG Lock which is a must to run macOS. If you do not want to disable it, you have to set Kernel>Quirks>AppleXcpmCfgLock to YES for a workaround. 

| Command | Explanation   |
| ----------- | ------- |
| setup_var 0x889 0x00  | Disables WakeOnLan	     |

This command disables wake on lan BIOS settings so laptop can sleep on battery and AC without problem. Without disabling this setting, your laptop will have sleep issues on AC. On battery sleep works well because it is set to disable on battery by default. No mandatory to run macOS but advised for proper sleep and wake functions.  

</details>

<b> To Do List and Workarounds </b>

* Config file does not include SMBIOS parameters which is a must. One needs to provide own values. MacSerial by Acidanthera is a good way to obtain proper serial and motherboard serial numbers. UUID can be generated with terminal command “uuidgen”. Builtin ethernet, wifi or thunderbolt device MAC address can be used as ROM value. For working imessage and facetime all should be set in a sensible way and make sure that they are not used by someone else either hackintosh or real Mac. When you change a value ( SN, MLB, UUID or ROM ) you should change all other values to prevent apple servers being suspicious about your account.

* USBMap.kext is set to Macbookpro14,1. If you want to use a different SMBIOS, you should also change the correspond model name in the info.plist inside the kext in order to make usb ports working. Fingerprint device is closed to save battery and avoid long waiting before root access. It does not work because apple does not allow to use third party ones.

* For voice over support you have to download audio files from [Team Acidanthera GitHub](https://github.com/acidanthera/OcBinaryData/tree/master/Resources) and place it in OC / Resources/Audio. You have to enable quirk UEFI>Audio>PlayChime for bootchime. 

* If you dual boot like me explained above, you can disable quirk Misc>Boot>ShowPicker. In this way, it will directly start booting macOS for you as a normal Mac after Dell logo.

* To enable Virtualization, Booter>Quirks>DevirtualiseMmio can be set to YES as a workaround but this is not the preferred method and can cause problem.

<b> Dual Booting </b>

I have two seperate ssd drives listed above. Windows is installed to 256gb and Macos is installed to 128GB. I do not boot Windows10 through Opencore. Both ssd drives are partitioned GUID partition schema type and both use their own bootloader. You can make switch with F12 key when you see DELL logo on starts. I strictly do not recommend booting windows through OpenCore.

<b> CREDITS </b>

[Team Acidanthera](https://github.com/acidanthera) for OpenCore boot loader and AppleALC, Brightness Keys, Lilu, RealtekRTL8111, VirtualSMC and its plugins, VoodooPS2Controller and Whatevergreen kexts. 

[Team VoodooI2C](https://github.com/VoodooI2C/VoodooI2C) for VoodooI2C and VoodooI2CHID kexts.

[Team OpenIntelWireless](https://github.com/OpenIntelWireless) for IntelBluetooth firmware and Airportitlwm kexts.

[Piker-Alpha](https://github.com/Piker-Alpha) for ssdtPRGen script

[@dbookuz](https://github.com/dbookuz) , [DeMikeyyy](https://github.com/DeMikeyyy) , [wadimw](https://github.com/wadimw) and [uzairblaoch](https://github.com/uzairblaoch) for sharing their knowledge and testing EFI folders.

#################

Thanks to everyone who helped me with patience and developers for maintaing kexts, drivers, scripts and patches.

This whole process is made because of fucking educational purposes. 
