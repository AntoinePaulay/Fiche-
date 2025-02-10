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
passwd : modifier un mot de passe
adduser : ajout d'utilisateurs
deluser : suppression d'utilisateurs
usermod : modifier un utilisateur
chfn : modifier la description d'un utilisateur
chsh : modifier le shell par défaut d'un utilisateur
chage : modifier durée de validité
newusers : création d'utilisateurs par lot
pwck : vérification du format des fichier passwd et shadow
newgrp : prendre un nouveau groupe
groupadd : ajout d'un groupe
groupdel : suppression d'un groupe
groupmod : modifier un groupe
grpck : vérification du format des fichier group et gshadow


## Commandes utiles pour consulter les logs

cat, more, less, zcat, zmore, zless : consultation en entier
head, tail : consultation en partie
grep, zgrep : recherche par filtre
watch : exécution périodique de commande
dmesg : Cas particulier des logs du noyau
last, lastb : Cas particulier login/out utilisateurs et échec

### Les gestionnaires de paquets

Debian avec DPKG
Red Hat avec RPM

Debian a APT
Red Hat a YUM

![image](https://github.com/user-attachments/assets/15ba1642-db73-4dd3-9be4-13191f427d5d)

deb : dépôt binaire
deb-src : dépôt de code source








