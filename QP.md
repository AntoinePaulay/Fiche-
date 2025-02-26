Q.1.1 Le serveur SRVWIN01 a le rôle DHCP d'installé et correctement configuré et activé.
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

Q.1.2 Sur le serveur on exécute la commande ipconfig /all. On a une adresse 172.16.10.10/24. Si on exécute la même commande sur le client, on a une adresse 172.16.100.50/24.
Pourquoi un ping ne fonctionne pas entre ces 2 machines ?

Les 2 adresses de réseaux sont différente au niveau du 3ème octet, et le masque est en « /24 ». Cela
indique que ce sont des réseaux différents (au niveau d’IP). En l’absence de passerelle, ils ne peuvent
pas communiquer.

Q.1.3 Si le client a une configuration IP en DHCP, pour quelle(s) raison(s) il ne prend pas la première adresse configurée sur la plage des adresses IP mais une autre adresse ?

Les raisons peuvent être les suivantes :
– Réutilisation d’une adresse déjà utilisée si le bail DHCP n’est pas terminé
– La première adresse disponible est peut-être déjà prise par une autre machine
– La première adresse disponible est peut-être sur une plage en exception
– Le client a peut-être une adresse réservée sur la plage DHCP qui n’est pas cette première adresse

Q.1.4 Est-il possible de forcer un client à prendre une adresse IP spécifique en DHCP ? Si oui explique comment procéder sur le client et sur le serveur.

Oui c’est possible de forcer le client à avoir une adresse IP spécifique.
Sur le serveur on va créer une réservation d’adresse IP pour l’adresse MAC du client.
Le client doit être en DHCP, pas de configuration supplémentaire à faire.
Ensuite il faut redémarrer le client ou exécuter la commande « ipconfig /renew »

Q.1.5 Le résultat de la commande ipconfig /all donne aussi, sur le client et sur le serveur, une adresse IPv6 qui commence par fe80...
Quelle est cette adresse et à quoi sert-elle ?

Une adresse IPv6 qui commence par fe80 est une adresse de type unicast lien local (Link Local Address
ou LLA).
C’est une adresse non-routable, qui sert à communiquer sur un même réseau. Son préfixe réseau est
fe80 ::/10, mais en général on trouve fe80 ::/64. Elle est gérée dynamiquement par SLAAC.

Q.1.6 Quel autre type d'adresse IPv6 peut-on avoir sur une machine ? Précise le type, le préfixe, et la porté.

Il existe aussi en IPv6 :
– Les adresses unicast locales uniques (Unicast Local Address ou ULA), avec le préfixe fc00::/7 ou
fd00::/8, avec une portée au sein d’un réseau interne, mais non-routable sur internet
– Les adresses unicast globales (Global Unicast Address ou GUA), avec le préfixe 2000::/3 ou 2001
::/16, routable sur internet
– Les adresses de bouclage, qui sont ::1/128, qui correspond à la machine elle-même
– Les adresses multicast, avec le préfixe ff00::/8, pour envoyer des paquets à un groupe de
machines

Q.1.7 Est-il possible de configurer le service DHCPv6 sur le serveur SRVWIN01 avec des préfixes d'adresses Unicast Locales Uniques ?
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

Q.1.8 Entre 2 machines on exécute la commande ping avec le nom des machines au lieu de l'adresse IP. La commande s'exécute correctement.
Quel protocole permet cela ?
Le protocole qui permet cette résolution de nom est le DNS (Domain Name System).

Q.1.9 Si l'IPv6 est activé, le retour de la commande ping avec le nom d'une machine est une adresse IPv6. Pourquoi ?
Quelle option de la commande ping permet d'avoir une adresse IPv4 ?

Par défaut on a une adresse IPv6 car le protocole ICMP priorise l’IPv6 sur l’IPv4.

On peut avoir le retour d’adresse en IPv4 avec l’option « -4 » de la commande ping.

