# Gestion des logs

Certain daemon, certaines application gèrent par eux même leur fichier de log pour les autres on utilise un système global de gestion de log.

Le standard historique c'est la syslog, rsyslogd est sa dernière implémentation sous Linux. Il est peu a peu remplacer par la processus systemd-journald de systemd.

## Présentation

la syslog est le systeme historique de gestion des logs système sous Unix, il donne son nom à :

* Un Protocole réseau
  * Sur le port udp ou tcp 514
* Un Format de fichier de log
  * La Date
  * Le hostname
  * Le processus, daemon ou juste un tag
  * Une classe de log : catégorie.niveau-de-log
  * Le message

Le protocole réseaux car comme pour tout sous unix le système est penssé pour être utiliser sur le réseau

## les classes de log (facility)

Permet de distinguer les log systèmes

* Par classe d'émeteurs (facility)
  * 0 kernel messages
  * 1 user-level messages
  * 2 mail system
  * 3 system daemons
  * 4 security/authorization messages
  * 5 messages generated internally by syslogd
  * 6 line printer subsystem
  * 7 network news subsystem
  * 8 UUCP subsystem
  * 9 clock daemon
  * 10 security/authorization messages
  * 11 FTP daemon
  * 12 NTP subsystem
  * 13 log audit
  * 14 log alert
  * 15 clock daemon
  * 16-23 local 0 - local 7
* Et par niveau d'urgence du message (criticity)
  * 0 Emerg (emergency)
  * 1 Alert
  * 2 Crit
  * 3 Err
  * 4 Warning
  * 5 Notice
  * 6 Info
  * 7 Debug

## La commande logger

C'est un outil permettant de générer des log syslog ; vous consulterez le man et vous y noterez l'option `-p` permettant de spécifier la classe de log.

Vous pouvez alors utiliser cette commande dans vos scripts afin de tracer leurs exécutions dans les log système.

Exemple:

```bash
[root@localhost ~]$ logger -p local3.info test
[root@localhost ~]$ tail -1 /var/log/messages
May 12 18:07:45 localhost root: test
```

## Configuration `rsyslogd`

Sous CentOS le daemon est `rsyslogd` et la configuration globale du daemon est dans le fichier `/etc/rsyslog.conf`. `rsyslogd` est modulaire ainsi il existe une grosse quantité de modules

### Méthode de collecte

le daemon rsyslogd peu collecter les log des façon suivante :

* Un socket réseaux: SOCK_STREAM ou SOCK_DGRAM,
* Un tube nommé : (man mkfifo)
* Un socket local : /dev/log, /var/run/log

Attention, il existe de nombreuse attaques de deni de service sur les daemon syslogs il convien donc de ne pas laisser le port réseaux ouvert à tous.

Extrait de la configuration sur la collecte:

```bash
# The imjournal module bellow is now used as a message source instead of imuxsock.
$ModLoad imuxsock # provides support for local system logging (e.g. via logger command)
$ModLoad imjournal # provides access to the systemd journal
#$ModLoad imklog # reads kernel messages (the same are read from journald)
#$ModLoad immark  # provides --MARK-- message capability

# Provides UDP syslog reception
#$ModLoad imudp
#$UDPServerRun 514

# Provides TCP syslog reception
#$ModLoad imtcp
#$InputTCPServerRun 514
```

Ici seul un socket unix (fichier socket local) et une connexion au processus systemd-journald sont ouvert

### La destination des log

rsyslogd est un multiplexeur de log il reçois les log depuis n'importe quoi et n'import où puis Chaque log sera écrit suivant les règle de routrage de log.

Ces règles se basent sur les classes de log définies plus haut. Chaque ligne, règle, définie une destination et les classes de log (catégorie et/ou niveau) que nous redirigeons vers cette destination.

A noter :
La catégorie `*` pour toutes les catégories
Le niveau de log `*` pour tout les niveau
le niveau de log `none` qui permet d'exclure une catégorie de log 

En général on utilise un fichier de log ; on donnera alors son chemin complet.

Exemple : 

```bash
*.info;mail.none;authpriv.none;cron.none /var/log/messages
authpriv.*                               /var/log/secure
mail.*                                  -/var/log/maillog
local7.*                                 /var/log/boot.log
```

On peu aussi router les log vers un serveur de syslog : il porte un daemon syslog sur lequel les module d'entré TCP et ou udp sont activé (et le port 514 ouverts sur le firewall). Cet option de sortie nécessite la gestion d'un cache local paramétrer ainsi :

```bash
# An on-disk queue is created for this action. If the remote host is
# down, messages are spooled to disk and sent when it is up again.
$ActionQueueFileName fwdRule1 # unique name prefix for spool files
$ActionQueueMaxDiskSpace 1g   # 1gb space limit (use as much as possible)
$ActionQueueSaveOnShutdown on # save messages to disk on shutdown
$ActionQueueType LinkedList   # run asynchronously
$ActionResumeRetryCount -1    # infinite retries if host is down
# remote host is: name/ip:port, e.g. 192.168.0.1:514, port optional
*.* @@remote-host:514
```

Autre exemple de module de sortie :

* `:ommysql:` pour **o**utput **m**odule **mysql** permettant de sauvegarder les log dans une base de données MySQL
* `:omusrmsg:*` permettant l'envois d'une message à tout les utilisateurs tel qu'avec la command `wall` : 
  ```bash
  [root@localhost ~]$ logger -p local3.emerg alert alert 
  
  Broadcast message from systemd-journald@localhost.localdomain (Sun 2019-05-12 18:15:25 CEST):
  
  root[3574]: alert alert
  
  [root@localhost ~]$ 
  Message from syslogd@localhost at May 12 18:15:25 ...
  root:alert alert
  ```

## Rotation des logs avec logrotate

Logrotate est un programme qui gère la rotation des log.

```bash
[root@localhost ~]$ man logrotate
```

Il est lancé periodiquement via cron : `/etc/cron.daily/logrotate`

```bash
[root@localhost ~]$ cat /etc/cron.daily/logrotate
#!/bin/sh

/usr/sbin/logrotate -s /var/lib/logrotate/logrotate.status /etc/logrotate.conf
EXITVALUE=$?
if [ $EXITVALUE != 0 ]; then
    /usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]"
fi
exit 0
```


La configuration est donc gèrée via le fichier : `/etc/logrotate.conf`
ce fichier contien une clause `include /etc/logrotate.d` intégrant de morceau de configuration dépmoser par d'autres packet dans ce dossier
Exemple : /etc/logrotate.d/syslog

```bash
[root@localhost ~]$ cat /etc/logrotate.d/syslog
/var/log/cron
/var/log/maillog
/var/log/messages
/var/log/secure
/var/log/spooler
{
    missingok
    sharedscripts
    postrotate
	/bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true
    endscript
}
```

Ici on liste des fichiers de logs qui seront traité puis les option de traitement entre accolade, des fichier peuvent être manquants. le script de post rotation est partagé pour tout les fichiers. puis le script consiste en l'envois d'un kill -1 au daemon syslog. Vous consulterez par ailleur le `man logrotate.conf` pour maitriser cet outil trés complet.
