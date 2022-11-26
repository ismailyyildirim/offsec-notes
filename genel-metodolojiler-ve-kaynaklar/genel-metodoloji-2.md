# Genel Metodoloji -2-



## 1 - Ağ içindeki ana bilgisayarları keşfetme / Şirket Varlıklarını Keşfetme

## 2 - Network Taraması

Bir ana bilgisayara saldırmadan önce, ağdan bazı kimlik bilgilerini çalmayı veya pasif/aktif olarak (MITM) ağ içinde neler bulabileceğinizi öğrenmek için bazı verileri koklamayı sniff etmeyi tercih edebilirsiniz. Veya [Pentesting Network](broken-reference)' ü okuyabilirsiniz.

## 3 - Port Tarama - Servis Keşfi

Bir ana bilgisayardaki güvenlik açıklarını ararken yapılacak ilk şey, hangi servislerin hangi bağlantı noktalarında çalıştığını belirlemektir.&#x20;

## 4 - Version ve Sürüm açıklarını arama

Hangi servislerin çalıştığını ve belki de sürümlerini öğrendikten sonra, bilinen güvenlik açıklarını aramanız gerekir.&#x20;

## 5 - Pentesting Services

Çalışan herhangi bir servis için zero-day veya bir istismar yoksa, çalışan her hizmette yaygın yapılan yanlış yapılandırmaları aramalısınız.

Bu kitap veya not defterimin içerisindeki en yaygın servis ve hizmetleri (ve çok yaygın olmayan da dahil) test etmek için bir kılavuz bulacaksınız. Lütfen sol dizinde PENTESTING bölümünü arayın (Servisler varsayılan portlarına göre sıralanmıştır).

Pentesting Web bölümünden (en kapsamlı olduğu için) özel olarak başka bölümde bahsetmek istedim.

Ayrıca, exploit ve istismar aramak bilinen güvenlik açıklarının nasıl bulunacağına dair küçük bir kılavuz da [burada ](broken-reference)bulunabilir.

### 5.1 - Automatic Tools

### 5.2 - Brute - Force

Bazı seneryolarda bir Brute-Force, bir servisten ödün vermek için faydalı olabilir. Buradaki farklı servislerin kaba kuvvet saldırısına ait [CheatSheet'ini ](broken-reference)bulabilirsiniz.

## 6 - Phishing

Bu noktada bir güvenlik açığı bulamadıysanız, ağın içine girmek için biraz kimlik avına çıkmalısınız. Phishing metodolojimi [buradan ](broken-reference)okuyabilirsiniz.

## 7 - Getting Shell

Bir şekile hedef sistemde kod çalıştırmanın bir yolunu bulduktan sonra. Ardından, sistem içinde bir ters kabuk (reverse shell) elde etmek için kullanabileceğiniz olası araçların bir listesi çok faydalı olacaktır.

Özellikle Windows'ta antivirüslerden kaçınmak için biraz yardıma ihtiyacınız olabilir: [""(windows/av-bypass.md).""](broken-reference)

## 8 - Inside

Shell kabukları ile ilgili sorunlarınız varsa, burada pentesterlar için en kullanışlı komutların bir derlemesini bulabilirsiniz.

* [Linux](broken-reference)
* [Windows ](broken-reference)(CMD)
* [Windows (PS)](broken-reference)

## 9 - Exfiltration

Muhetemelen hedef sistemden bazı verileri çıkarmamız veya hatta bir şey (ayrıcalık yükseltme komut dosyaları gibi) tanıtmanız gerekecektir.

## 10 - Privilege Escalation

### 10.1 - Local Privesc

Sızdığınız sistemde root/Administrator yani yönetici değilseniz, ayrıcalıkları yükseltmenin bir yolunu bulmalısınız.

Burada, [Linux'ta ](broken-reference)ve [Windows'ta ](broken-reference)yerel olarak ayrıcalıkları yükseltmek için bir kılavuz bulabilirsiniz.

* [Authentication, Credentials, Token Privileges and UAC](broken-reference)
* [NTLM nasıl çalışır](broken-reference)
* Windows'ta kimlik bilgileri nasıl çalışır[ (steal credentials)](broken-reference)
* [Active Directory](broken-reference) hakkında bazı püf noktaları

Windows ve Linux Local Privesc ayrıcalık yükseltme yollarını numaralandırmak için [Suite PEAS](https://github.com/carlospolop/PEASS-ng) kullanılabilir.

### 10.2 - Domain Privesc

Burada, bir Active Directory'de ayrıcalıkları sıralamak, yükseltmek ve kalıcı olmak için en yaygın eylemleri açıklayan bir metodoloji bulabilirsiniz. Bu sadece bir bölümün alt bölümü olsa bile, bu süreç bir Pentesting/Kırmızı takım görevinde son derece hassas olabilir.

## 11 - POST

### 11.1 - Looting

Ana bilgisayar içinde daha fazla şifre bulup bulamayacağınızı veya kullanıcınızın ayrıcalıklarına sahip diğer makinelere erişiminiz olup olmadığını kontrol edin.

Windows'ta parolaları boşaltmanın farklı yollarını [burada ](broken-reference)bulun.

### 11.2 - Persistence

2 veya 3 farklı kalıcılık mekanizması kullanın, böylece sistemi tekrar kullanmanıza gerek kalmaz.

[Burada ](broken-reference)Active Directory için bazı kalıcılık tekniklerini bulabilirsiniz.

## 12 - Pivoting

Toplanan kimlik bilgileriyle diğer makinelere erişiminiz olabilir veya kurbanınızın bağlı olduğu yeni ağlarda yeni ana bilgisayarları keşfetmeniz ve taramanız (Pentest Metodolojisini yeniden başlatmanız anlamına gelir) gerekebilir.

Bu durumda tünel açma gerekli olabilir. [Burada ](broken-reference)tünel açma hakkında bir yazı bulabilirsiniz.

[Active Directory pentesting methodolojisi](broken-reference) hakkındaki gönderiye de mutlaka göz atmalısınız. Orada yatay hareket etmek, ayrıcalıkları yükseltmek ve kimlik bilgilerini boşaltmak için harika teknikler bulacaksınız.

[NTLM ](broken-reference)ile ilgili sayfayı da kontrol edin, windows ortamlarında pivot tekniklerinde bahsediyorum.

## MORE

### Exploiting

* Basic Linux Exploiting
* Basic Windows Exploiting
* Basic exploiting tools
