---
title: "A’ dan Z’ye Azure VM: Bölüm 3 – Yüksek Erişebilirlik Seçenekleri"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Compute
  - Azure High Availability
  - AzureAvailability 
---

Microsoft’un bulut hizmetlerini sağlamak için dünya genelinde bir çok fiziksel veri merkezi bulunmaktadır. Bu veri merkezleri  dünya genelinde Avrupa’dan Asya’ya, Amerika’dan Avusturalya’ya olmak üzere farklı kıtalarda yer almaktadır. Azure dünyasında veri merkezlerinin bulunduğu bu lokasyonlara *Bölge* (Azure Region) adı verilir ve her bölge, bir veya daha fazla veri merkezinden oluşur.  

Azure üzerinde bir kaynak oluşturacağınız zaman, ilgili kaynağın bulunduğunuz fiziksel lokasyona en yakın Azure bölgesine konumlandırmak kullanacağınız hizmetin performansını doğrudan etkileyecektir.  Bu sebebten fiziksel olarak Türkiye’de bulunan birinin Azure hizmetleri için Avrupa’daki veri merkezlerini (West Europe/North Europe) seçmesi mantıklı olacaktır. 

## Nedir bu SLA ?**

Azure üzerinde her servisin bir Hizmet Seviyesi Anlaşması (SLA - Service Level Agreement) bulunmaktadır. Örneğin, Azure üzerinde Premium Disk’e sahip bir sanal makine oluşturduğunuzda, Microsoft bu sanal makine için %99,9'luk bir SLA vermektedir. Bu da Microsoft dünyasında şu anlama gelir: Bu sanal makinenin bulunduğu Azure veri merkezlerinde, Azure Fabric Layer katmanında oluşabilecek donanımsal (depolama, ağ aygıtları, güç ve soğutma gibi) problemlerde dahi %99,9 ulaşılabilir olacağının sözünü veriyoruz. Bu durumda, sanal makine bir yıl içerisinde en fazla ~9 saat (8,77 saat) ulaşılamaz olabilir. 10. saatte bu sanal makine halen ulaşılamaz ise Microsoft vermiş olduğu sözü tutamamış olur. Bazı kuruluşlar için yılda ~9 saatlik kesinti riski kabul edilebilir olsa da, finans, havacılık gibi kritik kuruluşlar için bu kabul edilebilir değildir. Standart Disk kullanan sanal makineler için ise bu SLA %95'tir, yani yılda ~2 günlük kabul edilebilir kesinti anlamına gelir.

* [https://en.wikipedia.org/wiki/High_availability](https://en.wikipedia.org/wiki/High_availability)
* [https://www.azure.cn/en-us/support/sla/virtual-machines/](https://www.azure.cn/en-us/support/sla/virtual-machines/)


**Seçenekler Neler ?**


<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-high-availability-options.png?raw=true" width="85%" 
height="85%"/>

**Availability Set %99.95 SLA**

Kullanılabilirlik Seti  (Kullanılabilirlik Seti) birden fazla Azure sanal makinalarının aynı veri merkezi içerisinde fiziksel olarak farklı donanımlar üzerinde tutulmasını sağlar.  Availability Set ‘ler mantıksal olarak iki gruptan oluşur Update Domains ve Fault Domains:


<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-availability-set.png?raw=true" width="85%" height="85%"/>

•	Update Domains (UD): Azure veri merkezi içerisindeki aynı anda yeniden başlatılabilecek ve bakıma alınabilecek mantıksal VM gruplardır.
•	Fault Domains (FD): Azure veri merkezi içerisinde aynı donanımsal kaynaklarını (Power ve soğutma gibi) kullanan mantıksal VM gruplarıdır.

Availability Sets içerisine dahil ettiğiniz sanal makinalar (en az 2 VM) için verilen SLA %99.95 yani yilda en fazla 4.5 saatlik kabul edilebilir bir kesinti demektir. Availability Sets kullanımı ücretsizdir sadece sanal makinalar için ücretlendirilirsiniz.

Availability Sets kullanarak sanal makinamızın aynı veri merkezi içerisindeki erişebilirliğini garantiledik. *Peki ya bu veri merkezi bir felaket sonucu tamamen erişilemez olursa ?* şeklinde endişeleriniz varsa o zaman *Availability Zone* kullanabilirsiniz.

**Availability Zone %99.95 SLA**

Kullanılabilirlik Bölgesi (Kullanılabilirlik Bölgesi) en başta bahsettiğimiz gibi Azure dünyasında veri merkezlerinin bulunduğu lokasyonlara *Bölge* (Azure Region) adı verilir ve her bölge, bir veya daha fazla veri merkezinden oluşur.  Availability Zone ise aynı Azure bölgesi içerisinde bulunan ancak fiziksel olarak aralarında en az 400-500 km mesafe bulunan birbirinden tamamen bağımsız veri merkezlerıni ifade eder.  Bu veri merkezleri yüksek performanslı bir ağ ile birbirlerine bağlıdır ve gidiş-dönüş gecikmesi 2 ms'den daha azdır.


<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-availability-zone.png?raw=true" width="85%" height="85%"/>

Availability Zone içerisine dahil ettiğiniz sanal makinalar için verilen SLA %99.99 yani yilda en fazla ~ 53 dk lik kabul edilebilir bir kesinti demektir. Bir sanal makinayı Availability zone içerisine dahil edeceğiniz zaman sanal makinanın hangi bölgede (Zone) olması gerektiğini seçmeniz gerekir.  Birden fazla bölge seçmeniz durumunda sanal makinanın bir kopyası seçtiğiniz diğer bölgelerdede oluşturulur diğer bölgelerde oluşturulan makinalar içinde ayrıca ücretlendirilirsiniz. 

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-availability-zone-2.png?raw=true" width="85%" height="85%"/>

*Availability Zone*, sanal makinalar haricinde diğer Azure servisleri ile de kullanılabilir. Availability Zone destekleyen Azure Servisleri iki kategoriye ayrılır:

*Zonal Services (Bölgesel Servisler):* Belirli bir Zone içerisine aldığınız servislerdir. (VM, Managed Disks, IPs) 
Zone-Redundant Services (Bölge-Yedeklemeli Hizmetler): Zone’lar arasında platform replikasyonu kullanan servislerdir. (Zone-redundant storage, SQL Database)

*Daha fazlasi için:* 
[https://learn.microsoft.com/en-us/azure/reliability/availability-zones-service-support](https://learn.microsoft.com/en-us/azure/reliability/availability-zones-service-support)
