# Créer un service de sauvegarde

A partir d'une distribution vièrge (une installation minimale) faites les étappes suivantes

## Création de l'environnement

#### On créé un compte

Compte `rsyncd` dédié au daemon rsync

```bash
$ sudo useradd -r -s /sbin/nologin rsyncd
```

#### On créé un répertoir de stockage 

Et on le "donne" ce dossier à l'utilisateur rsyncd

```bash
$ sudo mkdir -p /data/bck
$ sudo chown rsyncd.rsyncd /data/bck
```

>  Dans la vrai vie on utiliserais un filesysteme dédié. 

#### Configuration du daemon

pour que rsync fonctionne en mode daemon il lui faut spécifier ces option via un fichier de configuration, cela est fait manuellement en lisant le man associé à ce fichier, voici le contenu que j'ai créé :

```bash
$ cat /etc/rsyncd.conf
timeout = 600
uid = rsyncd
gid = rsyncd
max connections = 5
fake super = yes
munge symlinks = yes
numeric ids = yes
syslog facility = local4
strict modes = yes
dont compress   = *.gz *.tgz *.zip *.z *.Z *.rpm *.deb *.bz2
[bck]
path = /data/bck/
refuse options = c delete
fake super = yes
auth users = backup
read only = no
list = yes
secrets file = /etc/rsyncd.secrets
hosts allow=192.168.56.0/24 127.0.0.1
```

Chacune des permières lignes permet de définir une option de fonctionnement du daemon:

* 10 minutes/600 secondes et le `timeout`. Passé ce délais sans activité le daemon coupe la connexion sur laquelle ce temps est atteins
* le daemon tournera avec l'identité rsyncd.rsyncd (user.group) 
* `fake super` est mode permettant de faire comme si le daemon tournais sous root et de sauvegarder les owner,group et droits sur le fichier
* Attention seul les adresse ip sur le réseaux 192.168.56.0/24 sont authorisée à se connecter, penssez à adapter ceci a votre situation.
* etc... Avec le man vous pouvez retrouver la signification de chacune de ces options

Ensuite on défini des canaux de synchronisation ici le canal `[backup]` pour lequel le path est `/data/bck` on l'utilise si on est authentifié comme le user rsync `backup` qui est défini dans le fichier `/etc/rsyncd.secrets`.

On créez alors le fichier de user et mot de passe, voici son contenu :

```bash
$ sudo cat /etc/rsyncd.secrets
backup:MoTdEpAsSe
```

Bien sur nous sécurisons ce fichier afin que seul root puisse le lire

```bash
$ sudo chown root.root /etc/rsyncd.secrets
$ sudo chmod 600 /etc/rsyncd.secrets
```

#### Installez rsync

sous une distribution "redhat like" :

```bash
$ sudo yum install rsync
```

ou sur une distribution "debian like" :

```bash
$ sudo apt-get install rsync
```

## Test / Validation

#### Lancez le daemon : 

```bash
$ sudo rsync --daemon &
```

Le processus daemon fonctionne alors en arrière plan

#### Testez la sauvegarde de /etc : 

```bash
$ rsync -avp /etc backup@127.0.0.1::bck/127.0.0.1/
```

Vérifiez si les fichiers sont bien sauvegardé dans /data/bck/127.0.0.1/ : ils appartiennent tous à `rsyncd.rsyncd`.

#### Testez une restoration : 

```bash
$ rsync -avp backup@127.0.0.1::bck/127.0.0.1/etc/rsyncd.conf /tmp/
$  ls -al /tmp/rsyncd.conf
-rw-r--r--. 1 root root 407  6 mai   17:02 /tmp/rsyncd.conf
```

le fichier a bien été restauré dans /tmp avec les bons droits

La commande attr vous permet de regarder les attributs caché des fichiers, ici on retrouve les droits d’origine du fichier permettant au logiciel rsync de remettre en place les droits d’origne à la restauration des fichiers. (option `fake root`)

##### gestions des droits par rsync :

On créé un fichier avec des droits spécifiques et on le sauvegarde
```bash
$ sudo touch /etc/testfile
$ sudo chown sshd.rsyncd /etc/testfile
$ sudo chmod 1664 /etc/testfile
$ sudo rsync -avp /etc backup@127.0.0.1::bck/127.0.0.1/
```

La commande `attr` permet de consulter les attributs étendus de fichiers sur le filesystem xfs

