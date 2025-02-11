 ### Internet Protocol

* Protocole d'interconnexion de réseaux physiques
* Standard IETF
* Couche 3 (couche réseau) du modèle OSI
* 2 versions actuellement en activité
  * IP version 4
  * IP version 6
* Permet Internet: réseau mondial accessible à tous

* Noeud : équipement supportant IP
* Routeur : noeud transmettant les paquets dont il n'est pas le destinataire
* Hôte : noeud qui n'est pas un routeur
* Couche supérieure : protocole transporté par IP (TCP, UDP, ICMP…)
* Lien : protocole sous IP (Ethernet, WiFi, ATM…)
* Voisins : noeuds attaché au même lien
* Interface : moyen d'accès au lien d'un noeud
* Adresse : identifiant IP d'interface (ou d'un ensemble d'interface)
* Paquet : entête IP + charge utile

* Évidemment, on ne manipule pas directement les adresses en binaire…
Notation décimal pointée - RFC 1166
Chaque octet est converti en base 10 - 4 octets => 4 nombres
Séparés par des .
Ex : 
00000001 00000000 00000000 00000011 => 1.0.0.3
11000000 10101000 00000000 11111110 => 192.168.0.254

* Il existe des plages d'adresses IP réservées à des usages particuliers
Adresses réservées pour les réseaux privés RFC 1918
10.0.0.0/8
172.16.0.0/12 (172.16.0.0 - 172.31.255.255)
192.168.0.0/16
Adresses de bouclage (localhost) : 127.0.0.0/8
Adresse du réseau actuel (si inconnu) : 0.0.0.0/8
Adresses multicast : 224.0.0.0/4
Adresse de diffusion (locale !) sur réseau inconnu : 255.255.255.255/32

![](../Resources/Capture1.png)


![](../)




### IPv6

* ::1   => Boucle locale (loopback)
* ::    => Adresse indéfinie
* ff00::/8 => Adresses multicast
* fe80::/10 => Adresses unicast lien local
* fc00::/7 => Adresses unicast locales uniques (RFC 4193)
* reste  => Adresses unicast globales (Internet/publiques)

