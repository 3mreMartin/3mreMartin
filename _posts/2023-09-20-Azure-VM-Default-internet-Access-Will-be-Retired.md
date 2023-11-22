---
title: "Azure VM`ler için Varsayılan Internet Erişimi Kullanımi Emekli Oluyor."
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure App Service
---
  
Belirli senaryolar Azure VM’lerin doğrudan internet bağlantısına sahip olmasını gerektirir. Azure sanal makineler, organizasyon ihtiyaçlarına uygun olarak NAT Gateway, Load Balancer veya Instance Level - Public IP kullanarak doğrudan internete bağlanabilirler. Her bir yöntemin ortak özelliği, internete erişim için kullanılan Public IP'nin size ait, sizin yönetiminizde ve başkaları tarafından kullanılmamasıdır. Bu yöntemler kullanılmadığında, Azure VM’ler, varsayılan olarak **Default Outbound Access** olarak adlandırılan yöntemle internete bağlanırlar.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-direct-internet-acess.png?raw=true" width="95%" height="95%" />
  
Varsayılan bu yöntemde Azure VM’ler internet bağlantısı için IP havuzundan atanan rastgele bir Public IP adresini kullanırlar. Bu Public IP Microsoft’a ait olduğudan makinaların Stop/Deallocate durumlarında değişebilileceği gibi öncesinde hangi Public IP’yi kullanacağınızı belirlemeninde bir yolu yoktur ve bu sebebten üretim ortamlarında kullanılması önerilmez.

Microsoft tüm bu sebebleri göz önünde bulundurarak güvenlik sebebi ile **30 Eylül 2025** itibarı ile artık bu yöntemin kullanımdan kaldıracağını duyurdu. 

 Aksiyon almak için iki senemiz daha olsada yazının devamında Azure VM’ler için sahip olduğumuz diğer internet bağlantı seçeneklerini inceliyor olacağız:

*	Azure Load Balancer Frontend IP’sini kullanarak
*	Subnet’e NAT Gateway konumlandırarak
*	Sanal Makinaya Public IP Atayarak

## Öncesinde SNAT Nedir ? SNAT Port Nedir ? SNAT Port Exhaustion Nedir ?

Basitçe ifade etmek gerekirse, yerel bir ağa bağlı her bir cihazın192.168.x.x formatında bir özel IP'si bulunmaktadır. Bu özel IP adresi sadece aynı yerel ağda  bulunan diğer cihazlardan erişilebilir. Bu formattaki bir IP adresi, internet dünyasında yani yerel ağınız dışındaki dünyada yönlendirilebilir olmadığı için kullanılamaz. Bu sebeble, yerel ağa bağlı bir cihazdan internet dünyasında bulunan bir kaynağa erişmek istediğinizde, bu özel IP adresinin genellikle bir ağ geçidi yada güvenlik duvarı aracılığıyla Public IP adresi ile değiştirilmesi ve hedef port bilgisinin bu bağlantıya eklenmesi gereklidir ve bu işleme SNAT (Source Network Address Translation) adı verilir.

**SNAT Port ve SNAT Port Exhaustion Nedir ?**

TCP ve UDP iletişiminde kaynak ve hedef bilgilerini belirlemek için her iki protokolde 16 bit ayrılmıştır  buda kullanılabilecek 65535 port demektir.  Bu portların 0-1023 arası tanınmış (well-known) portlar, 1024-49151 arası kayıtlı (Registered) portlar, ve 49152-65535 arası Kısa ömürlü (Ephemeral) portlardır. Tanınmış ve Kayıtlı portlar  sunucu rolününe sahip (http, ftp v.b)  bağlantılarda kullanılırken, Kısa ömürlü portlar ise istemci tarafından bir sunucuya bağlanırken kullanılır. 
Internet ortamındaki bir kaynağa erişeceğiniz zaman sizin için Kısa Ömürlü bir port tahsis edilir ve aslında yapabileceğiniz bağlantı sayısı sahip olduğunuz kısa ömürlü port sayısı (49152-65535) kadardır.  Kısa ömürlü portlar kullanıldıktan bir süre sonra tekrar iletişim kurma ihtimaline karşı kısa bir süre (idle timeout) bekletilirler. Bağlantı yapmak için kullanabileceğimiz yeni Kısa Ömürlü port kalmadığında bu duruma Port Exhaustion yani portların tükenmesi adı verilir. Port Exhaustion durumu SNAT yapılan cihaz yada servis üzerinde (Azure Load Balancer, NAT Gateway, Firewall) meydana gelir. Bu durumda yeni bağlantı yapabilmek için  idle timeout süresinin dolması yani port’ların tekrardan kullanılabilir olmasını beklemek gerekir. Bu süre Azure Load, NAT gateway ve Azure Firewall için varsayılanda 4dk dır.

