Dans la mise en œuvre d'une solution HA (High Availability), quels sont les éléments indispensables ?
Les éléments indispensables sont :\
  .La redondance matérielle (serveur, stockage)\
  .Un système de répartition de charge (Load Balancing)\
  .Un système de basculement automatique en cas d'erreur ou de panne (Failover)\
  .Une copie (réplication) des données en temps réel\ 
  .Un système de supervision\ 
  .Un PRA et PCA ou PRI et PCI\

  
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



Pour envoyer un message privé à Bob, Alice utilise-expression 1 -de Bob pour rendre « illisible » le « texte en clair » et Bob utilise-expression 2 -pour transformer le texte « illisible » en « texte en clair ». Ce processus représente un chiffrement- expression 3 -.

Expression 1 : La clé publique Expression 2 : Sa clé privée Expression 3 : asymétrique



Que voulez-vous dire les termes PaaS, IaaS et SaaS ?
PaaS : PaaS (Platform as a Service) : Fournit une plateforme pour développer et gérer des applications sans se soucier de l'infrastructure nécessaire. 2.IaaS : IaaS (Infrastructure as a Service) : Offre des ressources informatiques virtualisées sur Internet, éliminant le besoin d'acheter ou de gérer une infrastructure physique. \ 3.SaaS : SaaS (Software as a Service) : Logiciels hébergés dans le cloud permettant aux utilisateurs accèdent et utilisent via Internet, souvent via un navigateur web. \


Alternative au stockage local => stockage via le réseau
Mutualisation à l'échelle d'un ensemble de machines
Une seule gestion pour des machines
Machines déplaçables (Notamment machines virtuelles)
2 approches : 
SAN : Storage Area Network
NAS : Network Attached Storage


fdisk -l
lsblk -f

Mappage  3 solution :

. GPO
. Script
. User + dossier pratager 

sudo tail -n10 /var/log/auth.log


adduser

addgroup


sudo usermod -aG <nom groupe> <utilisateur>

cat /etc/passwd
less /etc/passwd

WDS (Windows Deployment Services)

Technologie de Microsoft permettant d'installer un système d'exploitation Windows via le réseau.
Successeur de RIS (Remote Installation Service)
Permet d’installer Windows Vista~10 et server 2008~2016
Rôle disponible sur les versions serveur (depuis 2008 sp2)

Utilité :
Déploiement d’images WIM
Par PXE, fourniture d’images de démarrage (par TFTP)
Utilisation de fichier de réponse xml pour une automatisation

Poste client DHCP + dossier partager

NTP 123 
TFTP 69