Q.1.10 À partir du serveur, je veux pouvoir faire un ping vers le client à partir de son nom, et également avec un alias de ce nom, par exemple CLIENT_TEST, comment procéder ?

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

Q.3.1 Quels sont les matériels A et B ? Quel est leur rôle sur un réseau ? Sur quelle couche du modèle OSI fonctionnent-ils ?

Matériel A (type, rôle sur un réseau, couche du modèle OSI) ?
Le matériel réseau A est un commutateur, communément appelé switch.
Son rôle est de connecter les ordinateurs entre-eux et de faciliter leur
communication avec les adresses MAC.
Il intervient au niveau de la couche 2 du modèle OSI.

Matériel B (type, rôle sur

Q.3.2 Rempli le tableau suivant pour les PC du réseau :

| PC  | Adresse Réseau | 1ère @IP | Dernière @IP | @IP Diffusion |
|---|---|---|---|---|
| PC1 | 10.10.0.0/16 | 10.10.0.1 | 10.10.255.254 | 10.10.255.255 |
| PC2 | 10.11.0.0/16 | 10.11.0.1 | 10.11.255.254 | 10.11.255.255 |
| PC3 | 10.10.0.0/16 | 10.10.0.1 | 10.10.255.254 | 10.10.255.255 |
| PC4 | 10.10.0.0/16 | 10.10.0.1 | 10.10.255.254 | 10.10.255.255 |
| PC5 | 10.10.0.0/15 | 10.10.0.1 | 10.11.255.254 | 10.11.255.255 |
	
Q.3.3 Quel est le rôle de A pour un ping de PC1 vers PC3 ?
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

Q.3.4 Quel est le rôle de B pour un ping de PC2 vers PC4 ?
Explique si cette communication a réussi.

Rôle de B pour un ping de PC2 vers PC4
PC2 a l’adresse de réseau 10.11.0.0/16 et PC4 a l’adresse 10.10.0.0/16. Ils ne sont donc pas sur le
même réseau. B est un commutateur (ou switch) et ne peut pas transmettre des paquets IP entre des
hôtes de réseaux différents.
Donc B n’a aucun rôle dans cette communication.

Communication réussi ?

Non la communication échoue parce que les 2 machines ne sont pas sous le même réseau.

Q.3.5 Le résultat d'un ping de PC5 vers PC2 est 10.11.80.2 icmp_seq=1 timeout.
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

Q.3.6 Quels sont les matériels C et D ? Quel est leur rôle sur un réseau ? Sur quelle couche du modèle OSI fonctionnent-ils ?

Matériel C et D (type, rôle sur un réseau, couche du modèle OSI) ?
Ce sont des routeurs.

Le rôle d’un routeur est de faire communiquer des réseaux différents entre-eux. Les PC du réseaux
10.10.0.0/16 peuvent communiquer avec les machines des autres réseaux. Il se sert des adresses IP et
intervient au niveau de la couche 3 du modèle OSI.

Q.3.7 Comment PC3 peut envoyer des paquets IP en dehors de son réseau ?
Quel matériel l'aide dans cette tâche ?

Moyen de PC3 pour sortir du réseau ?
PC3 va utiliser sa passerelle par défaut.

Matériel du réseau servant à cette tâche ?

Q.3.8 Le matériel trouvé à la question précédente remplit-il ce rôle pour tous les PC du réseau (PC1~5) ?

Le matériel servant à PC3 sert aussi pour tous les autres PC du réseau ?
Tout les PC sauf PC2 qui a l’adresse IP 10.11.80.2 avec l’adresse de réseau 10.11.0.0/16, et donc qui
n’est pas dans le même réseau que celui de l’interface g2/0 du routeur C.

Q.3.9 Sur C, pour l'interface g1/0, que signifie le g, le 1, et le 0 ?

Pour le matériel C, pour le label « g1/0 », Que signifie le « g » ?
Le « g » indique que l’interface réseau est en Gigabit, ou Gigabit Ethernet.

Le « 1 » ?
Dans « g1/0 » le « 1 » correspond au numéro du module (ou slot). La numérotation commence à 0, donc
« g1/0 » indique que l’on est sur le 2ème module.

Le « 0 » ?
Dans « g1/0 » le « 0 » correspond au numéro de l’interface réseau du module correspondant, soit le port.
La numérotation commence à 0, donc « g1/0 » indique que l’on est sur le 1er port.

Q.3.10 Sur C, que faut-il configurer pour atteindre le réseau 172.16.5.0/24 et au moyen de quelle commande ?
Cette action est-elle effective au niveau du routeur ou d'une interface en particulier ?

Ligne de commande ?
Il faut configurer une règle de routage, soit sur le routeur C : « ip route 172.16.5.0 255.255.255.0
10.12.2.253 ». Cela indique au routeur que les paquets à destination du réseau 172.16.5.0/24 doivent
passer par la passerelle 10.12.2.253.

Périmètre de la commande ?
Cette commande est effective au niveau du routeur C et pas d’une interface réseau en particulier.

Q.3.11 Rempli le tableau ci-dessous qui contient les informations de la trame ethernet suivant l'emplacement sur le réseau pour un ping depuis PC1 vers le réseau 172.16.5.0/24.

| Emplacement sur le réseau | Adresse MAC destination | Adresse MAC source | @IP Source | @IP Destination |
|---|---|---|---|---|
| Entre PC1 et A | CA:01:DA:D2:00:08 | 00:50:79:66:68:00 | 10.10.4.1 | 172.16.5.x |
| Entre B et C | CA:01:DA:D2:00:08 | 00:50:79:66:68:00 | 10.10.4.1 | 172.16.5.x |
| Entre C et D | CA:03:9E:EF:00:38 | CA:01:DA:D2:00:1C | 10.10.4.1 | 172.16.5.x |
| Après D | xx:xx:xx:xx:xx:xx | CA:03:9E:EF:00:54 | 10.10.4.1 | 172.16.5.x |

Ne rempli que le ping request (pas le reply).


1. Assurer le support utilisateur en centre de service

1.1 Du point de vue d'ITIL, quelle est la différence entre un incident et un problème ?

Selon ITIL (Information Technology Infrastructure Library) :
– Incident : C'est un événement imprévu qui perturbe ou diminue la
qualité d'un service informatique. L'objectif avec un incident est de
rétablir le service le plus rapidement possible.
– Problème : C'est la cause sous-jacente ou inconnue d' un ou plusieurs
incidents. L'objectif de la gestion des problèmes est d'identifier et de
résoudre ces causes, de manière à prévenir la récurrence des incidents
associés.

1.2 Quels sont les différents moyens de prendre le contrôle à distance d'une machine ?

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

1.3 Donne les différentes étapes à respecter dans une résolution d'incident par téléphone.

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

2. Exploiter des serveurs Windows et un domaine Active Directory


2.1 Qu'est-ce qu'un rôle FSMO ?

Un rôle FSMO (Flexible Single Master Operation) est la possibilité pour un
contrôleur de domaine, sur un domaine Active Directory, de pouvoir effectuer
des tâches particulières.
Il existe 5 rôles FSMO :
– Maître de schéma
– Maître d’attribution de nom de domaine
– Maître RID
– Maître d’infrastructure
– Émulateur PDC

2.2 En quoi la réplication entre contrôleurs de domaine est primordiale sur un domaine ?

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

3. Exploiter des serveurs Linux

3.1 Quel est le résultat de la commande suivante : chmod u+x /home/tssr/factures/export.sh ?

Cette commande ajoute le droit d'exécution « +x » au propriétaire « u » pour
le fichier « /home/tssr/factures/exports.sh ».

3.2 Quelle commande dois-tu écrire dans un terminal sur un OS Debian pour ajouter l'adresse IP 172.16.8.16/24 à l'interface enp0s8 ?

La commande est :
ip address add 172.16.8.16/24 dev enp0s8
Ensuite il faudra activer l’interface avec “ip link set dev enp0s8 up”

3.3 L'utilisateur Wilder ne parvient plus à accéder au dossier travaux. Explique la cause probable et donne les outils (commandes) pour diagnostiquer et résoudre le problème.
    
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

3.4 D’après les éléments de la capture ci-dessous, si on ajoute un disque dur supplémentaire qui n'a qu'une seule partition, comment se nommera t'elle ?

![image](https://github.com/user-attachments/assets/3d38aad2-57c6-4509-a94e-217aee393fc8)

Le périphérique actuel est /dev/sda1, donc si on ajoute un disque dur avec
une seule partition, elle se nommera sdb1.


3.5 Donne 2 commandes qui te permettent de visualiser les disques et les partitions d'un serveur Linux.

On peut utiliser « lsblk » et « fdisk –l ».

4. Exploiter un réseau IP

4.1 Une entreprise a un réseau 192.160.16.0/25. Elle souhaite le découper en 4 sous-réseaux de taille identiques.
Pour les 2 premiers sous-réseaux, donne l’adresse de réseaux, le masque (en notation CIDR), la première adresse disponible, la dernière adresse disponible, ainsi que l'adresse de broadcast.

![image](https://github.com/user-attachments/assets/1836c571-a512-41f5-a67c-03e8e999de18)

4.2 Complète le tableau de conversion suivant :

| Décimal	| Binaire |	Hexadécimal |
|---|---|---|
| 9 |  0000 1001  | 0x 09  |
| 127	|	0111 1111 | 0x7F |
| 255	| 1111 1111 | 0x FF
| 16	| 00010000 | 0x 10 |

4.3 Pour le schéma ci-dessous :

Quels sont les liens trunk ?

Ce sont les liens entre les switchs et le lien entre un switch et le routeur.

Quelle méthode de routage intervlan est utilisée ?

Le routeur se charge de router entre les VLANs avec des id de vlans différents
sur le même medium. La méthode de routage est « Router-on-a-stick ».

*![image](https://github.com/user-attachments/assets/a636a2c0-c9d0-45e1-ac5e-b89c4f39c95b)


4.4 Sur un réseau IP, 2 PC ont la configuration IP suivante :

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

4.5 Des ordinateurs sont connectés sur un switch qui n'a qu'un seul vlan, avec les configurations IP suivantes :

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

4.6 Quelles sont les actions possibles à mettre en œuvre pour sécuriser un réseau sans fil ?

Les actions possibles sont :
– Changer le nom du SSID : Modifiez le nom par défaut du réseau
– Désactiver la diffusion du SSID (le cacher)
– Durcir le chiffrement (par exemple WPA2 Entreprise ou WPA3)
– Changer le mot de passe par défaut : Choisir un mot de passe fort et
unique
– Utilisez un réseau invité pour les personnes extérieures à l’entreprise
– Mettre à jour le firmware des bornes wifi

4.7 Quelles sont les routes statiques à ajouter sur le routeur Routeur1 pour permettre la communication entre PC0 et PC3 ?


![image](https://github.com/user-attachments/assets/a19eed81-4136-4c2a-95f4-ac3417499ca2)

![image](https://github.com/user-attachments/assets/e9acd5f1-4cf5-4e4f-bddd-b28d1f494033)

4.8 Complète le tableau suivant avec les informations sur les différents services/protocoles :

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


4.9 Sur quels ports du switch peut-on brancher ce téléphone IP ?

![image](https://github.com/user-attachments/assets/a19e3e58-2f9b-493e-bc1d-53fe00de5b46)

Le téléphone IP a un chargeur donc on peut le brancher sur tous les ports,
donc de 1 à 8.


4.10 

![image](https://github.com/user-attachments/assets/1e883bc2-db80-48a2-9ba1-72f93b5b0441)

5. Maintenir des serveurs dans une infrastructure virtualisée
   
5.1 Explique ce qu'est un cluster d'hyperviseur. Quel est l’intérêt d'une telle structure ?

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

5.2 Qu'est-ce qu'un container ? Donne différentes solutions de conteneurisation.

Définition :
Un conteneur est une unité standardisée qui permet d’encapsuler, d’isoler, du
code et toutes ses dépendances. L’application ainsi contenue s’exécute de
manière fiable quel que soit l’environnement ou l’OS.
Exemples de solutions :
Docker,LXC

5.3 Que représente les lignes de code ci-dessous ? Comment les utiliser ?

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

5.4 Pour la copie d'écran ci-dessous, quelle devrait être ta démarche dans une telle situation ?

![image](https://github.com/user-attachments/assets/46a8c127-e38e-4045-9e47-cc183275e794)

On voit une erreur « CRITICAL » sur le service « Swap usage ». On voit « 0% free »
dans la colonne « Status informations ».
Il faut donc augmenter la taille du swap.

5.5 Que veulent dire les termes PaaS, IaaS, et SaaS ?

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

5.6 Dans la mise en oeuvre d'une solution HA (High Availability), quels sont les élements indispensable ?

Les éléments indispensables sont :
– La redondance matérielle (serveur, stockage)
– Un système de répartition de charge (Load Balancing)
– Un système de basculement automatique en cas d’erreur ou de panne
(Failover)
– Une copie (réplication) des données en temps réel
– Un système de supervision
– Un PRA et PCA

6. Maintenir et sécuriser les accès à Internet et les interconnexions des réseaux
6.1 Quel est l'impact des ACL ci-dessous sur la machine 172.16.0.10 ? Peut-on fusionner ces ACL pour n'en former qu'une seule ? Si oui fais-le.

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
   
6.2 Pour les commandes ci-dessous les 2 chaînes de caractères en entrée sont de tailles différentes et pourtant la longueur du résultat des commandes est identique. Pourquoi ?

    wilder@Ubuntu:~$ echo -n "test message" | sha512sum
    950b2a7effa78f51a63515ec45e03ecebe50ef2f1c41e69629b50778f11bc080002e4db8112b59d09389d10f3558f85bfdeb4f1cc55a34217af0f8547700ebf3  -

    wilder@Ubuntu:~$ echo -n "ce message n'a aucun rapport avec le précedent !" | sha512sum
    0096a6b7b1ff9714c8a0ecd308e1c952ec2f956f0f5ae28ec29b3e6b68f16a127ea4c379c4aafce2e4f97c029874628f4d3376440ae87c34f83b225c973f1d0a  -

La commande passé derrière le « pipe » est « sha512sum » qui est une
fonction de hachage cryptographiques.
Ce type de fonction de chiffrement produit toujours une sortie de taille fixe,
quelle que soit la taille de l'entrée. C'est une caractéristique essentielle qui
garantit que la sortie (le hachage) a une longueur constante.

6.3 Sur l'infrastructure réseau représentée par le schéma ci-dessous, que faut-il faire pour que l'on puisse accéder de manière sécurisée au serveur web depuis internet ?

![image](https://github.com/user-attachments/assets/890b9791-2d40-4478-87a4-2d1808d00e09)

6.4 Quels types de VPN sont représentés dans les illustrations suivantes ?

VPN A : VPN type accès distant (host to network)

![image](https://github.com/user-attachments/assets/ecea8230-8de4-40c5-be5b-8bf73f1b45fb)

VPN B : VPN type site à site

![image](https://github.com/user-attachments/assets/4f1c94ce-5aa4-46c7-a106-e1b07040c999)

6.5 Par rapport au schéma ci-dessous, complète le texte en dessous avec les bons termes.

![image](https://github.com/user-attachments/assets/5dd4feae-c378-4067-8b6e-7f9f1305a529)

Pour envoyer un message privé à Bob, Alice utilise- expression 1 -de Bob pour rendre « illisible » le « texte en clair » et Bob utilise- expression 2 -pour transformer le texte « illisible » en « texte en clair ». Ce processus représente un chiffrement- expression 3 -.

Expression 1 :
La clé publique
Expression 2 :
Sa clé privée
Expression 3 :
asymétrique

6.6 Tu as ci-dessous un extrait d’un guide de configuration d’un tunnel VPN site à site en IPSec/ISAKMP. Traduit ce passage en Français.

When ISAKMP negotiations begin, the peer that initiates the negotiation sends all of its policies to the remote peer, and the remote peer tries to find a match. The remote peer checks all of the peer's policies against each of its configured policies in priority order (highest priority first) until it discovers a match.

Texte traduit :
Lorsque les négociations ISAKMP commencent, le nœud qui initie la
négociation envoie toutes ses politiques au nœud distant, et le nœud distant
essaie de trouver une correspondance.
Le nœud distant vérifie toutes les politiques du nœud initiateur contre chacune
de ses politiques configurées dans un ordre de priorité (la plus haute priorité
en premier) jusqu'à ce qu'il trouve une correspondance.

6.7 En matière de sécurité informatique, indique 3 types de menaces (risques et attaques) auxquelles peut être confronté un SI (ne rentre pas dans les détails).

Menace 1 :
Les attaque par déni de service (DDoS)
Menace 2 :
Les ransomware
Menace 3 :
Les attaque virales

7. Mettre en place, assurer et tester les sauvegardes et les restaurations des éléments de l'infrastructure
7.1 Qu'est-ce que la règle 3-2-1 ?
   
   La règle du 3-2-1 est une pratique courante :
– 3 copies (prod + 2 sauvegardes)
– 2 types de support différents
– 1 copie hors-site

7.2 Quelles sont les différents types de sauvegardes à mettre en place en entreprise ?

Les différents types de sauvegarde sont :

– Les sauvegarde complète
– Les sauvegarde incrémentale
– Les sauvegarde différentielle

7.3 Indiquer les différences entre une sauvegarde, de l'archivage, et un clonage.

Une sauvegarde est une copie de données en production actuellement
utilisées. On peut les restaurer en cas de perte ou de corruption.
L’archivage est un stockage à long terme de données qui ne serve plus à la
production, mais qui doivent être conservées pour des raisons légales ou
historiques. Les données sont supprimées de la production.
Le clonage est la réplique exacte d'un système ou d'un disque dur, souvent
utilisé pour déployer des configurations identiques sur plusieurs machines.

8. Exploiter et maintenir les services de déploiement des postes de travail
8.1 Quels avantages apporte la mise en place d'un service centralisé de mises à jour logicielles au sein d'une entreprise ? Indique une solution que tu connais et explique son fonctionnement.

Avantages :
Gestion simplifié et centralisée, politique de sécurité unifiée, réduction des
coût de maintenance.
Exemple de solution :
WSUS (Windows Server Update Services) est un rôle sur un serveur
Windows qui permet de gérer, de télécharger, et de publier des mises à jour à
des postes clients au sein d’un parc informatique.

8.2 Quels sont les inconvénients liés à la mise en place d'une solution de terminaux clients légers par rapport à des postes fixes ?

8.2
Les inconvénient sont les suivants :
– Dépendance au réseau
– Point de défaillance unique (serveur central par exemple)
– Coût initial (infrastructure de serveur pour les clients légers)
– Personnalisation limitée (au niveau utilisateur)

8.3 Que fait l’exécution de la ligne de commande suivante ? Dans quel contexte est-elle utilisée ?

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


8.4 Dans le cadre d'un déploiement de postes clients Linux, quelle est l'utilité d'un dépôt local de paquet ?

Il permet :
– Une accélération des mises à jour sur les clients
– La même version des paquets sur l’ensemble du parc informatique
– Une amélioration de la sécurité car un contrôle est fait par les membres
de l’IT sur les paquets






