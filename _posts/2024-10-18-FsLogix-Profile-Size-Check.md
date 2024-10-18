---
title: "Fslogix Profil Boyutlarinin Takip Edilmesi"
classes: wide
author_profile: true
categories:
  - Azure
tags:
  - Azure Virtual Desktop
  - Public Preview
---
Azure Virtual Desktop ortamlarinda kullanici profil yönetimi için kullandığımız Fslogix kullanıcı verilerini konteyner (Profil yada ODFC) 
adını verdiğimiz VHD/VHDX formatındaki sanal diskler içerisinde saklanmasına olanak sağlar. Bu konteynerlar için varsayılan boyut 30 GB olmakla birlikte gerekli olması durumunda **SizeInMBs** anahtarını değiştirerek yükseltilebilir. Konteyner boyutlarının takip altında tutulması son kullanıcılara kötü bir deneyim yaşatmamak adına önemlidir neticede sanal disk kullanimi %100 ulaşmış kullanıcılar sağlıklı bir şekilde çalışmaya devam edemezler. Konteyner boyutlarını takip etmek için AVD ortamlarında **WVDCheckpoints** günlüklerini aktif ettikten sonra aşağıdaki Log Analytics sorgusunu kullanarak VHD/VHDX boyutlarını takip edebilir ve gerekli uyarıları oluşturabilirsiniz.


WVDCheckpoints
| where (Name=="ProfileLoggedOff" or Name=="ODFCLoggedOff")
and (Source=="RDAgent" or Source=="FSLogix")
and TimeGenerated>ago(360d)
| extend ProfileType=iff(Name=="ProfileLoggedOff","Profile","ODFC")
| summarize arg_max(TimeGenerated, *) by UserName, ProfileType
| extend ["VHD Size On Disk"]=todouble(replace_string(replace_string(tostring(Parameters.VHDSizeOnDisk),",",""),".","")),
["VHD Free Space"]=todouble(replace_string(tostring(Parameters.VHDFreeSpace),",",".")),
["VHD Max Size"]=todouble(replace_string(tostring(Parameters.MaxVHDSize),",","."))
| where ["VHD Size On Disk"]!=""
| project UserName,TimeGenerated,ProfileType,
["VHD Size On Disk"],
["VHD Free Space"],
["VHD Max Size"],
Usage=100*(["VHD Max Size"]-["VHD Free Space"])/["VHD Max Size"]
| order by ["Usage"] desc
| where Usage > 80



 

