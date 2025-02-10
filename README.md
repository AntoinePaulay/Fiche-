### Bash 

#! /bin/bash
0 : sortie normale, tout va bien

 ## Variable

* $0 : Nom du script invoqué
* $# : Nombre d'arguments du script
* $* : les arguments du script en un seul mot
* $@ : les arguments du script en mots séparés
* $1 : Le premier argument
* $2 : Le deuxième argument
…
* $? : Le code de sortie (status code) de la dernière commande
* $$ : Le process ID du shell
* $! : Le process ID du dernier job en arrière plan

variable=modified

# Comparaison de chaînes

* s1 = s2 : vrai si les chaînes sont identiques
* s1 != s2 : vrai si les chaînes sont différentes
* -z s1 : vrai si s1 est vide
* -n s1 : vrai si s1 n'est pas vide

# Comparaison de nombres

* n1 -eq n2 : vrai si les nombres sont égaux
* n1 -ne n2 : 0 si les nombres sont différents
* n1 -lt n2 : n1 < n2
* n1 -le n2 : n1 <= n2
* n1 -gt n2 : n1 > n2
* n1 -ge n2 : n1 >= n2

Structure conditionnelle if
if condition
then
	instructions
elif conditions2 (optionnel)
then
	instructions
else	 (optionnel)
	instructions
fi

Structure conditionnelle case
case valeur in
valeur1) instructions;;
valeur2 | valeur3) instructions;;
…
*) instructions par défaut;;
esac

Structure itérative for
for variable in liste
do
instructions
done

Structure conditionnelle while
while <condition>
do
instructions
done

Déclaration de fonction
function nom()
{
instructions
}

Linux
* passwd : modifier un mot de passe
* adduser : ajout d'utilisateurs
* deluser : suppression d'utilisateurs
* usermod : modifier un utilisateur
* chfn : modifier la description d'un utilisateur
* chsh : modifier le shell par défaut d'un utilisateur
* chage : modifier durée de validité
* newusers : création d'utilisateurs par lot
* pwck : vérification du format des fichier passwd et shadow
* newgrp : prendre un nouveau groupe
* groupadd : ajout d'un groupe
* groupdel : suppression d'un groupe
* groupmod : modifier un groupe
* grpck : vérification du format des fichier group et gshadow


## Commandes utiles pour consulter les logs

* cat, more, less, zcat, zmore, zless : consultation en entier
* head, tail : consultation en partie
* grep, zgrep : recherche par filtre
* watch : exécution périodique de commande
* dmesg : Cas particulier des logs du noyau
* last, lastb : Cas particulier login/out utilisateurs et échec

### Les gestionnaires de paquets

Debian avec DPKG
Red Hat avec RPM

Debian a APT
Red Hat a YUM

![image](https://github.com/user-attachments/assets/15ba1642-db73-4dd3-9be4-13191f427d5d)

deb : dépôt binaire
deb-src : dépôt de code source


### Mise en place Disque Debian

* La gestion du stockage

Pre-requis :
Avoir une VM Linux avec un 2ème disque de 10 Go

# 1. Trouver le nom du disque

```bash
lsblk
fdisk -l
```

# 2. Création de partitions

Créer 3 partitions avec `fdisk` :
	* /dev/sdb1 --> **ext4** (5 Go)
	* /dev/sdb2 --> **ext4** (3 Go)
	* /dev/sdb3 --> **swap** (espace disque restant)

```bash
sudo fdisk /dev/sdb
```

Puis :
- `n` pour une nouvelle partition
- `1` ou `2` ou `3` pour le numéro de partition
- Touche entrée pour le début du secteur
- `+5G` pour la taille (de la partition 1) (ou touche entrée pour la dernière partition)
- Modifier le type de la partition avec `t`
- Mettre `83` pour un type **ext4** et `82` pour un type **swap**
- `w` pour enregistrer et quitter

Pour supprimer complètement les signatures (et les partitions) sur un disque : 
```bash
sudo wipefs -a /dev/sdb
```
> À ne pas faire sinon tout est perdu !

# 3. Formatage

```bash
sudo mkfs.ext4 -L DATA /dev/sdb1
sudo mkfs.ext4 -L DOSSIER_PERSO /dev/sdb2
sudo mkswap /dev/sdb3
```

Pour vérifier les labels :
```bash
lsblk -o NAME,LABEL
```
Activation du swap :
```bash
sudo swapon /dev/sdb3
```

> Pas besoin formater ni de monter cette partition swap (et donc pas besoin de la démonter)

# 4. Partition /dev/sdb1
## a. Montage dans /mnt

```bash
sudo mkdir /mnt/data
sudo mount /dev/sdb1 /mnt/data
```

## b. Montage du dossier "Important" dans le /home

* Dans `/mnt/data`, créer 2 dossiers :
	* **Important** avec un fichier **fichier_important**
	* **confidentiel** avec un fichier **fichier_confidentiel**
- Créer un dossier `/home/wilder/Data_Perso`
- Monter le dossier **Important** dans `/home/wilder/Data_Perso` :

```bash
sudo mount --bind /mnt/data/Important /home/wilder/Data_Perso`
```

> `--bind` permet de monter un répertoire existant à plusieurs emplacements

# 5. Partition /dev/sdb2

```bash
cd ~
mkdir maPartition2
sudo mount /dev/sdb2 /home/wilder/maPartition2
```

Pb de droits :
```bash
sudo chown wilder:wilder /home/wilder/maPartition2
chmod 755 /home/wilder/maPartition2
```

# 6. Montage automatique au démarrage

Dans /etc/fstab :

```bash
/dev/sdb1  /mnt/data  ext4  defaults  0  0
/mnt/data/Important /home/wilder/Data_Perso none bind 0 0
/dev/sdb2 /home/wilder/maPartition2 ext4  defaults  0  0
/dev/sdb3  none  swap  sw  0  0
```

> Tester `/etc/fstab` avec `sudo mount -a` --> check erreurs avant de rebooter

# 7. Montage automatique au démarrage avec UUID de disque

Trouver l'UUID du disque avec la commande `sudo blkid`.
Dans /etc/fstab :

```bash
UUID="<uuid_de_sdb1>" /mnt/data  ext4  defaults  0  0
/mnt/data/Important /home/wilder/Data_Perso none bind 0 0
UUID="<uuid_de_sdb2>" /home/wilder/maPartition2 ext4  defaults  0  0
UUID="<uuid_de_sdb3>"  none  swap  sw  0  0







