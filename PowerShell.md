###  Cmdlet Powershell
* Disable-LocalUser : permet de désactiver un compte utilisateur.
* Enable-LocalUser : permet d’activer un compte utilisateur.
* Get-LocalUser : liste l’ensemble des comptes utilisateur locaux * présents sur le poste de travail.
* New-LocalUser : crée un nouveau compte utilisateur local.
* Get-LocalGroup : liste l’ensemble des groupes de sécurité locaux présents sur le poste de travail.
* New-LocalGroup : crée un nouveau groupe de sécurité local.
* Remove-LocalGroup : supprime un groupe de sécurité.
* Add-LocalGroupMember : ajoute un membre dans un groupe local.
* Get-LocalGroupMember : récupère les membres présents dans un groupe local.

* Syntaxe : $NomVariable = valeur
* Détruire une variable : Remove-Variable

 ## Quelques cmdlet possèdent le paramètre ComputerName :

* Restart-Computer
* Test-Connection
* Clear-EventLog
* Get-EventLog
* Get-HotFix
* Get-Process
* Get-Service
* Set-Service
* Get-WinEvent
* Get-WmiObject

* Une exécution de code à distance peut se faire avec le cmdlet Enter-PSSession.
Le service WinRM doit être démarré sur l’ordinateur distant.
Exit-PSSession clos une session distante

* Le cmdlet Invoke-Command permet d'exécuter du code à distance.



## Variables spéciales

* $? : Représente l'état d'exécution de la dernière opération (True ou False)

* $_ : Contient l'objet actuel dans l'objet pipeline. Vous pouvez utiliser cette variable dans des commandes qui exécutent une action sur chaque objet ou sur des objets sélectionnés dans un pipeline.

* $ARGS : Représente un tableau des paramètres non déclarés et/ou des valeurs de paramètre qui sont passés à une fonction, un script ou un bloc de script.

* $Null : Variable automatique qui contient une valeur NULL ou vide. Vous pouvez utiliser cette variable pour représenter une valeur absente ou indéfinie dans les commandes et les scripts.

* $True / $False : Représente True ou False. Peut être utilisé dans les commandes et les scripts.

* (*) remplace zéro ou plusieurs caractères 
* ? remplace exactement un caractère
* Les regex ou expressions régulières


* -eq (equal to) et -ne (not equal to)

* -gt (greater than) et -lt (less than)

* -ge (greater than or equal to) et -le (less than or equal to)

* -like et -notLike (avec les wildcards)

* -match et -notMatch (avec les expressions régulières)

* -not ou ! inverse le code de sortie, (NON logique)

* s1 -eq s2 : vrai si les chaînes sont identiques
* s1 -ne s2  : vrai si les chaînes sont différentes

* [String]::IsNullOrEmpty(s1) : vrai si s1 est vide

* ![String]::IsNullOrEmpty(s1) : vrai si s1 n’est pas vide

* n1 -eq n2 : vrai si les nombres sont égaux

* n1 -ne n2 : faux si les nombres sont différents

* n1 -lt n2 : n1 < n2

* n1 -le n2 : n1 <= n2

* n1 -gt n2 : n1 > n2

* n1 -ge n2 : n1 >= n2

* Test-Path p : vrai si p existe

## Structure conditionnelle



* If (condition)
{
 instructions
}
Else
{
 instructions
}

* Switch (condition)
{
 valeur1 {ScriptBlock1}
valeur2 {ScriptBlock2}
…
default {ScriptBlock par défaut}
}

* For (initialisation; condition; mise-à-jour)
{
 bloc d’instructions
}

* Foreach (element in collection)
{
 bloc d’instructions
}

* While (condition)
{
 bloc d’instructions
}

* La boucle do while est comme la boucle while, sauf que la condition est réalisée à la fin, donc il y a au moins 1 passage dans la boucle.

Do
{
 bloc d’instructions
}
While (condition)

* La boucle do until exécute le bloc de script jusqu’à ce que la condition soit réalisée.

Do
{
 bloc d’instructions
}
Until (condition)

* Déclaration de fonction
function nom
{
instructions
}

* 
* 
* 
* 
* 
* 
* 
* 
* 