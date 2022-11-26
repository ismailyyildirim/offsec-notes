# Pasif Bilgi Toplama

### DNS enumeration

(Domain Name System) Etki Alanı Adı Sistemi (DNS) numaralandırması, bir hedefle ilişkili DNS sunucularını ve kayıtlarını belirleme işlemidir:

* **Address (A)** records containing the IP addresses for domains
* **Mail Exchange (MX)** records containing mail addresses
* **Canonical Name (CNAME)** records used for aliasing domains and identifying subdomains within DNS records
* **Name Server (NS)** records showthe authoritative (or main) name server for the domain
* **State of Authority (SOA)** records have important information about the domain such as the primary name server, timestamp showing last update and the party responsible for the domain
* **Pointer Records (PTR)** map an IPv4 address to the CNAME on the host, aka ‘reverse record’ because it connects a record with an IP address to a hostname instead of the other way around
* **TXT records** may include additional information (e.g. configuration)

### whois

Kayıt şirketi, alan sahibi, iletişim bilgileri ve DNS sunucusu gibi alanla ilgili genel bilgileri almak için bir whois araması kullanılabilir:

```
whois google.com    
```

### Nslookup

Nslookup (Ad Sunucusu araması), DNS kayıtları için alan adı sistemini sorgulamak için kullanılır:

```
nslookup google.com
```

\--type= seçeneğini ve ardından DNS kayıt türünü kullanarak DNS kayıtlarını sorgulayın:

```
nslookup --type=A google.com
```

Etki alanı için tüm DNS kayıtlarını görüntülemek üzere DNS kayıt türü olarak 'rastgele' herhangi birini kullanabilirsiniz:

```
nslookup -type=any google.com
```

#### SPF Record

Gönderen Politikası Çerçevesi (SPF) kaydı, hangi posta sunucularının alanınız adına e-posta göndermesine izin verildiğini tanımlayan bir DNS kaydı türüdür. SPF kayıtları, istenmeyen posta gönderenlerin belirli bir alandan sahte 'Gönderen' adreslere sahip iletiler göndermesini engeller. Alıcı bir posta sunucusu, iletinin meşru bir sunucudan gelip gelmediğini kontrol etmek için gönderen etki alanının SPF kaydını kullanır.

### Host

Ana bilgisayar, alan adlarını IP adreslerine dönüştürmek için kullanılabilir ve bunun tersi de geçerlidir:

```
host google.com
```

#### Zone transfers

DNS sunucuları genellikle birbirleriyle eşitlenmesi gereken yedek/ikincil sunuculara sahiptir. Çoğaltma yöntemine bölge aktarımı denir. Halka açık bölge aktarımları etkinleştirilmiş DNS sunucuları, tahmin yoluyla bulunamayacak sunucuları ortaya çıkarabilir. Bölge aktarımları genellikle DNS sunucuları için devre dışı bırakılır, ancak yine de kontrol edilmeye değerdir.

Ana bilgisayarı kullanarak bölge aktarım yeteneğini kontrol edin, ad sunucusunu almak için bu komutu kullanın:

```
host -t ns google.com
```

Ardından, sonraki komutta name server sunucusunu argüman olarak kullanın:

```
host -t axfr -l google.com ns1.google.com
```

### Dig

Dig 'host' a çok benzer.

örneğin 'google.com' alanı için MX kayıtlarını alın:

```
dig -t mx google.com
```

Tüm kayıtları istemek için 'any' parametresini belirtin:

```
dig -t any google.com
```

Zone Transferlerini test etmek için aşağıdaki komutu kullanın: (zonetransfer kasıtlı olarak savunmasızdır)

```
dig axfr @nsztm1.digi.ninja zonetransfer.me
```

### DNSenum

DNSenum, bitişik olmayan IP bloklarını keşfetmek için DNS bilgilerini numaralandırır. Ayrıca, DNS üzerinde bölge aktarımlarını dener:

```
dnsenum google.com
```

### DNS Recon

DNSrecon, DNS kayıtlarını sorgulamak ve bölge aktarımlarını denemek için kullanılan başka bir otomatik araçtır.

```
dnsrecon -d google.com
```

### Sublist3r

Sublist3r, bir alan adı için alt alan adlarını keşfetmek üzere popüler arama motorlarını kullanarak alt alan adlarını keşfeder. Ayrıca, DNS kayıtlarını ve alt alan adlarını numaralandırmak için bir sözcük listesi kullanan 'Subbrute' adlı tümleşik bir aracı kullanarak alt etki alanlarını tahmin edebilir:

```
sublist3r -d google.com
```

Subbrute ile kaba kuvvet yapmak için, komuta -b seçeneğini kullanın ve -t seçeneğiyle kullanılacak ek iş parçacığı sayısını kontrol edin:

```
sublist3r -d google.com -b -t 100
```

## Email harvesting

### The Harvester

Harvester, çeşitli arama motorlarında e-posta toplamak için kullanılır. Bir kuruluşun bir kamu çalışanı dizini yoksa, bu, kimlik avı için e-posta adreslerini toplamanın veya son veritabanı dökümlerinde parola aramanın hızlı bir yolu olabilir.

```
theharvester -d microsoft.com -b google -l 5
```

**domain** -d ile ve veri kaynağı -b (Google) ile belirtilir. Arama sonuçları -l seçeneği ile sınırlandırılabilir.

