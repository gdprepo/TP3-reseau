# TP3-reseau


# TP 3 - Plusieurs réseaux : routage statique
# I. Création et utilisation simples d'une VM CentOS
## 4. Configuration réseau d'une machine CentOS

-   _a._  Commande pour prouver que vous avez internet depuis la VM ( Un peu raccourcie ):
```
[root@localhost ~]# curl www.google.com
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="fr"><head><meta content="text/html; charset=UTF-8" http-equiv="Content-Type"><m#ccc;height:30px}.lsbb{display:block}.ftl,#fll a{display:inline-blocksaisie\x22,\x22psrc\x22:\x22Cette suggestion a bien t supprime de votre \\u003Ca href\x3d\\\x22/history\\\x22\\u003Ehistorique Web\\u003C/a\\u003E.\x22,\x22psrl\x22:\x22Supprimer\x22,\x22sbit\x22:\x22Recherche par image\x22,\x22srch\x22:\x22Recherche Google\x22},\x22ovr\x22:{},\x22pq\x22:\x22\x22,\x22refpd\x22:true,\x22rfs\x22:[],\x22sbpl\x22:24,\x22sbpr\x22:24,\x22scd\x22:10,\x22sce\x22:5,\x22stok\x22:\x22oi4N-z_nn7qQMJN_PPivto1S3F8\x22,\x22uhde\x22:false}}';google.pmc=JSON.parse(pmc);})();(function(){var r=['aa','async','ipv6','mu','sf'];google.plm(r);})();</script>     </body></html>[root@localhost ~]#
```
-   _b._  Prouvez que votre PC hôte et la VM peuvent communiquer:
Windows vers VM:
```
PS C:\Users\Gabin> ping 192.168.127.10

Envoi d’une requête 'Ping'  192.168.127.10 avec 32 octets de données :
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 192.168.127.10:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```
VM vers Windows:
```
[root@localhost ~]# ping 192.168.127.1
PING 192.168.127.1 (192.168.127.1) 56(84) bytes of data.
64 bytes from 192.168.127.1: icmp_seq=1 ttl=128 time=0.242 ms
64 bytes from 192.168.127.1: icmp_seq=2 ttl=128 time=0.281 ms
64 bytes from 192.168.127.1: icmp_seq=3 ttl=128 time=0.345 ms
64 bytes from 192.168.127.1: icmp_seq=4 ttl=128 time=0.352 ms
64 bytes from 192.168.127.1: icmp_seq=5 ttl=128 time=0.348 ms
64 bytes from 192.168.127.1: icmp_seq=6 ttl=128 time=0.386 ms
64 bytes from 192.168.127.1: icmp_seq=7 ttl=128 time=0.350 ms
^C
--- 192.168.127.1 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6002ms
rtt min/avg/max/mdev = 0.242/0.329/0.386/0.046 ms
[root@localhost ~]#
```
-   _c._ Table de routage  sur VM:
```
[root@localhost ~]# ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101
```

## 5. Faire joujou avec quelques commandes

###   ping
   * ping  hôte -> VM
