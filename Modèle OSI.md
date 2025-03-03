
| |  |  Couche | Fonction |
|---|---|---|---|
| Pour  | Physique |  Couche 1 | 	Transmission des signaux sous forme numérique ou analogique |
| Le | Liaison | Couche 2  | 	Adressage physique (adresse MAC)|
| Reseau | Réseau | Couche 3| Détermine le parcours des données et l'adressage logique (adresse IP) |
| Tout | Transport | Couche 4 | Connexion de bout en bout, connectabilité et contrôle de flux ; notion de port (TCP et UDP) |
| Se | Session | Couche 5 | Communication Interhost, gère les sessions entre les différentes applications |
| Passe | Présentation | Couche 6 | Gère le chiffrement et le déchiffrement des données, convertit les données machine en données exploitables par n'importe quelle autre machine |
| Automatiquement | Application | Couche 7 | 	Point d'accès aux services réseau |


![image](https://github.com/user-attachments/assets/16c3a1a4-4c65-49b0-ae2d-9871839f798b)




### Modèle OSI
![]()
Couche 7: Application (PDU: données applicatives)
Définie le type ou la signification des informations à échanger

Couche 6: Présentation (PDU: données)
Chargée de la syntaxe et du format des données.

Couche 5: Session (PDU: données)
Assure l’ouverture et la fermeture du dialogue entre les ordinateurs, la reprise après coupure et la sécurité.

Couche 4: Transport (PDU: segment ou datagramme)
Responsable du transfert de bout en bout des informations (découpage des données, contrôle de flux, réordonnancement).

Couche 3: Réseau (PDU: paquet)
Assure le routage et l’interconnexion de réseaux hétérogènes.

Couche 2: Liaison (PDU: trame)
Gère les méthodes d’accès au média, gère le contrôle d’erreurs.

Couche 1: Physique (PDU: bit)
Définie l’interface, les connecteurs, le câblage.
