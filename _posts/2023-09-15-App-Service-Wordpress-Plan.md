---
title: "Azure: App Service üzerinde Ücretsiz WordPress Barındırma Planı"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure App Service
  - Public Preview
---
  📢 **Public Preview**
  
 Microsoft, 2022 Ağustos'unda Azure App Service üzerinde WordPress kullanımını farklı barındırma planı seçenekleriyle (Basic, Standard ve Premium) genel kullanıma sunduğunu duyurmuştu, tabii ki ücreti karşılığında. Bu ücretlendirmeden dolayı uygulamalarını App Service üzerinde taşımak isteyen kullanıcıların bu hizmeti öncesinde deneme ve test etme şansı bulunmamaktaydı. Microsoft bu durumu fark etmiş olmalı ki, geçtiğimiz günlerde WordPress için ücretsiz bir barındırma planını kullanıma sundu.

  <img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/wordpress-hosting-plans.png?raw=true" width="100%" height="100%" />

 Bu yeni barındırma planı, ücretsiz App Service F1+ deneme sürümü ile Azure Database for MySQL'den oluşmakta. Ücretsiz olan F1 barındırma planı herhangi bir SLA'ye sahip olmadığından ve günlük CPU kotası uygulandığından sadece geliştirme ve test amacıyla kullanılması önerilmekte. App Service F1 SKU'nun scale-out desteği bulunmuyor, ancak gerekli testleri tamamladıktan sonra App Service SKU'sunu daha yüksek bir SKU ile değiştirebilirsiniz.

 **Ücretsiz dedik ama…**

 WordPress için bu App Service planı tüm abonelik türlerinde tamamen ücretsiz kullanabiliyor olsanızda Mysql database için abonelik türüne ve kullanım süresine göre ücret ödemeniz gerekekte. Detayları aşağıdaki tablodan inceleyebilirsiniz:

 <table>
    <thead>
        <tr>
            <th style="width: 200px; text-align:left;">Subscription</th>
            <th style="width: 200px;text-align:left;">App Service F1</th>
            <th style="width: 400px;text-align:left;">Azure Database for MySQL B1ms</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Free Account</td>
            <td>Ücretsiz</td>
            <td>12 ay boyunca ayda 750 saat</td>
        </tr>
        <tr>
            <td>Student Account</td>
             <td>Ücretsiz</td>
            <td>12 ay boyunca ayda 750 saat</td>
        </tr>
        <tr>
            <td>Pay as you go with Free Offer</td>
            <td>Ücretsiz</td>
            <td>12 ay boyunca ayda 750 saat</td>
        </tr>
        <tr>
            <td>Pay as you go</td>
            <td>Ücretsiz</td>
            <td>Ücretli</td>
        </tr>
    </tbody>
</table>

 **Aklınızda Bulunsun.…**

* App Service F1 Temel veya Standart planlara kıyasla sınırlı yeteneklere sahiptir.
* MySQL için Azure Veritabanı, bu ücretsiz barındırma planı için 750 saatlik ani hızlandırılabilir B1ms örneğine sahiptirç
* CDN, Front Door, Blob Storage ve E-posta Hizmeti gibi Ücretli hizmetlerle entegrasyon, ücretsiz barındırma planına dahil değildir.
* Local Storage Caching 500 MB ile sınırlıdır.
