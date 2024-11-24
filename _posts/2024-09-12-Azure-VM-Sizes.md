---
title: "A’ dan Z’ye Azure VM: Bölüm 5 – Azure VM Boyutları"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Compute
  - Azure VM Size
  - Constrained vCPU
  - VM Compute Unit
---
Azure sanal makinalar boyutları, belirli amaçlar için optimize edilmiş farklı aileler ve türler olarak kategorize edilmiştir. Farklı VM aileleri, yaygın kullanım senaryoları için tasarlanmıştır ve belirli sayıda CPU çekirdeği ve GB RAM içerir. Azure üzerinde bir sanal makina oluşturduktan sonra hyper-v ve Vmware ‘da olduğu gibi CPU ve RAM boyutu yükseltmek yada azaltmak mümkün olmadığından Azure üzerinde dağıtmak istediğinizde hangi VM serisini seçeceğiniz önemlidir.

Azure VM boyutları, farklı özellikler ve spesifikasyonları belirtmek için; *[Family] + [Sub-family]* + [# of vCPUs] + [Constrained vCPUs]* + [Additive Features] + [Accelerator Type]* + [Version]* şeklinde bir adlandırma kurallarını takip eder. İsimdeki her karakter, VM'nin farklı yönlerini temsil eder. Bunlar, VM ailesi, sanal CPU (vCPU) sayısı ve premium depolama veya dahil edilen hızlandırıcılar gibi ek özellikleri içermektedir.


<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-size.png?raw=true" width="85%" 
height="85%"/>

**Family:** ilk karakter VM ailesini temsil eder. Çoğu aile tek harf kullanılarak temsil edilir, ancak GPU boyutları (ND serisi, NV serisi, vb.) gibi diğerleri iki harf kullanır. 
**Subfamily:**  ikinci karatker VM ‘in üye olduğu alt aileyi temsil eder. Alt ailelerin çoğu tek bir büyük harfle gösterilir, ancak diğerleri (örneğin Ebsv5 serisi) özellik farklılıkları nedeniyle hala ana ailelerinin alt aileleri olarak kabul edilir.
**vCPU:** VM'nin vCPU sayısını belirtir.
**Feature 1:**  CPU için hiçbir özellik harfi listelenmemişse, seri Intel x86-64 CPU'ları kullanır. CPU AMD ise, **a** olarak listelenir. CPU ARM tabanlıysa (Microsoft Cobalt veya Ampere Altra), **p** olarak listelenir.
**Feature 2-3:** Bir boyut adında herhangi bir sayıda ek özellik bulunabilir. Bu ek özellikler hiç olmayabilir (örneğin, Dv5 serisi) veya üç tane olabilir (örneğin, Dplds_v6 serisi). Ek özellikler, VM'nin sunduğu belirli yetenekleri veya kaynakları belirtmek için kullanılır.

•	a = AMD-based processor
•	b = Block Storage performance
•	d = diskful
•	i = isolated size
•	l = low memory
•	m = memory intensive
•	p = ARM Cpu
•	t = tiny memory
•	s = Premium Storage
•	C = Confidential
•	NP = node packing

**Version:** Bu, VM aile serisinin sürümünü belirtir. Bir VM ailesinin sürümleri, birden fazla CPU çip mimarisini kapsayabilir.

##  Constrained vCPU Nedir ?

Kısıtlanmış (Constrained) vCPU modelinde, Microsoft bir VM için kullanıma açık vCPU sayısını sınırlar. Örneğin 64 vCPU olan bir VM, 32 vCPU olarak sınırlandırılabilir. Buna neden ihtiyaç vardır ?  Bir çok veri tabanı uygulaması , yüksek bellek, depolama ve I/O bant genişliğine ihtiyaç duysada, sanal makinanın yüksek çekirdek sayısına sahip olması performansa etkilemez.  Ayrıca bu tür iş yükleri (SQL Server, Oracle gibi) genellikle çekirdek başına lisanslama yapar ve bu lisanslama sistemi, ideal spesifikasyonlara sahip ancak fazla vCPU sayısına sahip bir sanal makina boyutunun, lisanslama maliyetlerinde önemli bir artışa neden olabileceği anlamına gelir işte bu noktada Azure, aynı bellek, depolama alanı ve G/Ç bant genişliğini korurken yazılım lisanslama maliyetini düşürmeye yardımcı olmak için daha düşük vCPU sayısıyla önceden tanımlanmış VM boyutları sunar. 


<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-constrained-cpu.png?raw=true" width="85%" 
height="85%"/>

Kısıtlanmış sanal makina yani Compute fiyatları, normal sanal makinarla aynıdır.   Kısıtlanmış sanal makinalardaki amaç çekirdek bazlı lisanlama maliyetlerini (SQL Server Enterprise için %75) düşürmektir. 

**Türe göre VM boyut ailelerinin listesi**

**A Serisi Sanal Makinalar (Giriş düzeyi ekonomik):** Bu VM'ler genellikle maliyet bilincinin performanstan daha önemli olduğu küçük dağıtımlarda kullanılır. Bu nedenle, yalnızca nadir durumlarda, müşteriyle etkileşimde bulunmayan sanal makinelerde kullanılmalıdır.

**B Serisi Sanal Makinalar:** B serisi, genellikle düşük ila orta düzeyde temel CPU kullanımıyla çalışan, ancak bazen talep arttığında önemli ölçüde daha yüksek CPU kullanımına geçmesi gereken iş yükleri için düşük maliyetli bir seçenek sunan ekonomik sanal makinelerdir.  
Geleneksel Azure sanal makineleri sabit CPU performansı sağlarken, B serisi sanal makineler CPU performans sağlama için kredi kullanan tek VM türüdür. B serisi VM'ler ne kadar CPU tüketildiğini izlemek için bir CPU kredi modeli kullanır - sanal makine, bir iş yükü temel CPU performans eşiğinin altında çalıştığında CPU kredileri biriktirir ve temel CPU performans eşiğinin üzerinde çalıştığında tüm kredileri tüketilene kadar kredileri kullanır. B serisi boyutları için 'Tüketilen CPU Kredileri', 'Kalan CPU Kredileri' ve 'Yüzde CPU' gibi B serisi kredi modeline özgü metriklere Azure Monitoring aracılığı ile zamanlı olarak izlenebilir.

**D Serisi Sanal Makinalar (Genel Amaçlı):** D serisi VM'ler, hızlı CPU'lar ve optimal CPU-bellek yapılandırması sunarak çoğu üretim iş yükü için uygun hale gelir. DSv3 serisi örnekler, D serisi ile aynı bellek ve disk yapılandırmalarına sahip olsalar da, daha güçlü CPU'lara sahiptir. Ancak, bu CPU çekirdekleri hiper işleme (hyper-threading) özelliğine sahiptir; bu, her iki CPU çekirdeğinin arkasında tek bir fiziksel CPU çekirdeği bulunduğu anlamına gelir.

**E Serisi Sanal Makinalar (Bellek içi için optimize edilmiş):** E serisi, SAP HANA gibi yoğun bellek uygulamaları için optimize edilmiştir. Bu VM'ler, yüksek bellek-çekirdek oranları ile yapılandırılmıştır, bu da onları ilişkisel veritabanı sunucuları için, orta ila büyük önbellekler ve bellek içi analizler için uygun hale getirir. E serisi VM'ler, sırasıyla 2 ila 64 vCPU ve 16-432 GiB RAM aralığında sunulmaktadır. E serisi, Azure Premium SSD'lerini de destekler.

**N Serisi Sanal Makinalar (GPU):** N serisi, GPU yeteneklerine sahip Azure Sanal Makine ailelerinden biridir. GPU'lar, hesaplama ve grafik yoğun iş yükleri için idealdir ve yüksek kaliteli görselleştirme, derin öğrenme ve tahmine dayalı analiz gibi senaryolar için uygundur.

*Kullanılabilir tüm boyutların listesine buradan erişebilirsiniz:*
[https://learn.microsoft.com/tr-tr/azure/virtual-machines/sizes/overview](https://learn.microsoft.com/tr-tr/azure/virtual-machines/sizes/overview)

*Ayrıca aşağıki linki kullanarak sanal makina performans ve fiyat karşılaştırması yapabilirsiniz:*
[https://cloudprice.net/](https://cloudprice.net/)

##  Azure Compute Unit (ACU) Nedir ?

ACU; Azure sanal makinelerinin performansını ölçmek için kullanılan bir ölçü birimidir ve belirli bir VM boyutunun sağladığı işlem gücünü ifade eder ve kullanıcıların farklı VM boyutları arasında karşılaştırma yapmalarına yardımcı olur. ACU, CPU ve bellek gibi kaynakların yanı sıra genel performans üzerinde etkili olan diğer faktörleri de dikkate alarak, Azure üzerinde sunulan sanal makinelerin performansını standart bir ölçüde sunar. Bu sayede, kullanıcılar ihtiyaçlarına en uygun VM boyutunu seçerken daha bilinçli kararlar verebilir. 
ACU, şu anda Küçük (Standard_A1) sanal makinenin 100 olarak standardize edilmesiyle tanımlanmıştır ve diğer tüm SKU'lar, bu SKU'nun bir standart benchmark'ı ne kadar daha hızlı çalıştırabileceğini yaklaşık olarak temsil eder. Bu sayede, farklı VM boyutlarının performansını karşılaştırmak daha kolay hale gelir.

*Azure sanal makinalar için ACU listesine buradan erişebilirsiniz:*
[https://learn.microsoft.com/en-us/azure/virtual-machines/acu/](https://learn.microsoft.com/en-us/azure/virtual-machines/acu/)