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

AD ortamında bir kullanıcı yada grubun SID ‘ini bulmak için şu powershell komutunu kullanabilirsiniz: *get-aduser -Identity username | select name, SID*


*Daha fazlasi için:* 
[https://learn.microsoft.com/en-us/fslogix/how-to-configure-object-specific-settings](https://learn.microsoft.com/en-us/fslogix/how-to-configure-object-specific-settings)

