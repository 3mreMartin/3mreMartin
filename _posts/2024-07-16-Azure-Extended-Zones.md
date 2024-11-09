---
title: "Azure Extended Zone & Genişletilmiş Azure Bölgeleri"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Compute
  - Azure Extended Zones
---

 **Azure Extended Zones**, düşük gecikme süresi sağlamak ve regülasyon sorunlarını ortadan kaldırmak için metropol alanlarda, sanayi merkezlerinde veya belirli yerleşim bölgelerinde konumlandırılan ve sadece kritik hizmetler sunan veri merkezi uzantısıdır. Veri merkezi uzantısı denmesinin sebebi bu bölgeler temelde, Azure ekosistemine Microsoft Wide Area Network Ağı (WAN) aracılığıyla bağlı olan, ancak çok daha küçük bir yapıya sahip olan veri merkezleri olmasıdır. 

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-extended-zones.png?raw=true" width="85%" height="85%"/>

Genişletilmiş Bölgeler, normal Azure veri merkezlerinin yerine daha yerel bir kurulumda seçilmiş kritik hizmetleri sunar. Bu hizmetlerin kontrol düzlemi (control plane) Azure bölgesinde kalırken, veri düzlemi (data plane) Genişletilmiş Bölgelere dağıtılır. 

Örnek ile açıklamak gerekirse; Washington merkezli bir kuruluşunuzun olduğunu düşünün, tüm sunucularınız size en yakın Azure bölgesi olan West US 2 bölgesindeki Azure veri merkezinde konumlandırmış olsun.  Aradaki mesafe göz önüne alındığında Los Angeles ofisinde bulunan kullanıcılar, West US 2 veri merkezinde bulunan uygulama yada servislere erişmek istediklerinde networksel bir geçikme yaşayacaklardır. Bu noktada aynı uyguma yada servisleri Los Angeles’ta bulunan genişletilmiş Azure bölgelerine dağıtarak bu geçikme problemini ortadan kaldırabilirsiniz. Önizleme olarak duyurulan Genişletilmiş Bölgeler şuan için sadece Los Angeles şehrinde kullanılabilmektedir.  

**Genişletilmiş Bölgeler ile şuan için kullanılabilecek servisler:**

**Compute:** Azure Kubernetes Service, Azure Virtual Desktop, Virtual Machine Scale Sets, Virtual machines
**Networking:** DDoS, ExpressRoute, Private Link, Standard Load Balancer,Standard public IP, Virtual Network,Virtual network peering
**Storage:** Managed disks, Premium Page Blobs, Premium Block Blobs, Premium Files, Data Lake Storage Gen2, Hierarchical Namespace, Data Lake Storage Gen2
**BCDR:** Azure Site Recovery, Azure Backup

Daha fazlasi için: https://learn.microsoft.com/en-us/azure/extended-zones/overview

