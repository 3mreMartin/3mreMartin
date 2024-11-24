---
title: "A’ dan Z’ye Azure VM: Bölüm 6 – Azure VM Hibernation"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Compute
  - Azure VM Hibernation

---
Hazırda bekletme (Hibernation), kullanılmayan sanal makinelerin durdurularak **Compute** maliyetlerini azaltmanıza olanak tanıyan etkili bir maliyet yönetim özelliğidir. Bir sanal makine hazırda bekletildiğinde, makinenin belleğinde bulunan veriler korunur, ancak sanal makine serbest bırakılır **(deallocated)** ve makine kullanımı için faturalandırılmaz. Yalnızca sanal makineye bağlı depolama (işletim sistemi diski, veri diskleri) ve ağ kaynakları (IP adresleri vb.) için faturalandırılırsınız.


**Hazırda Bekletme Örnek Senaryolar**

**Azure Virtual Desktop:** Sanal masaüstü altyapısı (VDI) gibi ortamlarda, kullanıcılar masaüstlerini kullanılmadığında hazırda beklemeye alarak kaynak tüketimini minimize edebilir.
**Geliştirme ve Test Ortamları:** Geliştirme ve test sunucuları genellikle dalgalanan kullanım desenlerine sahiptir. Geliştiriciler, üzerinde çalışmadıkları zaman ortamları hazırda beklemeye alarak kaynakları optimize edebilir.
**Uzun Başlatma Süreleri:** Büyük bellek gereksinimleri nedeniyle başlatma süresi uzun olan uygulamalar için hazırda bekletme, sanal makinelerin önceden hazır duruma gelmesini sağlar. Böylece uygulama yüklü bir şekilde hızlıca yeniden başlatılabilir, verimlilik ve kullanıcı deneyimi artar.
**Maliyet Tasarrufu:** Kullanılmayan sanal makineleri hazırda beklemeye alarak, sürekli çalışmaya ihtiyaç duymayan iş yükleri için kaynak kullanımını önemli ölçüde azaltabilirsiniz.


## Hazırda Bekletme ile Desteklenmeyen Özellikler:

•	Ephemeral OS disks
•	Shared disks
•	Availability Sets
•	Virtual Machine Scale Sets Uniform
•	Spot VMs
•	Managed images
•	Azure Backup
•	Capacity reservations

## Genel Sinirlamalar:

•	Mevcut VM’lerde hazırda bekletme modunu etkinleştiremezsiniz.
•	Hazırda bekletme modu etkinleştirilmişse bir VM’yi yeniden boyutlandıramazsınız.
•	Bir VM hazırda bekletme modundayken, VM ile ilişkili herhangi bir diski veya NIC’yi ekleyemez, çıkaramaz veya değiştiremezsiniz
•	Bir VM hazırda bekletme moduna alındığında, VM’yi daha sonra başlatmak için yeterli kapasitenin olmasını sağlayacak bir kapasite garantisi yoktur. Bu durumda makinanın tekrardan başlatılması gerekir (Kapasite rezervasyonları, hazırda bekletilen VM’ler için kapasiteyi garanti etmez.)
•	İşletim sistemi üzerinden VM’i hazırda bekletme, VM’nin hazırda bekleme durumuna geçmesine neden olmaz ve VM faturalandırılmaya devam eder.
•	Etkinleştirilen bir VM’de hazırda bekletme modunu devre dışı bırakamazsınız.

**Hazırda Bekletme Nasil Çalışır ?**

Bir VM hazırda beklettiğinizde Azure, VM’nin işletim sistemine diski askıya alma eylemi gerçekleştirmesi için sinyal gönderir. Azure, VM’nin bellek içeriğini işletim sistemi diskinde depolar ve ardından VM’nin konumunu serbest bırakır. VM yeniden başlatıldığında bellek içerikleri işletim sistemi diskinden tekrar belleğe aktarılır. Daha önce VM’nizde çalışmakta olan uygulamalar ve işlemler, hazırda bekletme modundan önceki durumdan devam eder.

Hazirda bekleme bir sanal makina oluştururken etkinleştirebileceği gibi mevcut sanal makinalar içinde etkinleştirilebilir.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-Hibernation.png?raw=true" width="85%" height="85%"/>

 *Daha fazlası için*
[Hibernating Windows virtual machines](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/hibernate-resume-windows?tabs=enableWithPortal%2CenableWithPSExisting%2CPortalDoHiber%2CPortalStatCheck%2CPortalStartHiber%2CPortalImageGallery)
