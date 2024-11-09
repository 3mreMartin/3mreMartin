---
title: "A’ dan Z’ye Azure VM: Bölüm 1 – Lisanslama"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Compute
  - AHUB
---

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-ahub.png?raw=true" width="85%" height="85%"/>

## Azure Hybrid Benefit Nedir ?


Azure Hybrid Benefit, Microsoft’un Azure üzerindeki Windows Server lisans maliyetlerini düşürmek amacıyla kullanıcılara sunduğu bir programdır. Bu program kapsamında, Yazılım Güvencesine (Software Assurance) sahip aktif bir lisans anlaşmanız veya aboneliğiniz varsa, sahip olduğunuz şirket içi Windows lisanslarınızı Azure üzerinde kullanabilmenize olanak tanır.


## Lisans Gereksinimleri

 Azure Hybrid Benefit'ten yararlanabilmek için Windows Server için şu lisanslara sahip olmanız gerekmekte:

* Aktif Software Assurance veya aboneliği olan Windows Server Standard.
* Aktif Software Assurance veya aboneliği olan Windows Server Datacenter.

## Lisans Adedi

Her sanal makine için minimum 8 çekirdek lisansına (Datacenter veya Standard sürümü) ihtiyaç vardır. Örneğin, 4 çekirdekli bir örnek çalıştırıyorsanız bile, yine de 8 çekirdek lisansı gereklidir.

Azure hesaplayıcısı üzerinde *D2a V4* bir sanal makinas için lisans dahil ve lisansız (AHUB) olacak şekilde fiyat karşılaştırması şu şekildedir:

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-ahub-cost.png?raw=true" width="85%" height="85%" />

## Nasıl Aktif Edilir ?

Azure Hybrid Benefit Azure portal üzerinden bir sanal makina oluşturken aktif edilebileceği gibi mevcut sanal makinalar içinde **Azure Portal -> Virtual Machines-> Operation System** altında bulunan **Licensing** kısmından aktif edilebilir.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-enable-ahub.png?raw=true" width="85%" height="85%" />

Birden fazla Windows server üzerinde Azure Hybrid Benefit’i aktif etmek için [bu](https://github.com/NeilBird/PowerShell-Snippnets/tree/main) script’i kullanabilirsiniz:

**Windows 10/11?**

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-ahub-win10.png?raw=true" width="85%" height="85%" />

Mevcut Windows 10/11 lisanslarınızı Azure üzerinde kullanabilmek için sahip olduğunuz lisansların **Multi-Tenant Hosting Rights** haklarına sahip olması gerekir. Yani tek bir kullanıcıya atanan bir lisansın, Windows 11 Enterprise E3/E5 ya da Virtual Desktop Access lisansı gibi birden fazla platform/cihazda kullanılabilirliği desteklemesi gerekir. Bu kullanılabilirlik, kullanıcının aynı Windows’u hem laptopta hem Azure'da hem de AVD ortamında kullanabilmesine olanak sağlar.