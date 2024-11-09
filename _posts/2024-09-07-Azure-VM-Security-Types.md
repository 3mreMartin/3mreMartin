---
title: "A’ dan Z’ye Azure VM: Bölüm 2 – Sanal Makina Güvenlik Türleri"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Compute
---

Azure üzerinde bir sanal makina oluşturmak istediğinizde, o makina için sahip olmak istediğiniz Güvenlik Türünü (Security Type) seçmeniz gerekir.  Güvenlik türü, bir sanal makine için kullanılabilecek farklı güvenlik özelliklerini ifade eder. Microsoft; **Standard**, **Trusted Lauch** ve **Confidential Virtual Machine** olmak üzere sanal makinalar ile kullanabileceğimiz üç farklı güvenlik türü sunar.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-security-type.png?raw=true" width="85%" height="85%"/>


## Standard

Azure portal üzerinden bir sanal makina oluşturmak istediğinizde varsayılan güvenlik türü olarak Trusted launch seçili gelmektedir.  Standart güvenlik türü, en temel güvenliği sağlamayı amaçladığından varsayılan olarak sunulmaz. Standart güvenlik türü, sanal makinenizin daha yüksek güvenlik seviyeleriyle uyumlu olmayacağı durumlarda kullanılmalıdır.

## Trusted Launch Virtual Machines

Güvenilir Başlatma (Trusted Launch), Azure tarafından sanal makineler için sunulan bir güvenlik özelliğidir. Bu özellik, bootkit ve rootkit saldırılarına karşı geliştirilmiş koruma sağlamak üzere tasarlanmıştır. Böylece sanal makinelerinizin güvenli bir şekilde başlamasını ve çalışırken güvende kalmasını garanti etmektedir. 

Güvenilir Başlatma, özellikle önyükleme sürecini ve sanal makinelerinizin durumunu korumak için güvenli önyükleme, virtual trusted platform module (vTPM), ve virtual machine console (VMC) gibi gelişmiş teknolojileri kullanarak  sanal makinelerinizin önyükleme sürecini hedef alan kötü niyetli tehditlere karşı korunmasını sağlar.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-trusted-launch.png?raw=true" width="85%" height="85%"/>

**Güvenilir Başlatma (Secure Boot)**

Güvenilir Başlatma'nın temelinde, sanal makineleriniz için Güvenli Önyükleme (Secure Boot) bulunmaktadır. Güvenli Önyükleme, platform donanımında uygulanan bir özelliktir ve kötü amaçlı yazılım tabanlı rootkit'lerin ve bootkit'lerin kurulmasını engeller. Güvenli Önyükleme, yalnızca imzalı işletim sistemlerinin ve sürücülerin önyüklemesine izin vererek, sanal makinenizdeki yazılım yığınında bir "güven kökü" oluşturur. Güvenli Önyükleme etkinleştirildiğinde, tüm işletim sistemi önyükleme bileşenleri boot loader, kernel, kernel drivers) güvenilir yayıncıların imzasını gerektirir. Hem Windows hem de bazı Linux dağıtımları Güvenli Önyüklemeyi destekler. Eğer Güvenli Önyükleme, görüntünün güvenilir bir yayıncı tarafından imzalandığını doğrulamazsa, sanal makine önyükleme işlemini gerçekleştiremez.

**vTPM**

Güvenilir Başlatma, Azure sanal makineleri için sanal Trusted Platform Module (vTPM) de sunar. Bu sanallaştırılmış donanım Güvenilir Platform Modülü, TPM 2.0 standardına uygundur ve anahtarlar ile ölçümler için özel bir güvenli depo işlevi görür. 

Güvenilir Başlatma, vTPM aracılığıyla bulut üzerinden uzaktan güvenilirlik doğrulaması (attestation) yapar. Bu doğrulamalar, platform sağlık kontrollerini gerçekleştirmek için kullanılır ve güvene dayalı kararlar almakta yardımcı olur. Bir sağlık kontrolü olarak, Güvenilir Başlatma, sanal makinenizin doğru bir şekilde önyüklendiğini kriptografik olarak sertifikalandırabilir.

