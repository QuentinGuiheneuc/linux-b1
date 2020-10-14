# Notions DNS

## Présentation

Le service DNS : Domain Name Services est indispensable à l'utilisation d'internet. Il permet d'utiliser les services par leur nom, un appel au service DNS transforme le nom en l'adresse ip qui porte le service.

C'est un service distribué. Un serveur fait authorité sur un domaine et peu déléguer cette authorité pour ses sous domaines.

Exemple : Les serveurs de l'afnic font authorité sur le domaine .fr et délègue l'authorité à d'autre serveurs pour chacun des sous domaines en quelquechose.fr, ceux-ci peuvent à leur tour déléguer l'authorité à des sous-sous domaines etc...

La commande  `dig` permet d'interroger les services DNS exemple :

```bash
$ dig A ftp.rototo.fr @8.8.8.8 +short
rototo.fr.
213.186.33.5
```

Ici on interroge le serveur DNS récursif à l'adresse IP 8.8.8.8 pour connaitre l'adresse ip du service rototo.fr.

Le serveur de récursion (8.8.8.8) interroge l'arborescances des serveurs authoritatifs pour atteindre le serveur faisant authorité sur le domaine rototo.fr qui retourne alors l'enregistrement de type A (pour l'adresse ip V4) pour le nom ftp.

Il existe plusieurs types d'enregistrement :

* A pour une adresse ipV4
* AAAA pour une adresse ipV6
* CNAME pour un common name (il retourne une autre noms DNS)
* NS Name Server : pour definir des serveurs faisant authorité
* MX Mail eXchange pour definir les serveur gérant les mails pour le domaine

## Le-resolver-DNS

Le resolver DNS est l'ensemble des fonctions logiciels qui interrogent l'infrastucture DNS pour transformer un nom en adresse ip.

Le resolver est configuré via le fichier /etc/resolv.conf

Ce fichier contiens principalement les entrées `nameserver` listant les serveurs dns récursifs à interroger mais il peu aussi contenir des options comme `search` définissant les domaines par défaut sur lesquel le resolver va rechercher un nom.

exemple :

```bash
search lab.local
timeout 3
nameserver 8.8.4.4
nameserver 8.8.8.8
```

> __Le cache local:__ Dans certains cas, le host dispose en local d'un serveur DNS qui effectue la résolution (ou forward la requête à un autre serveur) puis conserve le résultat de la requête en dans son cache. resolv.conf définiera alors 127.0.0.1 ou 127.0.1.1 en nameserver.
