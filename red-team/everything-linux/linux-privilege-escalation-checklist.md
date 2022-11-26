# Linux Privilege Escalation - Checklist

### **Best tool to look for Linux local privilege escalation vectors:** [**LinPEAS**](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)

### [System Information](broken-reference)

* [ ] Get **OS information**
* [ ] Check the [**PATH**](broken-reference), any **writable folder**?
* [ ] Check [**env variables**](broken-reference), any sensitive detail?
* [ ] Search for [**kernel exploits**](broken-reference) **using scripts** (DirtyCow?)
* [ ] **Check** if the [**sudo version** is vulnerable](broken-reference)
* [ ] [**Dmesg** signature verification failed](broken-reference)
* [ ] More system enum ([date, system stats, cpu info, printers](broken-reference))
* [ ] [Enumerate more defenses](broken-reference)

### [Drives](broken-reference)

* [ ] **List mounted** drives
* [ ] **Any unmounted drive?**
* [ ] **Any creds in fstab?**

### [**Installed Software**](broken-reference)

* [ ] **Check for**[ **useful software**](broken-reference) **installed**
* [ ] **Check for** [**vulnerable software**](broken-reference) **installed**

### [Processes](broken-reference)

* [ ] Is any **unknown software running**?
* [ ] Is any software running with **more privileges than it should have**?
* [ ] Search for **exploits of running processes** (especially the version running).
* [ ] Can you **modify the binary** of any running process?
* [ ] **Monitor processes** and check if any interesting process is running frequently.
* [ ] Can you **read** some interesting **process memory** (where passwords could be saved)?

### [Scheduled/Cron jobs?](broken-reference)

* [ ] Is the [**PATH** ](broken-reference)being modified by some cron and you can **write** in it?
* [ ] Any [**wildcard** ](broken-reference)in a cron job?
* [ ] Some [**modifiable script** ](broken-reference)is being **executed** or is inside **modifiable folder**?
* [ ] Have you detected that some **script** could be or are being [**executed** very **frequently**](broken-reference)? (every 1, 2 or 5 minutes)

### [Services](broken-reference)

* [ ] Any **writable .service** file?
* [ ] Any **writable binary** executed by a **service**?
* [ ] Any **writable folder in systemd PATH**?

### [Timers](broken-reference)

* [ ] Any **writable timer**?

### [Sockets](broken-reference)

* [ ] Any **writable .socket** file?
* [ ] Can you **communicate with any socket**?
* [ ] **HTTP sockets** with interesting info?

### [D-Bus](broken-reference)

* [ ] Can you **communicate with any D-Bus**?

### [Network](broken-reference)

* [ ] Enumerate the network to know where you are
* [ ] **Open ports you couldn't access before** getting a shell inside the machine?
* [ ] Can you **sniff traffic** using `tcpdump`?

### [Users](broken-reference)

* [ ] Generic users/groups **enumeration**
* [ ] Do you have a **very big UID**? Is the **machine** **vulnerable**?
* [ ] Can you [**escalate privileges thanks to a group**](broken-reference) you belong to?
* [ ] **Clipboard** data?
* [ ] Password Policy?
* [ ] Try to **use** every **known password** that you have discovered previously to login **with each** possible **user**. Try to login also without a password.

### [Writable PATH](broken-reference)

* [ ] If you have **write privileges over some folder in PATH** you may be able to escalate privileges

### [SUDO and SUID commands](broken-reference)

* [ ] Can you execute **any command with sudo**? Can you use it to READ, WRITE or EXECUTE anything as root? ([**GTFOBins**](https://gtfobins.github.io))
* [ ] Is any **exploitable SUID binary**? ([**GTFOBins**](https://gtfobins.github.io))
* [ ] Are [**sudo** commands **limited** by **path**? can you **bypass** the restrictions](broken-reference)?
* [ ] [**Sudo/SUID binary without path indicated**](broken-reference)?
* [ ] [**SUID binary specifying path**](broken-reference)? Bypass
* [ ] [**LD\_PRELOAD vuln**](broken-reference)
* [ ] [**Lack of .so library in SUID binary**](broken-reference) from a writable folder?
* [ ] [**SUDO tokens available**](broken-reference)? [**Can you create a SUDO token**](broken-reference)?
* [ ] Can you [**read or modify sudoers files**](broken-reference)?
* [ ] Can you [**modify /etc/ld.so.conf.d/**](broken-reference)?
* [ ] [**OpenBSD DOAS**](broken-reference) command

### [Capabilities](broken-reference)

* [ ] Has any binary any **unexpected capability**?

### [ACLs](broken-reference)

* [ ] Has any file any **unexpected ACL**?

### [Open Shell sessions](broken-reference)

* [ ] **screen**
* [ ] **tmux**

### [SSH](broken-reference)

* [ ] **Debian** [**OpenSSL Predictable PRNG - CVE-2008-0166**](broken-reference)
* [ ] [**SSH Interesting configuration values**](broken-reference)

### [Interesting Files](broken-reference)

* [ ] **Profile files** - Read sensitive data? Write to privesc?
* [ ] **passwd/shadow files** - Read sensitive data? Write to privesc?
* [ ] **Check commonly interesting folders** for sensitive data
* [ ] **Weird Location/Owned files,** you may have access to or alter executable files
* [ ] **Modified** in last mins
* [ ] **Sqlite DB files**
* [ ] **Hidden files**
* [ ] **Script/Binaries in PATH**
* [ ] **Web files** (passwords?)
* [ ] **Backups**?
* [ ] **Known files that contains passwords**: Use **Linpeas** and **LaZagne**
* [ ] **Generic search**

### [**Writable Files**](broken-reference)

* [ ] **Modify python library** to execute arbitrary commands?
* [ ] Can you **modify log files**? **Logtotten** exploit
* [ ] Can you **modify /etc/sysconfig/network-scripts/**? Centos/Redhat exploit
* [ ] Can you [**write in ini, int.d, systemd or rc.d files**](broken-reference)?

### [**Other tricks**](broken-reference)

* [ ] Can you [**abuse NFS to escalate privileges**](broken-reference)?
* [ ] Do you need to [**escape from a restrictive shell**](broken-reference)?
