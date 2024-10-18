---
title: "Public IPsiz - Özel Azure Bastion Dağıtımı"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Bastion
  - Public Preview
---
  📢 **Preview**


Azure Bastion, Azure Portalı aracılığıyla Azure Sanal makinelere güvenli RDP/SSH bağlantısı yapabilmemize olanak sağlayan bir PaaS servisidir.
Azure Bastion gelen RDP/SSH bağlantı taleplerini sanal ağınızda bulunan sanal makinalara yönlendirir ancak bunu yapabilmesi için Bastion’ın Azure Porta aracılı ile dış dünyadan erişebiliyor olması gerekir ve bu sebebten bir Public IP adresine sahiptir.
 
<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-bastion-0.png?raw=true" width="100%" height="100%" />


Azure Bastion, Portal aracılı ile sanal makinalar arasında bir köprü görevi görsede sahip olduğu Public IP adresi sebebi ile genelde güvenlik ekipleri tarafından pek sevilmez :)

## Özel Bastion Dağıtımı (private-only)

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-bastion-1.png?raw=true" width="100%" height="100%" />

Premium SKU Bastion ile gelen  yeni **private-only**  dağıtım modeli ile Bastion Host’ın Public IP adres gereksinimi ortadan kaldırılmış durumda.  Şuan için önizleme olarak kullanıma sunulan bu modelde Azure Bastion ve Portal arasındaki iletişim tamamen kapalı devre olarak Private IP adresi üzerinden gerçekleşmekte. Kapalı devre olarak yapılandırılan bu Bastion dağıtımını kullanabilmek için kullanıcıların tabiki AzureBastion Subnet’i ile networksel bağlantısının (ExpressRoute, VPN, Peerin) olması gerekmektedir.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/Azure-Bastion-Private.png?raw=true" width="100%" height="100%" />