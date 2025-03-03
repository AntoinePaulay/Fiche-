### Q.1.1 Le serveur SRVWIN01 a le rôle DHCP d'installé et correctement configuré et activé.
Quelles peuvent être les raisons pour lesquelles le client ne reçoit pas de configuration IP du serveur ?

Les raisons pour lesquelles le client ne reçoit pas de configuration IP du serveur sont :
– Au niveau du serveur :
o Il est éteint
o Le service DHCP est inactif ou non-démarré
o Le pool DHCP est plein
– Au niveau du client :
o La configuration IP est en statique
o Il n’est pas dans le même réseau IP que le serveur DHCP
o Le service DHCP est inactif ou non-démarré
o En cas de présence de routeur, les trames broadcast en provenance du client sont bloquée

### Q.1.2 Sur le serveur on exécute la commande ipconfig /all. On a une adresse 172.16.10.10/24. Si on exécute la même commande sur le client, on a une adresse 172.16.100.50/24.
Pourquoi un ping ne fonctionne pas entre ces 2 machines ?

Les 2 adresses de réseaux sont différente au niveau du 3ème octet, et le masque est en « /24 ». Cela
indique que ce sont des réseaux différents (au niveau d’IP). En l’absence de passerelle, ils ne peuvent
pas communiquer.

### Q.1.3 Si le client a une configuration IP en DHCP, pour quelle(s) raison(s) il ne prend pas la première adresse configurée sur la plage des adresses IP mais une autre adresse ?

Les raisons peuvent être les suivantes :
– Réutilisation d’une adresse déjà utilisée si le bail DHCP n’est pas terminé
– La première adresse disponible est peut-être déjà prise par une autre machine
– La première adresse disponible est peut-être sur une plage en exception
– Le client a peut-être une adresse réservée sur la plage DHCP qui n’est pas cette première adresse

### Q.1.4 Est-il possible de forcer un client à prendre une adresse IP spécifique en DHCP ? Si oui explique comment procéder sur le client et sur le serveur.

Oui c’est possible de forcer le client à avoir une adresse IP spécifique.
Sur le serveur on va créer une réservation d’adresse IP pour l’adresse MAC du client.
Le client doit être en DHCP, pas de configuration supplémentaire à faire.
Ensuite il faut redémarrer le client ou exécuter la commande « ipconfig /renew »

### Q.1.5 Le résultat de la commande ipconfig /all donne aussi, sur le client et sur le serveur, une adresse IPv6 qui commence par fe80...
Quelle est cette adresse et à quoi sert-elle ?

Une adresse IPv6 qui commence par fe80 est une adresse de type unicast lien local (Link Local Address
ou LLA).
C’est une adresse non-routable, qui sert à communiquer sur un même réseau. Son préfixe réseau est
fe80 ::/10, mais en général on trouve fe80 ::/64. Elle est gérée dynamiquement par SLAAC.

### Q.1.6 Quel autre type d'adresse IPv6 peut-on avoir sur une machine ? Précise le type, le préfixe, et la porté.

Il existe aussi en IPv6 :
– Les adresses unicast locales uniques (Unicast Local Address ou ULA), avec le préfixe fc00::/7 ou
fd00::/8, avec une portée au sein d’un réseau interne, mais non-routable sur internet
– Les adresses unicast globales (Global Unicast Address ou GUA), avec le préfixe 2000::/3 ou 2001
::/16, routable sur internet
– Les adresses de bouclage, qui sont ::1/128, qui correspond à la machine elle-même
– Les adresses multicast, avec le préfixe ff00::/8, pour envoyer des paquets à un groupe de
machines

### Q.1.7 Est-il possible de configurer le service DHCPv6 sur le serveur SRVWIN01 avec des préfixes d'adresses Unicast Locales Uniques ?
Dans ce cas, si le client a une configuration IP en DHCP, est-ce qu'il va avoir automatiquement une adresse IPv6 Unicast Locales Uniques ?

Oui on peut configurer avec un DHCPv6 des plages d’adresses unicast locales uniques.
Sur le serveur, on peut prendre la configuration suivante :
– Préfixe ULA dans la plage fd00::/8, par exemple fd12 :3456 :789a ::/48
– Type d’étendue : Autre
– Durée de validité : mettre la durée du bail
Sur le client, il suffit que le service DHCPv6 soit activé.
Non cela ne suffit pas, car il faut aussi que les Router Advertisement (RA) soient activés pour donner
différentes informations au client. Or Windows Server ne peut pas gérer les RA nativement. Donc à moins
d’utiliser une solution tiers pour la gestion des RA (comme un routeur ou un système Linux avec radvd)
cela ne fonctionnera pas.

### Q.1.8 Entre 2 machines on exécute la commande ping avec le nom des machines au lieu de l'adresse IP. La commande s'exécute correctement.
Quel protocole permet cela ?
Le protocole qui permet cette résolution de nom est le DNS (Domain Name System).

### Q.1.9 Si l'IPv6 est activé, le retour de la commande ping avec le nom d'une machine est une adresse IPv6. Pourquoi ?
Quelle option de la commande ping permet d'avoir une adresse IPv4 ?

Par défaut on a une adresse IPv6 car le protocole ICMP priorise l’IPv6 sur l’IPv4.

On peut avoir le retour d’adresse en IPv4 avec l’option « -4 » de la commande ping.

### Q.1.10 À partir du serveur, je veux pouvoir faire un ping vers le client à partir de son nom, et également avec un alias de ce nom, par exemple CLIENT_TEST, comment procéder ?

Méthode 1 : sans serveur DNS
On utilise le fichier hosts qui est sur les systèmes Microsoft Windows sous
c :\Windows\System32\drivers\etc\hosts.
Dans ce fichier on met l’adresse IP associée au nom du client.
Par exemple si l’adresse IP du client CLIENT1 est 192.168.100.20, la ligne dans le fichier hosts sera :
« 192.168.100.20 CLIENT1 CLIENT_TEST »
Méthode 2 : avec un serveur DNS
Sur le serveur qui a le rôle DNS, on créer un enregistrement A (en IPv4) ou AAAA (en IPv6) qui lie le nom
du client à son adresse IP.
Ensuite on créer un enregistrement CNAME, c’est un alias) qui pointe vers le nom du client dans le DNS.

![image](https://github.com/user-attachments/assets/f368fec4-bd82-4f6f-adc2-6b350dbbcc55)

### Q.3.1 Quels sont les matériels A et B ? Quel est leur rôle sur un réseau ? Sur quelle couche du modèle OSI fonctionnent-ils ?

Matériel A (type, rôle sur un réseau, couche du modèle OSI) ?
Le matériel réseau A est un commutateur, communément appelé switch.
Son rôle est de connecter les ordinateurs entre-eux et de faciliter leur
communication avec les adresses MAC.
Il intervient au niveau de la couche 2 du modèle OSI.

Matériel B (type, rôle sur

### Q.3.2 Rempli le tableau suivant pour les PC du réseau :

| PC  | Adresse Réseau | 1ère @IP | Dernière @IP | @IP Diffusion |
|---|---|---|---|---|
| PC1 | 10.10.0.0/16 | 10.10.0.1 | 10.10.255.254 | 10.10.255.255 |
| PC2 | 10.11.0.0/16 | 10.11.0.1 | 10.11.255.254 | 10.11.255.255 |
| PC3 | 10.10.0.0/16 | 10.10.0.1 | 10.10.255.254 | 10.10.255.255 |
| PC4 | 10.10.0.0/16 | 10.10.0.1 | 10.10.255.254 | 10.10.255.255 |
| PC5 | 10.10.0.0/15 | 10.10.0.1 | 10.11.255.254 | 10.11.255.255 |
	
### Q.3.3 Quel est le rôle de A pour un ping de PC1 vers PC3 ?
Explique si cette communication a réussi.

– PC1 construit un paquet ICMP &quot;echo request&quot; et l&#39;encapsule dans une trame Ethernet. L&#39;adresse
MAC destination de cette trame est l&#39;adresse MAC de PC3, et l&#39;adresse MAC source est celle de
PC1
– Le switch A reçoit la trame sur l&#39;un de ses ports. Il consulte sa table MAC pour trouver l&#39;adresse
MAC de PC3 (cette table associe les adresses MAC des appareils aux ports du switch sur
lesquels ils sont connectés)
– Si le switch A trouve l&#39;adresse MAC de PC3 dans sa table, il transmet la trame uniquement sur le
port auquel PC3 est connecté. Sinon, il diffuse la trame sur tous les ports sauf celui d&#39;origine.
– PC3 reçoit la trame, la désencapsule, traite la requête ping et envoie une réponse &quot;echo reply&quot; à
PC1 en suivant le même chemin inverse.

Communication réussie ?
PC1 et PC3 ont la même adresse de réseau 10.10.0.0/16, ils sont donc sur le même réseau IP. Le
matériel A va transmettre les trames d’un matériel vers un autre car un seul vlan 10.10.0.0/16 est
configuré sur A, donc la communication a réussi.

### Q.3.4 Quel est le rôle de B pour un ping de PC2 vers PC4 ?
Explique si cette communication a réussi.

Rôle de B pour un ping de PC2 vers PC4
PC2 a l’adresse de réseau 10.11.0.0/16 et PC4 a l’adresse 10.10.0.0/16. Ils ne sont donc pas sur le
même réseau. B est un commutateur (ou switch) et ne peut pas transmettre des paquets IP entre des
hôtes de réseaux différents.
Donc B n’a aucun rôle dans cette communication.

Communication réussi ?

Non la communication échoue parce que les 2 machines ne sont pas sous le même réseau.

### Q.3.5 Le résultat d'un ping de PC5 vers PC2 est 10.11.80.2 icmp_seq=1 timeout.
Celui d'un ping de PC2 vers PC5 est No gateway found.
Explique ces 2 résultats.

Explication du résultat du ping de PC5 vers PC2
Pour PC5 (10.10.4.7) :
Adresse de réseau : 10.10.0.0/16
1ère adresse disponible : 10.10.0.1
Dernière adresse disponible : 10.11.255.254
Adresse de diffusion : 10.11.255.255

Pour PC2 (10.11.80.2) :
Adresse de réseau : 10.11.0.0/16
1ère adresse disponible : 10.11.0.1
Dernière adresse disponible : 10.11.255.254
Adresse de diffusion : 10.11.255.255

L’adresse IP de PC2 (10.11.80.2) fait partie de la plage d&#39;adresse du réseau de PC5 (10.10.0.1 -
10.11.255.254). Donc les paquets « request » de PC5 parviennent au PC2 (même si le CIDR est
différent).

L&#39;adresse IP de PC5 (10.10.4.7) ne fait pas partie des adresses de la plage du réseau de PC2 (10.11.0.1
- 10.11.255.254). Donc pour PC2 l’adresse de PC5 ne fait pas partie de son réseau, d’où les paquets
« reply » qui ne sont pas envoyé et le resultat : « icmp_seq=1 timeout ».

