---
title: "Azure Monitor Maliyetlerinizi Günlükleri Arşivleyerek Azaltin"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Monitor
  - Log Analytics
---

Azure Monitor, Günlükleri (Logs) Etkileşimli ve Arşiv olmak üzere iki farklı şekilde saklar

**Etkileşimli Saklama (Interactive retention)**
Varsayılan olarak 30 gün olarak saklanan günlükler ek maliyet ile 2 yıla kadar saklanabilir. Gerekli olması durumunda varsayılan bu süre (API yada CLI kullanarak) 4 güne kadar düşürülebilir ancak fiyatlandırmaya 30 gün dahil olduğundan maliyetlerde bir etkisi bulunmaz.

**Arşiv (Archive)**
Daha eski, daha az kullanılan günlüklerin workspace üzerinde daha düşük bir maliyetle tutmanızı sağlar. Arşivlenen günlüklere **search jobs** ve **restore** kullanılarak erişilebilir. Bu günlükler  12 yıla kadar arşivlenmiş durumda tutulabilir.

**Saklama ve Arşivleme Nasıl Çalışır?**

 <img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/azure-log-archive.png?raw=true" width="100%" height="100%" />

Her bir Log Analytics workspace üzerinde tüm tablolara uygulanan varsayılan bir saklama süresi vardır. Bu süre varsayılan olarak 30 gündür ve 2 yila kadar uzatılabilir. 

Etkileşimli saklama süresince günlükler izleme, sorun giderme ve analiz için kullanılabilir durumdadır. Teknik olarak bu günlüklere daha fazla ihtiyaç duymadığınızda ancak uyumluluk  gereği bu günlüklerin saklanması gerektiğinde maliyetlerden tasarruf etmek için günlükler arşivlenebilir. 

Varsayılan olarak, çalışma alanınızdaki tüm tablolar, çalışma alanının etkileşimli saklama (Data Retention) ayarını devralır ve arşivleri yoktur. Arşivleme her bir tablo için ayrı olarak ayarlanabilir.  Arşivlenen günlükler için toplam saklama süresi olarak 4383 güne (12 yıl) kadar çıkartılabilir. Arşivlenen günlüklere Seach-Job kullanarak yada Restore(Geri yükleme) ile erişilebilir.

## Search Jobs & Restore

**Search Jobs**

Belirli bir zaman aralığındaki  Arşivlenmiş Günlükleri geri yüklemek için Arama işleri (Search Jobs) kullanılır. Arama işleri sorgu sonuçlarını aynı workspace üzerindeki yeni bir tabloya gönderir. Sorgu sonuçları bir arama işi başlar başlamaz kullanılabilir ancak sonuçların listelenmesi gecikmeli olabilir.

**Limitasyonlar:**

* Arama tarih aralığı bir yıla kadardır.
* 24 saatlik zaman aşımına kadar uzun süreli aramalar desteklenir.
* Sonuçlar, kayıt kümesindeki bir milyon kayıtla sınırlıdır.
* Eşzamanlı yürütme, çalışma alanı başına beş arama işiyle sınırlıdır.
* Çalışma alanı başına 100 arama sonucu tablosuyla sınırlıdır.
* Çalışma alanı başına günde 100 arama işi yürütmesiyle sınırlıdır.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/auzre-log-seach-job.png?raw=true" width="100%" height="100%" />

Arama işleri, belirli bir tablodaki büyük hacimli verileri taramayı amaçlamaktadır. Bu nedenle, arama işi sorguları her zaman bir tablo adıyla başlamalıdır. Arama işleri kullanıldığında KQL sorgu dilinde sadece belirli komutlar desteklenmektedir:

* where
* extend
* project
* project-away
* project-keep
* project-rename
* project-reorder
* parse
* parse-where 

**Restore**

Geri yükleme (Restore), Arama işlerinde  olduğu gibi arşivlenen günlüklere ulaşmak için kullanılır. Arama işleri belirli kriterlere göre verilere erişmek kullanılırken, Geri yükleme belirli bir zaman aralığındaki bir veri kümesinde sorgu çalıştırmak için kullanılır. Geri yüklenen günlükler orijinal zaman damgalarını korur bu sebeben geri yüklenen günlükler için bir sorgu çalıştırırken zaman aralığının  belirtilmesi gerekmektedir.   

