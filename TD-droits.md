# TD droits

## Spécification des droits

### Ennoncé

Apache httpd server est l'un des serveur web les plus courant pour héberger un site web.

Dans les distribution type debian, il est intégré pour s'exécuter avec l'identité `www-data.www-data` (user.group).

apache httpd serveur  est installé sur un système type debian et héberge plusieurs sites web : site1 site2 site3 chacun ont une sous arborescence dédié dans /var/wwww : /var/www/site(X)

* Evidemment, l'identité d'apache dois avoir accès en lecture à l'arborescence des trois site web afin de le servir aux visiteurs.

* Les sites sont régulièrement mis à jour, donc chaque propriétaire de site devra avoir un compte dédié à la livraison d'une nouvelle version du site web.

* On souhaite bien sur interdir à chaque propriétaire de site d'écrire sur l'arborescence d'un autre site

* Chaque site propose aux visiteur d'uploader du contenu dans le dossier /var/www/site(X)/upload

* Pour des raisons de sécurité apache ne doits pas avoir le droits d'écrire sur l'arborescence sauf dans le dossier d'upload

### Question

Vous devez alors definir :

* les comptes et groupes à créer (l'identité du serveur web apache est déja créé)
* Les droits, propriétaire et groupe des dossiers `/var/www/site?`,
* Les droits, propriétaire et groupe des sous dossiers des site web exemple : `/var/www/site1/html`
* et en particulier sur les trois dossiers `/var/www/site?/upload`

## Pour les audacieux et les audacieuses

Créez un script bash qui prend en argument le nom du site et qui créer le compte associé et la sous arborescence avec les droits correspondants.

Un script bash est un fichier text dont la première ligne est `#!/bin/bash`, dans ce script `$1` est la variable contenant le premier argument passé au script.

Enfin le script est simplement une suite de commande.
