---
description: >-
  Take your time. Don't try to do too much at once. Remember small steps - don't
  try to jump to root using rshell. Maybe run a few commands to see what they do
---

# 🐧 Everything Linux

<figure><img src="../../.gitbook/assets/linux.png" alt=""><figcaption><p>Linux Dosya Sistemi</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

## Linux Privilege Escalation Examples

```
https://github.com/Tib3rius/Pentest-Cheatsheets/blob/master/privilege-escalation/linux/linux-examples.rst
```

## MindMap

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption><p>MindMap for PE</p></figcaption></figure>

### Çalıştırmak için yararlı komutlar

```
https://github.com/mubix/post-exploitation/wiki/Linux-Post-Exploitation-Command-List
```

### aranacak şeyler

* Yanlış yapılandırılmış hizmetler (cronjobs)
  * ayrıcalıklı bir kullanıcı olarak çalışan herhangi biri?
* Yanlış dosya izinleri (exportfs, sudo)
* Yanlış yapılandırılmış ortam ($PATH)
* Binary with SUID bit
* Bilinen güvenlik açıklarına sahip yazılım veya işletim sistemi
* Docker usergroup
