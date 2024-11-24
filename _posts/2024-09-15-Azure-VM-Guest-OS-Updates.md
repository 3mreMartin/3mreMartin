---
title: "A’ dan Z’ye Azure VM: Bölüm 6 – Azure VM Guest OS Updates"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Compute
  - Azure OS Updates

---
<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-os-updates.png?raw=true" width="85%" height="85%"/>


**Hotpatch**
 Desteklenen Windows  sanal makinelerinde işletim sistemi güvenlik güncellemelerini yeniden başlatma gereki olmadan yükleyebilmenizi sağlar. Bu yöntem, çalışan işlemlerin bellekteki kodlarını, işlemi yeniden başlatmaya gerek kalmadan yamalayarak çalışır. Hotpatch Azure ve Azure Stack HCI üzerindeki sanal makinalarda, Server 2022 Datacenter versiyonlarını desteklemektedir ve varsayılan olarak Azure portal aracılığı ile bir sanal makina oluşturduğunuzda aktif olarak gelmektedir. 


## Patch Orchestration Options

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-patch-orchestration-options.png?raw=true" width="85%" height="85%"/>

Yama orkestrasyon seçeneklerinde güncellemelerin Windows yada Linux sanal makinalara nasıl dağıtılacağını seçebileceğiniz dört farklı seçenek bulunmaktadır:


**Azure-orchestrated (AutomaticByPlatform)**

Bu seçenek VM guest patching olarakta karşınıza çıkabilir. Bir sanal makina için güncellemelerin Azure tarafından yönetilmesini seçerseniz, ilgili sanal makinaya **Microsoft.CPlat.Core.PatchExtension** eklentisi otomatik olarak yüklenir. Yüklenen bu eklenti sayesinde Kritik ve Güvenlik yamaları, sanal makinaya otomatik olarak indirilir ve uygulanır. Bu süreç, yeni yamalar yayımlandığında her ay otomatik olarak başlatılır. Yama değerlendirmesi (assesment) ve kurulumu otomatik olarak gerçekleştirilir ve gerektiğinde sanal makinanın yeniden başlatılmasını da gerçekleştirir.

Yama değerlendirmesi bir kaç günde bir ve 30 gün içerisinde bir kaç kez olacak şekilde değerlendirilir, böylece o sanal makina için uygun yamalar belirlenir. Kritik ve Güvenlik olarak sınıflandırılmayan diğer yamalar bu değerlendirmeye dahil edilmez. Belirlenen yamalar sanal makinanın zaman dilimine bağlı yoğun olmayan saatlerde otomatik olarak uygulanır.

Azure platformu global seviyede oluşabilecek dağıtım hatalarını önlemek için güncellemeler aşamalı şekilde dağıtılır.  Örneğin Batı Avrupa ve Kuzaey Avrupa gibi eşleştirilmiş Azure bölgeleri (Paired) aynı anda güncellenmez.  Aynı Azure bölgesinde ise farklı Kullanılabilirlik Bölgelerinde bulunan sanal makinalar yada bir kullanılabilirlik kümesinin parçası olmayan sanal makineler aynı anda güncellenmez.  

* Bu mod hem Linux hem de Windows VM'leri için desteklenir
* Bu mod, sanal makine için otomatik VM misafir yamanmasını (Guest patching) etkinleştirir ve sonraki yama kurulumları Azure tarafından koordine edilir.
* Yükleme işlemi sırasında bu mod, VM'de kullanılabilir yamaları değerlendirecek ve ayrıntıları Azure Resource Graph’a kaydedecektir.
* Windows VM'leri için bu modu ayarlamak, Windows sanal makinesindeki yerel Otomatik Güncelleştirmeleri de devre dışı bırakır.


**AutomaticByOS:**

* Bu mod sadece  Windows VM'leri için desteklenir
* Bu mod, Windows sanal makinesinde Otomatik Güncellemeleri etkinleştirir ve yamalar, Otomatik Güncellemeler aracılığıyla sanal makineye kurulur.
* Bu mod, başka bir yama modu belirtilmediği takdirde varsayılan olarak bir Windows VM için ayarlanır.

**Manuel:**

* Bu mod sadece  Windows VM'leri için desteklenir
* Bu mod, Windows sanal makinesinde Otomatik Güncelleştirmeleri devre dışı bırakır.
* Bu mod, özel yama çözümleri kullanılırken ayarlanmalıdır


**ImageDefault:**

* Bu mod sadece  Linux VM'leri için desteklenir
* Bu mod, VM'yi oluşturmak için kullanılan görüntüdeki varsayılan yama yapılandırmasını dikkate alır.
* Bir Linux VM için başka bir yama modu belirtilmemişse, bu mod varsayılan olarak ayarlanır.

Hotpatch ve  Patch orchestration ayarları portal aracılığı ile bir sanal makina oluştururken yapılabileceği gibi, mevcut sanal makinalar içinde Azure Update Manager altından değiştirilebilir.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-os-update-manager.png?raw=true" width="85%" height="85%"/>

