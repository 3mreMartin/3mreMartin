---
title: "Azure Özel Alt Ağlar & Private Subnet"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Network
  - Preview
---
📢 **Public Preview**

Azure sanal makinalar doğrudan internet bağlantısı için; NAT Gateway, Load Balancer yada sahip oldukları Public IP’leri kullanarak internete bağlanabilirler. Bu yöntemlerden herhangi biri kullanılmadığı zaman sanal makinalar Default Outbound Access olarak adlandırılan yöntemle yani IP havuzundan atanan rastgele bir Public IP adresini kullanarak internete bağlanır. Microsoft 30 Eylül 2025 itibarı ile  bu yöntemin kullanımdan kaldıracağını duyurmuştu. 

Bu konu ile alakalı daha fazla ayrıntı için  Azure VM`ler için Varsayılan Internet Erişimi Emekli Oluyor adlı yazımı okumanızı tavsiye ederim.

**Ne yapmamız lazım ?**

Default Outbound Access kullanarak internete bağlanan mevcut sanal makinalarınız için 30 Eylül 2025 tarihine kadar gereksinimlerinize uygun olan ve önerilen yöntemlerden birini tercih etmeniz gerekiyor.  (Nat Gateway, Load Balancer, Public IP, Azure Firewall). **Yeni oluşturacağınız Subnet** ‘ler için ise; şuan için önizlemede olan **Private Subnet** özelliğini kullanarak Default Outbound Access‘i devre dışı bırakabilirsiniz.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/Azure-Private-Subnet.png?raw=true" width="95%" height="95%" />

Bu özellik sadece yeni bir Virtual Network ile birlikte Subnet oluştururken kullanılabilmekte. Private Subnet özelliği aktif edilmiş bir Subnet’teki sanal makinalar internet bağlantısı için önerilen yöntemlerden birini kullanarak internete bağlanmaya devam edebilirler.