```
PS C:\Users\Gabin> ping 192.168.127.10

Envoi d’une requête 'Ping'  192.168.127.10 avec 32 octets de données :
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 192.168.127.10:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```
   * ping  VM -> hôte
   ![](https://github.com/wewlr17/TP3-reseau/blob/master/image%20tp3/vm_hote.PNG)
-   afficher la table de routage
    -   de l'hôte
    ```
    PS C:\Users\Gabin> route print
    ===========================================================================
    Liste d'Interfaces
      4...54 e1 ad 4b 28 52 ......Intel(R) Ethernet Connection (4) I219-V
      2...0a 00 27 00 00 02 ......VirtualBox Host-Only Ethernet Adapter
     11...02 00 4c 4f 4f 50 ......Npcap Loopback Adapter
     12...0a 00 27 00 00 0c ......VirtualBox Host-Only Ethernet Adapter #2
      6...fa 59 71 8e 63 36 ......Microsoft Wi-Fi Direct Virtual Adapter
      7...f8 59 71 8e 63 37 ......Microsoft Wi-Fi Direct Virtual Adapter #2
     26...00 50 56 c0 00 01 ......VMware Virtual Ethernet Adapter for VMnet1
     14...00 50 56 c0 00 08 ......VMware Virtual Ethernet Adapter for VMnet8
     17...f8 59 71 8e 63 36 ......Intel(R) Dual Band Wireless-AC 8265
     25...f8 59 71 8e 63 3a ......Bluetooth Device (Personal Area Network)
      1...........................Software Loopback Interface 1
    ===========================================================================

    IPv4 Table de routage
    ===========================================================================
    Itinéraires actifs :
    Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
              0.0.0.0          0.0.0.0      10.33.3.253      10.33.0.216     35
            10.33.0.0    255.255.252.0         On-link       10.33.0.216    291
          10.33.0.216  255.255.255.255         On-link       10.33.0.216    291
          10.33.3.255  255.255.255.255         On-link       10.33.0.216    291
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
          169.254.0.0      255.255.0.0         On-link   169.254.226.150    281
      169.254.226.150  255.255.255.255         On-link   169.254.226.150    281
      169.254.255.255  255.255.255.255         On-link   169.254.226.150    281
         192.168.56.0    255.255.255.0         On-link      192.168.56.1    281
         192.168.56.1  255.255.255.255         On-link      192.168.56.1    281
       192.168.56.255  255.255.255.255         On-link      192.168.56.1    281
        192.168.127.0    255.255.255.0         On-link     192.168.127.1    281
        192.168.127.1  255.255.255.255         On-link     192.168.127.1    281
      192.168.127.255  255.255.255.255         On-link     192.168.127.1    281
        192.168.145.0    255.255.255.0         On-link     192.168.145.1    291
        192.168.145.1  255.255.255.255         On-link     192.168.145.1    291
      192.168.145.255  255.255.255.255         On-link     192.168.145.1    291
        192.168.222.0    255.255.255.0         On-link     192.168.222.1    291
        192.168.222.1  255.255.255.255         On-link     192.168.222.1    291
      192.168.222.255  255.255.255.255         On-link     192.168.222.1    291
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
            224.0.0.0        240.0.0.0         On-link      192.168.56.1    281
            224.0.0.0        240.0.0.0         On-link     192.168.127.1    281
            224.0.0.0        240.0.0.0         On-link       10.33.0.216    291
            224.0.0.0        240.0.0.0         On-link     192.168.145.1    291
            224.0.0.0        240.0.0.0         On-link     192.168.222.1    291
            224.0.0.0        240.0.0.0         On-link   169.254.226.150    281
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      255.255.255.255  255.255.255.255         On-link      192.168.56.1    281
      255.255.255.255  255.255.255.255         On-link     192.168.127.1    281
      255.255.255.255  255.255.255.255         On-link       10.33.0.216    291
      255.255.255.255  255.255.255.255         On-link     192.168.145.1    291
      255.255.255.255  255.255.255.255         On-link     192.168.222.1    291
      255.255.255.255  255.255.255.255         On-link   169.254.226.150    281
    ===========================================================================
    Itinéraires persistants :
      Aucun


    ```
    
    -   de la VM
    
    ```
    [gabin@localhost ~]$ ip route
    default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
    10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
    192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101    
    ```
    -   mettre en évidence la ligne qui leur permet de discuter (dans chacune des tables)
   **192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10 metric 101**
-   depuis la VM utilisez  `curl`  (ou  `wget`) pour télécharger un fichier sur internet
```
[gabin@localhost ~]$ curl -o google.com www.google.com
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 11342    0 11342    0     0   126k      0 --:--:-- --:--:-- --:--:--  127k
```
-   depuis la VM utilisez  `dig`  pour connaître l'IP de :
    -   `ynov.com`
    ```
    [gabin@localhost ~]$ dig ynov.com

    ; <<>> DiG 9.9.4-RedHat-9.9.4-72.el7 <<>> ynov.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32995
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 13, ADDITIONAL: 27

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4096
    ;; QUESTION SECTION:
    ;ynov.com.                      IN      A

    ;; ANSWER SECTION:
    ynov.com.               9527    IN      A       217.70.184.38
    ```
    
    -   `google.com`
    ```
    [gabin@localhost ~]$ dig google.com
    ; <<>> DiG 9.9.4-RedHat-9.9.4-72.el7 <<>> google.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 38862
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 13, ADDITIONAL: 27

    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 4096
    ;; QUESTION SECTION:
    ;google.com.                    IN      A

    ;; ANSWER SECTION:
    google.com.             28      IN      A       216.58.204.110

    ```
    
# II. Notion de ports et de SSH
## Exploration des ports locaux

- SS
TCP : 
```
[gabin@localhost ~]$ ss -t
State       Recv-Q Send-Q Local Address:Port                 Peer Address:Port               
ESTAB       0      36     192.168.127.10:blackjack            192.168.127.1:clvm-cfg         
```
Listening :
```
[gabin@localhost ~]$ ss -lt
State       Recv-Q Send-Q Local Address:Port                 Peer Address:Port               
LISTEN      0      100      127.0.0.1:smtp                           *:*
LISTEN      0      128              *:blackjack                      *:*
LISTEN      0      100            ::1:smtp                          :::*
LISTEN      0      128             :::blackjack                     :::*
```

ss -n : 
```
[gabin@localhost ~]$ ss -tn
State       Recv-Q Send-Q Local Address:Port                Peer Address:Port
ESTAB       0      36     192.168.127.10:22               192.168.127.1:1476
```

ss -p :
```
[root@localhost ~]# ss -pt
State       Recv-Q Send-Q Local Address:Port                 Peer Address:Port               
ESTAB       0      0      192.168.127.10:blackjack            192.168.127.1:bnetgame              users:(("sshd",pid=4015,fd=3))
```
Port 22 :
```
[root@localhost ~]# ss -ptn
State       Recv-Q Send-Q Local Address:Port                Peer Address:Port
ESTAB       0      36     192.168.127.10:22                  192.168.127.1:1119                users:(("sshd",pid=4015,fd=3))
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcwOTIyMjY1MiwtMTExMTYwMDg4Nyw4OD
YzMzQ3ODcsLTIwODg3NDY2MTIsNzMwOTk4MTE2XX0=
-->
