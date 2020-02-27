macos Catalina işletim sistemini çalıştırmak için gerekli EFI klasörü

<b> İndirmeler için releases kısmını ziyaret edin </b>

![](ss200214.png)

<b>Özellikler</b>

* Intel i7-7700HQ CPU
* Intel HD Graphics 630 / nVidia GTX 1050 Ti
* 16GB 2400MHz DDR4 RAM
* 15.6” 1080p IPS Display
* 128GB Samsung M.2 SSD (SATA)/ 256GB Samsung 860 EVO SSD
* Intel Dual Band WiFi - 8265

<b>Bilinen Sorununlar</b>

* Dahili WiFi çalışmaz ( Son günlerde ortaya çıkan bir kext ile bu konuda gelişmeler bulunmakta. Her ne kadar dahili wifi kartını çalıştırmak 
yarım saate kadar mümkün olsa işlem her zaman hata ile sonuçlanıp bilgisayarı resetliyor. Uyumlu bir wifi kartı alıp bilgisayarınıza takabilirsiniz. Benim şu an
bu seçenekle ilgili bir isteğim yok. )

* SDCard Okuyucu ( Hiç kullanmadığım için çalışıp çalışmadığıyla ilgili bir fikrim yok. Önemli bir ihtiyaçsa gerekli düzenlemelerle çalışabileceği kanısındayım.

* 2.1 ses ( Subwoofer etkinleştirilebilse de batarya kullanımını olumsuz etkilediğini düşündüğüm için subwoofer olmadan ses kullanmaktayım. )

* HDMI ( Nvidia ekran kartına bağlı olduğu için çalışmaz. Bu ekran kartı enerji tüketimini olumsuz etkilediği için SSDT ile kapatılmıştır. Optimus teknolojisi macos ortamında desteklenmemektir.


<b>Bios Ayarları</b>

* Secure Boot kapalı

* SATA istemcisi AHCI ( hali hazırda windows kullanmaktaysanız bu konuda google üzerinden arayarak daha detaylı bilgi edinin. )

<b> İki işletim sistemi çalıştırmak </b>

Yukarıda listelendiği gibi laptopumda iki tane SSD sürücü var. İki sürücü de GPT olarak biçimlendirilmiş durumda. Her işletim sistemi kendi yükleyicisi ile açılmakta. Opencore üzerinden windows10 yüklemesi yapmıyorum. Bunun yerine açılışta F12 tuşuna basarak bir seferlik boot yöneticisine girip oradan değiştiriyorum.

<b>EFI klasörünün içindeki dosyaların açıklaması</b>

						                      	###ACPI###
	
	SSDT-ALS0		      --- Sahte bir ışık sensörü ekler
	SSDT-BRTK		      --- F11 ve F12 aydınlatma tuşlarını etkinleştirir
	SSDT-DGPU 		    --- Nvidia ekran kartını devre dışı bırakır
	SSDT-HPET 		    --- IRQ düzenlemeleri ve RTC düzenlemeleri sağlar
	SSDT-I2C		      --- İzleme dörtgenini etkinleştirir
	SSDT-PLUG 		    --- İşlemci için düzenleme sağlar
	SSDT-PNLF 		    --- Ekran aydınlığını etkinleştirir
	SSDT-PRW 		      --- Güç yönetimini düzzenler
	SSDT-SBUS-MHCH		--- ACPI örneği oluşturur
	SSDT-USB		      --- USB portları için güç düzenlemeleri sağlar
	SSDT-XOSI		      --- İşletim sistemi çağrılarına macos'u ekler
							
							                      ###Drivers###
	
	ApfsDriverLoader	--- Apfs dosya sistemi sürücüsü
	FwRuntimeServices	--- Hafıza haritalandırması için gereklidir ( Aptiomemoryfix.efi yerine kullanılır )
	HFSPlus			      --- HFS+ dosya sistemi sürücüsü
	*OcQuirks		      --- "FwRuntimeServices" sürücüsünü Clover bootloader için kullanılır hale getirir.
				
							                      ###Kexts###
	
	AppleALC		          --- ALC 256 kodeği için ses etkinleştirmesi sağlar
	CPUFriend		          --- İşlemci için düzgün güç yönetimi sağlar
	CPUFriendDataProvider	--- CPUFriend için gerekli verileri sağlar
	Lilu			            --- Diğer kernel eklentileri için temel oluşturur
	RealtekRTL8111		    --- Ethernet portunu etkinleştirir
	SATA100			          --- 100 serisi çipset için destek sağlar
	SMCBatteryManager	    --- Bataryayı etkinleştirir
	USBPorts		          --- USB portlarını düzenler ( google üzerinden usb haritalandırması ile daha fazla bilgi edinilebilir. )
	VerbStub		          --- Kulaklık cızırtı sorununu çözer
	VirtualSMC		        --- SMC emülatörü sağlar
	VoodooI2C		          --- İzleme dörtgenini etkinleştirir
	VoodooI2CHID		      --- İzleme dörtgeni için gerekli verileri sağlar
	VoodooPS2Controller	  --- Klavyeyi etkinleştirir
  
  
  <b>Yapılacaklar listesi</b>
  
  * İndirilen dosyadaki config dosyası SMBIOS değerleri boş bırakılmıştır. Bu değerleri herkesin kendisi oluşturması gerekir. İnternette bununla ilgili bir çok rehber bulunmaktadır.
  ROM adresi için ethernet MAC adresini kullanabilirsiniz. Macserial uygulaması ile seri ve ana kart seri numaraları oluşturabilirsiniz. UUID terminal komutu "uuidgen" ile oluşturabilir.
  Ben sistemimde tüm imessage ve facetime dahil Apple servislerini kullanmaktayım.
  
  * USBPort.kext kendi laptopum için hazırlanmıştır. Sizin cihazınız için bios sürümü farklılığı gibi bir çok sebepten ötürü değişiklik gösterebilir.
  Eğer sağlanan kext ile USB2 ve USB3 cihazlarınızı kullanamıyorsanız kendi kernel eklentinizi oluşturmanız gerekebilir. Hackintool uygulaması arayüz sağladığı için tavsiye edilir.
  
  * CpuFriendDataProvider.kext ayarları boşta 800 mhz ve dengeli güç olarak ayarlanmıştır. Değişiklik yapmak isterseniz Corpnewt tarafından hazırlanan CPUFriendFriend piton kodlarıyla kendi
  isteğiniz değerleri oluşturabilirsiniz.
  
  * Corpnewt tarafından hazırlanan SSDTTime piton kodları bilgisayarnızla ilgili gerekli SSDT hazırlamaları için kullanabilir.
  
  * İndirdiğiniz dosyayı arşivden çıkarın. Config üzerinde SMBIOS ile ilgili gerekli ayarlamaları yapıp kaydetdikten sonra EFI klasörünü kopyalayıp EFI bölümüne yapıştırın. EFI bölümüne ulaşmak için
  bir uygulama kullanabilirsiniz. Ya da terminal üzerinden önce "diskutil list" komutu ile bağlı sürücüleri görebilir uygun sürücünün adresini not alıp yine terminalde "sudo diskutil mount diskXsY" komutu ile bağlayabilirsiniz. 
  X ve Y yerine sağladığınız veriler gelmelidir.
  
  <b>Bu EFI klasörleri macos işletim sistemi yüklendikten sonra kullanılmak için tasarlanmıştır.</b>
  
  OSX86 camiasında bulunan herkese teşekkür ederim.
  
  Tüm bu mevzu eğitimsel amaçların siklenmesi ile oluşturulmuştur.
