---
title: "Azure Backup MARS Ajanı Parolasını Azure Key Vault’ta Güvenli Bir Şekilde Saklayın."
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Key Vault
  - Azure Backup
  - Preview
---
📢 **Public Preview**

Azure Backup, Microsoft Azure Recovery Services (MARS) ajanini kullanarak dosya/klasör ve system state verilerinizi Azure Recovery Service Vault üzerine yedeklemenize olanak tanır. Bu veriler MARS ajanının kurulumu sırasında sağladığınız (mimimum 16 karakter) bir parola **(passphrase)** kullanılarak şifrelenir. 

Bu parola, yedekleme verilerini almak ve geri yüklemek için gereklidir bu sebebten harici güvenli bir noktada saklanması gerekir. Bu paralonun yedekleme için kullanılan makina üzerinde saklanması bir güvenlik ihlali durumunda saldırganın yedeklerinize zarar verebilmesi anlamınada gelir. 

**Bu Parolayı Kaybederseniz ?**

Orijinal makina (yedeklemelerin alındığı makina) kullanılabilir durumdaysa ve hala aynı Recovery Service Vault‘a  kayıtlıysa bu parolayı yeniden oluşturabilirsiniz. Bu durumda aklınıza söyle bir soru gelebilir  “Peki eski parola kullanarak alınmış  yedeklere ne olacak ?”

Microsoft aslında verilenizi bu parola ile şifrelemez. Bunun yerine veriler Azure Recovery Service Vault üzerine gönderilmeden önce, korunan makinaya ait  GUID anahtarına dayalı bir **Data Encryption Key (DEK)** anahtarını kullanılarak şifreler.
Sonrasinda bu DEK anahtarı oluşturmuş olduğunuz parola ile tekrar şifrelenir ve bu işleme **Key Envelop Key (KEK)** adı verilir. KEK, daha önce bahsetmiş olduğumuz DEK anahtarını şifrelemek için kullanılır sonrasında şifrelenmiş DEK anahtarı, Recovery Service Vault üzerinde tutulur.

<img src="https://github.com/martin3mre/martin3mre/blob/main/assets/images/Mars-Agent-Reset-Passphase.png?raw=true" width="55%" height="55%" />


Bu parolayı değiştirmeniz durumunda; eski parola kullanılarak şifrelenmiş DEK anahtarı on-premises’e gönderilir ve eski parola kullanılarak şifrelemesi kaldırılır. Sonrasında yeni parola ile tekrardan şifrelenen DEK anahtarı tekrardan Recovery Service Vault’a gönderilir. Bu sayede her seferinde şifre çözme ve yeniden şifreleme için büyük miktarda veriyi geri göndermeye gerek kalmaz.

**Özetlemek gerekirse…**
<table>
    <thead>
        <tr>
            <th style="width: 200px; text-align:left;">Orijinal Makine</th>
            <th style="width: 200px;text-align:left;">Parola/Passphrase</th>
            <th style="width: 400px;text-align:left;">Yedeklenmiş Veriler</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Mevcut</td>
            <td>Kullanım Dışı</td>
            <td>Orijinal makineniz  mevcutsa ve hala aynı Recovery Service Vault’a kayıtlıysa parolayı yeniden oluşturabilirsiniz ve yedeklenmiş verilere ulaşılabilirsiniz.</td>
        </tr>
        <tr>
            <td>Kullanım Dışı</td>
            <td>Mevcut</td>
            <td>MARS ajanını mevcut parolayı kullanarak başka makina üzerine yükleyebilir ve yedeklenmiş verilere ulaşılabilirsiniz.</td>
        </tr>
        <tr>
            <td>Kullanım Dışı</td>
            <td>Kullanım Dışı</td>
            <td>Yedeklenmiş verileri kurtarmak mümkün değildir.</td>
        </tr>
    </tbody>
</table>


**Konumuza dönelim…**

Tüm bu ayrıntılardan anlaşılacağı üzere bu parola (passphrase) bizler için hayati önem taşımaktadır ve bu sebeten güvenli bir harici konumda saklanmalıdır ! Geçtiğimiz aylarda duyurulan yeni bir özellikle sayesinde artık bu şifreleme anahtarını Azure Key Vault üzerinde saklayabilirsiniz.

<img src="https://github.com/martin3mre/martin3mre/blob/main/assets/images/Mars-Agent-Passphase-Key-Vault.png?raw=true" width="65%" height="65%" />


>**Not** Şuan için ön izlemede olan bu özellik yalnızca **MARS 2.0.9254.0** veya üzeri bir sürümle Azure genel bölgelerinde desteklenmektedir. 