```bash
$ sudo yum install attr
$ sudo attr -g rsync.%stat /data/bck/127.0.0.1/etc/testfile
Attribute "rsync.%stat" had a 17 byte value for /data/bck/127.0.0.1/etc/testfile:
101664 0,0 74:996
$ id rsyncd
uid=998(rsyncd) gid=996(rsyncd) groups=996(rsyncd)
$ id sshd
uid=74(sshd) gid=74(sshd) groups=74(sshd)
```

on retrouve bien le mode : `1664` l'id du `sshd`: 74 et le gid de `rsyncd`: 996 dans les attributs étendus du fichier sauvegarde.

#### Stoppez le daemon : 

Afin d'utiliser `killall` il nous faut l'installer :

```bash
$ sudo yum search killall
.../...
$ sudo yum install psmisc
.../...
$ sudo rpm -ql psmisc | grep killall
/usr/bin/killall
/usr/share/man/man1/killall.1.gz
```

On kill alors rsync 
```bash
$ sudo killall rsync
```

## Mise en place du service

#### Installation et configuration xinetd

On install le super server xinetd c'est un service réseaux qui écoute les port tcp ou udp listé dans sa config et lance le daemon associé sur sollicitation de ce port.

```bash
$ sudo yum install xinetd
.../...
$ sudo rpm -ql xinetd | grep /sbin/
/usr/sbin/xinetd
$ sudo rpm -ql xinetd | grep /etc | head -4
/etc/sysconfig/xinetd
/etc/xinetd.conf
/etc/xinetd.d/chargen-dgram
/etc/xinetd.d/chargen-stream
```

Création du fichier de config xinetd pour rsync (il faut créer ce fichier)

```bash
$ sudo cat /etc/xinetd.d/rsync 
service rsync 
{ 
        disable = no 
        flags           = IPv6 
        socket_type     = stream 
        wait            = no 
        user            = root 
        server          = /usr/bin/rsync 
        server_args     = --daemon 
        log_on_failure  += USERID 
}
```

Activation et démarrage du service : 

```bash
$ sudo systemctl enable xinetd
$ sudo systemctl start xinetd
```

On vérifie que xinetd écoute bien sur le port 873 (le port du protocole rsync)
```bash
$ sudo ss -anptl | grep 873
LISTEN     0      64          :::873                     :::*                   users:(("xinetd",pid=3655,fd=5))
```

#### Test local

on relance la sauvegarde de /etc cette fois xinetd doit démarrer la daemon

```bash
$ sudo rsync -avp /etc backup@127.0.0.1::bck/127.0.0.1/
Password: 
sending incremental file list
etc/
etc/xinetd.conf
etc/sysconfig/
etc/sysconfig/xinetd
etc/systemd/system/multi-user.target.wants/
etc/systemd/system/multi-user.target.wants/xinetd.service -> /usr/lib/systemd/system/xinetd.service
etc/xinetd.d/
etc/xinetd.d/chargen-dgram
etc/xinetd.d/chargen-stream
etc/xinetd.d/daytime-dgram
etc/xinetd.d/daytime-stream
etc/xinetd.d/discard-dgram
etc/xinetd.d/discard-stream
etc/xinetd.d/echo-dgram
etc/xinetd.d/echo-stream
etc/xinetd.d/rsync
etc/xinetd.d/tcpmux-server
etc/xinetd.d/time-dgram
etc/xinetd.d/time-stream

sent 59,560 bytes  received 912 bytes  24,188.80 bytes/sec
total size is 27,150,693  speedup is 448.98
```

Je note que le processus rsync s'est bien arrêté à la fin de la saucegarde:

```bash
$  ps -ef | grep rsync | grep -v grep
$
```

## Mise en place de la sauvegare sur un serveur distant :

Installez rsync	 (il sera utiliser en mode client)

```bash
$ sudo yum install rsync
```

Créez un script de sauvegarde

```bash
$ sudo vi /usr/local/sbin/backup
$ sudo chmod o-rwx /usr/local/sbin/backup
```

Contenu :

```bash
#!/bin/bash
# script de sauvegarde
# 
# configuration : 
backupserveur=192.168.56.XXX
canal=bck
backupuser=backup
export RSYNC_PASSWORD=MoTdEpAsSe

host=$(hostname -s)
if test `id -u` -ne O
then
	echo “you need to be root”
	exit 2
fi

rsync -ap / $backupuser@$backupserveur::$canal/$host/
```

On notera que le mot de passse est mis en place dans une variable : `RSYNC_PASSWORD` qui est connu du client rsync pour contenir le mot de passe. Cela évite de le saisir en interactif ou qu'il soit visible dans un `ps -ef` sur la ligne de commande.

On pourra alors lancer la sauvegarde tout les soir via [cron](./cron.md)
avec la simple commande `/usr/local/sbin/backup`
