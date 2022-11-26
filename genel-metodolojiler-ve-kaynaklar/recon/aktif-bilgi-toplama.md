# Aktif Bilgi Toplama

## Port and service scanning

Bir hedef hakkında ne kadar çok şey keşfederseniz, istismar için o kadar çok fırsatınız olur. Ağ koşulları değişebileceğinden, çeşitli araçların (ve her araç için çeşitli seçeneklerin) nasıl kullanılacağını bilmek iyidir.

### Netdiscover

Bu araç, canlı makineler için bir ağı taramak için kullanılır:

```
netdiscover -r 192.168.1.1/24
```

### Nmap

Nmap harika. Pek çok komut ve seçenek vardır, ancak aşağıda hem laboratuvar hem de sızma testi senaryolarında iyi çalışan yaygın olarak kullanılan bazı komutlar verilmiştir.

Host discovery (ping scan):

```
nmap -sn 192.168.1.1/24
```

Host discovery (specific range):

```
nmap -sn 192.168.1.1-100
```

Daha detaylı nmap taraması yapmak için [nmap-cheat](../../red-team/nmap.md) sheet sayfasına gitmenizi öneririm.



### FTP

Check if anonymous FTP access is available:

```
ftp [host]
Username: anonymous
Password: anything
```

Test if you can navigate, list, read, get or put files:

```
cd ..          # move up one directory
pwd            # print working directory
dir -C         # list files
mkdir [folder] # make a directory
get [file]     # get a file
put [file]     # send a file
```

## SMTP

Netcat ile bir SMTP sunucusuna bağlanabilir ve e-posta adreslerinin geçerli olup olmadığını kontrol etmek için vrfy komutunu çalıştırabilirsiniz. Ayrıca expn ile mail listesi üyeliğini de kontrol edebilirsiniz.

```
nc -nv [host] 25
(UNKNOWN) [host] 25 (smtp) open
VRFY root
250 2.1.5 root <root@host.com>
```

## SMB

Sunucu İleti Bloğu (Server Message Block)(SMB), yerel bir ağda paylaşılan dosyalara ve yazıcılara erişim sağlayan bir dosya paylaşım protokolüdür. SMB'nin eski sürümleri, EternalBlue gibi büyük açıklara karşı savunmasız olma eğilimindedir.

Versions:



| SMB Version | Windows version                                                          |
| ----------- | ------------------------------------------------------------------------ |
| CIFS        | Microsoft Windows NT 4.0                                                 |
| SMB 1.0     | Windows 2000, Windows XP, Windows Server 2003 and Windows Server 2003 R2 |
| SMB 2.0     | Windows Vista & Windows Server 2008                                      |
| SMB 2.1     | Windows 7 and Windows Server 2008 R2                                     |
| SMB 3.0     | Windows 8 and Windows Server 2012                                        |
| SMB 3.0.2   | Windows 8.1 and Windows Server 2012 R2                                   |
| SMB 3.1.1   | Windows 10 and Windows Server 2016                                       |

SMB, Nmap taramaları kullanarak keşfedilebilen şu bağlantı noktalarını kullanır:

* netbios-ns 137/tcp - NETBIOS Name Service
* netbios-ns 137/udp
* netbios-dgm 138/tcp - NETBIOS Datagram Service
* netbios-dgm 138/udp
* netbios-ssn 139/tcp - NETBIOS session service
* netbios-ssn 139/udp
* microsoft-ds 445/tcp - Active Directory

### SMBclient

Linux/Unix makineleri, SMB paylaşımlarına göz atabilir, bağlanabilir ve dosya aktarabilir.

Belirli bir ana bilgisayarda hangi paylaşımların mevcut olduğunu görmek için:

```
smbclient -L [host]
```

Bir ana bilgisayarda 'genel' olarak paylaşılan bir dizine ulaşmak için:

```
smbclient \\\\host\\public mypasswd

Server time is Sat Aug 10 15:58:44 1996
Timezone is UTC+10.0
Domain=[WORKGROUP] OS=[Windows NT 3.51] Server=[NT LAN Manager 3.51]
smb: \>
```

smb isteminden mevcut komutları görüntüleyin:

```
smb: \> h
ls             dir            lcd            cd             pwd            
get            mget           put            mput           rename         
more           mask           del            rm             mkdir          
md             rmdir          rd             prompt         recurse        
translate      lowercase      print          printmode      queue          
cancel         stat           quit           q              exit           
newer          archive        tar            blocksize      tarmode        
setmode        help           ?              !
```

