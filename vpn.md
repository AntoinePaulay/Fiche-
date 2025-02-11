### VPN

* Les protocoles AH et ESP sont des options d'IP
* Entêtes supplémentaires qui s'intercalent entre IP et le protocole transporté
* Ex : TCP dans IP => TCP dans ESP dans IP
* Mode transport :
  Le protocole de niveau 4 circule dans AH/ESP puis dans IP
  Les entêtes IP circulent en clair sans authentification/intégrité
* Mode tunnel :
Un paquet IP complet est encapsulé dans AH/ESP puis dans un (autre) IP
L'ensemble du paquet IP encapsulé est authentifié, intègre et (éventuellement) chiffré (ESP)


