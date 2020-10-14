# Td syslog

## Exercice 1

popez 2 VM puis :
* sur l'une, 
  * mettez en place la gestion des logs entrants via le réseaux
  * routez ces logs vers des fichiers dédié à cela
* Sur l'autre 
  * router tout les logs vers ce serveur de syslog

## Exercice 2

Créez un script `/usr/local/bin/launch-cron`. Celui-ci devra être positioné devant la ligne de commande de crontab afin de l'encapsuler.

Exemple :

```bash
0 7 * * * /usr/local/bin/launch-cron /usr/local/bin/backup
```

Ce script :

* Notera la date courante : la date de début, 
* Utilisera logger pour tracer dans les log système `local7` le lancement du traitement `$1`
* Lancera le traitement (utilisation de $@ pour tous les arguments) 
* En fin de traitement, le script tracera la fin du batch en donnant sont code retour et la durée de traitement.

# Rendu

Sur votre dépo ajoutez un dossier syslog avec le script et un fichier README.md décrivant vos actions de mise en oeuvre. 
Attention vous devrez être précis concis et exhaustif. 
Par exemple vous sauvegarderez les fichiers de configurations avant toute modification puis vous copiez coller un `diff` du fichier de config comparant les version avant et après modification. si vous travaillez proprement cela se vera tout de suite.