### Nmap SMB scripts

Nmap, özellikle SMB protokolü için komut dosyalarına sahiptir (yukarıya bakın).

Bir ana bilgisayarı bilinen tüm SMB güvenlik açıklarına karşı taramak için:

```
nmap -p 139,445 --script=smb-vuln* [host]
```

Belirli bir SMB güvenlik açığı için bir hedefi taramak istiyorsanız, örneğin MS08-067 (uzaktan kod yürütülmesine izin verir), bu komutu çalıştırabilirsiniz:

```
nmap -p 139,445 --script=smb-vuln-ms08-067 [host]
```

#### MS17-010 EternalBlue script

EternalBlue, Shadow Brokers tarafından Nisan 2017'de sızdırılan açıklardan biridir. SMBv1 protokolündeki kritik bir güvenlik açığından yararlanır ve Windows 7, 8, 8.1 ve Windows Server 2003/2008 dahil olmak üzere birçok Windows kurulumunu uzaktan kod yürütmeye karşı savunmasız bırakır. /2012(R2)/2016.

```
nmap -p 445 [host] --script=smb-vuln-ms17-010
```

### Rpcclient

Rpcclient, parola gerektirmeyen bir bağlantı olan boş bir oturum kullanan istemci tarafı MS-RPC işlevleri (bağlantı noktası 445) için kullanılan bir Linux aracıdır. Boş oturumlar, eski sistemlerde varsayılan olarak etkinleştirildi, ancak o zamandan beri Windows XP SP2 ve Windows Server 2003'ten devre dışı bırakıldı.

```
rpcclient -U "" [host]
rpcclient $> querydominfo
rpcclient $> srvinfo
rpcclient $> enumdomusers
rpcclient $> queryuser [username]
rpcclient $> getdompwinfo
```

