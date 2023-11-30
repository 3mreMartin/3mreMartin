---
title: "Azure Site Recovery Data Change Rate is beyond Supported Limits Hatası"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Site Recovery
---
Varsayılan olarak Azure Site Recovery, ardışık replikasyon döngüleri arasında 54 MB/s’e kadar veri değişikliğini (Churn) destekler.  Bu da Azure Site Recovery ile korunan bir makinanın, saniyede 54 MB ‘lik bir değişikliğe sahip olabileceği anlamına gelir. Bu değer Azure Site Recovery ile korunan Azure VM ‘ler için **High Churn Support** desteği ile VM başına 100 MB/s ‘e kadar çıkartılabilir. Bu derece yüksek bir veri değişimi genelde veri tabanı gibi yoğun işlemlerin sürdürüldüğü VM’lerde ortaya çıkar. 

Bu limit Azure Site Recovery ile korunan on-premises makinalar için Standard Disk ile 2MB/s, Premium Disk ile 20 MB/s ’e kadar çıkabilir  Ayrıca bir Process Server,  Cache Disk boyutuna bağlı olarak **günlük** maximum 2 TB veri değişliğini Azure üzerine kopyalayabilir.

Bazı durumlarda Azure Site Recovery ile kopyalanan on-premises makinanın disklerindeki veri değişim hızı (yazma bayt/sn), hedef disk türü için desteklenen sınırıdan daha fazla olabilir. Bu durumda Azure Site Recovery üzerinde **Data change rate beyond supported limits** şeklinde bir  hata ile karşılaşabilirsiniz.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/Data-change-rate-beyond-supported-limits.png?raw=true" width="100%" height="100%" />

**Ne yapmamız lazım ?**

Yüksek veri değişimin olduğu belirli zamanlarda bu hata ile karşılaşmanız normaldir ancak bu hata sürekli olarak karşınıza çıkıyorsa çözüm olarak; Azure üzerine kopyalanan on-premises makinanın hedef disk türünü değiştirmelisiniz. Bu seçenek  veri değişim oranın 20MB/s’den az olması durumunda işe yarayacaktır. 

1.	**Azure Portal -> Disks ->** Hatayı aldığınız diski seçin.
2.	SAS URL'sinin oluşturulduğunu belirten uyarıyı seçerek iptal edin. (SAS URL sonrasında ASR tarafından otomatik olarak tekrar oluşturulacak)
3.	**Size + Performance** altından disk boyutu değiştirin. [Buradan](https://learn.microsoft.com/en-us/azure/site-recovery/azure-to-azure-troubleshoot-replication#azure-site-recovery-limits) disk boyutuna göre desteklenen değişim oranlarını görebilirsiniz.
