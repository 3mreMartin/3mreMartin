---
title: "Dev/Test Ortamlarınız İçin Düşük Maliyetli Azure Bastion Developer SKU"
classes: wide
excerpt: 
author_profile: true
excerpt: 
categories:
  - Azure
tags:
  - Azure Bastion
  - Public Preview
---
📢 **Public Preview**

Dün itibarı ile **Azure Bastion Developer SKU** genel önizleme olarak kullanıma sunuldu. 

Düşük maliyetli Developer SKU, diğer Bastion SKU'larının sahip olduğu ek özelliklere ve ölçeklendirme yeteneklerine sahip olmamakla birlikte aynı anda doğrudan sadece bir Azure VM'e güvenli RDP/SSH bağlantısına izin veriyor. 

Geliştirme ve Test kullanıcıları için uygun olan bu yeni SKU'nun dağıtımıda diğer SKU'lardan biraz farklı. 
Basic ve Standard SKU'da oluguğu, Developer SKU size ait bir Bastion Host üzerinde çalışmadığından kendine ait bir **AzureBastionSubnet** ihtiyacıda bulunmamakta. 
Yani Developer SKU kaynakları paylaşılan ortak bir bastion hostu üzerinde çalışmakta olup kaynakları diğer müşteriler ile paylaşılmaktadır. 

Developer SKU ile kullanılabilir özelliklere aşağıdaki tablodan göz atabilirsiniz:

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-bastion-SKU.png?raw=true" width="95%" height="95%" />

Developer SKU şuan için sadece aşağıdaki Azure bölgelerinde kullanılabilir olmakla birlikte VNet peering desteklenmemektedir.

* Central US EUAP
* East US 2 EUAP
* West Central US
* North Central US
* West US
* North Europe

Daha fazlası için: [https://learn.microsoft.com/en-ie/azure/bastion/quickstart-developer-sku](https://learn.microsoft.com/en-ie/azure/bastion/quickstart-developer-sku)