## Azure VM’ler için Internet Erişim Seçekleri

**1.Azure Load Balancer**

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-loadbalancer-internet-acess.png?raw=true" width="95%" height="95%" />

Azure Load Balancer arkasına konumlandırılan sanal makinalar, Load Balancer üzerindeki Giden Kuralları (Outbound Rules) tanımlamalarına  göre SNAT yaparak Load Balancer’in Public IP yada IP’lerini kullanarak internete bağlanır. Load Balancer üzerinde SNAT port tahsisi manuel yada varsayılan olarak ayarlanabilir.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-loadbalancer-port-allocatin.png?raw=true" width="65%" height="65%" />

Internet bağlantısı için standard load balancer kullanıldığında SNAT port tahsisi için manuel  yöntemin kullanılması önerilir. Varsayılan yöntemde SNAT tahsisi arka uç havuzundaki (Bakend)makina sayısını temel alınacağından SNAT Port Exhaustion durumunun olması kuvvetli bir ihtimaldir. Varsayılan yöntemde tek bir Frontend IP’ye sahip Load Balancer için makina sayısına göre varsayılan SNAT port adedi şu şekildedir:

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-load-balancer-snat-limits.png?raw=true" width="95%" height="95%" />

10 makina için varsayılanda 1024 SNAT portu kullanılabilirken, manuel yapılandırma ile 6400 SNAT portuna sahip olabilir, daha fazla SNAT kullanılabilir SNAT portu için Load Balancer üzerine ikinci bir Public IP ekleyebilirsiniz.

**2.Azure NAT Gateway**

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-nat-gateway-internet.png?raw=true" width="95%" height="95%" />

Azure sanal makinalar aynı subnet’te bulunan NAT Gateway’in Public IP yada IP Prefix’lerini kullanarak internete bağlanır. NAT gateway birden fazla Subnet ile ilişkilendirilebilir.
Azure NAT gateway, Azure Load Balancer’dan farklı olarak sadece giden internet bağlantısı için kullanılır.  NAT gateway bir subnet ile ilişkilendirildiği zaman ekstra bir yapılandırmaya gerek yoktur, System Route’ları internet yönündeki tüm trafiğin NAT Gateway’e yönlendirilecek şekilde otomatik olarak yapılandırılır.

Hangi durumdalarda Nat Gateway tercih edilmeli:
1.	Internet bağlantısı için Static ve öngörülebilir Public IP’lere ihtiyacınız varsa. 
2.	İnternete trafik gönderen dinamik veya büyük iş yüklerine sahipseniz
3.	Azure Firewall veya Load Balancer kullanıldığında SNAT port Exhaustion durumu varsa. 

NAT Gateway her bir Public IP için 64512 SNAT bağlantı noktası destekler ve 16 Public IP adresi ile ilişkilendirilebilir. 16 Public IP adresi NAT gateway üzerinde kullanılabilir 1 milyon SNAT bağlantı noktası demektir.  NAT gateway daha az sayıda Public ile daha fazla SNAT bağlantısı sunmanın yanında ayrıca On-demand SNAT port allocation özelliği sayesinde **SNAT port exhaustion** probleminin önüne geçer.

**3.Instance Level Public IP**

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-instance-level-ippng?raw=true" width="95%" height="95%" />

Sanal makinalar internet bağlantısı için Network Interface’lerine atanan Public IP’leri kullanılır. 
Public IP ataması ile direk internet bağlantısına sahip olacağınız gibi aynı zamanda diğer kaynakların bu public ip üzerinden sizin makinanıza erişebilir olması demektir. Buda ciddi güvenlik riskleri demektir. Birden fazla sanal makinaya public IP atamak ve NSG kurallarını yönetmek bu hata riskini arttıracaktır.