**Limitasyonlar:**

* Verileri en az iki gün için geri yüklenebilir.
* 60 TB'a kadar geri yükleme desteklenir.
* Bir çalışma alanında aynı anda en fazla iki geri yükleme çalıştırılabilir.
* Belirli bir zamanda belirli bir tabloda yalnızca bir etkin geri yükleme çalıştırılabilir.
* Zaten etkin bir geri yüklemeye sahip bir tabloda ikinci bir geri yüklemenin yürütülmesi başarısız olur.
* Haftada tablo başına en fazla dört geri yükleme gerçekleştirilebilir.

## Ücretlendirme Modeli


**Search Jobs**

Arama işlerinde ücretlendirme tararnan veri miktarı ve sonuçlara göre uygulanır.

Arama işi yürütme: Arama işinin taradığı veri miktarı.
Arama işi sonuçları: Normal günlük verileri alma fiyatlarına göre, arama işinin bulduğu ve sonuçlar tablosuna alınan veri miktarı.

Örneğin, tablonuzda günde 500 GB yer varsa, 30 günü aşan bir arama için sizden 15.000 GB için taranan veri ücreti alınır. Arama işi, arama sorgusuyla eşleşen 1.000 kayıt bulursa, bu 1.000 kaydı sonuçlar tablosuna almanız için ücretlendirilirsiniz.

**Restore**

Geri yüklenen günlüklerin ücreti, geri yüklediğiniz verilerin hacmine ve her geri yüklemeyi sakladığınız süreye bağlıdır. Ücretler, geri yükleme başına minimum 2 TB geri yüklenen veri hacmine tabidir. Daha az veriyi geri yüklerseniz minimum 2 TB tutarında ücretlendirilirsiniz.

Ücretler, geri yükleme süresine göre eşit olarak dağıtılır. Geri yükleme işlemi daha erken sonlandırılsa bile minimum ücret 12 saatlik geri yükleme süresi için olacaktır.

Örneğin, tablonuzda günde 500 GB veri varsa ve 10 günlük veriyi geri yüklerseniz, geri yüklenen verileri kapatana kadar günde 5000 GB ücretlendirilirsiniz.

## Arşivlenen Günlüklerin Temizlenmesi

Her iki yöntemdede çalışma alanındaki veri saklamaya ilişkin ekstra ücretlerden kaçınmak için geri yüklenen yada arama işleri ile getirilen tabloları silmeniz önerilir.

<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/auzre-log-delete-archive-logs.png?raw=true" width="100%" height="100%" />


## Benzersiz Saklama Dönemlerine Sahip Tablolar

Varsayılan olarak, Usage ve AzureActivity günlükleri ücretsiz olarak 90 gün boyunca saklanır. Eğer saklama süresini Workspace seviyesinde (Data Retention) 90 günün üzerine cıkardığınızda bu tabloların saklama süresinide ücretsiz olarak arttırmış olursunuz. cıkardığınızda bu tabloların saklama süresinide ücretsiz olarak arttırmış olursunuz. Application Insights kaynaklarıyla ilgili tabloların verileride 90 gün boyunca ücretsiz olarak saklanır. Dilerseniz bu toblolarında her biri için ayrı ayrı saklama süresi ayarlayabilirsiniz.

* AppAvailabilityResults
* AppBrowserTimings
* AppDependencies
* AppExceptions
* AppEvents
* AppMetrics
* AppPageViews
* AppPerformanceCounters
* AppRequests
* AppSystemEvents
* AppTraces

## Arşivlenen Günlükler için Ücretlendirme

Arşivlenmiş günlüklerin bakım ücreti, GB cinsinden arşivlediğiniz veri hacmine ve verileri arşivlediğiniz gün sayısına veya gün sayısına göre hesaplanır.
Etkileşimli saklama (Interactive retention) ile günlükleri 31 gün ücretsiz olarak saklayabilirsiniz (Microsoft Sentinel ile 90 gün) sonrasında GB başına $0.10 olarak ücretlendirilir. Bu fiyatlandırma Arşiv (Archive)’lenen günlükler için GB başına $0.02 ‘dır. 

