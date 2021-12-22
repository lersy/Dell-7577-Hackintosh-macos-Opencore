# Dell Inspiron Gaming 7577 & macOS 

Dell Inspiron 7577 model notebook ile OpenCore bot yöneticisi aracılığıyla macOS yüklenebilmesi için yapılandırılmış EFI dosyaları

Her ne kadar ben bu dosyalar ile notebook üzerinde macOS yükleme, güncelleme ve yükseltme işlemleri yapabilsem de aynı durum sizin için geçerli olmayabilir. Aynı model isimlerine sahip olsalar bile iki cihaz arasında farklılıklar olabilmektedir. Lütfen tüm işlemleri kendi insiyatifinizle uygulayın.

İndirilebilir EFI dosyaları için Releases kısmını ziyaret edin.

<b> Bu sayfayı dikkatle okuyup anlamadan önce indirilen dosyayı kullanmaya çalışmanız halinde hata ile karşılacaksınız.</b>

# Desteklenen macOS sürümleri

| İşletim Sistemi		| Bilinen Problemler 			| Test Durumu 	|
| ----------- 		| ---------- 				| ------	|
| macOS Monterey  	| Genel Problemler + Bluetooth          		| Evet             	|
| macOS Big Sur  	| Genel  Problemler         	| Hayır            	|
| macOS Catalina  	| Genel Problemler          	| Hayır              	|
| macOS Mojave	| Genel Problemler          	| Hayır              	|

> Tablo 21December dosyası içindir. Her zaman en son yayınlanan dosya tavsiye edilir.

# Notebook Donanımı 

| Cihaz Türü		| Modeli 	|
|-------------	| --------- 	|			
| İşlemci			| Intel i7-7700HQ |
| Grafik		| Intel HD Graphics 630 / Nvidia GTX 1050 Ti |
| RAM		| 16GB 2400MHz DDR4 RAM |
| Ağ		| Intel Dual Band WiFi 8265 & Bluetooth |
| Ekran		| 15.6” 1080p IPS Display |
| Depolama		| 128GB Samsung M.2 SSD (SATA) / 256GB Samsung 860 Evo SSD |

# Genel Problemler

* Nvidia 1050ti grafik kartı çalışmaz. ( Nvidia Optimus teknolojisi macOS üzerinde desteklenmez.)

* SDCard Okuyucu ( Belki çalışıyordur ama bir fikrim yok. Hiç kullanmadım. )

* 2.1 Audio ( Subwoofer desteğini farklı bir AppleALC kimliği ile etkinleştimek mümkün görünse de kullandığım AppleALC kimliği daha uygun ve kulaklık problemi yaratmamaktadır. Dilerseniz AppleALC GitHub sayfası üzerinde detaylara bakabilirsiniz. Notebookta bulunan kodek ALC256. )

# BIOS Sürümü ve Ayarları

> BIOS Sürümü: 1.15.0

* Secure Boot kapatın 

* SATA Operation kısmında AHCI seçin

<details>
<summary>Tavsiye edilen başarılı yükleme sonrası gelişmiş BIOS ayarları </summary>

	
Burada bahsedilen adımlar başarılı bir yükleme ve kullanım için zorunlu olmasa da kullanım açısından tavsiye edilen yöntemlerdir. Ben bu gelişmiş ayarlarla kullanıyor olmama rağmen config dosyasından bunları çıkarttım çünkü yeni başlayanların kafasını karıştırabiliyor.

  
<b> Önemli Uyarı 1: Bu kısımda yapacağınız değişiklikler BIOS güncellemesi ya da aynı sürümü tekrar yüklemeniz sonrasında varsayılan ayarlara geri dönecektir. Eğer BIOS pilini fiziksel olarak söküp takarsanız yine sıfırlanır. Aynı adımların tekrar uygulanması gerekir.</b>


Bu kısımdaki komutları girebilmeniz için aşağıdaki adımı uygulayın;

OpenCore seçici kısmında AdvancedBIOSSetings uygulamasını çalıştırın

Aşağıda açıklamalarıya birlikte verilmiş kodları uygulama ekranında yazıp “Enter” tuşu ile uygulayın. İşlemleri bitirdiğinizde “reboot” yazarak sistemi tekrar başlatabilirsiniz (tırnak işaretleri hariç)


| Komut			| Açıklama 		|
|---------		| ----------- 		|			
| setup_var 0x4DE 0x00	| CFG Lock Kapatır	|

> CFG LOCK HAKKINDA UYARI: Bu komutu uyguladıktan sonra, config dosyası içerisinden Kernel>Quirks>AppleXcpmCfgLock ayarını kapalı duruma getirin. İkisinin bi arada kullanılması çakışmalar yaratabilir. Bu özelliği kapatmak için komutu uyguladık.

  </details>  
  
# Config Ayarları

* Config dosyası sistemi çalıştırabilmek için gereken SMBIOS değerlerini ( MLB, ROM, SystemProductName, SystemSerialNumber and SystemUUID ) içermez. Her kullanıcı kendi değerlerini oluşturmak zorundadır. Google üzerinde konu ile ilgili araştırma yapın. Kullandığınız değerlerin gerçek bir Mac ya da Hackintosh kullanıcısı tarafından kullanılmadığından emin olmalısınız. Aksi takdirde Apple serverları kimliğinizi red edip uygulamaların kullanılmasına izin vermez. Ben iMessage, FaceTime dahil tüm hizmetleri kullanmaktayım. Test ettiğim SMBIOS modelleri: MacbookPro14,1 ; MacbookPro14,2 ; MacbookPro14,3’dür. Sistem MacbookPro15,1 vb gibi SMBIOS modelleri ile de yükleme yapabilir. İşlemci ailesi bu MacbookPro sınıfında olduğu için bu değerleri seçtim ve yıllardır bunları kullanıyorum.

> You should provide values for PlatformInfo>Generic> MLB, ROM, SystemProductName, SystemSerialNumber and SystemUUID

> Oluşturduğunuz SMBIOS değerlerini config dosyası içinde PlatformInfo>Generic kısmına yazmalısınız. ( MLB, ROM, SystemProductName, SystemSerialNumber ve SystemUUID
kısımları doldurulmalıdır )

* Eğer OpenCore ekranını görmeden direkt olarak macOS yüklemenmesini istiyorsanız config dosyası içinde Misc>Boot>ShowPicker değerini NO yapın ve Misc>Security>ScanPolicy kısmına 65795 değerini girin.

# macOS ve Windows’u OpenCore ile Yükleme

<b> Hayır bunu yapmayın! </b> Windows yüklenmesinin OpenCore üzerinden yüklenmesine kesinlikle karşıyım ve hiçbir artısı yoktur. En iyi çoklu seçim yapma şekli F12’ye basarak yapılandır.
# Disclaimer

Notebookunuzda oluşabilecek hiçbir sorundan beni sorumlu tutamazsanız. Eğer sizi arayıp polis olduğunu söyleyenler olursa kimlik bilgilerinizi de vermemelisiniz. Boktan hayatınız için kendinizi suçlayın.

<strong> Tüm bu işlemler eğitimsel amaçların siklenmesi ile olmuştur. Ticari kullanım yapılamaz. </strong>