Explication du résultat du ping de PC2 vers PC5
L&#39;adresse IP de PC5 (10.10.4.7) ne fait pas partie des adresses de la plage du réseau de PC2 (10.11.0.1
- 10.11.255.254). Donc pour PC2 l’adresse de PC5 ne fait pas partie de son réseau.
Si aucune passerelle n’a été configurée sur PC2, le ping retourn « No gateway found ».

### Q.3.6 Quels sont les matériels C et D ? Quel est leur rôle sur un réseau ? Sur quelle couche du modèle OSI fonctionnent-ils ?

Matériel C et D (type, rôle sur un réseau, couche du modèle OSI) ?
Ce sont des routeurs.

Le rôle d’un routeur est de faire communiquer des réseaux différents entre-eux. Les PC du réseaux
10.10.0.0/16 peuvent communiquer avec les machines des autres réseaux. Il se sert des adresses IP et
intervient au niveau de la couche 3 du modèle OSI.

### Q.3.7 Comment PC3 peut envoyer des paquets IP en dehors de son réseau ?
Quel matériel l'aide dans cette tâche ?

Moyen de PC3 pour sortir du réseau ?
PC3 va utiliser sa passerelle par défaut.

Matériel du réseau servant à cette tâche ?

### Q.3.8 Le matériel trouvé à la question précédente remplit-il ce rôle pour tous les PC du réseau (PC1~5) ?

Le matériel servant à PC3 sert aussi pour tous les autres PC du réseau ?
Tout les PC sauf PC2 qui a l’adresse IP 10.11.80.2 avec l’adresse de réseau 10.11.0.0/16, et donc qui
n’est pas dans le même réseau que celui de l’interface g2/0 du routeur C.

### Q.3.9 Sur C, pour l'interface g1/0, que signifie le g, le 1, et le 0 ?

Pour le matériel C, pour le label « g1/0 », Que signifie le « g » ?
Le « g » indique que l’interface réseau est en Gigabit, ou Gigabit Ethernet.

Le « 1 » ?
Dans « g1/0 » le « 1 » correspond au numéro du module (ou slot). La numérotation commence à 0, donc
« g1/0 » indique que l’on est sur le 2ème module.

Le « 0 » ?
Dans « g1/0 » le « 0 » correspond au numéro de l’interface réseau du module correspondant, soit le port.
La numérotation commence à 0, donc « g1/0 » indique que l’on est sur le 1er port.

### Q.3.10 Sur C, que faut-il configurer pour atteindre le réseau 172.16.5.0/24 et au moyen de quelle commande ?
Cette action est-elle effective au niveau du routeur ou d'une interface en particulier ?

Ligne de commande ?
Il faut configurer une règle de routage, soit sur le routeur C : « ip route 172.16.5.0 255.255.255.0
10.12.2.253 ». Cela indique au routeur que les paquets à destination du réseau 172.16.5.0/24 doivent
passer par la passerelle 10.12.2.253.

Périmètre de la commande ?
Cette commande est effective au niveau du routeur C et pas d’une interface réseau en particulier.

### Q.3.11 Rempli le tableau ci-dessous qui contient les informations de la trame ethernet suivant l'emplacement sur le réseau pour un ping depuis PC1 vers le réseau 172.16.5.0/24.

| Emplacement sur le réseau | Adresse MAC destination | Adresse MAC source | @IP Source | @IP Destination |
|---|---|---|---|---|
| Entre PC1 et A | CA:01:DA:D2:00:08 | 00:50:79:66:68:00 | 10.10.4.1 | 172.16.5.x |
| Entre B et C | CA:01:DA:D2:00:08 | 00:50:79:66:68:00 | 10.10.4.1 | 172.16.5.x |
| Entre C et D | CA:03:9E:EF:00:38 | CA:01:DA:D2:00:1C | 10.10.4.1 | 172.16.5.x |
| Après D | xx:xx:xx:xx:xx:xx | CA:03:9E:EF:00:54 | 10.10.4.1 | 172.16.5.x |

Ne rempli que le ping request (pas le reply).


1. Assurer le support utilisateur en centre de service

### 1.1 Du point de vue d'ITIL, quelle est la différence entre un incident et un problème ?

Selon ITIL (Information Technology Infrastructure Library) :
– Incident : C'est un événement imprévu qui perturbe ou diminue la
qualité d'un service informatique. L'objectif avec un incident est de
rétablir le service le plus rapidement possible.
– Problème : C'est la cause sous-jacente ou inconnue d' un ou plusieurs
incidents. L'objectif de la gestion des problèmes est d'identifier et de
résoudre ces causes, de manière à prévenir la récurrence des incidents
associés.

### 1.2 Quels sont les différents moyens de prendre le contrôle à distance d'une machine ?

Les différents moyens de prendre le contrôle à distance d’une machine sont :
– Le bureau à distance Windows : qui utilise Remote Desktop Protocol
(RDP)
– Les logiciels dédiés : comme VNC, TeamViewer, AnyDesk, ou Microsoft
SCCM
– Les commandes à utiliser dans un terminal : comme le Remote
PowerShell, ou les connexions SSH (principalement pour les systèmes
basés sur Unix/Linux), à noter que l’on peut utiliser SSH avec X11 pour
un retour graphique
– Les outils basés sur le web : Comme Chrome Remote Desktop.
– Également les connexions VPN : en établissant un tunnel VPN, on peut
accéder au réseau distant et ensuite utiliser des outils locaux pour se
connecter aux machines.

### 1.3 Donne les différentes étapes à respecter dans une résolution d'incident par téléphone.

Dans une résolution d’incident par téléphone, voici les différentes étapes à
respecter :
– Identification / Détection
– Notification
– Enregistrement
– Catégorisation et priorisation
– Diagnostic et investigation
– Suivi (ou escalade)
– Résolution (et documentation)
– Clôture

### 2. Exploiter des serveurs Windows et un domaine Active Directory


### 2.1 Qu'est-ce qu'un rôle FSMO ?

Un rôle FSMO (Flexible Single Master Operation) est la possibilité pour un
contrôleur de domaine, sur un domaine Active Directory, de pouvoir effectuer
des tâches particulières.
Il existe 5 rôles FSMO :
– Maître de schéma
– Maître d’attribution de nom de domaine
– Maître RID
– Maître d’infrastructure
– Émulateur PDC