Eğer bu süreç başarısız olursa, muhtemelen sanal makineniz yetkisiz bir bileşen çalıştırdığı için, Microsoft Defender for Cloud bütünlük uyarıları gönderir. Uyarılar, hangi bileşenlerin bütünlük kontrollerini geçemediğine dair detayları içerir.

**Virtualization-based Security**

Sanallaştırma tabanlı güvenlik (VBS) güvenli ve izole bir bellek alanı oluşturmak için hipervizörü kullanır. Windows, bu alanları, zafiyetlere ve kötü niyetli istismarların etkilerine karşı artırılmış koruma ile çeşitli güvenlik çözümleri çalıştırmak için kullanır. 

Güvenilir Başlatma, hipervizör kod bütünlüğünü (hypervisor code integrity -HVCI) ve Windows Defender Kimlik Koruyucusu'nu (Credential Guard) etkinleştirmenizi mümkün kılar.

HVCI, Windows çekirdek modu süreçlerini kötü niyetli veya doğrulanmamış kodun enjekte edilmesi ve çalıştırılmasına karşı koruyan güçlü bir sistem önlemesidir. Çekirdek modu sürücülerini ve ikili dosyaları çalıştırmadan önce kontrol eder, imzasız dosyaların belleğe yüklenmesini engeller. Yüklenmesine izin verilen yürütülebilir kodun, yüklendikten sonra değiştirilmesini önleyen kontroller gerçekleştirir.

**Desteklenmeyen özellikler:**

* Azure Site Recovery
*	Managed Image
*	Nested virtualization
*	Linux VM Hibernation

Trusted Lauch için desteklenen sanal makina ve işletim sistemlerinin listesine buradan erişebilirsiniz:
*https://learn.microsoft.com/en-gb/azure/virtual-machines/trusted-launch#virtual-machines-sizes*
*https://learn.microsoft.com/en-gb/azure/virtual-machines/trusted-launch#operating-systems-supported*

## Confidential virtual machines

Gizli sanal makinalar (Confidential virtual machines), Güvenilir Başlatma (Trusted Launch) güvenlik seçeneğinin sunduğu tüm özelliklerine sahiptir ve buna ek olarak ekstra gizlilik ve bütünlük garantisi sunar.

**Confidential OS disk Encryption**

Azure Gizli Sanal Makineleri, yeni ve geliştirilmiş bir disk şifreleme şeması sunar. Bu şema, diskin tüm kritik bölümlerini korur. Ayrıca, disk şifreleme anahtarlarını sanal makinenin TPM'sine bağlar ve korunmuş disk içeriğini yalnızca sanal makineye erişilebilir hale getirir. 

Bu şifreleme anahtarları, hipervizör ve ana işletim sistemi gibi Azure bileşenlerini güvenli bir şekilde geçebilir. Saldırı potansiyelini en aza indirmek için, sanal makinenin ilk oluşturulması sırasında diski şifreleyen özel bir bulut hizmeti de bulunmaktadır. 
Gizli işletim sistemi disk şifrelemesi sanal makine oluşturma süresini uzatabileceğinden isteğe bağlıdır. Disk şifrelemesi için  platform-managed keys (PMK)  yada customer-managed key (CMK) seçeği kullanılabileceği gibi isterseniz disk şifrelemesini bu aşamada aktif etmeme seçeneğinizde mevcut. Disk şifreleme işletim sistemi diskine ek olarak geçici diskler içinde uygulanabilir. 

**Desteklenmeyen özellikler:**

*	Azure Batch
*	Azure Backup
*	Azure Site Recovery
*	Limited Azure Compute Gallery support
*	Shared disks
*	Accelerated Networking
*	Live migration
*	Screenshots under boot diagnostics

*Confidential Virtual Machines ile desteklenen sanal makina ve işletim sistemlerinin listesine buradan erişebilirsiniz:* 
* [https://learn.microsoft.com/en-us/azure/confidential-computing/confidential-vm-overview#size-support](https://learn.microsoft.com/en-us/azure/confidential-computing/confidential-vm-overview#size-support)
* [https://learn.microsoft.com/en-us/azure/confidential-computing/confidential-vm-overview#os-support](https://learn.microsoft.com/en-us/azure/confidential-computing/confidential-vm-overview#os-support)