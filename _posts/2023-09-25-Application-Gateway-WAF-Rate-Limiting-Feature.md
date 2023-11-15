---
title: "Application Gateway WAF & Rate Limiting Özelliği ile Tanışın"
classes: wide
excerpt: 
author_profile: true
categories:
  - Azure
tags:
  - Azure Application Gateway
  - Azure Web Application Firewall
  - Preview
---
📢 **Public Preview**

Bu blog yazısında Azure Application Gateway - Web Application Firewall için geliştirilen ve şuan için önizlemede olan yeni bir özellikten bahsetmet istiyorum. Bu yeni özelliğin adı **Rate limiting**. Bu özellik  belirli bir süre içinde belirli koşullarla eşleşen (*IP adresleri, coğrafyalar veya kullanıcı oturumları gibi*) istekleri sınırlamak için WAF üzerinde özel kurallar tanımlamanızı mümkün kılmakta. Bu özellik ile web uygulamanıza 1 dakika içerisinde belirli bir IP ‘den gelebilecek istek sayısını ya da 1 gün boyunca sadece belirli bir ülkeden gelebilecek istek sayısını limitleyerek uygulamanıza yönelik anormal düzeydeki trafiği tespit edebilir ve DDOS saldırısı gibi tehditlere karşı koruma sağlayabilirsiniz.

<img src="https://github.com/martin3mre/tr/blob/main/assets/images/Rate-Limiting-Feature.png?raw=true" width="95%" height="95%" />

**Bu özelliği kullanırken bilmeniz gerekenler:**

Rate limit kuralı içerisindeki eşik değeri (thresholds) WAF içerisindeki her bir uygulama için bağımsız olarak izlenir ve sayılır. Örneğin, Üç farklı Listenner ile ilişkilendirişmiş tek bir WAF politikanız varsa her Listenner'ın kendi sayaç ve eşik değerleri olacaktır. Ayrıca bu eşik değeri uygulamaya gelen trafiğinin ayrıntılı kontrolü için değil sadece anormal trafik oranlarını azaltmak ve uygulamanın kullanılabilirliğini sürdürmek için kullanmalıdır.

Rate limit kurulı içerisindeki eşik değeri aşıldığında uygulamanız için tüm trafik engelleneceğinden  *Geo Location* yada *None* gibi geniş kapsamlı gruplama kuralları kullanılırken dikkatli olunmalıdır.