### 2.2 En quoi la réplication entre contrôleurs de domaine est primordiale sur un domaine ?

La réplication entre contrôleurs de domaine est primordiale pour les raisons
suivantes :
– Elle garantie la cohérence et la disponibilité des données d'annuaire
dans un environnement réseau
– Elle permet de s'assurer que toutes les modifications (comme les ajouts
d'utilisateurs, les modifications de mots de passe, et les politiques de
groupe) sont uniformément réparties à travers tous les contrôleurs de
domaine, assurant ainsi que les utilisateurs ont accès aux informations
les plus à jour, peu importe à quel contrôleur de domaine ils se
connectent
– Elle amène de la tolérance de panne.

### 3. Exploiter des serveurs Linux

### 3.1 Quel est le résultat de la commande suivante : chmod u+x /home/tssr/factures/export.sh ?

Cette commande ajoute le droit d'exécution « +x » au propriétaire « u » pour
le fichier « /home/tssr/factures/exports.sh ».

### 3.2 Quelle commande dois-tu écrire dans un terminal sur un OS Debian pour ajouter l'adresse IP 172.16.8.16/24 à l'interface enp0s8 ?

La commande est :
ip address add 172.16.8.16/24 dev enp0s8
Ensuite il faudra activer l’interface avec “ip link set dev enp0s8 up”

### 3.3 L'utilisateur Wilder ne parvient plus à accéder au dossier travaux. Explique la cause probable et donne les outils (commandes) pour diagnostiquer et résoudre le problème.
    
      wilder@SRV08:~$ ls /home/wilder/travaux/
      ls: impossible d'ouvrir le répertoire /home/wilder/travaux/: Permission non accordée
      wilder@SRV08:~$

Cause probable : l’utilisateur "wilder" n'a pas les permissions nécessaires
pour accéder au dossier.
Pour vérifier les permissions : ls -ld /home/wilder/travaux/
Pour résoudre le problème on peut :
- Changer les permissions avec (par exemple) “chmod 750
/home/wilder/travaux”
- Modifier le propriétaire avec (par exemple) “chown wilder:wilder
/home/wilder/travaux/”

### 3.4 D’après les éléments de la capture ci-dessous, si on ajoute un disque dur supplémentaire qui n'a qu'une seule partition, comment se nommera t'elle ?

