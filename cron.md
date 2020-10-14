# Crontab

## Présentation

Une crontab est une table de definition des taches planifiées. Il peu en existe une par utilisateurs, et une globale pour tout le host.

Celle pour les utilisateur se présente sous la forme suivante :

```bash
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
#
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7)
# |  |  |  |  |
# *  *  *  *  * command to be executed
```

Le caratère `#`, comme pour le bash,  met la suite de la ligne en commentaire.

Tout d'abord, on liste les variables d'environnement à utilisée pour lancer les taches planifiée. Cela n'est pas indispensable car des valeur par défaut existent. Cela reste une bonne pratique que de les spécifier :

* SHELL : le shell de lancement
* PATH : le path !
* MAILTO : le destinataire mail des log et de alerts en cas d'échec d'une commande (code retour non null)

Chacunes des lignes qui suivent definissent une tache planifiée :

* La succession de caratère `*` définissent la planification toute les minutes de toute les heures de tout les jours de tout les mois. chacune des valeur peu être :
  * `*` (toutes valeur possible)
  * `valeur1,valeur2,valeur3` (ces valeurs)
  * `valeur1-valeur3` (toutes les valeure entre valeur1 et valeur3)
  * `*/temporisation` (toutes les temporisation)
* le compte qui doit lancer la commande **uniquement pour la crontab système**
* La ligne de commande à passer

### Exemple

Pour lancer la commande `/usr/local/bin/backup` tout les matin à 7 heure

```bash
0 7 * * * /usr/local/bin/backup >/var/log/backup.log 2>&1
```

Pour saouler tout le monde :

```bash
*/5 8-18 * * * /usr/bin/wall "Est ce que c'est énervant ?"
```

### Accès au service cron

le daemon crond doit être installé et démarrer au boot de la machine :

```bash
$ sudo systemctl status crond
● crond.service - Command Scheduler
   Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; vendor preset: enabled)
   Active: active (running) since lun. 2019-05-06 17:00:12 CEST; 2h 42min ago
 Main PID: 2558 (crond)
   CGroup: /system.slice/crond.service
           └─2558 /usr/sbin/crond -n
.../...
```

Pour l'accès des utilisateurs :

* `/etc/cron.allow` si ce fichier existe, alors cron est utilisable que pour les comptes listé dans ce fichier.
* `/etc/cron.deny` si cron.allow n'existe pas alors cron est interdit à l'utilisation pour les comptes listés dans ce fichier

**Bonne pratique** : créer le fichier `/etc/cron.allow`

## Utilisation

### commande crontab

* `crontab -l` liste le contenu de la crontab
* `crontab -e` ouvre un éditeur sur votre crontab
* `crontab -r` remove la crontab **attention vous vener de perdre toutes vos planifications de tache**
* `crontab` attend le nouveau contenu de la crontab sur l'entrée standard ([Ctrl]c pour annuler et ne pas écraser son ancienne crontab)

**Bonne pratique d'utilisation :**

Vous sauvegardez votre crontab dans le fichier $HOME/crontab , vous l'éditez, enfin vous écrasez votre crontab avec le nouveau contenu :

```bash
prompt$ crontab -l > $HOME/crontab
prompt$ vi $HOME/crontab
* cat $HOME/crontab | crontab
```

Ansi vous garder toujours une copie de la crontab dans votre HOME

### Contraintes pour les scripts

#### La gestion de l'environnement d'execution

Du fait de la gestion de parenté des processus ces commandes seront lancer par le daemon cron (crond) aussi ceux-ci n'éhériterons pas des variables d'environement dont vous disposer dans votre session. Vous devrez donc vous assurer que le script connais bien toute les variables qu'il utilise.

#### La gestion des sorties standard et d'erreur

Le script n'est plus lancer depuis un terminal donc afin que celles-ci ne soit pas perdu ou qu'elle ne produisent pas de mail inutile vous devrez gérer dans votre script ou dans votre crontab la redirection des sorties. Votre script ne doit rien attendre en entrée.

#### Le code retour

En cas de code retour non null la daemon crond envois un mail à l'adresse définie par la variable `MAILTO` afin d'alerte sur l'echec de cette exécution.

## Les crontab système

La crontab system est `/etc/crontab` :

```bash
# cat /etc/crontab
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```

On ajoute simplement le login qui doit exécuter les commande avant la commande

En plus de celle-ci, nous avons aussi le dossier /etc/cron.d qui contiens des fragment de crontab système:

```bash
$ sudo cat /etc/cron.d/0hourly
# Run the hourly jobs
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
01 * * * * root run-parts /etc/cron.hourly
```

Cette ligne vas lancer toute les heures l'ensemble des scripts situé dans le dossier `/etc/cron.hourly`

## at / batch

Il existe un autre service de lancement de tache at et ou batch permetent de lancer une tache une seul fois en différé cela fonctionne sur le même modèle que cron , on a un daemon crond ensuite on défiçni une seul fois les commande à lancer.

Cet outil est maintenat trés peu utiliser pour l'utiliser vous devrez installer le damon `atd` et utiliser la commande `at`. le jour ou vous en aurez besoin vous lirez le man associé.