Yukarıdaki komutlar, kullanıcılar dahil etki alanı bilgilerini döndürür. Daha fazla numaralandırma komutu [burada ](https://bitvijays.github.io/LFF-IPS-P3-Exploitation.html)mevcuttur.

srvinfo yanıtına özellikle dikkat edin, çünkü Google'da arama yapmak size tam olarak ihtiyacınız olan açığı verebilir. Saçma sapan gibi görünüyor:

```
    HOSTNAME            Wk Sv PrQ Unx NT SNT Samba Server
    platform_id     :    500
    os version      :    4.9
    server type     :    0x9a03
```

### Enum4Linux

Enum4linux, Windows ve Samba ana bilgisayarlarından gelen verileri numaralandırmak için kullanılır. SMB komutlarına o kadar aşina değilseniz, çok fazla bilgiyi hızlı bir şekilde çıkarabileceği için gerçekten yararlıdır:

```
enum4linux [host]

-U        get userlist
-M        get machine list*
-S        get sharelist
-P        get password policy information
-G        get group and member list
-d        be detailed, applies to -U and -S
-u user   specify username to use (default “”)
-p pass   specify password to use (default “”)
-a        Do all simple enumeration (-U -S -G -P -r -o -n -i).
-o        Get OS information
-i        Get printer information
```

Ayrıca, istismarları ararken yardımcı olan srvinfo aracılığıyla işletim sistemi bilgilerini de çekecektir:

```
HOSTNAME    Wk Sv PrQ Unx NT SNT Samba Server
```

## SNMP

Basit Ağ Yönetimi Protokolü (SNMP), genellikle savunmasız olan daha eski bir UDP tabanlı protokoldür. Genellikle birçok ağ bilgisini açığa çıkarabilen varsayılan yapılandırmalarda bırakılırlar.

SNMP Yönetim Bilgi Tabanı (MIB), bir işlevler ağacında düzenlenmiş ağ yönetimi bilgilerini içeren bir veritabanıdır.

### SNMPwalk

SNMPwalk, yönetilen cihazlar hakkında bilgi almak için MIB değerlerini sorgular. Geçerli bir SNMP salt okunur topluluk dizesi gerektirir.

SNMPv1 cihazında varsayılan topluluk dizisi 'public' ile SNMPwalk'ı çalıştırmak için:

```
snmpwalk -c public -v1 [host]
```

Enumerate based on a single object ID:

```
snmpwalk -c public -v1 [host] [OID]
```

Enumerate Windows users:

```
snmpwalk -c public -v1 [host] 1.3.6.1.4.1.77.1.2.25
```

Enumerate running Windows processes:

```
snmpwalk -c public -v1 [host] 1.3.6.1.2.1.25.4.2.1.2
```

Enumerate open TCP ports:

```
snmpwalk -c public -v1 [host] 1.3.6.1.2.1.6.13.1.3
```

Enumerate installed software:

```
snmpwalk -c public -v1 [host] 1.3.6.1.2.1.25.6.3.1.2
```

## Website scanning

Web sunucuları, bilgisayar korsanları için yaygın bir hedeftir, çünkü sistemde (ör. kabuk) veya hatta bir kuruluşun ağında bir yer edinmek için kullanılabilirler. Tarama genellikle algılanabilir, ancak daha fazla yararlanma fırsatlarını da belirleyebilir.

### Nikto

Nikto, bir web sunucusunu hızlı bir şekilde taramak için iyi olan popüler (ancak gürültülü) bir değerlendirme aracıdır:

```
nikto -h [host]
```

Specify a port:

nikto -h \[host] -p 8080

Birden çok bağlantı noktasını test edin:

```
nikto -h [target host] -p 80,88,443
```

Specify a port range:

```
nikto -h [target host] -p 80-88
```

#### Tarama Ayarları

```
0 – File Upload
1 – Interesting File / Seen in logs
2 – Misconfiguration / Default File
3 – Information Disclosure
4 – Injection (XSS/Script/HTML)
5 – Remote File Retrieval – Inside Web Root
6 – Denial of Service
7 – Remote File Retrieval – Server Wide
8 – Command Execution / Remote Shell
9 – SQL Injection
a – Authentication Bypass
b – Software Identification
c – Remote Source Inclusion
x – Reverse Tuning Options (i.e., include all except specified)
```

### Dirb

Dirb, bir sözlük kullanarak web nesnelerini tahmin eden bir web içeriği tarayıcısıdır.

```
dirb [http://host]
```

Özel bir kelime listesi de kullanabilir:

```
dirb [http://host] [wordlist]
```

Kelime listeleri burada bulunur:

```
/usr/share/wordlists/dirb/
```

Varsayılan olarak dirb, çoğu laboratuvar durumunda iyi çalışan common.txt'yi kullanır. Ancak, çok küçük bir saldırı yüzeyi olan bir makineyi sıralıyorsanız (örneğin, yalnızca 80 numaralı bağlantı noktası açıksa), bunun yerine big.txt'yi denemek isteyebilirsiniz.

### Dirbuster

Dirbuster, GUI'ye ve daha fazla kelime listesi dahil olmak üzere bazı ek özelliklere sahip bir web tarayıcısıdır:

```
dirbuster
```

Kelime listeleri burada bulunur:

```
/usr/share/dirbuster/wordlists/
```

### WPScan

WordPress popüler bir web sitesi/blog platformudur ve sıklıkla bilgisayar korsanları tarafından hedef alınır. Güvenlik açıkları genellikle topluluk tarafından geliştirilen modüller ve temalar aracılığıyla tanıtılır. WPScan, çeşitli modül/tema güvenlik açıklarını tarayan ve ayrıca kullanıcıları sıralayabilen bir araçtır.

WPScan'i en son sürümüne güncelleyin:

```
wpscan --update
```

Varsayılan tarama:

```
wpscan --url [http://host]
```



Enumerate specific options:

```
wpscan --url [http://host] --enumerate [p/vp/ap/t/vt/at]
```

Scan for all popular plugins:

```
wpscan --url [http://host] --enumerate p
```

Scan for vulnerable plugins:

```
wpscan --url [http://host] --enumerate vp
```

Scan for all plugins:

```
wpscan --url [http://host] --enumerate ap
```

Enumerate users:

```
wpscan --url [http://host] --enumerate u
```



## Further reading

* [Nmap port scanning techniques](https://nmap.org/book/man-port-scanning-techniques.html)
* [Nmap scripting engine](https://nmap.org/book/nse.html)
* [The story behind MS08-067](https://blogs.technet.microsoft.com/johnla/2015/09/26/the-inside-story-behind-ms08-067/)
* [SMB cheat sheet](https://www.tldp.org/HOWTO/SMB-HOWTO-8.html)
* [WAF detection and bypass](http://securityidiots.com/Web-Pentest/WAF-Bypass/waf-bypass-guide-part-1.html)