![image](https://github.com/user-attachments/assets/3d38aad2-57c6-4509-a94e-217aee393fc8)

Le périphérique actuel est /dev/sda1, donc si on ajoute un disque dur avec
une seule partition, elle se nommera sdb1.


### 3.5 Donne 2 commandes qui te permettent de visualiser les disques et les partitions d'un serveur Linux.

On peut utiliser « lsblk » et « fdisk –l ».

### 4. Exploiter un réseau IP

### 4.1 Une entreprise a un réseau 192.160.16.0/25. Elle souhaite le découper en 4 sous-réseaux de taille identiques.
Pour les 2 premiers sous-réseaux, donne l’adresse de réseaux, le masque (en notation CIDR), la première adresse disponible, la dernière adresse disponible, ainsi que l'adresse de broadcast.

![image](https://github.com/user-attachments/assets/1836c571-a512-41f5-a67c-03e8e999de18)

### 4.2 Complète le tableau de conversion suivant :

| Décimal	| Binaire |	Hexadécimal |
|---|---|---|
| 9 |  0000 1001  | 0x 09  |
| 127	|	0111 1111 | 0x7F |
| 255	| 1111 1111 | 0x FF
| 16	| 00010000 | 0x 10 |

### 4.3 Pour le schéma ci-dessous :

Quels sont les liens trunk ?

Ce sont les liens entre les switchs et le lien entre un switch et le routeur.

Quelle méthode de routage intervlan est utilisée ?

Le routeur se charge de router entre les VLANs avec des id de vlans différents
sur le même medium. La méthode de routage est « Router-on-a-stick ».

*![image](https://github.com/user-attachments/assets/a636a2c0-c9d0-45e1-ac5e-b89c4f39c95b)


### 4.4 Sur un réseau IP, 2 PC ont la configuration IP suivante :

Solution matérielle :
Installer un routeur pour interconnecter les deux sous-réseaux.
Chaque PC sera connecté à une interface du routeur, et le routeur assurera la
communication entre les deux sous-réseaux.
Modification du paramétrage :
Bien vérifier que PC1 utilise l’interface du routeur sur laquelle il est connecté
comme passerelle par défaut. Même chose pour PC2 pour l’autre interface.

PC1 : 192.168.1.54/24
PC2 : 192.168.2.74/24
Sans changer l'adresse IP des 2 PC, donne une solution matérielle et une modification du paramétrage à effectuer pour que les 2 PC puissent communiquer entre-eux.

### 4.5 Des ordinateurs sont connectés sur un switch qui n'a qu'un seul vlan, avec les configurations IP suivantes :

| PC |	Adresse IP	| Masque de sous-réseau |
|---|---|---|
| PC1 |	192.168.10.8	| 255.255.255.0 |
| PC2 | 192.168.10.12 | 255.255.255.0 |
| PC3	| 192.168.10.10	| 255.255.0.0 |
| PC4	| 192.168.11.9 |	255.255.255.0 |

Pour chaque ordinateur, indique en expliquant les communications ICMP réussies.

Pour PC1 ?
Il a l’adresse de réseau 192.168.10.0/24 qui est la même que celles de PC2
donc le ping fonctionne avec cette machine.
Cette adresse de réseau est également un sous-réseau de celle de PC3,
donc il peut renvoyer une trame « echo reply» à une demande « echo
request » de PC3.
Il ne peut pas communiquer directement avec PC4 car ils sont dans des
réseaux différents.
Pour PC2 ?
Son adresse de réseau est 192.168.10.0/24, donc il peut communiquer avec
PC1. Comme PC2, cette adresse est dans un sous-réseaux de l’adresse
réseau de PC3, donc il peut également envoyer une trame « echo reply» à
une demande « echo request ».
Pour PC3 ?
Il a l’adresse de réseau 192.168.0.0/16. Pour un ping vers PC1, PC3 prend
l’adresse IP destination avec son propre masque, ce qui donne l’adresse de
réseau « fictive » 192.168.0.0/16 pour PC1. PC3 considère alors que PC1 est
dans son réseau et peut envoyer une trame « echo request ». PC1 peut
répondre à cette requête.
Même chose pour PC2.

Pour PC4, PC3 « considère » qu’il a l’adresse de réseau 192.168.0.0/16 et
donc lui envoie une trame « echo request ».
Pour PC4 ?
Son adresse de réseau est 192.168.11.0/24. Elle est différente de celle de
PC1. Les 2 machines ne peuvent pas communiquer.
Même constat avec PC2.
Pour PC3, PC4 considère que son adresse de réseau est 192.168.10.24,
donc ils ne sont pas dans le même réseau et PC4 ne peut pas envoyer une
trame de réponse à la demande « echo request » de PC3.
En conclusion, seul les PC1, PC2, et PC3 peuvent communiquer entre-eux.

### 4.6 Quelles sont les actions possibles à mettre en œuvre pour sécuriser un réseau sans fil ?

Les actions possibles sont :
– Changer le nom du SSID : Modifiez le nom par défaut du réseau
– Désactiver la diffusion du SSID (le cacher)
– Durcir le chiffrement (par exemple WPA2 Entreprise ou WPA3)
– Changer le mot de passe par défaut : Choisir un mot de passe fort et
unique
– Utilisez un réseau invité pour les personnes extérieures à l’entreprise
– Mettre à jour le firmware des bornes wifi

### 4.7 Quelles sont les routes statiques à ajouter sur le routeur Routeur1 pour permettre la communication entre PC0 et PC3 ?


![image](https://github.com/user-attachments/assets/a19eed81-4136-4c2a-95f4-ac3417499ca2)

![image](https://github.com/user-attachments/assets/e9acd5f1-4cf5-4e4f-bddd-b28d1f494033)

### 4.8 Complète le tableau suivant avec les informations sur les différents services/protocoles :

| Acronyme | Nom complet | Port(s) par défaut TCP | Port(s) par défaut UDP |
|---|---|---|---|
| HTTP | Hypertext Transfer Protocol | 80 |  |
| FTP/FTPS | File Transfer Protocol / File Transfer Protocol Secure | 20 (données), 21 (commandes) |  |
| SSH | Secure Shell | 22 |  |
| TFTP | Trivial File Transfer Protocol |  | 69 |
| SMTP | Simple Mail Transfer Protocol | 25 (non-sécurisé), 587, 465 (sécurisé) |  |
| IMAP | Internet Message Access Protocol | 143 (non sécurisé), 993 (sécurisé) |  |
| LDAP | Lightweight Directory Access Protocol | 389 (non sécurisé), 636 (sécurisé) |  |
| POP3 | Post Office Protocol version 3 | 110 (non sécurisé), 995 (sécurisé) |  |
| DNS | Domain Name System | 53 |  53 |
| NTP | Network Time Protocol |  | 123 |
| POP3S | Post Office Protocol version 3 sécurisé | 995 |  |


### 4.9 Sur quels ports du switch peut-on brancher ce téléphone IP ?

![image](https://github.com/user-attachments/assets/a19e3e58-2f9b-493e-bc1d-53fe00de5b46)

Le téléphone IP a un chargeur donc on peut le brancher sur tous les ports,
donc de 1 à 8.


### 4.10 

![image](https://github.com/user-attachments/assets/1e883bc2-db80-48a2-9ba1-72f93b5b0441)

### 5. Maintenir des serveurs dans une infrastructure virtualisée
   
### 5.1 Explique ce qu'est un cluster d'hyperviseur. Quel est l’intérêt d'une telle structure ?

   Définition :
Un cluster d'hyperviseurs est un regroupement de plusieurs serveurs (ou
nœuds) sur lesquels sont installés des hyperviseurs, permettant de gérer des
machines virtuelles (VM).
Intérêt :
L’intérêt est de pouvoir répartir les VM sur plusieurs nœuds pour avoir de la
haute disponibilité, une répartition de charge, et une gestion centralisée des
ressources. Des protocoles comme le ceph peuvent être mis en place pour
cela.
On a ainsi une continuité de service même en cas de défaillance d'un serveur.

### 5.2 Qu'est-ce qu'un container ? Donne différentes solutions de conteneurisation.

Définition :
Un conteneur est une unité standardisée qui permet d’encapsuler, d’isoler, du
code et toutes ses dépendances. L’application ainsi contenue s’exécute de
manière fiable quel que soit l’environnement ou l’OS.
Exemples de solutions :
Docker,LXC

### 5.3 Que représente les lignes de code ci-dessous ? Comment les utiliser ?

    FROM ubuntu:latest

    # Installation de packages
    RUN apt-get update && apt-get install -y \
    bash \
    nano \
    && rm -rf /var/lib/apt/lists/*

    # Répertoire local
    RUN mkdir /data

    # Dossier de travail
    WORKDIR /data

    # Image en mode interactif
    CMD ["bash", "-i"]

Signification :
Ces lignes de code sont le contenu d’un fichier Dockerfile. Il permet de
construire une image Docker.
Voici les actions faites :
– Il part de l'image Ubuntu
– Installe les paquets bash et nano
– Supprime les sources téléchargées
– Crée un répertoire « /data » et définit ce répertoire comme répertoire de
travail
– Lance une session Bash interactive lorsque le conteneur est démarré
Utilisation :
Pour exécuter ce code, on le sauvegarde dans un fichier nommé
« Dockerfile », puis on construit l'image avec la commande « docker build ».

### 5.4 Pour la copie d'écran ci-dessous, quelle devrait être ta démarche dans une telle situation ?

![image](https://github.com/user-attachments/assets/46a8c127-e38e-4045-9e47-cc183275e794)

On voit une erreur « CRITICAL » sur le service « Swap usage ». On voit « 0% free »
dans la colonne « Status informations ».
Il faut donc augmenter la taille du swap.

### 5.5 Que veulent dire les termes PaaS, IaaS, et SaaS ?

PaaS :
PaaS (Platform as a Service) : Fournit une plateforme pour développer et
gérer des applications sans se soucier de l'infrastructure nécessaire.
IaaS :
IaaS (Infrastructure as a Service) : Offre des ressources informatiques
virtualisées sur Internet, éliminant le besoin d'acheter ou de gérer une
infrastructure physique.
SaaS :
SaaS (Software as a Service) : Logiciels hébergés dans le cloud auxquels les
utilisateurs accèdent et utilisent via Internet, souvent via un navigateur web.

### 5.6 Dans la mise en oeuvre d'une solution HA (High Availability), quels sont les élements indispensable ?

Les éléments indispensables sont :
– La redondance matérielle (serveur, stockage)
– Un système de répartition de charge (Load Balancing)
– Un système de basculement automatique en cas d’erreur ou de panne
(Failover)
– Une copie (réplication) des données en temps réel
– Un système de supervision
– Un PRA et PCA

### 6. Maintenir et sécuriser les accès à Internet et les interconnexions des réseaux
### 6.1 Quel est l'impact des ACL ci-dessous sur la machine 172.16.0.10 ? Peut-on fusionner ces ACL pour n'en former qu'une seule ? Si oui fais-le.

    access-list 100 deny icmp host 172.16.0.10 172.17.0.0 0.255.255.255
    access-list 100 permit ip any any
    access-list 101 deny tcp host 172.16.0.10 host 220.0.0.60 eq www
    access-list 101 deny tcp host 172.16.0.10 host 220.0.0.60 eq 443
    access-list 101 permit ip any any

Impact pour la machine :
Pour l’ACL 100 :
- Elle bloque le trafic ICMP (donc le ping) de 172.16.0.10 vers le réseau
172.17.0.0/24
- Elle autorise tout le reste du flux réseau

Pour l’ACL 101 :
- Elle bloque tout le trafic HTTP et HTTPS (donc le web) de 172.16.0.10
vers la machine 220.0.0.60
- Elle autorise tout le reste du flux réseau

Fusion des ACL :
access-list 110 deny icmp host 172.16.0.10 172.17.0.0 0.255.255.255
access-list 110 deny tcp host 172.16.0.10 host 220.0.0.60 eq www
access-list 110 deny tcp host 172.16.0.10
   
### 6.2 Pour les commandes ci-dessous les 2 chaînes de caractères en entrée sont de tailles différentes et pourtant la longueur du résultat des commandes est identique. Pourquoi ?

    wilder@Ubuntu:~$ echo -n "test message" | sha512sum
    950b2a7effa78f51a63515ec45e03ecebe50ef2f1c41e69629b50778f11bc080002e4db8112b59d09389d10f3558f85bfdeb4f1cc55a34217af0f8547700ebf3  -

    wilder@Ubuntu:~$ echo -n "ce message n'a aucun rapport avec le précedent !" | sha512sum
    0096a6b7b1ff9714c8a0ecd308e1c952ec2f956f0f5ae28ec29b3e6b68f16a127ea4c379c4aafce2e4f97c029874628f4d3376440ae87c34f83b225c973f1d0a  -

La commande passé derrière le « pipe » est « sha512sum » qui est une
fonction de hachage cryptographiques.
Ce type de fonction de chiffrement produit toujours une sortie de taille fixe,
quelle que soit la taille de l'entrée. C'est une caractéristique essentielle qui
garantit que la sortie (le hachage) a une longueur constante.

### 6.3 Sur l'infrastructure réseau représentée par le schéma ci-dessous, que faut-il faire pour que l'on puisse accéder de manière sécurisée au serveur web depuis internet ?

![image](https://github.com/user-attachments/assets/890b9791-2d40-4478-87a4-2d1808d00e09)

### 6.4 Quels types de VPN sont représentés dans les illustrations suivantes ?

VPN A : VPN type accès distant (host to network)

![image](https://github.com/user-attachments/assets/ecea8230-8de4-40c5-be5b-8bf73f1b45fb)

VPN B : VPN type site à site

![image](https://github.com/user-attachments/assets/4f1c94ce-5aa4-46c7-a106-e1b07040c999)

### 6.5 Par rapport au schéma ci-dessous, complète le texte en dessous avec les bons termes.

![image](https://github.com/user-attachments/assets/5dd4feae-c378-4067-8b6e-7f9f1305a529)

Pour envoyer un message privé à Bob, Alice utilise- expression 1 -de Bob pour rendre « illisible » le « texte en clair » et Bob utilise- expression 2 -pour transformer le texte « illisible » en « texte en clair ». Ce processus représente un chiffrement- expression 3 -.

Expression 1 :
La clé publique
Expression 2 :
Sa clé privée
Expression 3 :
asymétrique

### 6.6 Tu as ci-dessous un extrait d’un guide de configuration d’un tunnel VPN site à site en IPSec/ISAKMP. Traduit ce passage en Français.

When ISAKMP negotiations begin, the peer that initiates the negotiation sends all of its policies to the remote peer, and the remote peer tries to find a match. The remote peer checks all of the peer's policies against each of its configured policies in priority order (highest priority first) until it discovers a match.

Texte traduit :
Lorsque les négociations ISAKMP commencent, le nœud qui initie la
négociation envoie toutes ses politiques au nœud distant, et le nœud distant
essaie de trouver une correspondance.
Le nœud distant vérifie toutes les politiques du nœud initiateur contre chacune
de ses politiques configurées dans un ordre de priorité (la plus haute priorité
en premier) jusqu'à ce qu'il trouve une correspondance.

### 6.7 En matière de sécurité informatique, indique 3 types de menaces (risques et attaques) auxquelles peut être confronté un SI (ne rentre pas dans les détails).

Menace 1 :
Les attaque par déni de service (DDoS)
Menace 2 :
Les ransomware
Menace 3 :
Les attaque virales

### 7. Mettre en place, assurer et tester les sauvegardes et les restaurations des éléments de l'infrastructure
### 7.1 Qu'est-ce que la règle 3-2-1 ?
   
   La règle du 3-2-1 est une pratique courante :
– 3 copies (prod + 2 sauvegardes)
– 2 types de support différents
– 1 copie hors-site

### 7.2 Quelles sont les différents types de sauvegardes à mettre en place en entreprise ?

Les différents types de sauvegarde sont :

– Les sauvegarde complète
– Les sauvegarde incrémentale
– Les sauvegarde différentielle

### 7.3 Indiquer les différences entre une sauvegarde, de l'archivage, et un clonage.

Une sauvegarde est une copie de données en production actuellement
utilisées. On peut les restaurer en cas de perte ou de corruption.
L’archivage est un stockage à long terme de données qui ne serve plus à la
production, mais qui doivent être conservées pour des raisons légales ou
historiques. Les données sont supprimées de la production.
Le clonage est la réplique exacte d'un système ou d'un disque dur, souvent
utilisé pour déployer des configurations identiques sur plusieurs machines.

### 8. Exploiter et maintenir les services de déploiement des postes de travail
### 8.1 Quels avantages apporte la mise en place d'un service centralisé de mises à jour logicielles au sein d'une entreprise ? Indique une solution que tu connais et explique son fonctionnement.

Avantages :
Gestion simplifié et centralisée, politique de sécurité unifiée, réduction des
coût de maintenance.
Exemple de solution :
WSUS (Windows Server Update Services) est un rôle sur un serveur
Windows qui permet de gérer, de télécharger, et de publier des mises à jour à
des postes clients au sein d’un parc informatique.

### 8.2 Quels sont les inconvénients liés à la mise en place d'une solution de terminaux clients légers par rapport à des postes fixes ?

### 8.2
Les inconvénient sont les suivants :
– Dépendance au réseau
– Point de défaillance unique (serveur central par exemple)
– Coût initial (infrastructure de serveur pour les clients légers)
– Personnalisation limitée (au niveau utilisateur)

### 8.3 Que fait l’exécution de la ligne de commande suivante ? Dans quel contexte est-elle utilisée ?

    C:\Windows\System32\sysprep\sysprep.exe /oobe /generalize /shutdown

Signification :
La commande sysprep s’exécute dans cet exemple avec les commandes
suivantes :
– /oobe : démarre la machine avec des fenêtres pour guider l’utilisateur

– /generalize : supprime les informations spécifiques au système, ce qui
permet de réutiliser cette image sur d'autres machines (Id, ...)
– /shutdown : éteint la machine après que sysprep soit finalisé
Contexte d’utilisation :
Cette commande s’utilise pour préparer une image Windows en tant que
« master » pour un déploiement de masse.


### 8.4 Dans le cadre d'un déploiement de postes clients Linux, quelle est l'utilité d'un dépôt local de paquet ?

Il permet :
– Une accélération des mises à jour sur les clients
– La même version des paquets sur l’ensemble du parc informatique
– Une amélioration de la sécurité car un contrôle est fait par les membres
de l’IT sur les paquets






 # Attention Reponse Générée par IA

1.C'est quoi le MDM ?

Le MDM (Mobile Device Management) est une solution logicielle permettant aux entreprises de gérer, sécuriser et surveiller les appareils mobiles utilisés par leurs employés, tels que les smartphones, tablettes et ordinateurs portables 2 4 .

Voici les principaux aspects du MDM :

Gestion centralisée : Le MDM permet aux administrateurs informatiques de configurer, surveiller et contrôler les appareils mobiles à distance depuis une plateforme centralisée 2 4 .

Sécurité : Il renforce la sécurité en appliquant des politiques comme le verrouillage d'écran, le chiffrement des données et la possibilité d'effacer à distance les appareils perdus ou volés 3 4 .

Déploiement d'applications : Le MDM facilite la distribution et la mise à jour des applications sur les appareils de l'entreprise 2 .

Conformité : Il aide à assurer que les appareils respectent les normes de sécurité et les réglementations de l'entreprise 4 5 .

Support : Il offre des fonctionnalités de dépannage à distance pour aider les utilisateurs 3 .

Inventaire et suivi : Le MDM permet de gérer l'inventaire des appareils et de leur utilisation 3 .

Le MDM est particulièrement utile dans les environnements de travail hybrides et à distance, où les employés utilisent divers appareils et systèmes d'exploitation 4 . Il aide les entreprises à optimiser la productivité tout en maintenant un niveau élevé de sécurité et de contrôle sur leurs ressources mobiles 

2.Donne moi les règles à respecter pour avoir un mot de passe sécurisé

Voici les principales règles à respecter pour avoir un mot de passe sécurisé :

Longueur : Utilisez un mot de passe d'au moins 12 caractères 1 4 .

Complexité : Incluez un mélange de minuscules, majuscules, chiffres et caractères spéciaux 1 4 .

Unicité : Utilisez un mot de passe différent pour chaque compte ou service 3 4 .

Évitez les informations personnelles : N'utilisez pas de données liées à votre vie privée (date de naissance, nom d'entreprise, etc.) 2 4 .

Pas de suites logiques : Évitez les séquences simples comme "azerty" ou "1234" 2 .

Renouvellement : Changez régulièrement vos mots de passe, surtout en cas de soupçon de piratage 2 .

Utilisation d'un gestionnaire : Envisagez d'utiliser un gestionnaire de mots de passe pour stocker et générer des mots de passe complexes 4 .

Double authentification : Activez la double authentification lorsque c'est possible pour une sécurité renforcée 2 .

Mémorisation : Utilisez des techniques comme la création d'une phrase dont vous ne gardez que les premières lettres pour créer un mot de passe complexe mais mémorisable 4 .

Stockage sécurisé : Ne stockez pas vos mots de passe sur des supports non sécurisés comme des post-it ou des fichiers texte non chiffrés 4 .

En suivant ces règles, vous améliorez considérablement la sécurité de vos mots de passe et réduisez les risques de piratage de vos comptes.

Donne moi trois types d'attaque sur une infrastructure

3.L'ingénierie sociale, c'est quoi ?

L'ingénierie sociale est une stratégie de manipulation psychologique utilisée par des cybercriminels pour tromper les individus et les inciter à divulguer des informations sensibles ou à effectuer des actions compromettant leur sécurité 1 2 . Cette technique repose sur l'exploitation des vulnérabilités humaines plutôt que sur des failles techniques des systèmes informatiques 1 3 .

Les principales caractéristiques de l'ingénierie sociale sont :

Manipulation psychologique : Les attaquants exploitent les émotions et le comportement humain pour gagner la confiance de leurs victimes 1 3 .

Usurpation d'identité : Les cybercriminels se font souvent passer pour des personnes ou des sources fiables 1 .

Objectifs variés : Les attaques visent à obtenir des informations confidentielles, des accès non autorisés, ou à inciter les victimes à télécharger des logiciels malveillants 2 3 .

Diverses formes d'attaques : L'ingénierie sociale peut se manifester par des e-mails de phishing, des appels téléphoniques frauduleux, ou même des interactions en personne 3 5 .

Efficacité redoutable : Cette méthode est considérée comme l'une des principales causes de compromission des réseaux, car elle contourne les dispositifs de sécurité technique 3 .

Pour se protéger contre l'ingénierie sociale, il est crucial de sensibiliser et ancien régulièrement les employés, d'établir des politiques de sécurité strictes, et d'utiliser des outils de sécurité avancés 5 . La vigilance et la méfiance face aux demandes inhabituelles ou urgentes sont essentielles pour prévenir ces attaques 

4.Dans mon infra, j'ai 5 PC avec les IP suivantes :
PC1 : 192.168.10.20/16
PC2 : 192.168.10.25/16
PC 3 : 192.168.11.10/15
PC 4: 192.168.11.11/16
PC 5 : 192.168.12.25/16

Avec quels PC chacun peut il communiquer ?

Analyse des sous-réseaux
PC1, PC2, PC4 et PC5 ont un masque /16 (255.255.0.0)

PC3 à un masque /15 (255.254.0.0)

Communication entre les PC
PC1 (192.168.10.20/16) peut communiquer avec :

PC2, PC4, PC5 (même sous-réseau /16)

PC3 (sous-réseau plus large /15 qui englobe 192.168.10.0 et 192.168.11.0)

PC2 (192.168.10.25/16) peut communiquer avec :

PC1, PC4, PC5 (même sous-réseau /16)

PC3 (sous-réseau plus large /15 qui englobe 192.168.10.0 et 192.168.11.0)

PC3 (192.168.11.10/15) peut communiquer avec :

Tous les autres PC (son sous-réseau /15 englobe les adresses 192.168.10.0 à 192.168.11.255)

PC4 (192.168.11.11/16) peut communiquer avec :

PC1, PC2, PC5 (même sous-réseau /16)

PC3 (sous-réseau plus large /15 qui englobe 192.168.11.0)

PC5 (192.168.12.25/16) peut communiquer avec :

PC1, PC2, PC4 (même sous-réseau /16)

PC3 (sous-réseau plus large /15 qui englobe 192.168.12.0)

En résumé, PC3 peut communiquer avec tous les autres PC grâce à son masque de sous-réseau plus large (/15). Les autres PC (PC1, PC2, PC4, PC5) peuvent communiquer entre eux et avec PC3, car ils partagent le même sous-réseau /16 ou sont inclus dans le sous-réseau /15 de PC3.

5.Dans l'entreprise, on veut utiliser le logiciel propriétaire "Machin". Mon collègue me dit qu'il a un truc pour ne pas payer : on peut retirer via une commande la limite de renouvellement de la période d'essai. Qu'en penses tu ?

Ce que votre collègue propose est illégal et contraire à l'éthique. Contourner les limitations d'un logiciel propriétaire comme "Machin" est une violation des conditions d'utilisation et du droit d'auteur 1 2 . Voici pourquoi cette approche est problématique :

Violation de licence : Les logiciels propriétaires sont protégés par des licences qui impliquent les conditions d'utilisation. Contourner ces restrictions est une infraction 1 .

Risques légaux : L'entreprise s'expose à des poursuites judiciaires et à de lourdes amendes pour utilisation non autorisée de logiciel 2 .

Risques de sécurité : Modifier le logiciel peut nuire à sa sécurité et exposer l'entreprise à des vulnérabilités 3 .

Absence de support : L'utilisation non autorisée privée de l'entreprise du support technique et des mises à jour de sécurité essentielles 2 .

Problèmes éthiques : Cette pratique nuit aux développeurs et peut décourager l'innovation dans l'industrie du logiciel 1 .

Au lieu de cela, l'entreprise devrait envisager des alternatives légales et éthiques :

Négocier une licence adaptée aux besoins de l'entreprise

Explorateur des options de tarification flexibles, comme des licences réseau ou des abonnements 3

Considérer des alternatives open source ou des logiciels gratuits qui répondent aux besoins de l'entreprise 6

En conclusion, il est crucial de respecter les termes des licences logicielles pour maintenir l'intégrité et la conformité de l'entreprise 

6.C'est quoi l'intérêt des OS légers ? Peux tu m'expliquer ce qu'est une architecture 0 client ? Quel est le désavantage?

Les systèmes d'exploitation (OS) légers présentent plusieurs avantages :

Sécurité renforcée : Ils offrent un système en lecture seule, limitant les risques de modifications non autorisées et de stockage de données sensibles 1 .

Gestion centralisée : Les administrateurs peuvent facilement gérer et mettre à jour ces systèmes à distance 1 .

Démarrage rapide : Ils permettent une connexion quasi instantanée au bureau virtuel 1 .

Réduit : Ils obligatoirement moins de maintenance et ont une durée de vie plus longue 2 .

Respect de l'environnement : Ils sont économes en énergie et réduisent le besoin de remplacer fréquemment le matériel 1 .

L'architecture zéro client, également appelée client zéro, est une approche minimaliste des postes de travail. Contrairement aux clients légers, les clients zéros n'ont pas de système d'exploitation local ni de capacité de stockage 2 . Ils se connectent directement à un serveur central pour aux ressources et applications.

Le principal désavantage de l'architecture zéro client est sa dépendance totale au réseau. En cas de problème de connectivité, les utilisateurs ne peuvent pas travailler 2 . De plus, les clients zéros peuvent manquer de flexibilité et avoir des limitations de performances, surtout pour les applications exigeantes 2 . Ils sont également parfois liés à des éditeurs spécifiques, ce qui peut restreindre leurs cas d'utilisation

7.C'est quoi un cluster d'hyperviseur ?

Un cluster d'hyperviseur, également appelé cluster de basculement d'hyperviseur, est une configuration de haute disponibilité qui regroupe plusieurs serveurs physiques (nœuds) exécutant un hyperviseur en une seule entité logique. Cette architecture offre plusieurs avantages clés :

Haute disponibilité : Le cluster assure la continuité de service des machines virtuelles en cas de défaillance d'un nœud 1 3 .

Répartition de charge : Il permet d'équilibrer automatiquement la charge de travail entre les différents nœuds 2 .

Évolutivité : On peut facilement ajouter des nœuds pour augmenter les ressources disponibles 4 .

Migration dynamique : Les machines virtuelles peuvent être déplacées d'un nœud à l'autre sans interruption de service 2 .

Gestion centralisée : L'ensemble du cluster peut être administré comme une seule entité 1 .

Optimisation des ressources : Les ressources matérielles sont utilisées plus efficacement grâce à la consolidation des charges de travail 2 .

Dans un cluster d'hyperviseur, chaque nœud exécute le même hyperviseur (par exemple, VMware ESXi, Microsoft Hyper-V, ou Nutanix AHV) et partage un stockage commun. Cette configuration permet d'assurer la redondance et la continuité des opérations en cas de panne matérielle ou de maintenance planifiée, ce qui en fait une solution idéale pour les environnements critiques nécessitant une disponibilité 

8.Explique moi ce qu'est la haute disponibilité

La haute disponibilité (HA) est la capacité d'un système informatique à fonctionner de manière continue et fiable, avec un minimum d'interruptions. Voici les principaux aspects de la haute disponibilité :

Objectifs
Assurer une disponibilité quasi-continue (99,999% du temps ou "cinq neufs")

Minimiser les temps d'arrêt et les interruptions de service

Garantir la continuité des opérations, même en cas de panne

Caractéristiques
Redondance : Utilisation de composants redondants pour éviter les points de défaillance uniques

Répartition de charge : Distribution du trafic entre plusieurs serveurs

Basculement automatique : Transfert rapide vers des systèmes de secours en cas de panne

Réplication des données : Copie des données sur plusieurs systèmes pour assurer leur disponibilité

Importance
Maintien de la productivité et de la satisfaction client

Réduction des pertes financières liées au temps d'arrêt

Amélioration de la résilience face aux pannes matérielles et logicielles

Renforcement de la compétitivité, particulièrement dans les secteurs critiques

Mise en œuvre
Utilisation de clusters de serveurs

Mise en place de systèmes de sauvegarde et de récupération efficaces

Surveillance proactive et gestion des incidents

Conception d'architectures évolutives et flexibles

La haute disponibilité est cruciale pour les entreprises modernes, car elle garantit que les services restent accessibles et performants, même face aux défis techniques ou aux catastrophes imprévues

9.C'est quoi la règle 3-2-1 ?

La règle de sauvegarde 3-2-1 est une stratégie de protection des données qui consiste à :

Conserver 3 copies des données :

La copie originale

Deux copies de sauvegarde supplémentaires

Utiliser 2 types de supports de stockage différents :

Par exemple, un disque dur local et un stockage cloud

Garder 1 copie hors site :

Stockée dans un emplacement géographique différent ou dans le cloud

Cette méthode offre plusieurs avantages :

Réduit les risques de perte totale des données

Protège contre divers scénarios de défaillance (pannes matérielles, erreurs humaines, catastrophes naturelles, cyberattaques)

Assurer une meilleure continuité des opérations

Facilite la récupération des données en cas de besoin

La règle 3-2-1 est largement reconnue comme une bonne pratique en matière de sauvegarde des données, offrant un équilibre entre sécurité et praticité pour les entreprises et les particuliers

10.Explique moi ce qu'est le Raid 0 / le Raid 1 / le Raid 5 / le Raid 10.
Lesquels sont performants ? Quelle est la tolérance de perte pour chacun ?

Les différents types de RAID (Redundant Array of Independent Disks) sont des configurations de stockage qui offrent diverses combinaisons de performances, capacité et tolérance aux pannes. Voici une explication des RAID 0, 1, 5 et 10 :

RAID 0
Fonctionnement : Les données sont réparties (striping) sur plusieurs disques.

Performance : Très élevée en lecture et écriture.

Tolérance aux pannes : Aucune. La perte d'un seul disque entraîne la perte de toutes les données 1 5 .

RAID 1
Fonctionnement : Les données sont dupliquées (mirroring) sur deux disques ou plus.

Performance : Bonne en lecture, mais plus lente en écriture que le RAID 0 5 .

Tolérance aux pannes : Élevée. Peut supporter la perte d'un disque sans perte de données 5 .

RAID 5
Fonctionnement : Les données et les informations de parité sont réparties sur au moins trois disques.

Performance : Bonne en lecture, mais plus lente en écriture due au calcul de parité 2 3 .

Tolérance aux pannes : Peut supporter la perte d'un seul disque 2 3 .

RAID 10
Fonctionnement : Combinez le striping du RAID 0 et le miroir du RAID 1.

Performances : Très élevées, similaires au RAID 0 1 4 .

Tolérance aux pannes : Élevée. Peut supporter la perte de plusieurs disques tant qu'ils ne sont pas dans le même sous-ensemble miroir 1 .

Performance et tolérance aux pannes
Performances : RAID 0 et RAID 10 sont les plus performants 4 .

Tolérance aux pannes :

RAID 0 : Aucune tolérance

RAID 1 : Peut perdre un disque

RAID 5 : Peut perdre un disque

RAID 10 : Peut perdre plusieurs disques (selon la configuration)

En résumé, le RAID 0 offre les meilleures performances mais sans tolérance aux pannes, tandis que le RAID 10 combine haute performance et bonne tolérance aux pannes. Le RAID 1 et le RAID 5 offrent un bon équilibre entre performances et redondance, avec le RAID 5 permettant une meilleure utilisation de l'espace disque.

11.A quoi ça sert la redondance ?

La redondance en informatique sert principalement à :

Améliorer la disponibilité : Elle garantit que les systèmes restent opérationnels même en cas de défaillance, notamment les temps d'arrêt 1 2 .

Assurer la continuité d'activité : En cas d'incident, la redondance permet de maintenir les services essentiels en fonctionnement 3 .

Renforcer la tolérance aux pannes : Les systèmes redondants peuvent résister aux défaillances matérielles ou logicielles sans perturbation significative 1 4 .

Améliorer les performances : La répartition de la charge entre les composants redondants peut optimiser les performances globales du système 1 4 .

Augmenter la sécurité des données : La redondance permet de répliquer les données sur des serveurs distants, prévenant ainsi la perte d'informations en cas de défaillance ou de cyberattaque 4 5 .

Renforcer la résilience : En fournissant des composants ou des systèmes de secours, la redondance agit comme un filet de sécurité contre les défaillances inattendues 5 .

Améliorer la fiabilité globale : En intégrant des éléments de sauvegarde, la redondance contribue à créer un environnement informatique plus robuste et fiable 5 .

En somme, la redondance est une stratégie clé pour garantir la stabilité, la fiabilité et la continuité des opérations informatiques dans les entreprises modernes.
 
12.Un employé quitte l'entreprise. Quelles sont les bonnes pratiques à mettre en place ?
J'ai une infra avec un Active Directory monté
Voici les bonnes pratiques à mettre en place lors du départ d'un employé, en tenant compte de votre infrastructure avec Active Directory :

Notification et planification

Confirmez officiellement la date de départ

Élaborez une feuille de route détaillée du processus de départ

Planifiez un rétroplanning des actions à réaliser 3

Communication

Informez l'équipe du départ de manière claire et transparente

Annoncez le départ rapidement pour éviter les rumeurs 3

Transmission des connaissances

Organisez des sessions de passion avec le collaborateur sortant

Documentez les tâches et responsabilités du poste 2

Gestion des accès

Désactivez le compte utilisateur dans Active Directory

Modifier les mots de passe des comptes associés

Supprimer l'utilisateur des groupes de distribution email 6

Révoquez l'accès aux systèmes d'information et ressources numériques 2

Récupération du matériel

Collectez tous les équipements fournis par l'entreprise (ordinateur, téléphone, badge, etc.) 3

Gestion administrative

Préparez la vente de tout compte

Établissez les documents de fin de contrat (certificat de travail, attestation Pôle Emploi) 3

Entretien de sortie

Conduisez un entretien pour recueillir les feedbacks de l'employé sur son expérience 2

Sécurité des données

Vérifiez qu'aucune donnée sensible n'a été copiée ou transférée

Assurez-vous que tous les accès ont été correctement révoqués 4

Mise à jour des systèmes

Supprimez ou archivez le compte utilisateur dans les différents systèmes

Mettre à jour les listes de diffusion et les organigrammes

Suivi post-départ

Vérifiez que toutes les étapes du processus d'offboarding ont été complétées

Assurez-vous que la transition des responsabilités s'est bien déroulée

En suivant ces bonnes pratiques, vous assurerez une transition en douceur et sécurisée lors du départ d'un employé, tout en protégeant les intérêts de votre entreprise.

16.Je veux permettre au nouveau site de l'entreprise d'accéder au réseau du siège. Qu'est ce que je mets en place ? Explique moi le fonctionnement
17.Quels moyens peux tu me donner pour prendre la main à distance sur un poste?

Voici plusieurs moyens efficaces pour prendre la main à distance sur un poste :

Logiciels dédiés
TeamViewer : Solution populaire et polyvalente, permettant un accès rapide et sécurisé à distance 1 5 .

AnyDesk : Offre une connectivité fluide sur divers appareils avec une faible latence 2 .

Zoho Assist : Idéal pour l'assistance en temps réel et l'accès sans surveillance, avec des fonctionnalités de sécurité avancées 2 3 .

Splashtop : Solution performante pour le support à distance sur plusieurs plateformes 4 .

Solutions intégrées
Microsoft Remote Desktop : Outil gratuit intégré à Windows, efficace pour l'accès à distance entre les appareils Windows 2 .

Chrome Remote Desktop : Extension du navigateur Chrome permettant un accès à distance simple 3 .

Assistance rapide Windows : Outil intégré à Windows pour une assistance ponctuelle 3 .

Solutions pour entreprises
Scalefusion UEM : Plateforme complète pour la gestion et le support à distance des appareils d'entreprise 2 .

ManageEngine Remote Access Plus : Outil professionnel pour la gestion centralisée des postes de travail 3 7 .

GLPI : Logiciel open source de gestion de services informatiques incluant des fonctionnalités de prise en main à distance 6 .

Ces solutions offrent différentes fonctionnalités adaptées à divers besoins, que ce soit pour un usage personnel, professionnel ou en entreprise. Il est important de choisir l'outil le plus approprié en fonction de vos exigences spécifiques en termes de sécurité, de fonctionnalités et de compatibilité avec les systèmes d'exploitation utilisés.
18.On veut installer une téléphonie interne dans l'entreprise. Qu'est ce qui doit être mis en place ?

Pour installer une téléphonie interne dans votre entreprise, plusieurs éléments doivent être mis en place :

Infrastructure de base
Standard téléphonique : C'est le cœur du système qui gère les appels internes et externes. Il peut être physique (PABX) ou virtuel (IPBX) 1 4 .

Réseau : Une infrastructure réseau robuste est nécessaire, surtout si vous optez pour une solution VoIP 6 .

Équipements
Téléphones fixes : Des postes filaires IP pour les bureaux, offrant des fonctionnalités comme le transfert d'appel et la mise en attente 2 .

Téléphones sans fil (DECT) : Pour la mobilité au sein de l'entreprise 3 4 .

Casques : Essentiels pour le confort des utilisateurs, surtout dans les centres d'appels 4 .

Équipements de conférence : Pour les réunions à distance 2 .

Logiciels et services
Softphones : Applications permettant d'utiliser un ordinateur ou un smartphone comme téléphone 4 6 .

Serveur Vocal Interactif (SVI) : Pour orienter les appels entrants vers les différents services 1 .

Messagerie vocale : Pour enregistrer les messages en cas d'absence 6 .

Outils de collaboration : Intégration de fonctionnalités comme la vidéoconférence et le partage d'écran 6 7 .

Gestion et sécurité
Portail d'administration : Pour gérer facilement les lignes, les services et les configurations 7 .

Système de chiffrement : Pour sécuriser les communications, surtout avec les solutions IP 7 .

En mettant en place ces éléments, vous obtiendrez un système de téléphonie interne complet et adapté aux besoins de votre entreprise. Il est important de choisir une solution évolutive qui pourra s'adapter à la croissance de votre entreprise 6
19.Quelle est la différence entre un protocole et sa version sécurisée ?

La principale différence entre un protocole et sa version sécurisée réside dans l'ajout de mécanismes de sécurité pour protéger les communications :

Chiffrement des données : La version sécurisée utilise des algorithmes de chiffrement pour rendre les données illisibles aux niveaux non autorisés 2 3 .

Authentification : Les protocoles sécurisés mettent en place des mécanismes pour vérifier l'identité des parties communicantes, souvent via des certificats numériques 2 .

Intégrité des données : Des mécanismes sont ajoutés pour s'assurer que les données n'ont pas été altérées pendant la transmission 3 .

Établissement de connexion sécurisée : Les versions sécurisées utilisent généralement un processus d'établissement de liaison (handshake) pour négocier les paramètres de sécurité 2 .

Vulnérabilités réduites : Les versions sécurisées corrigent souvent les failles de sécurité connues dans les versions non sécurisées 3 .

Par exemple, HTTPS est la version sécurisée de HTTP. Il ajoute une couche de chiffrement SSL/TLS pour protéger les données échangées entre le client et le serveur 6 . De même, SFTP est une version sécurisée de FTP qui implémente le chiffrement et l'authentification pour la transmission de fichiers 1 .

En résumé, la version sécurisée d'un protocole vise à garantir la confidentialité, l'intégrité et l'authentification des communications, contrairement à la version non sécurisée qui se concentre uniquement sur la fonctionnalité de base.
20.C'est quoi la différence entre crypté et chiffré ?
21.Donne moi la différence entre un chiffrement symétrique et asymétrique. Peux tu me donner sommairement le fonctionnement d'une connexion utilisant ces chiffrements ?

22.Je veux empêcher les utililsateurs de mon infra d'accéder au terminal de commande. Comment faire ?

Pour empêcher les utilisateurs de votre infrastructure d'accéder au terminal de commande (CMD), vous pouvez utiliser une stratégie de groupe (GPO) dans un environnement Active Directory. Voici comment procéder :

Ouvrez la console de gestion des stratégies de groupe (GPMC).

Créer une nouvelle GPO ou modifier une GPO existante.

Naviguez vers : Configuration utilisateur > Stratégies > Modèles d'administration > Système.

Recherchez le paramètre "Désactiver l'accès à l'invitation de commandes".

Double-cliquez sur ce paramètre et sélectionnez « Activé ».

Appliquez le GPO aux unités d'organisation (OU) contenant les utilisateurs concernés.

Exécutez la commande "gpupdate /force" sur le serveur pour forcer la mise à jour des stratégies de groupe.

Cette GPO empêchera les utilisateurs d'accéder à l'invite de commandes (CMD) sur leurs postes de travail 1 3 .

Pour renforcer davantage la sécurité, vous pouvez également bloquer l'accès à PowerShell en ajoutant une règle dans la politique de restriction logicielle pour bloquer "powershell.exe" 2 .

N'oubliez pas de tester la configuration sur un groupe restreint d'utilisateurs avant de la déployer à grande échelle pour vous assurer qu'elle n'interfère pas avec les applications ou les tâches légitimes des utilisateurs.

23.Dans l'infra, nous avons des services web. Comment assurer que l'infra ne puisse pas être menacée à travers eux ?

Pour assurer que votre infrastructure ne soit pas menacée à travers vos services web, vous pouvez mettre en place plusieurs mesures de sécurité :

Pare-feu d'application web (WAF) :
Déployez un WAF pour filtrer et surveiller le trafic HTTP, protégeant ainsi vos applications web contre diverses attaques comme les injections SQL, le cross-site scripting (XSS) et les falsifications de requêtes intersites 4 .

Protection anti-DDoS :
Mettez en place une solution anti-DDoS pour prévenir les attaques par déni de service qui pourraient surcharger vos systèmes 2 3 .

Équilibrage de charge (Load Balancing) :
Utilisez des load balancers pour répartir le trafic et améliorer la résilience de vos services web 3 .

Réseau de diffusion de contenu (CDN) :
Implémentez un CDN pour améliorer les performances et offrir une couche supplémentaire de protection contre certaines attaques 3 .

Chiffrement TLS :
Assurez-vous que toutes les communications sont chiffrées en utilisant le protocole TLS pour garantir la confidentialité et l'intégrité des données échangées 3 .

Gestion des identités et des accès (IAM) :
Mettre en place un système IAM robuste pour contrôler et surveiller les accès aux ressources de votre infrastructure 5 .

Systèmes de détection et de prévention d'intrusion (IDS/IPS) :
Déployez ces systèmes pour analyser le trafic réseau et détecter les comportements suspects ou malveillants 5 .

Audits de sécurité réguliers :
Effectuez des audits de code source et des tests de pénétration pour identifier et corriger les vulnérabilités potentielles 2 .

Mises à jour et correctifs :
Maintenez tous les composants de votre infrastructure à jour, y compris les systèmes d'exploitation, les serveurs web et les applications 1 .

Surveillance continue :
Mettez en place une surveillance en temps réel de votre infrastructure pour détecter rapidement toute activité suspecte 5 .

En combinant ces différentes mesures, vous créez une défense en profondeur qui renforcera considérablement la sécurité de votre infrastructure web contre les menaces potentielles.

24.C'est quoi la différence entre UDP et TCP ?

25.De quoi a besoin un DNS pour fonctionner ? A Quoi sert il ?

26.Je veux empêcher les connexions à un site depuis l'infra. Comment faire ?

Pour empêcher les connexions à un site spécifique depuis votre infrastructure, vous avez plusieurs options :

Bloquer au niveau du routeur :

Accédez à l'interface d'administration de votre routeur

Recherchez les paramètres de contrôle parental ou de filtrage d'URL

Ajoutez le site que vous souhaitez bloquer à la liste des URL interdites

Appliquez les changements et redémarrez le routeur si nécessaire 1 2

Modifier le fichier hosts :

Ouvrez le fichier hosts (généralement situé dans C:\Windows\System32\drivers\etc\hosts sur Windows)

Ajouter une ligne au format : 0.0.0.0 www.example.com

Répétez pour chaque domaine à bloquer

enregistrez le fichier et redémarrez le navigateur 3 5

Utiliser un pare-feu réseau :

Configurez votre pare-feu réseau pour bloquer les connexions vers le site souhaité

Cette méthode est plus complète et s'applique à tous les appareils du réseau 4

Implémenter une solution de filtrage Web :

Utilisez un logiciel de sécurité ou une solution de gestion unifiée des terminaux

Ces outils offrent des fonctionnalités avancées de filtrage et de gestion centralisée 5

Configurer les extensions du navigateur :

Installer des extensions de blocage de sites Web sur les navigateurs utilisés dans votre infrastructure 5

Pour une solution efficace et facile à gérer, le blocage au niveau du routeur ou l'utilisation d'une solution de filtrage Web professionnel sont généralement recommandés pour les infrastructures d'entreprise.

27.C'est quoi ça :

table ip filter {
   chain input {
       type filter hook input priority 0;        
       ct state established accept        
       ip saddr 192.168.10.0/24 tcp dport 22 accept        
       tcp dport 80 drop        
       policy drop
   }
}

Ce code représente une configuration de pare-feu utilisant nftables, le successeur d'iptables pour le filtrage de paquets dans le noyau Linux. Voici une explication détaillée de cette configuration :

table ip filter: Définit une table nommée "filter" pour le protocole IP.

chain input: Crée une chaîne nommée "input" dans ce tableau.

type filter hook input priority 0: Configurez cette chaîne pour intercepter le trafic entrant avec une priorité de 0.

ct state established accept: Acceptez les paquets faisant partie de connexions déjà établies.

ip saddr 192.168.10.0/24 tcp dport 22 accept: Autoriser les connexions SSH (port 22) provenant du réseau 192.168.10.0/24.

tcp dport 80 drop: Rejetez tout le trafic HTTP entrant (port 80).

policy drop: Définit la politique par défaut de la chaîne comme "drop", ce qui signifie que tout trafic non précisé autorisé sera rejeté.

Cette configuration crée un pare-feu simple qui :

Autoriser le trafic des connexions établies

Permet les connexions SSH depuis un réseau spécifique

Bloquer les connexions HTTP entrantes

Rejeter tout autre trafic entrant

C'est une approche de sécurité restrictive qui n'autorise que le trafic spécifiquement permis.






