---
title: "AVD - FSLogix ve Kullanıcı ve Grup Nesnelerine Özel Yapılandırmalar"
classes: wide
author_profile: true
categories:
  - Azure Virtual Desktop
tags:
  - FSlogix
  - Fslogix Object Spesific Settings
---
 
FSLogix Azure Virtual Desktop ortamlarında kullanici profil yönetimi için kullandığımız bir çözümdür.  Fslogix kullanıcı profil yönetimi için Profil konteyneri ve ODFC konteyneri olmak üzere iki farklı konteyner seçeneği sunar. Profil konteynerı, kullanıcı profili ile ilgili verilerin bir VHD(x)'de depolanmasına olanak sağlarken, ODFC konteynerı Microsoft Office uygulamalarına özgü profil içeriğini depolamak için kullanılır.

Her iki konteyner seçeneğindede yapılandırma ayarları Windows kayıt defteri içerisinde makina seviyesinde uygulanır: 

* Profil konteyneri: HKEY_LOCAL_MACHINE\ Software \FSLogix\Profiles
* ODFC konteyneri: HKEY_LOCAL_MACHINE\Software\Policies\FSLogix\ODFC

 **Fslogix Kullanıcı ve Grup Nesnelerine Özel Konteyner Ayarları**

 Nesneye özgü (object specific) ayarların kullanılması, belirli bir kullanıcı veya grup için özel  konteyner ayarlarına sahip olmanızı sağlar.  Özel konteyner ayarlarını kullanarak  belirli kullanıcı profilleri için; farklı depolama lokasyonları kullanabilir, Cloud Cache’i devreye alabilir ve ya yapılacak bir değişiklik öncesinde belirli testler yapabilirsiniz. Nesneye özgü ayarların önceliklendirme sırası şu şekildedir:

1.	Object specific (user)
2.	Object specific (group)
3.	System-wide settings (Varsayılan Ayarlar)

Yani sistem genelindeki ayarlarda VHDLocation, Nesne özelinde (kullanıcı yada group) CCDlocation yapılandırması mevcut ise, CCDlocation yapılandırması baskın olacaktır.


<img src="https://github.com/martinemre/martinemre.github.io/blob/main/assets/images/avd-fslogix-object-specific-settings.png?raw=true" width="100%" height="100%" />

AD ortamında bir kullanıcı yada grubun SID ‘ini bulmak için şui powershell komutunu kullanabilirsiniz: *get-aduser -Identity username | select name, SID*

*Daha fazlasi için:* 
[https://learn.microsoft.com/en-us/fslogix/how-to-configure-object-specific-settings](https://learn.microsoft.com/en-us/fslogix/how-to-configure-object-specific-settings)










  
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
