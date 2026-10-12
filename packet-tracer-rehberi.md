# Cisco Packet Tracer Öğrenme Rehberi

Son güncelleme: 12 Ekim 2026

## İçindekiler

- [1. Başlarken: Packet Tracer arayüzü ve kısayollar](#1-başlarken-packet-tracer-arayüzü-ve-kısayollar)
- [2. IOS CLI temelleri: modlar, kısayollar, kısaltmalar](#2-ios-cli-temelleri-modlar-kısayollar-kısaltmalar)
- [3. Fabrika ayarları ve default değerler](#3-fabrika-ayarları-ve-default-değerler)
- [4. Switch ilk kurulum: her labda yapılacaklar](#4-switch-ilk-kurulum-her-labda-yapılacaklar)
- [5. VLAN, trunk, native VLAN ve VTP](#5-vlan-trunk-native-vlan-ve-vtp)
- [6. Switch güvenliği: port-security, DHCP snooping, BPDU guard](#6-switch-güvenliği-port-security-dhcp-snooping-bpdu-guard)
- [7. STP, Rapid-PVST+ ve EtherChannel](#7-stp-rapid-pvst-ve-etherchannel)
- [8. Inter-VLAN routing: router-on-a-stick ve L3 switch](#8-inter-vlan-routing-router-on-a-stick-ve-l3-switch)
- [9. Router temelleri ve yönlendirme: static, RIP, OSPF, EIGRP](#9-router-temelleri-ve-yönlendirme-static-rip-ospf-eigrp)
- [10. Ağ servisleri: DHCP, NAT/PAT, ACL, NTP, Syslog, CDP/LLDP, HSRP](#10-ağ-servisleri-dhcp-natpat-acl-ntp-syslog-cdplldp-hsrp)
- [11. Show komutları ve sorun giderme](#11-show-komutları-ve-sorun-giderme)
- [12. Adım adım örnek projeler](#12-adım-adım-örnek-projeler)
- [13. Trikler, sık yapılan hatalar ve Packet Tracer tuzakları](#13-trikler-sık-yapılan-hatalar-ve-packet-tracer-tuzakları)
- [14. Temel komutlar: hızlı başvuru](#14-temel-komutlar-hızlı-başvuru)

## 1. Başlarken: Packet Tracer arayüzü ve kısayollar

Bu rehber sırayla okunacak şekilde dizildi: önce CLI ve switch temelleri, sonra router ve servisler, en sonda adım adım projeler. Her komut bloğunu Packet Tracer'da kendin yaz; kopyala-yapıştır yerine elle yazmak komutları ezberletir.

### Rehberi nasıl kullanmalısın

1. Bölüm 2 (CLI temelleri) ve Bölüm 4 (switch ilk kurulum şablonu) ezbere gelene kadar her yeni labda tekrar et.
2. Her konudan sonra Bölüm 12'deki ilgili projeyi sıfırdan kur.
3. Bir şey çalışmayınca Bölüm 11'deki sorun giderme sırasını uygula, rastgele komut deneme.
4. Çalışan her labı `.pkt` olarak kaydet; ileride şablon olarak kullanırsın.

VLAN oluşturma, porta VLAN atama, trunk, IP verme, kaydetme gibi temel komutlara hızlıca bakmak için doğrudan Bölüm 14'e git; orada her işin kodu göreve göre sıralı.

### Arayüzün bölümleri

| Bölüm | Nerede | Ne işe yarar |
| --- | --- | --- |
| Workspace (Logical) | Ortadaki büyük alan | Cihazları sürükleyip bıraktığın ve kabloladığın topoloji alanı |
| Physical workspace | Üst sekme | Şehir, bina, rack görünümü; kablosuz ve mesafe senaryoları için |
| Device-Type Selection | Sol alt köşe | Cihaz kategorileri: Network Devices, End Devices, Connections |
| Device-Specific Selection | Kategorinin sağı | Seçilen kategorideki modeller (2960, 1941, PC-PT vb.) |
| Common Tools Bar | Sağ veya üst araç çubuğu | Select, Inspect, Delete, Note, PDU ekleme araçları |
| Realtime / Simulation | Sağ alt köşe | Gerçek zamanlı çalışma veya paketleri adım adım izleme |
| Cihaz penceresi | Cihaza tıklayınca | Physical, Config, CLI (router/switch) veya Desktop (PC) sekmeleri |

### Hangi cihazı seçmelisin

| Model | Tür | Portlar | Ne zaman kullan |
| --- | --- | --- | --- |
| 2960-24TT | Layer 2 switch | Fa0/1-24, Gi0/1-2 | Erişim katmanı, VLAN, port-security, STP labları |
| 3560-24PS | Layer 3 (multilayer) switch | Fa0/1-24, Gi0/1-2 | Inter-VLAN routing (SVI), `ip routing`, dağıtım katmanı |
| 3650-24PS | Layer 3 switch | Gi1/0/1-24, Gi1/1/1-4 | Yeni nesil L3 lab; önce güç kaynağı takman gerekir (aşağıda) |
| 1941 | Router | Gi0/0, Gi0/1 | Temel routing, NAT, DHCP; seri port için modül gerekir |
| 2911 | Router | Gi0/0, Gi0/1, Gi0/2 | 3 LAN portu lazım olduğunda; router-on-a-stick |
| ISR4331 | Router | Gi0/0/0, Gi0/0/1, Gi0/0/2 | Güncel CCNA labları; arayüz isimleri üç basamaklı |
| PC-PT / Laptop-PT | Uç cihaz | Fa0 | Ping, IP Configuration, Command Prompt, Terminal |
| Server-PT | Uç cihaz | Fa0 | DHCP, DNS, HTTP, TFTP, Syslog, NTP, AAA servisleri |

### Kablo türleri

| Kablo | Simge rengi | Hangi cihazlar arasında | Hatırlatma |
| --- | --- | --- | --- |
| Copper Straight-Through | Düz siyah çizgi | Farklı türler: PC-Switch, Switch-Router, Server-Switch | En çok kullanacağın kablo |
| Copper Cross-Over | Kesikli siyah çizgi | Aynı türler: Switch-Switch, Router-Router, PC-PC, PC-Router, Hub-Switch | PC-Router doğrudan bağlanırsa cross-over gerekir |
| Console (Rollover) | Açık mavi | PC'nin RS 232 portu - cihazın Console portu | Sonra PC > Desktop > Terminal ile bağlanırsın |
| Serial DCE / DTE | Kırmızı, saat simgeli uç DCE | Router-Router WAN bağlantısı | DCE ucunda `clock rate 64000` yazılır |
| Fiber | Turuncu | SFP/fiber portlar arası | Gi portlarının fiber olup olmadığına Physical sekmesinden bak |
| Automatic (şimşek) | Sarı şimşek | Her şey | Öğrenirken kullanma; kablo seçmeyi öğrenemezsin |

Kural: "Aynı tür cross-over, farklı tür straight". Switch ve hub bir grup, router ve PC bir grup sayılır. Bazı modeller Auto-MDIX ile yanlış kabloyu tolere eder, ama lablarda doğru kablo beklenir.

### Packet Tracer kısayolları

| Kısayol | İşlev |
| --- | --- |
| Ctrl+N / Ctrl+O / Ctrl+S | Yeni dosya / Aç / Kaydet |
| Ctrl+Z / Ctrl+Y | Geri al / Yinele (PT 7.3 ve sonrası) |
| Shift+R | Realtime moduna geç |
| Shift+S | Simulation moduna geç |
| Shift+L / Shift+P | Logical / Physical çalışma alanı |
| Esc | Select aracına dön (her şeyi iptal eder) |
| Del | Delete aracı |
| I | Inspect aracı (MAC tablosu, ARP, routing tablosu görme) |
| N | Not ekleme (topolojiye IP planını yazmak için) |
| P | Simple PDU ekle (iki cihaz arası hızlı ping testi) |
| C | Complex PDU ekle |
| Ctrl tuşuna basılı tutarak cihaz seçme | Aynı cihazdan art arda birden fazla yerleştirme |

Kısayollar sürüme göre küçük farklar gösterebilir; araç simgesinin üzerine gelince ipucu kutusunda kısayol yazar.

### Bağlantı ışıkları ne anlatır

| Renk | Anlamı | Ne yapmalısın |
| --- | --- | --- |
| Yeşil | Fiziksel ve protokol olarak up | Bir şey yapma |
| Turuncu (amber) | STP listening/learning ya da blocking | 30 saniye bekle veya Fast Forward Time'a bas; blocking normal olabilir |
| Kırmızı | Down | Router portunu `no shutdown` ile aç, kabloyu ve karşı portu kontrol et |
| Işık yok | Port yok sayılıyor veya cihaz kapalı | Cihazın gücünü (Physical sekme) kontrol et |

### Arayüz trikleri

- Router'a seri port eklemek için: cihaza tıkla, Physical sekmesinde güç düğmesini kapat, HWIC-2T modülünü boş yuvaya sürükle, gücü tekrar aç. Güç açıkken modül takılmaz.
- 3650 switch kutudan güç kaynağısız gelir. Physical sekmesinde AC-POWER-SUPPLY modülünü boş yuvaya sürükle, yoksa cihaz kapalı kalır.
- Router'ın Config sekmesinde yaptığın her ayar alttaki "Equivalent IOS Commands" kutusunda komut olarak görünür. GUI'den bir şey yapıp komutunu oradan öğrenebilirsin.
- Options > Preferences > "Always Show Port Labels" açarsan kablolarda port isimleri görünür; topoloji okumak çok kolaylaşır.
- STP veya OSPF'in toparlanmasını beklemek yerine sağ alttaki Fast Forward Time düğmesine birkaç kez bas.
- Simulation modunda Edit Filters ile sadece ICMP, ARP veya DHCP paketlerini göster; ekran sadeleşir.
- Bir PC'den diğerine ping atmadan önce IP'leri Note aracıyla topolojinin yanına yaz. Hata ayıklarken tablo aramazsın.

## 2. IOS CLI temelleri: modlar, kısayollar, kısaltmalar

Cisco CLI'da her komut belirli bir modda çalışır; komut "Invalid input" veriyorsa çoğunlukla yanlış moddasın. Prompt'a bakmak, hangi modda olduğunu söyleyen tek ipucudur.

### Modlar

| Mod | Prompt | Nasıl girilir | Ne yapılır |
| --- | --- | --- | --- |
| User EXEC | `Switch>` | Cihaza bağlanınca | Sadece basit `show`, `ping`, `traceroute` |
| Privileged EXEC | `Switch#` | `enable` | Tüm `show` ve `debug`, `copy`, `reload`, `erase`, `clock set` |
| Global Configuration | `Switch(config)#` | `configure terminal` | Cihaz geneli ayarlar: hostname, şifre, VLAN, routing |
| Interface Config | `Switch(config-if)#` | `interface fa0/1` | Tek port ayarı |
| Interface Range | `Switch(config-if-range)#` | `interface range fa0/1 - 10` | Birden fazla portu tek seferde ayarlama |
| Line Config | `Switch(config-line)#` | `line console 0` veya `line vty 0 15` | Konsol ve uzak erişim ayarları |
| VLAN Config | `Switch(config-vlan)#` | `vlan 10` | VLAN adı verme |
| Router Config | `Router(config-router)#` | `router ospf 1` | Routing protokolü ayarları |
| Subinterface | `Router(config-subif)#` | `interface g0/0.10` | Router-on-a-stick alt arayüzleri |
| DHCP Config | `Router(dhcp-config)#` | `ip dhcp pool LAN10` | DHCP havuzu |

Geri dönmek için: `exit` bir seviye yukarı çıkar, `end` veya Ctrl+Z doğrudan `#` moduna döner, `disable` ise `#`'den `>`'ye iner.

```
Switch> enable
Switch# configure terminal
Switch(config)# interface fastEthernet 0/1
Switch(config-if)# description PC1 baglantisi
Switch(config-if)# exit
Switch(config)# end
Switch#
```

```text
                 enable                 configure terminal
+-------------+          +-----------------+          +----------------------+
| User EXEC   | -------> | Privileged EXEC | -------> | Global Configuration |
| Switch>     | <------- | Switch#         | <------- | Switch(config)#      |
+-------------+          +-----------------+          +----------------------+
                disable                exit / end / Ctrl+Z
                                                                  |
     +------------------------------------------------------------+
     |
     +-- interface fa0/1  -->  Switch(config-if)#      port ayarları
     +-- line vty 0 15    -->  Switch(config-line)#    console / vty
     +-- vlan 10          -->  Switch(config-vlan)#    VLAN adı
     +-- router ospf 1    -->  Switch(config-router)#  routing protokolü

Alt modlardan: exit -> bir üst mod, end veya Ctrl+Z -> doğrudan Switch#
```

Her ok o moda geçiren komutu gösterir; port, hat, VLAN ve routing alt modlarına her zaman Global Configuration'dan girilir.

### Klavye kısayolları (CLI içinde)

| Kısayol | İşlev |
| --- | --- |
| Tab | Yarım yazılan komutu tamamlar (`conf` + Tab = `configure`) |
| `?` | O noktada yazılabilecek her şeyi listeler |
| Ctrl+Z | Hangi config modunda olursan ol `#` moduna döner |
| Ctrl+C | Komutu iptal eder, config modundan çıkar |
| Ctrl+Shift+6 | Uzun süren ping, traceroute veya "Translating..." DNS beklemesini keser |
| Yukarı ok / Ctrl+P | Bir önceki komut (geçmiş) |
| Aşağı ok / Ctrl+N | Bir sonraki komut |
| Ctrl+A / Ctrl+E | Satır başına / satır sonuna git |
| Esc+B / Esc+F | Bir kelime geri / ileri |
| Ctrl+W | İmlecin solundaki kelimeyi siler |
| Ctrl+U | İmlecin solundaki her şeyi siler |
| Space | `--More--` çıktısında bir sayfa ilerler |
| Enter | `--More--` çıktısında bir satır ilerler |
| q veya herhangi bir tuş | `--More--` çıktısını kapatır |

### Yardım sistemini kullanmak

```
Switch# sh?                 ! "sh" ile başlayan komutlar: show
Switch# show ?              ! show'dan sonra gelebilecek her şey
Switch# show ip ?           ! sadece ip ile ilgili alt komutlar
Switch(config)# interface ? ! port türlerini listeler
```

Not: `sh?` (boşluksuz) "bu harflerle başlayan komutlar" demektir, `sh ?` (boşluklu) ise "bu komuttan sonra ne gelir" demektir.

| Hata mesajı | Anlamı | Çözüm |
| --- | --- | --- |
| `% Ambiguous command` | Kısaltma birden fazla komuta uyuyor (`s` = show, set…) | Bir iki harf daha yaz |
| `% Incomplete command` | Komutun devamı eksik | Sonuna `?` yazıp eksik parçayı gör |
| `% Invalid input detected at '^' marker` | `^` işaretinin olduğu yerde hata var | Yazımı veya modu kontrol et |
| `Translating "shw"...domain server` | Yanlış yazılan kelimeyi hostname sanıp DNS'e soruyor | Ctrl+Shift+6, kalıcı çözüm `no ip domain-lookup` |

### En çok kullanılan kısaltmalar

| Kısa | Uzun hali |
| --- | --- |
| `en` | `enable` |
| `conf t` | `configure terminal` |
| `int fa0/1` / `int g0/0` | `interface FastEthernet0/1` / `interface GigabitEthernet0/0` |
| `int ra fa0/1-24` | `interface range FastEthernet0/1 - 24` |
| `no sh` | `no shutdown` |
| `sh run` | `show running-config` |
| `sh start` | `show startup-config` |
| `sh ip int br` | `show ip interface brief` |
| `sh int status` | `show interfaces status` (switch) |
| `sh vl br` | `show vlan brief` |
| `sh ip ro` | `show ip route` |
| `wr` | `write memory` (= `copy running-config startup-config`) |
| `copy run start` | `copy running-config startup-config` |

### Bilinmesi gereken kalıplar

`do` ile config modundan çıkmadan show çalıştır:

```
Switch(config-if)# do show ip interface brief
Switch(config)# do wr
```

`no` ile bir ayarı geri al, `default` ile portu fabrika ayarına döndür:

```
Switch(config-if)# no description
Switch(config)# no vlan 30
Switch(config)# default interface fa0/5
```

Çıktıyı süzmek için pipe (`|`) kullan:

```
Switch# show running-config | include hostname
Switch# show running-config | section line
Switch# show running-config | begin interface Vlan
Switch# show ip interface brief | exclude unassigned
Router# show ip route | include O
```

Running-config RAM'dedir ve elektrik gidince silinir; startup-config NVRAM'dedir. Her değişiklikten sonra `copy run start` (veya `wr`) yazmazsan PT dosyayı kaydetsen bile cihaz yeniden başladığında ayarlar kaybolur.

```
Switch# copy running-config startup-config
Destination filename [startup-config]?   <- Enter'a bas
Building configuration...
[OK]
```

## 3. Fabrika ayarları ve default değerler

Kutudan çıkan bir Cisco switch tak-çalıştır çalışır ama hiçbir güvenliği yoktur; router ise portları kapalı gelir ve sen açmadan trafik geçirmez. Default değerleri bilmek, "neden çalışmıyor" sorusunun yarısını cevaplar.

### Switch ve router: kutudan çıktığı gibi

| Ayar | Switch (2960) default | Router (1941/2911) default |
| --- | --- | --- |
| Hostname | `Switch` | `Router` |
| Portlar | Açık (`no shutdown`) | Kapalı (`administratively down`) |
| Şifreler | Yok (console, enable, vty) | Yok |
| Telnet/SSH | VTY şifresi olmadığı için reddedilir: "Password required, but none set" | Aynı |
| VTY hatları | `line vty 0 15` | Genelde `line vty 0 4` |
| `ip domain-lookup` | Açık (yanlış komutu DNS'e sorar) | Açık |
| `service password-encryption` | Kapalı (şifreler düz metin görünür) | Kapalı |
| `logging synchronous` | Kapalı (log mesajları yazdığın komutun ortasına düşer) | Kapalı |
| `exec-timeout` | 10 dakika | 10 dakika |
| Konsol ayarı | 9600 baud, 8 data bit, parity yok, 1 stop bit, flow control yok | Aynı |
| Config register | Yok (switch) | `0x2102` (normal açılış); `0x2142` şifre kurtarma |
| `ip routing` | Yok (2960 L2); 3560/3650'de kapalı | Açık |
| CDP | Açık, 60 sn gönderim, 180 sn holdtime | Açık |
| LLDP | Kapalı (30 sn / 120 sn) | Kapalı |
| Speed / duplex | auto / auto | auto / auto |
| MTU | 1500 byte | 1500 byte |

### Switch'e özel default'lar

| Ayar | Default | Neden önemli |
| --- | --- | --- |
| VLAN | Tüm portlar VLAN 1'de | VLAN 1 silinemez, adı değiştirilemez |
| Silinemeyen VLAN'lar | 1, 1002, 1003, 1004, 1005 | `show vlan brief`'te hep görünürler |
| Native VLAN | 1 | Trunk'ta etiketsiz giden VLAN; güvenlik için değiştir |
| Yönetim arayüzü | `interface Vlan1` var ama shutdown ve IP'siz | IP verip `no shutdown` yapmazsan switch'e uzaktan ulaşamazsın |
| `ip default-gateway` | Yok | Başka ağdan switch'e telnet/SSH için şart |
| Port modu (DTP) | `dynamic auto` | İki `dynamic auto` port trunk olmaz, access kalır |
| STP modu | PVST+ (her VLAN için ayrı ağaç) | Rapid için `spanning-tree mode rapid-pvst` |
| Bridge priority | 32768 (+ VLAN no = 32769 VLAN 1 için) | Eşitse en düşük MAC root olur, yani genelde en eski switch |
| STP timer'ları | Hello 2 sn, Forward Delay 15 sn, Max Age 20 sn | Port yeşil olana kadar 30-50 sn bekleme bu yüzden |
| STP port cost | 10 Mbps = 100, 100 Mbps = 19, 1 Gbps = 4, 10 Gbps = 2 | Root'a giden en ucuz yolu belirler |
| MAC tablo yaşlanma | 300 saniye | Tablodan silinen MAC'e gelen çerçeve flood edilir |
| VTP | Mode server, domain boş, versiyon 1, revision 0 | Yeni switch'i ağa takmadan önce revision'ı sıfırla (Bölüm 5) |
| Port-security | Kapalı; açılınca max 1 MAC, violation `shutdown` | İhlalde port err-disabled olur |

### Routing default'ları

| Kaynak | Administrative Distance |
| --- | --- |
| Connected | 0 |
| Static | 1 |
| EIGRP summary | 5 |
| eBGP | 20 |
| EIGRP internal | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP external | 170 |
| iBGP | 200 |
| Bilinmeyen / güvenilmez | 255 (tabloya girmez) |

Aynı ağ iki kaynaktan öğrenilirse AD'si küçük olan tabloya girer. Floating static route bu kuralı kullanır: `ip route 0.0.0.0 0.0.0.0 10.0.0.2 130` yedek yol olur, çünkü 130 > 110.

| Protokol | Default'lar | Mutlaka değiştir |
| --- | --- | --- |
| RIP | Versiyon 1 (classful), update 30 sn, invalid 180, flush 240, max 15 hop | `version 2` ve `no auto-summary` |
| OSPF | Hello 10 / Dead 40 (Ethernet ve point-to-point), priority 1, reference bandwidth 100 Mbps | `auto-cost reference-bandwidth 1000` (Gigabit ağlarda) |
| EIGRP | Hello 5 / Hold 15, metrik bandwidth + delay (K1=K3=1), AS numarası her iki uçta aynı olmalı | Eski IOS'ta auto-summary açık; yine de `no auto-summary` yaz |
| HSRP | Priority 100, hello 3 / hold 10, preempt kapalı, sanal MAC `0000.0c07.acXX` | Aktif olması gerekene `priority 110` + `preempt` |

### Akılda tutulacak sayılar

| Konu | Değer |
| --- | --- |
| Standard ACL numaraları | 1-99, 1300-1999 |
| Extended ACL numaraları | 100-199, 2000-2699 |
| Her ACL'in sonu | Görünmez `deny any` |
| Normal VLAN aralığı | 1-1005 (`vlan.dat` dosyasında tutulur) |
| Extended VLAN aralığı | 1006-4094 (VTP transparent veya v3 gerekir) |
| IOS DHCP lease süresi | 1 gün |
| Port numaraları | FTP 20/21, SSH 22, Telnet 23, SMTP 25, DNS 53, DHCP 67/68, TFTP 69, HTTP 80, POP3 110, NTP 123, SNMP 161/162, HTTPS 443, Syslog 514 |

Trik: VLAN bilgisi `startup-config`'te değil, flash'taki `vlan.dat` dosyasında durur. Switch'i tamamen sıfırlamak için ikisini de silmelisin:

```
Switch# erase startup-config
Switch# delete flash:vlan.dat
Delete filename [vlan.dat]?  <- Enter
Delete flash:/vlan.dat? [confirm]  <- Enter
Switch# reload
System configuration has been modified. Save? [yes/no]: no
```

## 4. Switch ilk kurulum: her labda yapılacaklar

Her yeni switch'e aynı 12 adımı uygula: isim, şifreler, banner, yönetim IP'si, gateway, SSH ve kaydetme. Bu şablonu ezberlersen CCNA lablarının ilk 5 dakikası otomatiğe biner.

### Kontrol listesi

1. `hostname` ver (SSH anahtarı üretmek için de şart).
2. `no ip domain-lookup` ile yanlış yazım beklemelerini kapat.
3. `enable secret` ile ayrıcalıklı mod şifresi koy.
4. Console hattına şifre, `login`, `logging synchronous`, `exec-timeout`.
5. VTY hatlarına şifre veya `login local`, `transport input ssh`.
6. `service password-encryption` ile düz metin şifreleri gizle.
7. `banner motd` ile uyarı mesajı.
8. Yönetim SVI'ına IP ver ve `no shutdown` yap.
9. `ip default-gateway` tanımla.
10. SSH: domain adı, kullanıcı, RSA anahtarı, `ip ssh version 2`.
11. Kullanılmayan portları kapat ve kullanılmayan bir VLAN'a koy (Bölüm 6).
12. `copy running-config startup-config`.

### Tam şablon (kopyala, IP ve isimleri değiştir)

```
enable
configure terminal
 hostname SW1
 no ip domain-lookup
 enable secret class123
 service password-encryption
 banner motd #Yetkisiz erisim yasaktir!#
!
 line console 0
  password cisco123
  login
  logging synchronous
  exec-timeout 5 0
  exit
!
 vlan 99
  name MGMT
  exit
 interface vlan 99
  description Yonetim
  ip address 192.168.99.11 255.255.255.0
  no shutdown
  exit
 ip default-gateway 192.168.99.1
!
 ip domain-name lab.local
 username admin privilege 15 secret Admin123
 crypto key generate rsa general-keys modulus 1024
 ip ssh version 2
 ip ssh time-out 60
 ip ssh authentication-retries 3
!
 line vty 0 15
  login local
  transport input ssh
  exec-timeout 5 0
  exit
!
 login block-for 120 attempts 3 within 60
 security passwords min-length 8
end
copy running-config startup-config
```

`crypto key generate rsa` tek başına yazılırsa "How many bits in the modulus \[512\]:" diye sorar; 1024 yaz. SSH versiyon 2 için en az 768 bit gerekir.

### Doğrulama

```
SW1# show running-config
SW1# show ip interface brief        ! Vlan99 up/up olmalı
SW1# show ip ssh                    ! SSH Enabled - version 2.0
SW1# show ssh                       ! açık SSH oturumları
SW1# show users                     ! kim bağlı
SW1# show interfaces vlan 99
```

PC'den test (Desktop > Command Prompt):

```
C:\> ping 192.168.99.11
C:\> ssh -l admin 192.168.99.11
Password: Admin123
SW1>
```

### Router ilk kurulumu: farklar

Router'da adımlar aynıdır, iki fark vardır: SVI yerine fiziksel porta IP verilir ve her port elle açılır. `ip default-gateway` yerine `ip route 0.0.0.0 0.0.0.0 <sonraki-hop>` kullanılır.

```
interface g0/0
 description LAN
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit
interface s0/0/0
 description R2'ye WAN
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000        ! sadece DCE ucunda
 no shutdown
 exit
```

### Bu bölümün trikleri

- `enable password` ve `enable secret` ikisi birden varsa secret geçerlidir. Password tip 7 ile şifrelenir ve internetteki araçlarla saniyede çözülür; secret MD5 (tip 5) hash'tir. Hep `secret` kullan.
- `service password-encryption` sadece o anki ve sonraki şifreleri gizler; kapatsan bile mevcut şifreler şifreli kalır.
- Yönetim SVI'ı (`interface vlan 99`) ancak o VLAN'da en az bir port up ise veya o VLAN'ı taşıyan bir trunk varsa up/up olur. "Vlan99 is up, line protocol is down" görürsen VLAN 99'da aktif port yoktur.
- Banner'daki ayırıcı karakter (`#`) mesajın içinde geçmesin; geçerse mesaj orada biter. Alternatif ayırıcı: `^C`, `$`, `%`.
- `login` yazıp `password` yazmazsan console'a bir daha giremezsin ("Password required, but none set"). Sıra: önce password, sonra login.
- `login local` kullanıyorsan `username` tanımlamadan VTY'ye girme; kendini dışarıda bırakırsın.
- Labda sürekli düşmemek için `exec-timeout 0 0` kullanabilirsin, ama gerçek ağda asla.
- Hostname vermeden `crypto key generate rsa` çalışmaz: "Please define a hostname other than Switch."

## 5. VLAN, trunk, native VLAN ve VTP

VLAN bir switch'i birden fazla mantıksal switch'e böler: farklı VLAN'daki iki PC aynı switch'te olsa bile router olmadan konuşamaz. Access port tek VLAN taşır, trunk port tüm VLAN'ları 802.1Q etiketiyle taşır.

### VLAN oluşturma ve porta atama

```
SW1(config)# vlan 10
SW1(config-vlan)# name SATIS
SW1(config-vlan)# vlan 20
SW1(config-vlan)# name MUHASEBE
SW1(config-vlan)# vlan 99
SW1(config-vlan)# name MGMT
SW1(config-vlan)# vlan 999
SW1(config-vlan)# name KULLANILMAYAN
SW1(config-vlan)# exit
!
SW1(config)# interface range fa0/1 - 10
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 10
SW1(config-if-range)# exit
SW1(config)# interface range fa0/11 - 20
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 20
```

VLAN'ı oluşturmadan `switchport access vlan 30` yazarsan switch VLAN 30'u kendisi oluşturur: "% Access VLAN does not exist. Creating vlan 30". İsim vermek için yine `vlan 30` moduna girmelisin.

### Doğrulama: show vlan brief

```
SW1# show vlan brief
VLAN Name                 Status    Ports
---- -------------------- --------- -------------------------------
1    default              active    Fa0/21, Fa0/22, Fa0/23, Fa0/24, Gi0/2
10   SATIS                active    Fa0/1, Fa0/2, Fa0/3, ... Fa0/10
20   MUHASEBE             active    Fa0/11, Fa0/12, ... Fa0/20
99   MGMT                 active
1002 fddi-default         active
...
```

Trunk portlar bu listede görünmez; onları `show interfaces trunk` ile görürsün.

### Trunk kurma

```
SW1(config)# interface g0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 99
SW1(config-if)# switchport trunk allowed vlan 10,20,99
SW1(config-if)# switchport nonegotiate
```

3560/3650 gibi L3 switch'lerde önce kapsülleme seçilmelidir, yoksa "Command rejected: An interface whose trunk encapsulation is Auto can not be configured to trunk mode" hatası alırsın:

```
DSW1(config-if)# switchport trunk encapsulation dot1q
DSW1(config-if)# switchport mode trunk
```

### allowed vlan tuzağı

| Komut | Sonuç |
| --- | --- |
| `switchport trunk allowed vlan 10,20` | Listeyi **tamamen değiştirir**: sadece 10 ve 20 geçer |
| `switchport trunk allowed vlan add 30` | Mevcut listeye 30'u ekler |
| `switchport trunk allowed vlan remove 20` | Listeden 20'yi çıkarır |
| `switchport trunk allowed vlan all` | Hepsine izin verir (default) |
| `switchport trunk allowed vlan except 1` | 1 hariç hepsi |

Sonradan VLAN 30 eklerken `add` yazmayı unutursan 10 ve 20 trunk'tan düşer ve yarı ağ bir anda kopar.

### DTP: iki uç hangi modda olursa ne olur

| Bir uç \\ Diğer uç | dynamic auto | dynamic desirable | trunk | access |
| --- | --- | --- | --- | --- |
| dynamic auto | access | trunk | trunk | access |
| dynamic desirable | trunk | trunk | trunk | access |
| trunk | trunk | trunk | trunk | çalışmaz (uyumsuz) |
| access | access | access | çalışmaz (uyumsuz) | access |

En iyi pratik: DTP'ye güvenme. Trunk olacak portta `switchport mode trunk` + `switchport nonegotiate`, PC portunda `switchport mode access` yaz.

### Native VLAN

- Native VLAN trunk üzerinde etiketsiz gider. İki uçta aynı olmalı.
- Farklıysa CDP şu uyarıyı basar: `%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on GigabitEthernet0/1 (99), with SW2 GigabitEthernet0/1 (1).`
- Güvenlik için native VLAN'ı kullanıcı olmayan bir VLAN'a (örneğin 99 veya 1000) taşı; VLAN hopping saldırısını zorlaştırır.

```
SW1# show interfaces trunk
Port      Mode   Encapsulation  Status     Native vlan
Gi0/1     on     802.1q         trunking   99

Port      Vlans allowed on trunk
Gi0/1     10,20,99
...
SW1# show interfaces fa0/1 switchport   ! Administrative ve Operational Mode
```

### VTP (VLAN Trunking Protocol)

VTP, VLAN listesini server'dan client'lara trunk üzerinden dağıtır. VLAN'ı tek yerde açarsın, diğer switch'ler öğrenir.

| Mod | VLAN oluşturabilir mi | VTP mesajını uygular mı | İletir mi |
| --- | --- | --- | --- |
| Server (default) | Evet | Evet | Evet |
| Client | Hayır | Evet | Evet |
| Transparent | Evet (sadece kendisi için) | Hayır | Evet (v2'de) |

```
SW1(config)# vtp domain SIRKET
SW1(config)# vtp mode server
SW1(config)# vtp password Vtp123
SW1(config)# vtp version 2
!
SW2(config)# vtp domain SIRKET
SW2(config)# vtp mode client
SW2(config)# vtp password Vtp123
!
SW1# show vtp status       ! Revision, domain, mod
SW1# show vtp password
```

VTP çalışması için: domain adı ve şifre birebir aynı olmalı (büyük/küçük harf dahil) ve switch'ler arası trunk olmalı.

VTP bombası: revision numarası daha yüksek olan bir switch ağa bağlanırsa tüm ağın VLAN listesini kendi listesiyle ezer. Başka labdan getirdiğin switch'i takmadan önce revision'ı sıfırla:

```
SW3(config)# vtp mode transparent
SW3(config)# vtp mode client      ! revision artık 0
```

### Voice VLAN (IP telefon)

IP telefon ve arkasındaki PC aynı kabloyu paylaşır: ses trafiği voice VLAN'da etiketli, PC trafiği access VLAN'da etiketsiz gider.

```
SW1(config)# vlan 150
SW1(config-vlan)# name SES
SW1(config)# interface fa0/5
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan 10
SW1(config-if)# switchport voice vlan 150
SW1(config-if)# mls qos trust cos
```

PT triki: IP Phone 7960 güçsüz gelir. Physical sekmesinde güç adaptörünü telefonun güç girişine sürükle, yoksa telefon hiç açılmaz.

### Bu bölümün trikleri

- Bir VLAN'ı silersen (`no vlan 10`) o VLAN'daki portlar hiçbir VLAN'a düşmez, inactive olur ve trafik geçirmez. Önce portları taşı, sonra VLAN'ı sil.
- `show vlan brief`'te bir port görünmüyorsa ya trunk'tur ya da silinmiş bir VLAN'a atanmıştır.
- Aynı VLAN'daki iki PC ping atamıyorsa sırayla bak: port doğru VLAN'da mı, trunk VLAN'ı taşıyor mu (allowed list), iki PC aynı subnet'te mi.
- VLAN isimleri büyük/küçük harfe duyarlı değildir ama lab kontrolünde (Check Results) birebir aranır; görevde ne yazıyorsa onu yaz.

## 6. Switch güvenliği: port-security, DHCP snooping, BPDU guard

Switch güvenliğinin üç temel kuralı: kullanılmayan portu kapat, PC portuna sadece bilinen MAC'i al, PC portundan gelen switch/DHCP sunucusu taklidini engelle. Aşağıdaki komutlar bu üç kuralı uygular.

### Kullanılmayan portlar

```
SW1(config)# interface range fa0/21 - 24, g0/2
SW1(config-if-range)# switchport mode access
SW1(config-if-range)# switchport access vlan 999
SW1(config-if-range)# shutdown
SW1(config-if-range)# description KULLANILMIYOR
```

### Port-security

```
SW1(config)# interface fa0/1
SW1(config-if)# switchport mode access
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security maximum 2
SW1(config-if)# switchport port-security mac-address sticky
SW1(config-if)# switchport port-security violation restrict
SW1(config-if)# switchport port-security aging time 10
```

Port `dynamic auto` modundayken port-security açılamaz: "Command rejected: FastEthernet0/1 is a dynamic port." Önce `switchport mode access` yaz.

| Violation modu | İhlal eden çerçeve | Log/SNMP mesajı | Violation sayacı | Port kapanır mı |
| --- | --- | --- | --- | --- |
| `protect` | Düşürülür | Yok | Artmaz | Hayır |
| `restrict` | Düşürülür | Var | Artar | Hayır |
| `shutdown` (default) | Düşürülür | Var | Artar | Evet, err-disabled |

MAC öğrenme yöntemleri:

- Statik: `switchport port-security mac-address 0001.4321.ABCD` ile elle yazılır.
- Dinamik: ilk görülen MAC'ler alınır, reload'da unutulur.
- Sticky: dinamik öğrenilir ve running-config'e yazılır. `copy run start` yaparsan kalıcı olur.

```
SW1# show port-security
SW1# show port-security interface fa0/1
SW1# show port-security address
SW1# show interfaces status err-disabled
```

Err-disabled portu geri açmak:

```
SW1(config)# interface fa0/1
SW1(config-if)# shutdown
SW1(config-if)# no shutdown
!
! veya otomatik kurtarma:
SW1(config)# errdisable recovery cause psecure-violation
SW1(config)# errdisable recovery interval 300
```

PT triki: port-security testini yeni PC eklemeden yapabilirsin. PC > Config > FastEthernet0 altında MAC Address kutusunu değiştir, ping at; ihlal oluşur.

### PortFast ve BPDU Guard

PortFast, PC portlarını STP'nin 30 saniyelik bekleme aşamalarını atlayıp hemen forwarding yapar. BPDU Guard ise o porttan BPDU (yani bir switch) gelirse portu err-disabled yapar.

```
SW1(config)# interface range fa0/1 - 20
SW1(config-if-range)# spanning-tree portfast
SW1(config-if-range)# spanning-tree bpduguard enable
!
! ya da tüm access portlar için global:
SW1(config)# spanning-tree portfast default
SW1(config)# spanning-tree portfast bpduguard default
```

PortFast'i asla switch'e giden porta açma; loop oluşabilir. Root Guard ise dağıtım switch'inin aşağı bakan portlarına yazılır ve oradan daha iyi bir root iddiası gelirse portu bloklar: `spanning-tree guard root`.

### DHCP Snooping

DHCP snooping sahte DHCP sunucularını engeller: sadece trusted portlardan DHCP Offer/Ack kabul edilir. Varsayılan olarak tüm portlar untrusted olur.

```
SW1(config)# ip dhcp snooping
SW1(config)# ip dhcp snooping vlan 10,20
SW1(config)# no ip dhcp snooping information option
SW1(config)# interface g0/1
SW1(config-if)# ip dhcp snooping trust          ! router/DHCP sunucusuna giden uplink
SW1(config-if)# exit
SW1(config)# interface range fa0/1 - 20
SW1(config-if-range)# ip dhcp snooping limit rate 10
!
SW1# show ip dhcp snooping
SW1# show ip dhcp snooping binding
```

Trik: `no ip dhcp snooping information option` yazmazsan switch DHCP isteğine Option 82 ekler ve bazı IOS DHCP sunucuları bu isteği reddeder. Snooping'i açınca PC'ler IP alamıyorsa ilk bakacağın yer burası.

### Dynamic ARP Inspection (DAI)

DAI sahte ARP cevaplarını (ARP spoofing, man-in-the-middle) engeller. DHCP snooping binding tablosunu kullanır, bu yüzden önce snooping açık olmalı.

```
SW1(config)# ip arp inspection vlan 10,20
SW1(config)# ip arp inspection validate src-mac dst-mac ip
SW1(config)# interface g0/1
SW1(config-if)# ip arp inspection trust
!
SW1# show ip arp inspection
```

Statik IP'li bir cihaz binding tablosunda olmadığı için DAI tarafından engellenir. Sunucu ve yazıcı gibi statik IP'liler için ARP ACL gerekir ya da o portu trust yaparsın.

### Diğer sertleştirme adımları

- PC portlarında `switchport nonegotiate` ile DTP'yi kapat.
- Native VLAN'ı kullanılmayan bir VLAN'a taşı (Bölüm 5).
- Uç portlarda `no cdp enable` ile topoloji bilgisinin sızmasını engelle.
- `no ip http server` ve `no ip http secure-server` ile web arayüzünü kapat.
- Telnet yerine sadece SSH: `transport input ssh`.
- VTY erişimini ACL ile yönetim ağına kısıtla (Bölüm 10, `access-class`).

## 7. STP, Rapid-PVST+ ve EtherChannel

STP, yedekli kabloların yarattığı döngüleri (broadcast fırtınası) bazı portları bloklayarak önler. EtherChannel ise birden fazla kabloyu tek mantıksal bağlantı yapar; STP onu tek port gördüğü için hiçbir kablo bloklanmaz.

### STP seçimi dört adımda

1. Root bridge: en düşük Bridge ID'ye (priority + MAC) sahip switch. Priority eşitse en düşük MAC kazanır.
2. Root port: root olmayan her switch'te root'a en düşük toplam maliyetle giden port.
   - Eşitlikte sırayla: komşunun düşük Bridge ID'si, komşunun düşük port priority'si, komşunun düşük port numarası.
3. Designated port: her kablo segmentinde root'a en yakın taraftaki port. Root bridge'in tüm portları designated'dır.
4. Geri kalan portlar: blocking (RSTP'de alternate veya backup).

| Durum (802.1D / PVST+) | Süre | Veri iletir mi | MAC öğrenir mi |
| --- | --- | --- | --- |
| Blocking | Max Age 20 sn'ye kadar | Hayır | Hayır |
| Listening | 15 sn | Hayır | Hayır |
| Learning | 15 sn | Hayır | Evet |
| Forwarding | - | Evet | Evet |

Rapid-PVST+ (802.1w) bu durumları discarding, learning, forwarding olarak üçe indirir ve saniyeler içinde toparlanır. Port rolleri: root, designated, alternate (root portun yedeği), backup.

### STP komutları

```
DSW1(config)# spanning-tree mode rapid-pvst
DSW1(config)# spanning-tree vlan 10,99 root primary      ! priority 24576 yapar
DSW1(config)# spanning-tree vlan 20 root secondary       ! priority 28672 yapar
!
! ya da elle (4096'nın katı olmalı: 0, 4096, 8192 ... 61440):
DSW2(config)# spanning-tree vlan 20 priority 4096
!
! port seviyesinde yol seçimini etkilemek:
SW1(config-if)# spanning-tree cost 10
SW1(config-if)# spanning-tree vlan 10 port-priority 64    ! 16'nın katı, default 128
```

```
SW1# show spanning-tree
SW1# show spanning-tree vlan 10
SW1# show spanning-tree summary
SW1# show spanning-tree interface g0/1 detail
```

`show spanning-tree vlan 10` çıktısında "This bridge is the root" satırı o switch'in root olduğunu söyler. Port tablosunda Role (Root/Desg/Altn) ve Sts (FWD/BLK) sütunlarına bak.

Yük paylaşımı triki: iki dağıtım switch'in varsa DSW1'i VLAN 10 için, DSW2'yi VLAN 20 için root yap. Böylece her iki uplink de bir VLAN için aktif çalışır, biri boşta beklemez.

Trik: root bridge'i mutlaka elle seç. Seçmezsen en düşük MAC kazanır; bu genelde en eski ve en yavaş switch'tir.

### EtherChannel modları

| Protokol | Mod | Karşı uç | Kanal kurulur mu |
| --- | --- | --- | --- |
| LACP (802.3ad, standart) | active | active veya passive | Evet |
| LACP | passive | passive | Hayır |
| PAgP (Cisco) | desirable | desirable veya auto | Evet |
| PAgP | auto | auto | Hayır |
| Statik | on | sadece on | Evet |

Karma kullanılmaz: LACP bir uç, PAgP diğer uç çalışmaz; `on` sadece `on` ile çalışır.

### Layer 2 EtherChannel kurulumu (LACP)

```
SW1(config)# interface range g0/1 - 2
SW1(config-if-range)# shutdown
SW1(config-if-range)# channel-group 1 mode active
Creating a port-channel interface Port-channel 1
SW1(config-if-range)# no shutdown
SW1(config-if-range)# exit
SW1(config)# interface port-channel 1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 99
SW1(config-if)# switchport trunk allowed vlan 10,20,99
```

Aynısını SW2'de `mode active` veya `mode passive` ile yap. Trunk ayarlarını Port-channel arayüzüne yaz; üye portlara otomatik uygulanır.

```
SW1# show etherchannel summary
Flags:  D - down        P - bundled in port-channel
        I - stand-alone s - suspended
        S - Layer2      R - Layer3   U - in use

Group  Port-channel  Protocol    Ports
------+-------------+-----------+----------------------
1      Po1(SU)       LACP        Gi0/1(P) Gi0/2(P)
```

Sağlıklı kanal: `Po1(SU)` ve tüm portlar `(P)`. `(s)` veya `(I)` görürsen üye portların ayarları farklıdır ya da mod eşleşmesi yanlıştır.

### Layer 3 EtherChannel (L3 switch'ler arası)

```
DSW1(config)# interface range g1/0/1 - 2
DSW1(config-if-range)# no switchport
DSW1(config-if-range)# channel-group 2 mode active
DSW1(config)# interface port-channel 2
DSW1(config-if)# no switchport
DSW1(config-if)# ip address 10.1.1.1 255.255.255.252
```

### Bu bölümün trikleri

- Üye portların hızı, duplex'i, modu (access/trunk), native VLAN'ı ve allowed listesi birebir aynı olmalı. Tek fark kanalı bozar.
- Kurarken portları `shutdown` yap, bitince aç. Yarım konfigürasyonla açık kalan portlar geçici loop veya err-disable yaratabilir.
- Yük dağıtım algoritmasını görmek için `show etherchannel load-balance`, değiştirmek için `port-channel load-balance src-dst-ip`.
- PT'de EtherChannel yeşillenmesi biraz sürer; Fast Forward Time'a birkaç kez bas.

## 8. Inter-VLAN routing: router-on-a-stick ve L3 switch

Farklı VLAN'lar arası trafik için bir Layer 3 cihaz gerekir. Küçük lablarda tek router portu ve alt arayüzler (router-on-a-stick), büyük ağlarda L3 switch üzerinde SVI kullanılır.

| Yöntem | Gerekenler | Artısı | Eksisi |
| --- | --- | --- | --- |
| Legacy (her VLAN'a bir router portu) | VLAN sayısı kadar router portu | Basit | Port sayısı hızla biter |
| Router-on-a-stick | 1 router portu + trunk + alt arayüzler | Ucuz, CCNA klasiği | Tüm VLAN'lar arası trafik tek kablodan geçer, darboğaz |
| L3 switch SVI | 3560/3650 + `ip routing` | Donanım hızında, ölçeklenir | Daha pahalı cihaz |

### Router-on-a-stick

Adresleme: VLAN 10 = 192.168.10.0/24, VLAN 20 = 192.168.20.0/24, VLAN 99 (yönetim/native) = 192.168.99.0/24. SW1 Gi0/1 router'ın G0/0 portuna bağlı.

```
R1(config)# interface g0/0
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface g0/0.10
R1(config-subif)# description VLAN10-SATIS
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0
R1(config-subif)# exit
R1(config)# interface g0/0.20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip address 192.168.20.1 255.255.255.0
R1(config-subif)# exit
R1(config)# interface g0/0.99
R1(config-subif)# encapsulation dot1Q 99 native
R1(config-subif)# ip address 192.168.99.1 255.255.255.0
```

Switch tarafı:

```
SW1(config)# interface g0/1
SW1(config-if)# switchport mode trunk
SW1(config-if)# switchport trunk native vlan 99
SW1(config-if)# switchport trunk allowed vlan 10,20,99
```

PC ayarları: VLAN 10'daki PC için IP 192.168.10.10/24, default gateway 192.168.10.1. VLAN 20'deki PC için gateway 192.168.20.1.

```
R1# show ip interface brief       ! G0/0.10, G0/0.20 up/up
R1# show vlans                    ! hangi alt arayüz hangi VLAN
R1# show ip route connected
```

### L3 switch ile (SVI)

```
DSW1(config)# ip routing
DSW1(config)# vlan 10
DSW1(config-vlan)# vlan 20
DSW1(config-vlan)# exit
DSW1(config)# interface vlan 10
DSW1(config-if)# ip address 192.168.10.1 255.255.255.0
DSW1(config-if)# no shutdown
DSW1(config-if)# interface vlan 20
DSW1(config-if)# ip address 192.168.20.1 255.255.255.0
DSW1(config-if)# no shutdown
DSW1(config-if)# exit
!
! erişim switch'ine giden trunk
DSW1(config)# interface g0/1
DSW1(config-if)# switchport trunk encapsulation dot1q
DSW1(config-if)# switchport mode trunk
!
! router'a giden routed port (switch portu L3'e çevrilir)
DSW1(config)# interface g0/2
DSW1(config-if)# no switchport
DSW1(config-if)# ip address 10.0.0.1 255.255.255.252
DSW1(config-if)# exit
DSW1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

### DHCP sunucusu başka ağdaysa: ip helper-address

DHCP Discover broadcast'tir ve router'dan geçmez. PC'nin gateway'i olan arayüze helper ekle:

```
R1(config)# interface g0/0.10
R1(config-subif)# ip helper-address 192.168.50.5
!
DSW1(config)# interface vlan 20
DSW1(config-if)# ip helper-address 192.168.50.5
```

### Inter-VLAN çalışmıyorsa kontrol sırası

1. PC'nin IP'si, maskesi ve gateway'i doğru mu? (`ipconfig` ile bak.)
2. PC kendi gateway'ine ping atabiliyor mu?
3. Switch portu doğru VLAN'da mı? (`show vlan brief`)
4. Router'a/L3 switch'e giden port trunk mı ve VLAN'lar allowed listesinde mi? (`show interfaces trunk`)
5. Router alt arayüzlerinde `encapsulation dot1Q` numarası VLAN numarasıyla aynı mı?
6. Router'ın fiziksel portu `no shutdown` mu?
7. L3 switch'te `ip routing` yazılı mı? (`show ip route` çıktısı "Default gateway is..." diyorsa routing kapalıdır.)

### Bu bölümün trikleri

- Alt arayüzde önce `encapsulation dot1Q`, sonra `ip address` yazılır. Tersini yaparsan "Configuring IP routing on a LAN subinterface is only allowed if that subinterface is already configured as part of an IEEE 802.10, IEEE 802.1Q, or ISL vLAN" hatasını alırsın.
- Alt arayüz numarası (`.10`) ile VLAN numarasının aynı olması zorunlu değil ama okunurluk için hep aynı yap.
- Router DTP konuşmaz; switch'teki port `dynamic auto` kalırsa trunk olmaz. Router'a bakan portta mutlaka `switchport mode trunk` yaz.
- L3 switch'te `ip routing` unutulursa her VLAN kendi gateway'ine ping atar, ama VLAN'lar birbirine ulaşamaz. En sık yapılan hata budur.
- SVI'ın up/up olması için VLAN veritabanında o VLAN'ın olması ve VLAN'da en az bir aktif portun olması gerekir.

## 9. Router temelleri ve yönlendirme: static, RIP, OSPF, EIGRP

Router sadece routing tablosunda olan ağlara paket gönderir. Tabloya girmenin üç yolu vardır: doğrudan bağlı ağ (C), elle yazılan static route (S) ve dinamik protokol (R, O, D).

### Routing tablosunu okumak

```
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, O - OSPF,
       D - EIGRP, EX - EIGRP external, IA - OSPF inter area,
       E2 - OSPF external type 2, * - candidate default

Gateway of last resort is 10.0.0.2 to network 0.0.0.0

     192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.10.0/24 is directly connected, GigabitEthernet0/0
L       192.168.10.1/32 is directly connected, GigabitEthernet0/0
O    192.168.30.0/24 [110/65] via 10.0.0.2, 00:01:23, Serial0/0/0
S*   0.0.0.0/0 [1/0] via 10.0.0.2
```

`[110/65]` = `[Administrative Distance / Metric]`. `via 10.0.0.2` sonraki hop, `Serial0/0/0` çıkış portu. "Gateway of last resort is not set" görürsen default route yoktur.

### Static route çeşitleri

```
! next-hop ile (en yaygın)
R1(config)# ip route 192.168.30.0 255.255.255.0 10.0.0.2
! çıkış portu ile (point-to-point seri hatlarda)
R1(config)# ip route 192.168.30.0 255.255.255.0 s0/0/0
! tam belirtilmiş (Ethernet'te en güvenlisi)
R1(config)# ip route 192.168.30.0 255.255.255.0 g0/1 10.0.0.2
! default route (internete çıkış)
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1
! floating static: AD 130, OSPF (110) düşerse devreye girer
R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.1.2 130
! host route: tek bir cihaza
R1(config)# ip route 192.168.30.50 255.255.255.255 10.0.0.2
! özet route: 192.168.0.0 - 192.168.3.255 tek satırda
R1(config)# ip route 192.168.0.0 255.255.252.0 10.0.0.2
```

Static route iki yönlü düşünülmelidir: R1'e R3'ün LAN'ını öğretip R3'e R1'in LAN'ını öğretmezsen ping gider ama cevap dönmez.

### Protokolleri karşılaştırma

| Özellik | RIPv2 | OSPF | EIGRP |
| --- | --- | --- | --- |
| Tür | Distance vector | Link state | Advanced distance vector (Cisco, artık açık) |
| AD | 120 | 110 | 90 (internal), 170 (external) |
| Metrik | Hop sayısı (max 15) | Cost = referans bant genişliği / port bant genişliği | Bant genişliği + gecikme |
| Güncelleme | 30 sn'de bir tüm tablo | Değişiklikte LSA | Değişiklikte kısmi |
| Multicast adresi | 224.0.0.9 | 224.0.0.5 / 224.0.0.6 | 224.0.0.10 |
| Ne zaman | Küçük lab, ilk öğrenme | CCNA'nın ana protokolü | Cisco ağlar, hızlı yakınsama |

### RIPv2

```
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network 192.168.10.0
R1(config-router)# network 10.0.0.0
R1(config-router)# passive-interface g0/0
R1(config-router)# default-information originate
!
R1# show ip protocols
R1# show ip route rip
R1# debug ip rip          ! kapatmak için: undebug all
```

RIP'te `network` komutu classful yazılır ve maske almaz; `network 10.0.0.0` 10.x.x.x'teki tüm portları RIP'e sokar.

### OSPF (tek alan)

```
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 192.168.10.0 0.0.0.255 area 0
R1(config-router)# network 10.0.0.0 0.0.0.3 area 0
R1(config-router)# passive-interface g0/0
R1(config-router)# auto-cost reference-bandwidth 1000
R1(config-router)# default-information originate
!
! alternatif: network yerine arayüz üzerinden
R1(config)# interface g0/0
R1(config-if)# ip ospf 1 area 0
R1(config-if)# ip ospf cost 10
R1(config-if)# ip ospf priority 0        ! DR/BDR seçimine katılmaz
```

| Subnet maskesi | Prefix | Wildcard |
| --- | --- | --- |
| 255.255.255.0 | /24 | 0.0.0.255 |
| 255.255.255.128 | /25 | 0.0.0.127 |
| 255.255.255.192 | /26 | 0.0.0.63 |
| 255.255.255.224 | /27 | 0.0.0.31 |
| 255.255.255.240 | /28 | 0.0.0.15 |
| 255.255.255.248 | /29 | 0.0.0.7 |
| 255.255.255.252 | /30 | 0.0.0.3 |
| 255.255.255.255 | /32 | 0.0.0.0 (tek host) |
| 255.255.0.0 | /16 | 0.0.255.255 |

Wildcard formülü: 255.255.255.255 eksi subnet maskesi. Örnek: 255.255.255.255 - 255.255.255.240 = 0.0.0.15.

OSPF komşuluğu kurulması için iki uçta aynı olması gerekenler:

- Area numarası
- Subnet ve maske
- Hello ve Dead süreleri
- Kimlik doğrulama ayarı
- MTU
- Router ID'ler ise farklı olmalı; port `passive-interface` olmamalı.

Router ID seçim sırası: elle yazılan `router-id` > en yüksek loopback IP > en yüksek aktif fiziksel port IP. DR/BDR seçimi: en yüksek priority, eşitse en yüksek Router ID. Seçim preemptive değildir; sonradan gelen daha iyi router DR'ı elinden almaz.

```
R1# show ip ospf neighbor          ! FULL/DR, FULL/BDR, 2WAY/DROTHER
R1# show ip ospf interface brief   ! cost, durum, komşu sayısı
R1# show ip ospf
R1# show ip ospf database
R1# show ip route ospf
R1# clear ip ospf process          ! router-id değişikliği için şart
```

Cost triki: default referans 100 Mbps olduğu için FastEthernet ve GigabitEthernet ikisi de cost 1 alır; OSPF hızlı ve yavaş yolu ayıramaz. `auto-cost reference-bandwidth 1000`'i ağdaki tüm router'lara yaz.

### EIGRP

```
R1(config)# router eigrp 100
R1(config-router)# eigrp router-id 1.1.1.1
R1(config-router)# network 192.168.10.0 0.0.0.255
R1(config-router)# network 10.0.0.0 0.0.0.3
R1(config-router)# no auto-summary
R1(config-router)# passive-interface g0/0
R1(config-router)# redistribute static     ! default route'u dağıtmak için
!
R1# show ip eigrp neighbors
R1# show ip eigrp topology       ! successor ve feasible successor
R1# show ip route eigrp
```

EIGRP'te AS numarası (100) tüm router'larda aynı olmalı, OSPF'teki process ID ise sadece yerel anlamlıdır ve farklı olabilir. Successor en iyi yoldur; feasible successor topoloji tablosunda hazır bekleyen yedektir, ana yol düşünce anında devreye girer.

### Bu bölümün trikleri

- Test için LAN kurmana gerek yok: `interface loopback 0` + `ip address 172.16.1.1 255.255.255.0` her zaman up olan sanal bir ağ verir.
- Seri bağlantıda DCE ucunu bulmak için `show controllers serial 0/0/0` yaz; çıktıda "DCE V.35, clock rate 64000" ya da "DTE" yazar.
- `passive-interface` LAN'a bakan portlarda routing güncellemelerini keser ama o ağ yine de duyurulur. Router'a bakan porta yazarsan komşuluk kurulmaz.
- Default route'u tüm ağa yaymak: RIP ve OSPF'te `default-information originate`, EIGRP'te `redistribute static`. Önce o router'da gerçek bir `ip route 0.0.0.0 0.0.0.0 ...` olmalı.
- `debug` komutları PT'de bile ekranı doldurur. İşin bitince `undebug all` (kısaca `u all`).

## 10. Ağ servisleri: DHCP, NAT/PAT, ACL, NTP, Syslog, CDP/LLDP, HSRP

Bu servislerin hepsi router'da birkaç satırla kurulur ve CCNA lablarının puanının büyük kısmını oluşturur. Her birinin sonunda doğrulama komutu var; kurduktan sonra mutlaka çalıştır.

### DHCP sunucusu (router üzerinde)

```
R1(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10
R1(config)# ip dhcp pool LAN10
R1(dhcp-config)# network 192.168.10.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.10.1
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# domain-name lab.local
R1(dhcp-config)# lease 7
!
R1# show ip dhcp binding          ! kime hangi IP verildi
R1# show ip dhcp pool
R1# show ip dhcp conflict
```

PC tarafı: Desktop > IP Configuration > DHCP seç. "DHCP request successful" görürsün. Command Prompt'ta `ipconfig /renew` ve `ipconfig /release` de çalışır.

Trikler:

- `excluded-address` havuzdan önce yazılır; gateway, sunucu ve yazıcı IP'lerini dağıtılmaktan korur.
- PC 169.254.x.x (APIPA) adresi alıyorsa DHCP sunucusuna ulaşamıyordur. VLAN, trunk ve `ip helper-address`'i kontrol et.
- PT'de Server-PT de DHCP verebilir: Services > DHCP, Service On, havuzu doldur, Add'e bas. Gateway ve DNS alanlarını unutma.
- Router'ı DHCP istemcisi yapmak (ISP'den IP almak): `interface g0/1` + `ip address dhcp`.

### NAT ve PAT

| Tür | Ne yapar | Komut |
| --- | --- | --- |
| Static NAT | Bir iç IP'yi her zaman aynı dış IP'ye eşler (web sunucusu) | `ip nat inside source static 192.168.10.50 203.0.113.10` |
| Dynamic NAT | İç IP'lere havuzdan dış IP verir | `ip nat inside source list 1 pool HAVUZ` |
| PAT (overload) | Tüm iç IP'ler tek dış IP'yi port numarasıyla paylaşır | `ip nat inside source list 1 interface g0/1 overload` |

PAT (en çok kullanılan):

```
R1(config)# access-list 1 permit 192.168.10.0 0.0.0.255
R1(config)# access-list 1 permit 192.168.20.0 0.0.0.255
R1(config)# interface g0/0
R1(config-if)# ip nat inside
R1(config-if)# interface g0/1
R1(config-if)# ip nat outside
R1(config-if)# exit
R1(config)# ip nat inside source list 1 interface g0/1 overload
```

Dynamic NAT havuzla:

```
R1(config)# ip nat pool HAVUZ 203.0.113.20 203.0.113.30 netmask 255.255.255.224
R1(config)# ip nat inside source list 1 pool HAVUZ
```

```
R1# show ip nat translations
R1# show ip nat statistics
R1# clear ip nat translation *
```

Trik: `show ip nat translations` boşsa önce içeriden dışarıya ping at, tablo trafikle dolar. Inside/outside portları ters işaretlemek en sık NAT hatasıdır. Router-on-a-stick'te her alt arayüze ayrı ayrı `ip nat inside` yazılır.

### ACL (erişim listeleri)

ACL satırları yukarıdan aşağı okunur, ilk eşleşen satır uygulanır ve en sonda görünmez bir `deny any` vardır. Bir porta yön başına (in/out) tek ACL uygulanabilir.

Yerleştirme kuralı:

- Standard ACL (sadece kaynak IP'ye bakar) hedefe en yakın yere konur.
- Extended ACL (kaynak, hedef, protokol, port) kaynağa en yakın yere konur.

Standard, numaralı:

```
! 192.168.20.0 ağı sunucu ağına erişemesin, diğerleri erişsin
R2(config)# access-list 10 deny 192.168.20.0 0.0.0.255
R2(config)# access-list 10 permit any
R2(config)# interface g0/0                 ! sunucu ağına bakan port
R2(config-if)# ip access-group 10 out
```

Extended, isimli:

```
! VLAN 10 sadece web (80/443) ve DNS kullanabilsin, ping atabilsin
R1(config)# ip access-list extended VLAN10-KURAL
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq 80
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq 443
R1(config-ext-nacl)# permit udp 192.168.10.0 0.0.0.255 any eq 53
R1(config-ext-nacl)# permit icmp 192.168.10.0 0.0.0.255 any
R1(config-ext-nacl)# deny ip any any log
R1(config-ext-nacl)# exit
R1(config)# interface g0/0.10
R1(config-subif)# ip access-group VLAN10-KURAL in
```

VTY erişimini kısıtlamak (`access-group` değil `access-class`):

```
R1(config)# access-list 5 permit 192.168.99.0 0.0.0.255
R1(config)# line vty 0 4
R1(config-line)# access-class 5 in
```

Tek bir satırı düzeltmek (isimli veya numaralı ACL'de sıra numarasıyla):

```
R1# show access-lists
Extended IP access list VLAN10-KURAL
    10 permit tcp 192.168.10.0 0.0.0.255 any eq www
    20 permit tcp 192.168.10.0 0.0.0.255 any eq 443
...
R1(config)# ip access-list extended VLAN10-KURAL
R1(config-ext-nacl)# no 20
R1(config-ext-nacl)# 15 permit tcp 192.168.10.0 0.0.0.255 any eq 22
```

Kestirmeler: `host 192.168.1.5` = `192.168.1.5 0.0.0.0`, `any` = `0.0.0.0 255.255.255.255`.

### NTP, Syslog ve zaman

```
R1# clock set 14:30:00 12 Oct 2026
R1(config)# clock timezone TRT 3
R1(config)# ntp server 192.168.50.10
R1(config)# service timestamps log datetime msec
R1(config)# logging host 192.168.50.10
R1(config)# logging trap warnings
!
R1# show clock
R1# show ntp status
R1# show ntp associations
R1# show logging
```

PT'de Server-PT > Services > NTP ve SYSLOG servislerini aç; Syslog sekmesinde router'dan gelen logları görürsün.

| Syslog seviyesi | Ad | Örnek |
| --- | --- | --- |
| 0 | Emergencies | Sistem kullanılamaz |
| 1 | Alerts | Hemen müdahale |
| 2 | Critical | Kritik durum |
| 3 | Errors | Hata |
| 4 | Warnings | Uyarı |
| 5 | Notifications | Port up/down (`%LINK-5-CHANGED`) |
| 6 | Informational | Bilgi |
| 7 | Debugging | Debug çıktıları |

Hatırlatma cümlesi: "Every Awesome Cisco Engineer Will Need Ice cream Daily". `logging trap 4` seçilen seviye ve daha küçük numaralı (daha ciddi) tüm mesajları gönderir.

### CDP ve LLDP

```
SW1# show cdp neighbors             ! komşu cihaz, port, platform
SW1# show cdp neighbors detail      ! komşunun IP'si ve IOS sürümü
SW1(config)# no cdp run             ! tüm cihazda kapat
SW1(config-if)# no cdp enable       ! tek portta kapat
!
SW1(config)# lldp run               ! standart protokol, default kapalı
SW1# show lldp neighbors
```

Trik: Topolojisi verilmemiş bir labda `show cdp neighbors detail` ile hangi portun hangi cihaza bağlı olduğunu ve komşunun IP'sini çıkarırsın.

### HSRP (yedekli gateway)

İki router aynı sanal IP'yi paylaşır; PC'ler gateway olarak sanal IP'yi kullanır. Aktif router düşerse standby devralır.

```
R1(config)# interface g0/0
R1(config-if)# ip address 192.168.10.2 255.255.255.0
R1(config-if)# standby version 2
R1(config-if)# standby 1 ip 192.168.10.1
R1(config-if)# standby 1 priority 110
R1(config-if)# standby 1 preempt
!
R2(config)# interface g0/0
R2(config-if)# ip address 192.168.10.3 255.255.255.0
R2(config-if)# standby version 2
R2(config-if)# standby 1 ip 192.168.10.1
!
R1# show standby brief
```

PC'lerin gateway'i 192.168.10.1 (sanal) olur. `preempt` yazmazsan R1 düşüp geri geldiğinde aktif rolü geri almaz.

## 11. Show komutları ve sorun giderme

Sorun giderirken aşağıdan yukarı git: önce kablo ve port, sonra VLAN/trunk, sonra IP ve routing, en son ACL/NAT. Her adımda tek bir show komutu cevabı verir.

### En çok kullanılan show komutları

| Komut | Ne gösterir | Ne zaman |
| --- | --- | --- |
| `show ip interface brief` | Portlar, IP'ler, Status/Protocol | Her şeyden önce |
| `show interfaces status` | Switch portları: VLAN, duplex, hız, err-disabled | Switch'te port sorunu |
| `show interfaces g0/1` | Hata sayacı, CRC, collision, duplex, MTU | Yavaşlık, duplex uyumsuzluğu |
| `show running-config` | O anki tüm ayarlar | Ne yaptığını görmek |
| `show vlan brief` | VLAN'lar ve access portları | VLAN sorunu |
| `show interfaces trunk` | Trunk portlar, native VLAN, allowed VLAN'lar | VLAN diğer switch'e geçmiyorsa |
| `show mac address-table` | Hangi MAC hangi portta | Bir PC'yi fiziksel olarak bulmak |
| `show ip arp` / PC'de `arp -a` | IP-MAC eşleşmeleri | Aynı ağda ping sorunu |
| `show ip route` | Routing tablosu | Başka ağa ulaşılamıyorsa |
| `show ip protocols` | Çalışan protokoller, network'ler, passive portlar | Routing protokolü sorunu |
| `show cdp neighbors detail` | Komşu cihazlar ve IP'leri | Topolojiyi çıkarmak |
| `show spanning-tree` | Root, port rolleri, blocking portlar | Turuncu ışık, loop |
| `show version` | IOS sürümü, uptime, config register, portlar | Cihazı tanımak |
| `show access-lists` | ACL satırları ve eşleşme sayıları | Trafik engelleniyorsa |
| `show ip nat translations` | NAT tablosu | İnternete çıkılamıyorsa |
| `show history` | Son yazılan komutlar | Ne yazdığını hatırlamak |

### Status / Protocol ne anlatır

| Status | Protocol | Anlamı | Çözüm |
| --- | --- | --- | --- |
| up | up | Çalışıyor | - |
| administratively down | down | `shutdown` durumda | `no shutdown` |
| down | down | Kablo yok, yanlış kablo veya karşı uç kapalı | Kabloyu ve karşı portu kontrol et |
| up | down | Fiziksel tamam, Layer 2 sorunlu | Seri hatta clock rate veya kapsülleme; SVI'da VLAN'da aktif port yok |
| err-disabled | down | Port-security, BPDU guard veya EtherChannel hatası | Sebebi düzelt, `shutdown` + `no shutdown` |

### Ping çıktısını okumak

| Sembol | Anlamı |
| --- | --- |
| `!` | Cevap geldi |
| `.` | Zaman aşımı, cevap yok |
| `U` | Hedef ulaşılamaz (bir router "yolum yok" dedi) |
| `Q` | Kaynak çok yoğun |
| `M` | Fragment gerekli ama DF biti set |
| `?` | Bilinmeyen paket |
| `&` | TTL süresi doldu |

Trik: ilk ping'te `.!!!!` (%80) görmek normaldir. İlk paket ARP çözümlemesi beklerken düşer; ikinci ping'te `!!!!!` olmalı.

Gelişmiş ping, kaynağı seçmeni sağlar. Router'ın LAN portundan ping atmak, dönüş rotasını da test eder:

```
R1# ping 192.168.30.10 source g0/0
R1# ping 192.168.30.10 repeat 100
R1# traceroute 192.168.30.10
C:\> tracert 192.168.30.10          ! PC'de
```

### Aşağıdan yukarı sorun giderme sırası

1. Fiziksel: Işıklar yeşil mi? Doğru kablo mu? `show ip interface brief`'te port up mi?
2. Veri bağlantı: Port doğru VLAN'da mı (`show vlan brief`)? Trunk VLAN'ı taşıyor mu (`show interfaces trunk`)? Port err-disabled mı?
3. Ağ: PC'nin IP/maske/gateway'i doğru mu (`ipconfig`)? PC gateway'e ping atabiliyor mu? Router'da hedef ağın route'u var mı (`show ip route`)? Dönüş route'u var mı?
4. Routing protokolü: Komşuluk kurulmuş mu (`show ip ospf neighbor`)? Network komutları doğru mu (`show ip protocols`)?
5. Filtreleme: ACL engelliyor mu (`show access-lists`, eşleşme sayacı artıyor mu)? NAT inside/outside doğru mu?
6. Uygulama: DNS çalışıyor mu (PC'de `nslookup`)? Sunucuda servis açık mı?

### Debug komutları

```
R1# debug ip icmp                 ! ping paketlerini gör
R1# debug ip ospf adj             ! OSPF komşuluk adımları
R1# debug ip dhcp server events   ! DHCP dağıtımı
R1# debug ip nat                  ! NAT çevirileri
R1# show debugging                ! açık debug'lar
R1# undebug all                   ! hepsini kapat
R1# terminal monitor              ! SSH/telnet oturumunda log görmek için
```

### Packet Tracer'ın kendi araçları

- Simulation modu: paketi Play ile adım adım ilerlet, zarfa tıklayıp her OSI katmanında ne olduğunu oku. Paket bir cihazda kırmızı X ile düşüyorsa sebebi orada yazar.
- Inspect aracı (I tuşu): switch'e tıkla, MAC tablosunu ve port durumunu pencerede gör.
- Simple PDU (P tuşu): iki cihaza tıklayarak ping gönder; sonucu sağ alttaki listede Successful/Failed olarak gör. Birden fazla testi kaydedip tek tıkla tekrar çalıştırabilirsin.
- Activity dosyalarında Check Results: hangi madde eksik, yüzde kaç tamamlandı gösterir.

## 12. Adım adım örnek projeler

Sekiz proje, her biri bir öncekinin üzerine kurulur: tek switch'ten başlayıp OSPF, NAT ve L3 kampüs ağına kadar gider. Her projeyi bitirince ayrı bir `.pkt` dosyası olarak kaydet; bir sonraki projeye o dosyadan başla.

| # | Proje | Öğrettikleri | Zorluk |
| --- | --- | --- | --- |
| 1 | İlk ağ | Kablolama, PC IP'si, switch temel ayarı, console, SSH | Kolay |
| 2 | İki switch, iki VLAN | VLAN, access, trunk, native VLAN | Kolay |
| 3 | Router-on-a-stick + DHCP | Alt arayüz, inter-VLAN, DHCP havuzu | Orta |
| 4 | Üç router, static route | Seri hat, clock rate, static ve default route | Orta |
| 5 | OSPF + internet çıkışı | OSPF, default-information, PAT | Orta |
| 6 | L3 switch kampüsü | SVI, VTP, EtherChannel, Rapid-PVST, DHCP relay | Zor |
| 7 | Kampüsü güvenli hale getir | SSH, ACL, port-security, DHCP snooping | Zor |
| 8 | Final: küçük şirket ağı | Hepsi, çözümsüz | Zor |

### Proje 1: İlk ağ (1 switch, 3 PC)

Topoloji: PC1 - Fa0/1, PC2 - Fa0/2, PC3 - Fa0/3 portlarına bağlı, tek 2960 switch (SW1). Ayrıca PC1'in RS 232 portundan SW1'in Console portuna console kablosu.

| Cihaz | IP | Maske | Gateway |
| --- | --- | --- | --- |
| PC1 | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |
| PC2 | 192.168.1.11 | 255.255.255.0 | 192.168.1.1 |
| PC3 | 192.168.1.12 | 255.255.255.0 | 192.168.1.1 |
| SW1 (VLAN 1) | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |

Adımlar:

1. Bir 2960 ve üç PC yerleştir, Copper Straight-Through ile bağla. Işıkların turuncudan yeşile dönmesini bekle.
2. Her PC'de Desktop > IP Configuration > Static seç ve tablodaki IP'leri gir.
3. PC1 > Desktop > Command Prompt: `ping 192.168.1.11`.
4. PC1 > Desktop > Terminal > OK ile switch'e console'dan bağlan ve aşağıdaki ayarları yaz.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW1
SW1(config)# no ip domain-lookup
SW1(config)# enable secret class123
SW1(config)# line console 0
SW1(config-line)# password cisco123
SW1(config-line)# login
SW1(config-line)# logging synchronous
SW1(config-line)# exit
SW1(config)# interface vlan 1
SW1(config-if)# ip address 192.168.1.2 255.255.255.0
SW1(config-if)# no shutdown
SW1(config-if)# exit
SW1(config)# ip default-gateway 192.168.1.1
SW1(config)# ip domain-name lab.local
SW1(config)# username admin privilege 15 secret Admin123
SW1(config)# crypto key generate rsa general-keys modulus 1024
SW1(config)# ip ssh version 2
SW1(config)# line vty 0 15
SW1(config-line)# login local
SW1(config-line)# transport input ssh
SW1(config-line)# end
SW1# copy running-config startup-config
```

Test:

- PC2'den `ping 192.168.1.2` (switch'e ping) başarılı olmalı.
- PC2'den `ssh -l admin 192.168.1.2` ile bağlan.
- SW1'de `show mac address-table`: üç PC'nin MAC'ini ve portlarını gör.

Meydan okuma: PC3'ün IP'sini 192.168.2.12 yap ve PC1'den ping at. Neden olmuyor? (Farklı subnet; aralarında router yok.)

### Proje 2: İki switch, iki VLAN, trunk

Topoloji: SW1 ve SW2, Gi0/1 portlarından Copper Cross-Over ile bağlı. Her switch'te Fa0/1'de bir VLAN 10 PC'si, Fa0/2'de bir VLAN 20 PC'si.

| Cihaz | Switch / Port | VLAN | IP |
| --- | --- | --- | --- |
| PC1 | SW1 Fa0/1 | 10 | 192.168.10.11/24 |
| PC2 | SW1 Fa0/2 | 20 | 192.168.20.11/24 |
| PC3 | SW2 Fa0/1 | 10 | 192.168.10.12/24 |
| PC4 | SW2 Fa0/2 | 20 | 192.168.20.12/24 |
| SW1 | VLAN 99 | 99 | 192.168.99.11/24 |
| SW2 | VLAN 99 | 99 | 192.168.99.12/24 |

SW1 (SW2'de aynısını hostname ve IP değiştirerek yaz):

```
enable
configure terminal
hostname SW1
vlan 10
 name SATIS
vlan 20
 name MUHASEBE
vlan 99
 name MGMT
exit
interface fa0/1
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
interface fa0/2
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
interface g0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,99
 switchport nonegotiate
interface vlan 99
 ip address 192.168.99.11 255.255.255.0
 no shutdown
end
copy running-config startup-config
```

Test:

- PC1 -> PC3 ping başarılı (aynı VLAN, trunk üzerinden).
- PC2 -> PC4 ping başarılı.
- PC1 -> PC2 ping başarısız. Bu doğru sonuç: farklı VLAN, router yok.
- `show interfaces trunk` ve `show vlan brief` çıktılarını incele.
- Simulation modunda PC1'den PC3'e ping at, trunk üzerindeki çerçeveyi aç: 802.1Q etiketinde VLAN 10 yazdığını gör.

Meydan okuma: SW2'de `switchport trunk allowed vlan 10,99` yaz (`add` olmadan). PC2-PC4 neden koptu?

### Proje 3: Router-on-a-stick + DHCP

Proje 2'nin üzerine bir 2911 router (R1) ekle: R1 G0/0 - SW1 G0/2 (straight-through). PC'leri DHCP'ye al.

SW1'e ek:

```
interface g0/2
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,99
```

R1:

```
enable
configure terminal
hostname R1
no ip domain-lookup
interface g0/0
 no shutdown
interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
interface g0/0.99
 encapsulation dot1Q 99 native
 ip address 192.168.99.1 255.255.255.0
exit
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8
end
copy running-config startup-config
```

Switch'lerde `ip default-gateway 192.168.99.1` yaz. PC'lerde IP Configuration > DHCP seç.

Test:

- Her PC 192.168.x.11 ve sonrasını almalı. R1'de `show ip dhcp binding`.
- PC1 -> PC2 ping artık başarılı (farklı VLAN, router üzerinden).
- PC1'de `tracert 192.168.20.12`: ilk hop 192.168.10.1 olmalı.

Meydan okuma: R1'de `interface g0/0` altında `shutdown` yaz. Neler koptu, neden? (Fiziksel port kapanınca tüm alt arayüzler kapanır.)

### Proje 4: Üç router, seri hat, static route

Topoloji: R1 - R2 - R3 zinciri, araları seri kablo. Her router'ın G0/0 portunda bir LAN (bir 2960 + bir PC, ya da PC router'a doğrudan cross-over ile).

Önce her 1941'e seri port ekle: Physical sekmesi > gücü kapat > HWIC-2T'yi boş yuvaya sürükle > gücü aç.

| Cihaz | Port | IP | Not |
| --- | --- | --- | --- |
| R1 | G0/0 | 192.168.1.1/24 | PC1: 192.168.1.10 |
| R1 | S0/0/0 | 10.0.12.1/30 | DCE |
| R2 | S0/0/0 | 10.0.12.2/30 | DTE |
| R2 | G0/0 | 192.168.2.1/24 | PC2: 192.168.2.10 |
| R2 | S0/0/1 | 10.0.23.1/30 | DCE |
| R3 | S0/0/0 | 10.0.23.2/30 | DTE |
| R3 | G0/0 | 192.168.3.1/24 | PC3: 192.168.3.10 |

PT triki: Serial DCE kablosunda ilk tıkladığın cihaz DCE ucu olur. R1'den R2'ye çekerken önce R1'e tıkla.

R1 (stub router, dışarıya tek yolu var, default route yeterli):

```
hostname R1
interface g0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
interface s0/0/0
 ip address 10.0.12.1 255.255.255.252
 clock rate 64000
 no shutdown
exit
ip route 0.0.0.0 0.0.0.0 10.0.12.2
```

R2 (ortadaki router, iki yönü de bilmeli):

```
hostname R2
interface g0/0
 ip address 192.168.2.1 255.255.255.0
 no shutdown
interface s0/0/0
 ip address 10.0.12.2 255.255.255.252
 no shutdown
interface s0/0/1
 ip address 10.0.23.1 255.255.255.252
 clock rate 64000
 no shutdown
exit
ip route 192.168.1.0 255.255.255.0 10.0.12.1
ip route 192.168.3.0 255.255.255.0 10.0.23.2
```

R3:

```
hostname R3
interface g0/0
 ip address 192.168.3.1 255.255.255.0
 no shutdown
interface s0/0/0
 ip address 10.0.23.2 255.255.255.252
 no shutdown
exit
ip route 0.0.0.0 0.0.0.0 10.0.23.1
```

Test:

- PC1'den `tracert 192.168.3.10`: sırasıyla 192.168.1.1, 10.0.12.2, 10.0.23.2, 192.168.3.10.
- R2'de `show ip route static`: iki S satırı. R1'de `S* 0.0.0.0/0`.
- R1'de `show controllers serial 0/0/0`: DCE ve clock rate 64000 yazmalı.

Meydan okuma: R2'de `no ip route 192.168.3.0 255.255.255.0 10.0.23.2` yaz. PC1'den PC3'e ping at: "Reply from 10.0.12.2: Destination host unreachable" kimden ve neden geliyor?

### Proje 5: OSPF ve internet çıkışı (PAT)

Proje 4'ü al, static route'ları sil, OSPF kur ve R1'in arkasına "internet" ekle.

Yeni cihazlar: ISP router (1941), R1 G0/1 - ISP G0/1 cross-over. ISP G0/0'a bir Server-PT (İNTERNET-SUNUCU).

| Cihaz | Port | IP |
| --- | --- | --- |
| R1 | G0/1 | 203.0.113.2/30 |
| ISP | G0/1 | 203.0.113.1/30 |
| ISP | G0/0 | 8.8.8.1/24 |
| İNTERNET-SUNUCU | Fa0 | 8.8.8.8/24, gateway 8.8.8.1 |

Static route'ları kaldır:

```
R1(config)# no ip route 0.0.0.0 0.0.0.0 10.0.12.2
R2(config)# no ip route 192.168.1.0 255.255.255.0 10.0.12.1
R2(config)# no ip route 192.168.3.0 255.255.255.0 10.0.23.2
R3(config)# no ip route 0.0.0.0 0.0.0.0 10.0.23.1
```

ISP (özel IP ağlarına route'u yoktur, gerçek internet gibi):

```
hostname ISP
interface g0/1
 ip address 203.0.113.1 255.255.255.252
 no shutdown
interface g0/0
 ip address 8.8.8.1 255.255.255.0
 no shutdown
```

R1:

```
interface g0/1
 ip address 203.0.113.2 255.255.255.252
 ip nat outside
 no shutdown
interface g0/0
 ip nat inside
interface s0/0/0
 ip nat inside
exit
ip route 0.0.0.0 0.0.0.0 203.0.113.1
router ospf 1
 router-id 1.1.1.1
 network 192.168.1.0 0.0.0.255 area 0
 network 10.0.12.0 0.0.0.3 area 0
 passive-interface g0/0
 default-information originate
exit
access-list 1 permit 192.168.0.0 0.0.255.255
ip nat inside source list 1 interface g0/1 overload
```

R2:

```
router ospf 1
 router-id 2.2.2.2
 network 192.168.2.0 0.0.0.255 area 0
 network 10.0.12.0 0.0.0.3 area 0
 network 10.0.23.0 0.0.0.3 area 0
 passive-interface g0/0
```

R3:

```
router ospf 1
 router-id 3.3.3.3
 network 192.168.3.0 0.0.0.255 area 0
 network 10.0.23.0 0.0.0.3 area 0
 passive-interface g0/0
```

Sunucu: Services > HTTP açık; Services > DNS aç, `www.ornek.com` için A kaydı 8.8.8.8 ekle. PC'lerin DNS Server alanına 8.8.8.8 yaz.

Test:

- R2'de `show ip ospf neighbor`: R1 ve R3 FULL durumda. Seri hatta DR seçimi olmadığı için "FULL/ -" görünür.
- R3'te `show ip route`: `O*E2 0.0.0.0/0` satırı default route'un OSPF ile geldiğini gösterir.
- PC3'ten `ping 8.8.8.8` ve Web Browser'da `www.ornek.com`.
- R1'de `show ip nat translations`: 192.168.3.10 -> 203.0.113.2 çevirisi.

Trik: R2 ve R3 LAN'larından gelen paketler R1'e S0/0/0'dan girer. O porta `ip nat inside` yazmazsan sadece R1'in kendi LAN'ı internete çıkar.

Meydan okuma: R2'den `ping 8.8.8.8` neden başarısız, ama PC2'den başarılı? (R2'nin kaynağı 10.0.12.2; ACL 1'de olmadığı için NAT'lanmaz ve ISP o adrese dönüş yolunu bilmez.)

### Proje 6: L3 switch kampüsü

Bir dağıtım switch'i (DSW1, 3560-24PS) iki erişim switch'ini (ASW1, ASW2, 2960) LACP EtherChannel ile bağlar. DSW1 VLAN'lar arası yönlendirme yapar, DHCP'yi sunucu verir.

Kablolama (hepsi Copper Cross-Over):

- ASW1 Fa0/23-24 <-> DSW1 Fa0/1-2 (Port-channel 1)
- ASW2 Fa0/23-24 <-> DSW1 Fa0/3-4 (Port-channel 2)
- SRV1 (Server-PT) -> DSW1 Fa0/10 (straight-through)
- Her ASW'de: Fa0/1'e VLAN 10 PC'si, Fa0/11'e VLAN 20 PC'si

```text
                  +-------------------------+   Fa0/10   +------------------+
                  |   DSW1 (3560-24PS)      |------------| SRV1 (Server-PT) |
                  | ip routing, SVI gateway |            | 192.168.30.10    |
                  |  her VLAN'ın .1 adresi  |            | DHCP, VLAN 30    |
                  +-------------------------+            +------------------+
                     ||                 ||
     Po1, LACP       ||                 ||  Po2, LACP
     DSW1 Fa0/1-2    ||                 ||  DSW1 Fa0/3-4
     ASW1 Fa0/23-24  ||                 ||  ASW2 Fa0/23-24
                     ||                 ||
         +--------------------+   +--------------------+
         |    ASW1 (2960)     |   |    ASW2 (2960)     |
         | MGMT 192.168.99.11 |   | MGMT 192.168.99.12 |
         +--------------------+   +--------------------+
          |             |            |             |
     +---------+   +---------+  +---------+   +---------+
     | VLAN 10 |   | VLAN 20 |  | VLAN 10 |   | VLAN 20 |
     | PC      |   | PC      |  | PC      |   | PC      |
     | Fa0/1   |   | Fa0/11  |  | Fa0/1   |   | Fa0/11  |
     +---------+   +---------+  +---------+   +---------+
```

`||` çift çizgileri EtherChannel'daki iki kabloyu gösterir; farklı VLAN'daki PC'ler arasındaki tüm trafik DSW1 üzerinden geçer.

| VLAN | Ad | Ağ | Gateway (DSW1 SVI) |
| --- | --- | --- | --- |
| 10 | OGRENCI | 192.168.10.0/24 | 192.168.10.1 |
| 20 | PERSONEL | 192.168.20.0/24 | 192.168.20.1 |
| 30 | SUNUCU | 192.168.30.0/24 | 192.168.30.1 (SRV1: .10) |
| 99 | MGMT | 192.168.99.0/24 | 192.168.99.1 (ASW1: .11, ASW2: .12) |
| 999 | KULLANILMAYAN | - | - |

DSW1:

```
hostname DSW1
vtp domain KAMPUS
vtp mode server
vtp password Kampus123
vlan 10
 name OGRENCI
vlan 20
 name PERSONEL
vlan 30
 name SUNUCU
vlan 99
 name MGMT
vlan 999
 name KULLANILMAYAN
exit
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30,99 root primary
!
interface range fa0/1 - 4
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
interface range fa0/1 - 2
 channel-group 1 mode active
interface range fa0/3 - 4
 channel-group 2 mode active
interface fa0/10
 switchport mode access
 switchport access vlan 30
 spanning-tree portfast
exit
!
ip routing
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 ip helper-address 192.168.30.10
 no shutdown
interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 ip helper-address 192.168.30.10
 no shutdown
interface vlan 30
 ip address 192.168.30.1 255.255.255.0
 no shutdown
interface vlan 99
 ip address 192.168.99.1 255.255.255.0
 no shutdown
end
copy running-config startup-config
```

ASW1 (ASW2'de aynısı, IP 192.168.99.12):

```
hostname ASW1
vtp domain KAMPUS
vtp mode client
vtp password Kampus123
spanning-tree mode rapid-pvst
interface range fa0/23 - 24
 switchport mode trunk
 switchport trunk native vlan 99
 channel-group 1 mode active
end
! VTP'nin VLAN'ları getirdiğini kontrol et:
show vlan brief
configure terminal
interface range fa0/1 - 10
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 spanning-tree bpduguard enable
interface range fa0/11 - 20
 switchport mode access
 switchport access vlan 20
 spanning-tree portfast
 spanning-tree bpduguard enable
interface range fa0/21 - 22, g0/1 - 2
 switchport mode access
 switchport access vlan 999
 shutdown
interface vlan 99
 ip address 192.168.99.11 255.255.255.0
 no shutdown
exit
ip default-gateway 192.168.99.1
end
copy running-config startup-config
```

SRV1: IP 192.168.30.10/24, gateway 192.168.30.1. Services > DHCP > Service On, iki havuz ekle:

| Pool Name | Default Gateway | DNS Server | Start IP | Subnet Mask | Max Users |
| --- | --- | --- | --- | --- | --- |
| VLAN10 | 192.168.10.1 | 192.168.30.10 | 192.168.10.11 | 255.255.255.0 | 100 |
| VLAN20 | 192.168.20.1 | 192.168.30.10 | 192.168.20.11 | 255.255.255.0 | 100 |

Her havuzu doldurduktan sonra Add'e bas; Save'e basarsan seçili havuzun üzerine yazarsın.

Test:

- DSW1'de `show etherchannel summary`: `Po1(SU)` ve `Po2(SU)`, tüm portlar `(P)`.
- ASW1'de `show vtp status`: revision DSW1 ile aynı.
- DSW1'de `show spanning-tree vlan 10`: "This bridge is the root".
- PC'ler DHCP ile IP alır. ASW1'deki VLAN 10 PC'si ASW2'deki VLAN 20 PC'sine ping atabilir.
- DSW1'de `show ip route`: dört VLAN ağı da C olarak görünür.

Meydan okuma: ASW1-DSW1 arasındaki iki kablodan birini sil. Ping kesiliyor mu? (Hayır; EtherChannel tek kabloyla devam eder.)

### Proje 7: Kampüsü güvenli hale getir

Proje 6'nın üzerine. ASW1 Fa0/21'i aç, VLAN 99'a al ve oraya bir ADMIN-PC bağla (192.168.99.50/24, gateway 192.168.99.1).

1. Üç switch'e de Bölüm 4'teki SSH şablonunu uygula. VTY'yi sadece yönetim ağına aç:

```
access-list 5 permit 192.168.99.0 0.0.0.255
line vty 0 15
 access-class 5 in
```

2. ASW'lerde erişim portlarına port-security:

```
interface range fa0/1 - 20
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict
```

3. ASW'lerde DHCP snooping (uplink Port-channel trusted):

```
ip dhcp snooping
ip dhcp snooping vlan 10,20
no ip dhcp snooping information option
interface port-channel 1
 ip dhcp snooping trust
```

4. DSW1'de öğrenciler personel ve yönetim ağına erişemesin, sunucuya erişsin:

```
ip access-list extended OGRENCI-KURAL
 deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
 deny ip 192.168.10.0 0.0.0.255 192.168.99.0 0.0.0.255
 permit ip any any
interface vlan 10
 ip access-group OGRENCI-KURAL in
```

Test:

- VLAN 10 PC'si -> VLAN 20 PC'si: başarısız. VLAN 10 PC'si -> 192.168.30.10: başarılı.
- DSW1'de `show access-lists`: deny satırının eşleşme sayacı artıyor.
- ADMIN-PC'den `ssh -l admin 192.168.99.11` başarılı; VLAN 20 PC'sinden başarısız.
- Bir PC'nin MAC'ini Config sekmesinden değiştir, ping at; `show port-security interface fa0/1` violation sayacı artar.

Meydan okuma: Personel (VLAN 20) öğrenciye (VLAN 10) ping atabiliyor mu? Neden hayır? (Ping gider ama öğrencinin cevabı VLAN 10'a girişte ACL'e takılır. Çözüm için ilk satıra `permit icmp 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255 echo-reply` ekle.)

### Proje 8: Final, küçük şirket ağı (çözümsüz)

Bu projede çözüm yok; her maddeyi bitirdikçe işaretle. Merkez binada bir ISR4331 (R-MERKEZ), bir 3650 (DSW, güç kaynağını takmayı unutma) ve iki 2960 var. Şubede bir 1941 (R-SUBE) ve bir 2960 var. Bir ISP router'ı ve arkasında 8.8.8.8 sunucusu internet görevi görür.

- [ ] 172.16.0.0/16 bloğunu VLSM ile böl: her VLAN için bir /24, her router arası bağlantı için bir /30. Planı Note aracıyla topolojiye yaz.
- [ ] Merkez VLAN'ları: 10 IK, 20 MUHASEBE, 30 IT, 40 MISAFIR, 50 SUNUCU, 99 MGMT, 999 KULLANILMAYAN. Şube: 10 ve 20.
- [ ] Tüm cihazlarda hostname, enable secret, console şifresi, banner, SSH v2, telnet kapalı.
- [ ] Trunk'larda native VLAN 99, allowed VLAN listesi, `nonegotiate`; kullanılmayan portlar VLAN 999'da ve kapalı.
- [ ] DSW ile iki erişim switch'i arasında LACP EtherChannel trunk.
- [ ] Rapid-PVST; DSW tüm VLAN'lar için root.
- [ ] Merkezde inter-VLAN routing DSW'de (SVI), şubede R-SUBE'de router-on-a-stick.
- [ ] DSW ile R-MERKEZ arası routed port (`no switchport`).
- [ ] OSPF area 0: DSW, R-MERKEZ, R-SUBE. LAN portları passive, `auto-cost reference-bandwidth 1000`, elle router-id.
- [ ] R-MERKEZ'de ISP'ye default route, `default-information originate`, 172.16.0.0/16 için PAT.
- [ ] DHCP: merkez VLAN'ları sunucudan (`ip helper-address`), şube VLAN'ları R-SUBE'den.
- [ ] MISAFIR VLAN'ı sadece internete çıksın, 172.16.0.0/16'ya erişemesin.
- [ ] Erişim portlarında port-security ve PortFast + BPDU guard; erişim switch'lerinde DHCP snooping.
- [ ] Sunucuda DNS (`www.ornek.com`), NTP ve Syslog; tüm router'lar NTP ve Syslog'u sunucuya bağlasın.
- [ ] Her PC `www.ornek.com` sayfasını açabilsin.
- [ ] Tüm cihazlarda `copy running-config startup-config`.

İpuçları:

- Katman katman ilerle ve her maddeden sonra test et; on maddeyi yazıp sonra test edersen hatayı bulamazsın.
- Dosyayı `final_v1.pkt`, `final_v2.pkt` diye sürümle; bir şey bozulursa önceki sürüme dönersin.
- MISAFIR ACL'inde DHCP (UDP 67) ve DNS (UDP 53) trafiğine `deny 172.16.0.0` satırından önce izin vermeyi unutma; yoksa misafirler IP alamaz ve isim çözemez.

## 13. Trikler, sık yapılan hatalar ve Packet Tracer tuzakları

Lablarda kaybedilen zamanın çoğu aynı 15-20 hatadan gelir. Aşağıdaki tabloyu belirtiye göre tara; sebep ve çözüm yanında.

### Belirti -> sebep -> çözüm

| Belirti | Muhtemel sebep | Çözüm |
| --- | --- | --- |
| Router portu kırmızı | Router portları default kapalı | `no shutdown` |
| Kablo takılı ama kırmızı | Yanlış kablo türü (ör. PC-Router straight) | Cross-over / straight kuralını uygula |
| Yeni bağlanan port 30 sn turuncu | STP listening/learning | Bekle, Fast Forward Time veya PC portlarına PortFast |
| Aynı VLAN, farklı switch'teki PC'ler konuşamıyor | Switch'ler arası trunk yok veya VLAN allowed listede yok | `show interfaces trunk` |
| PC kendi gateway'ine ping atamıyor | Port yanlış VLAN'da, gateway IP'si yanlış ya da alt arayüzde dot1Q numarası yanlış | `show vlan brief`, `ipconfig`, `show run int g0/0.10` |
| Gateway'e ping var, uzak ağa yok | Routing tablosunda hedef yok veya dönüş rotası yok | Her iki uçtaki router'da `show ip route` |
| VLAN'lar arası ping yok (L3 switch) | `ip routing` yazılmamış | `ip routing` |
| "Translating..." bekletiyor | Yanlış komut hostname sanıldı | Ctrl+Shift+6, kalıcı: `no ip domain-lookup` |
| Log mesajları yazdığın komutu bölüyor | `logging synchronous` yok | Console ve VTY'ye `logging synchronous` |
| Reload sonrası ayarlar gitti | Kaydedilmedi | `copy running-config startup-config` |
| Seri hat up/down | DCE ucunda clock rate yok | `clock rate 64000` (DCE ucunda) |
| OSPF komşusu yok | Area, subnet, hello/dead uyumsuz veya port passive | `show ip ospf interface`, `show ip protocols` |
| SSH bağlanmıyor | Hostname/domain/RSA eksik, `login local` ama user yok, `transport input` yanlış, VTY ACL | `show ip ssh`, `show run \| section line vty` |
| Port err-disabled | Port-security ihlali veya BPDU guard | `show interfaces status err-disabled`, sebebi düzelt, shut/no shut |
| PC 169.254.x.x aldı | DHCP sunucusuna ulaşılamıyor | VLAN, trunk, `ip helper-address`, DHCP snooping trust |
| NAT çalışmıyor | inside/outside ters, ACL iç ağı kapsamıyor, default route yok | `show ip nat statistics`, `show access-lists` |
| EtherChannel portları `(s)` veya `(I)` | Üye portların ayarları farklı ya da mod uyumsuz | Üyelere `default interface`, aynı ayarlarla yeniden kur |
| 3560'ta trunk komutu reddediliyor | Kapsülleme auto | Önce `switchport trunk encapsulation dot1q` |
| Native VLAN mismatch uyarısı | İki uçta farklı native VLAN | İki uçta aynı `switchport trunk native vlan` |
| Bağlantı çok yavaş, late collision | Duplex uyumsuzluğu (bir uç full, diğeri half) | İki ucu da auto veya ikisini de aynı değere sabitle |
| Komut `Invalid input` | Yanlış mod ya da PT bu komutu desteklemiyor | Prompt'a bak, `?` ile kontrol et |

### Packet Tracer'a özgü tuzaklar

- PT gerçek IOS'un bir simülasyonudur ve her komutu desteklemez. `?` listesinde bir komut yoksa hata sende değil, PT'dedir. Aynı işi yapan başka bir komut veya farklı bir cihaz modeli dene.
- Yeni bağlanan kablolarda ilk ping'ler STP ve ARP yüzünden düşebilir. Paniklemeden önce ikinci kez dene.
- Cihazı kapatıp açarsan (modül takarken) kaydedilmemiş running-config silinir. Modül takmadan önce `copy run start` yap.
- 3650 switch ve IP telefon güç modülü olmadan gelir; Physical sekmesinde tak.
- Serial DCE kablosunda ilk tıklanan cihaz DCE olur.
- Server-PT DHCP'sinde havuz ekledikten sonra Save değil Add'e bas; Save seçili havuzu ezer.
- PC'nin Desktop > IP Configuration ekranında Static/DHCP seçimini değiştirince eski IP kaybolur; DHCP'ye geçip geri dönersen yeniden yazman gerekir.
- Activity (`.pka`) dosyalarında Check Results harf harfine bakar: hostname'de `R1` yerine `r1` yazmak puan kaybettirir.
- PT bazen çöker. Ctrl+S'e sık bas ve önemli aşamalarda dosyayı yeni sürüm adıyla kaydet.

### Hız kazandıran trikler

- Konfigürasyonu Not Defteri'nde hazırla, sonra CLI'a yapıştır. Aynı ayarı beş switch'e saniyeler içinde verirsin; hostname ve IP'yi değiştirmen yeter.
- `show running-config interface g0/0` sadece o portun ayarlarını gösterir; uzun config'te kaybolmazsın.
- `show run | section router ospf`, `show run | include ip route` gibi süzgeçlerle ilgili kısmı anında bul.
- Config modundan çıkmadan kontrol: `do show ip int br`, `do show vlan br`, `do wr`.
- `interface range` ile 24 portu tek seferde ayarla; virgülle farklı aralıkları birleştir: `int range fa0/1 - 5, fa0/10 - 12, g0/1`.
- Bir portu tertemiz yapmak için tek tek `no` yazmak yerine `default interface fa0/5`.
- Topolojinin yanına Note ile IP planını yaz; hata ayıklarken tablo aramazsın.
- Simple PDU testlerini kaydet; her değişiklikten sonra tüm testleri tek tıkla tekrar çalıştır.
- Gerçek cihazda uzaktan riskli bir değişiklik yapacaksan önce `reload in 10` yaz. Kendini kilitlersen cihaz 10 dakika sonra kaydedilmiş ayarla açılır; sorun yoksa `reload cancel`.

### Subnetting kısayolu

Blok boyutu = 256 eksi maskenin son anlamlı okteti. Ağlar bu blok boyutunun katlarında başlar. Kullanılabilir host = 2^(host bit sayısı) - 2.

| Prefix | Maske | Blok boyutu | Kullanılabilir host | Ağ başlangıçları (son oktet) |
| --- | --- | --- | --- | --- |
| /24 | 255.255.255.0 | 256 | 254 | 0 |
| /25 | 255.255.255.128 | 128 | 126 | 0, 128 |
| /26 | 255.255.255.192 | 64 | 62 | 0, 64, 128, 192 |
| /27 | 255.255.255.224 | 32 | 30 | 0, 32, 64, 96, ... |
| /28 | 255.255.255.240 | 16 | 14 | 0, 16, 32, 48, ... |
| /29 | 255.255.255.248 | 8 | 6 | 0, 8, 16, 24, ... |
| /30 | 255.255.255.252 | 4 | 2 | 0, 4, 8, 12, ... (router arası linkler) |

Örnek: 192.168.1.77/27 hangi ağda? Blok 32; 77, 64 ile 96 arasında. Ağ 192.168.1.64, broadcast 192.168.1.95, hostlar .65-.94.

## 14. Temel komutlar: hızlı başvuru

"Şunu yapmak istiyorum, komutu neydi?" sorusunun cevabı bu bölümde. Komutlar göreve göre gruplandı; `!` ile başlayan kısımlar açıklamadır, yazman gerekmez.

### Modlar arası gezinme

```
enable                       ! > modundan # moduna
configure terminal           ! # modundan (config)# moduna
interface fa0/1              ! port ayarına gir
exit                         ! bir seviye geri
end                          ! doğrudan # moduna (Ctrl+Z ile aynı)
disable                      ! # modundan > moduna
```

### Temel cihaz ayarları

```
hostname SW1                                 ! cihaza isim ver
no ip domain-lookup                          ! yanlış komutta DNS beklemesini kapat
enable secret class123                       ! # moduna geçiş şifresi
service password-encryption                  ! düz şifreleri gizle
banner motd #Yetkisiz giris yasaktir#        ! giriş mesajı
line console 0                               ! console şifresi
 password cisco123
 login
 logging synchronous
 exec-timeout 5 0
line vty 0 15                                ! uzaktan erişim (switch 0 15, router 0 4)
 password cisco123
 login
 transport input ssh
clock set 14:30:00 12 Oct 2026               ! saat (# modunda)
```

### Kaydetme, silme, sıfırlama

```
copy running-config startup-config    ! kaydet (kısaca: wr)
show startup-config                   ! kayıtlı ayarlar
erase startup-config                  ! kayıtlı ayarları sil
delete flash:vlan.dat                 ! VLAN veritabanını sil (switch)
reload                                ! yeniden başlat (Save? sorusuna: no)
```

### VLAN işlemleri

```
! VLAN oluştur ve isim ver
vlan 10
 name SATIS
exit

! birden fazla VLAN'ı tek satırda oluştur
vlan 10,20,30

! VLAN'ın adını değiştir
vlan 10
 name PAZARLAMA

! VLAN sil (önce portlarını başka VLAN'a taşı!)
no vlan 30

! tek portu VLAN'a ata
interface fa0/1
 switchport mode access
 switchport access vlan 10

! port aralığını VLAN'a ata
interface range fa0/1 - 10
 switchport mode access
 switchport access vlan 10

! portu tekrar VLAN 1'e al
interface fa0/1
 no switchport access vlan

! kontrol
show vlan brief
show vlan id 10
show interfaces fa0/1 switchport
```

### Trunk işlemleri

```
interface g0/1
 switchport trunk encapsulation dot1q     ! sadece 3560/3650 gibi L3 switch'lerde
 switchport mode trunk                    ! trunk yap
 switchport trunk native vlan 99          ! native VLAN değiştir
 switchport trunk allowed vlan 10,20,99   ! sadece bu VLAN'lar geçsin (listeyi ezer)
 switchport trunk allowed vlan add 30     ! listeye ekle
 switchport trunk allowed vlan remove 20  ! listeden çıkar
 switchport nonegotiate                   ! DTP'yi kapat

show interfaces trunk
```

### Port işlemleri

```
interface fa0/1
 description PC1-Muhasebe       ! açıklama
 shutdown                       ! kapat
 no shutdown                    ! aç
 speed 100                      ! hız sabitle (auto / 10 / 100)
 duplex full                    ! duplex sabitle (auto / half / full)
 spanning-tree portfast         ! PC portu: STP beklemesini atla

interface range fa0/1 - 24      ! aralık
interface range fa0/1 - 5, fa0/10 - 12, g0/1   ! karışık aralık
default interface fa0/5         ! portu fabrika ayarına döndür (config modunda)

show interfaces status
show interfaces fa0/1
```

### Switch'e yönetim IP'si verme

```
interface vlan 99
 ip address 192.168.99.11 255.255.255.0
 no shutdown
exit
ip default-gateway 192.168.99.1
```

### SSH açma

```
hostname SW1
ip domain-name lab.local
username admin privilege 15 secret Admin123
crypto key generate rsa general-keys modulus 1024
ip ssh version 2
line vty 0 15
 login local
 transport input ssh
```

### Port-security

```
interface fa0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict    ! protect / restrict / shutdown

show port-security interface fa0/1
```

### STP ve EtherChannel

```
spanning-tree mode rapid-pvst
spanning-tree vlan 10 root primary
spanning-tree vlan 10 priority 4096

interface range g0/1 - 2
 channel-group 1 mode active          ! LACP (passive / desirable / auto / on)
interface port-channel 1
 switchport mode trunk

show spanning-tree vlan 10
show etherchannel summary
```

### Router arayüzü ve routing

```
interface g0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface s0/0/0
 ip address 10.0.0.1 255.255.255.252
 clock rate 64000                     ! sadece DCE ucunda
 no shutdown

interface g0/0.10                      ! router-on-a-stick alt arayüzü
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

ip routing                             ! L3 switch'te routing'i aç
ip route 192.168.3.0 255.255.255.0 10.0.0.2   ! static route
ip route 0.0.0.0 0.0.0.0 10.0.0.2             ! default route

router ospf 1
 router-id 1.1.1.1
 network 192.168.1.0 0.0.0.255 area 0
 passive-interface g0/0

router eigrp 100
 network 192.168.1.0 0.0.0.255
 no auto-summary

router rip
 version 2
 no auto-summary
 network 192.168.1.0
```

### DHCP, NAT, ACL

```
! DHCP
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool LAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8

! DHCP relay (sunucu başka ağdaysa, gateway arayüzüne)
interface g0/0.10
 ip helper-address 192.168.50.5

! PAT
access-list 1 permit 192.168.0.0 0.0.255.255
interface g0/0
 ip nat inside
interface g0/1
 ip nat outside
ip nat inside source list 1 interface g0/1 overload

! Standard ACL
access-list 10 deny 192.168.20.0 0.0.0.255
access-list 10 permit any
interface g0/1
 ip access-group 10 out

! Extended isimli ACL
ip access-list extended WEB-IZIN
 permit tcp 192.168.10.0 0.0.0.255 any eq 80
 deny ip any any
interface g0/0
 ip access-group WEB-IZIN in
```

### En önemli 15 show komutu

```
show running-config
show ip interface brief
show interfaces status
show vlan brief
show interfaces trunk
show mac address-table
show ip route
show ip protocols
show ip ospf neighbor
show spanning-tree
show etherchannel summary
show port-security interface fa0/1
show ip dhcp binding
show ip nat translations
show cdp neighbors detail
```

### PC (Command Prompt) komutları

```
ipconfig                  ! IP, maske, gateway
ipconfig /all             ! MAC ve DNS dahil
ipconfig /release         ! DHCP IP'sini bırak
ipconfig /renew           ! DHCP'den yeni IP al
ping 192.168.1.1          ! erişilebilirlik
ping -t 192.168.1.1       ! durdurulana kadar ping (Ctrl+C)
tracert 8.8.8.8           ! paketin geçtiği router'lar
arp -a                    ! ARP tablosu
nslookup www.ornek.com    ! DNS çözümleme
ssh -l admin 192.168.1.2  ! SSH ile bağlan
telnet 192.168.1.2        ! Telnet ile bağlan
```
