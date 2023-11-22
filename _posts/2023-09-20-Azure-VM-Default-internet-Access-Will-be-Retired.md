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
