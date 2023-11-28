---
title: "Azure VM'leri Hazırda Bekleterek Maliyetlerinizi Düşürün"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Cost
  - Preview
---
📢 **Public Preview**

Yeni gelen bu özellik ile artık Azure VM’leri Hyper-V yada VMware ortamlarından alışkın olduğunuz şekilde hazırda bekletebilirsiniz (hibernate). Bir VM hazırda bekletildiğinde  makinanın belleğinde bulunan veriler korunarak VM serbest bırakılır (deallocating). Azure üzerinde bir VM serbest bırakıldığında, VM kullanımı için **faturalandırılmaz** yalnızca VM'ye bağlı depolama (İşletim Sistemi diski, veri diskleri) ve ağ kaynakları (IP'ler vb.) için faturalandırılırsınız.

Şuan için önizlemede olan bu özellik ile test/geliştirme ve Azure Virtual Dekstop ortamlarındaki makinalar mesai bitiminden bir sonraki güne kadar hazırda bekletilerek ciddi tasarruf sağlanabilir. Hazırda bekletme makinanın belleğinde bulunan verileri koruduğudan dilediğiniz zaman uygulamaları yeniden açmaya gerek kalmadan kaldığından yerden çalışmaya devam edebilirsiniz.

Not: Hazırda bekletme makinanın belleğinde bulunan verileri makinanın diski içerisine yazarak koruyacağından yeterli disk alanı olduğundan emin olmalısınız.

Hazırda bekletme şuan için belirli sanal makina boyutu ve işletim sistemi sürümleriyle sınırlı. Tüm listeye [buradan](https://learn.microsoft.com/en-us/azure/virtual-machines/hibernate-resume?tabs=osLimitsWindows%2CenablehiberPortal%2CcheckhiberPortal%2CenableWithPortal%2CcliLHE%2CUbuntu18HST%2CPortalDoHiber%2CPortalStatCheck%2CPortalStartHiber%2CPortalImageGallery#operating-system-support-and-limitations) erişebirlisiniz.

## Genel Sınırlamalar:

* Mevcut VM'lerde hazırda bekletme modunu etkinleştiremezsiniz.
* Hazırda bekletme modu etkinleştirilmişse bir VM'yi yeniden boyutlandıramazsınız.
* Bir VM hazırda bekletme modundayken, VM ile ilişkili herhangi bir diski veya NIC'yi ekleyemez, çıkaramaz veya değiştiremezsiniz
* Bir VM hazırda bekletme moduna alındığında, VM'yi daha sonra başlatmak için yeterli kapasitenin olmasını sağlayacak bir kapasite garantisi yoktur. Bu durumda makinanın tekrardan başlatılması gerekir (Kapasite rezervasyonları, hazırda bekletilen VM'ler için kapasiteyi garanti etmez.) 
* İşletim sistemi üzerinden VM'i hazırda bekletme, VM'nin hazırda bekleme durumuna geçmesine neden olmaz ve VM faturalandırılmaya devam eder.
* Etkinleştirilen bir VM'de hazırda bekletme modunu devre dışı bırakamazsınız.

##  Hazırda Bekletme ile Desteklenmeyen Özellikler:

* Ephemeral OS disks
* Shared disks
* Availability Sets
* Virtual Machine Scale Sets Uniform
* Spot VMs
* Managed images
* Azure Backup
* Capacity reservations

## Hazırda Bekletme Nasil Çalışır ?

Bir VM hazırda beklettiğinizde Azure, VM'nin işletim sistemine diski askıya alma eylemi gerçekleştirmesi için sinyal gönderir. Azure, VM'nin bellek içeriğini işletim sistemi diskinde depolar ve ardından VM'nin konumunu serbest bırakır. VM yeniden başlatıldığında bellek içerikleri işletim sistemi diskinden tekrar belleğe aktarılır. Daha önce VM'nizde çalışmakta olan uygulamalar ve işlemler, hazırda bekletme modundan önceki durumdan devam eder. 

## Hazırda Mekletme Modunu Yapılandırma

1. Bu özelliği kullanabilmek  için ilk **Azure Portal -> Preview features** altından **Hibernation Preview** özelliğini kayıt etmeniz gerekmekte. Dilerseniz bu işlemi PowerShell yada [CLI](https://learn.microsoft.com/en-us/azure/virtual-machines/hibernate-resume?tabs=osLimitsLinux%2CenablehiberPortal%2CcheckhiberPS%2CenableWithPortal%2CcliLHE%2CUbuntu18HST%2CPortalDoHiber%2CPortalStatCheck%2CPortalStartHiber%2CPortalImageGallery#enabling-hibernation-feature-for-your-subscription) ile de yapabilirsiniz. 

2.	Hazırda bekletme özelliğini aktif ederek bir makina oluşturun. Bu işlem Windows yada Linux makina için gerekli Extension’ı makinaya otomatik olarak yükleyecektir. Windows makinalar için Azure VM Agent’ında makinaya yüklü olması gerekmekte.

<img src="https://github.com/martin3mre/martin3mre/blob/main/assets/images/azure-vm-hibernate-I.png?raw=true" width="80%" height="80%" />

3.	İşletim sistemini hazırda bekletme için yapılandırın. Windows makinalar için yüklenen hazırda bekletme Extension’ı işletim sistemini hazırda bekletme için otomatik olarak yapılandırır. Linux makinalar için [buradan](https://learn.microsoft.com/en-us/azure/virtual-machines/hibernate-resume?tabs=osLimitsLinux%2CenablehiberPortal%2CcheckhiberPS%2CenableWithPortal%2CcliLHE%2CUbuntu18HST%2CPortalDoHiber%2CPortalStatCheck%2CPortalStartHiber%2CPortalImageGallery#option-1-linuxhibernateextension) yararlanabilirsiniz.

Hazırda bekleme özelliği hazır, Azure portal üzerinden Hibernate seçeneği ile makinanızı hazırda bekleymeye alabilirsiniz.

<img src="https://github.com/martin3mre/martin3mre/blob/main/assets/images/azure-vm-hibernate-II.png?raw=true" width="80%" height="80%" />