---
title: "A’ dan Z’ye Azure VM: Bölüm 4 – Azure VM Spot Instance"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Compute
  - Azure VM Spot Instance
---


Spot Sanal Makinalar Microsoft’un bölgelerininin her birinde bulunan ve o an için kullanılmayan yedek kapasitesini bizlerin erişimine/kullanımına sunmasıdır. Bu yedek kapasitesi bölgeye, sanal makinanın boyutuna, gün ve saat dilimine göre hatta talep sayısına bağlı olarak değişiyor.Microsoft neden böyle birşey yapıp bu kadar ucuz maliyetli makinalar sunuyor ? En başındada söylediğimiz gibi “Kullanılmayan yedek kapasite’yi” bizlerin kullanımına sunuyor ve bu da şu demek oluyor “Bu kaynaklara ihtiyaç duydugum anda geri alabilirim” 😊 Bu geri alma işlemini Microsoft “Tahliye” (Eviction) olarak isimlendiriyor ve VM'leriniz iki senaryodan birinde tahliye edebiliyor:


•	**Capacity Only:** Azure kaynaklarına ihtiyaç duyulduğu zaman tahliye edilirsiniz.
•	**Price or Capacity:** Azure kaynaklarına ihtiyaç duyulduğu zaman yada makina fiyatının (Saatlik) belirtmiş olduğunuz tavan fiyatın üzerine çıkması durumunda tahliye edilirsiniz. Bu seçenekte kabul edebileceğiniz en yüksek saatlik birim fiyatı yazmanız gerekmektedir. Spot sanal makina fiyatlarının talep ve kaynak sayısına göre değiştiğini unutmaylım.

Sport Sanal Makina fiyatlarına göz attığınızda Pay-As-You-Go fiyatlarına göre %70-90 lık bir avantaj bulunmaktadır.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-vm-spot-instance-2.png?raw=true" width="85%" 
height="85%"/>

**Spot Sanal Makinalar**

•	Bir SLA’e sahip değildir.
•	Microsoft daha fazla kapasiteye ihtiyaç duyduğu takdirde (30 sn içerisinde) kaynaklarını geri alabilir.
•	Kullanılabilir Spot makina kapasitesi; makina boyutu, bölge, gün ve saat değerlerine göre değişir. 

**Limitasyonlar**

•	B-serisi ve Promo size sanal makina (Dv2, NV, NC, H v.b) ‘lar için kullanılamaz.
•	Spot Sanal Makinalar ephemeral OS disks kullanamazlar.
•	Spot Sanal Makinalar Microsoft Azure China 21Vianet hariç tüm Azure bölgelerinde kullanılabilir.

*Daha fazlasi için:* 
[https://learn.microsoft.com/en-us/azure/virtual-machines/spot-vms](https://learn.microsoft.com/en-us/azure/virtual-machines/spot-vms)
