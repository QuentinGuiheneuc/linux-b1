# Installation logiciel

Nous allons installer quelques logiciels différent afin de vous presenter différents cas.

## Cas 1 installation d'un server mumble

Installation d'une application hors distribution :  
Mumble-server ou murmur, n'est pas inclus dans la distribution Centos nous devons donc faire l'intégration de ce logiciel sur notre distribution manuellement.

On part donc d'une CentOS minimale.

### Gestion de repository supplémentaire

Le dépot de package EPEL est bien connu des utilisateur RedHat / CentOS, il s'agit d'un dépot de contribution à la distribution. Ces packages ne sont pas supporté par RedHat mais sont intégrés particulièrement pour cette distribution.

Installation du repository EPEL, de wget et de l'outil bzip2 :

```bash
[root@localhost ~]$ yum install epel-release wget bzip2
Loaded plugins: fastestmirror
…/…
```

Nous constatons bien la gestion des dépendances par yum

```bash
Resolving Dependencies
--> Running transaction check
---> Package bzip2.x86_64 0:1.0.6-13.el7 will be installed
---> Package epel-release.noarch 0:7-11 will be installed
---> Package wget.x86_64 0:1.14-18.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=======================================================================================
 Package                Arch             Version                Repository        Size
=======================================================================================
Installing:
 bzip2                  x86_64           1.0.6-13.el7           base              52 k
 epel-release           noarch           7-11                   extras            15 k
 wget                   x86_64           1.14-18.el7            base             547 k

Transaction Summary
=======================================================================================
Install  3 Packages

Total download size: 614 k
Installed size: 2.1 M
Is this ok [y/d/N]: y
```

Nous avons en retour la liste des packages à installer, et nous confirmons par `y` l'exécution de l'installation

```bash
Downloading packages:
warning: /var/cache/yum/x86_64/7/extras/packages/epel-release-7-11.noarch.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
Public key for epel-release-7-11.noarch.rpm is not installed
(1/3): epel-release-7-11.noarch.rpm                             |  15 kB  00:00:00
Public key for bzip2-1.0.6-13.el7.x86_64.rpm is not installed/s |  15 kB  --:--:-- ETA
(2/3): bzip2-1.0.6-13.el7.x86_64.rpm                            |  52 kB  00:00:00
(3/3): wget-1.14-18.el7.x86_64.rpm                              | 547 kB  00:00:00
---------------------------------------------------------------------------------------
Total                                                     605 kB/s | 614 kB  00:01
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Importing GPG key 0xF4A80EB5:
 Userid     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
 Fingerprint: 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
 Package    : centos-release-7-6.1810.2.el7.centos.x86_64 (@anaconda)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Is this ok [y/N]: y
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : wget-1.14-18.el7.x86_64                                  1/3
  Installing : epel-release-7-11.noarch                                 2/3
  Installing : bzip2-1.0.6-13.el7.x86_64                                3/3
  Verifying  : bzip2-1.0.6-13.el7.x86_64                                1/3
  Verifying  : epel-release-7-11.noarch                                 2/3
  Verifying  : wget-1.14-18.el7.x86_64                                  3/3

Installed:
  bzip2.x86_64 0:1.0.6-13.el7  epel-release.noarch 0:7-11  wget.x86_64 0:1.14-18.el7

Complete!
```

Avec l'outil `rpm` nous pouvons consulter le contenu du package epel-release

```bash
[root@localhost ~]$ rpm -ql epel-release
/etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
/etc/yum.repos.d/epel-testing.repo
/etc/yum.repos.d/epel.repo
/usr/lib/systemd/system-preset/90-epel.preset
/usr/share/doc/epel-release-7
/usr/share/doc/epel-release-7/GPL
[root@localhost ~]$ head -8 /etc/yum.repos.d/epel.repo
[epel]
name=Extra Packages for Enterprise Linux 7 - $basearch
#baseurl=http://download.fedoraproject.org/pub/epel/7/$basearch
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch
failovermethod=priority
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
```

Nous avons donc installer un package qui contient une déclaration de dépôt de package avec sa clé de validation de l’intégrité des packages.

Nous pouvons aussi consulter le contenu du package wget

```bash
[root@localhost ~]$ rpm -ql wget | grep -v share
/etc/wgetrc
/usr/bin/wget
[root@localhost ~]$ file /usr/bin/wget
/usr/bin/wget: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=6939130215a3270d64cad4f484b0ee10652f2327, stripped
```

Nous avons donc un fichier exécutable et un fichier de configuration

### Intégration sur l'hote

Le logiciel murmur est disponible sous forme de fichier tar.bz2 contenant le programe exécutable.

Récupération et décompression du programme murmur :

```bash
[root@localhost ~]$ mkdir /opt/src ; cd /opt/src
[root@localhost src]$ wget -q https://github.com/mumble-voip/mumble/releases/download/1.2.19/murmur-static_x86-1.2.19.tar.bz2
[root@localhost src]$ tar -xjf ./murmur-static_x86-1.2.19.tar.bz2
```

Installation du programme murmur :
On créer une sous arbo dans `/usr/local`, on y place le programme, on place aussi un fichier de configuration sur `/etc`

```bash
[root@localhost src]$ mkdir /usr/local/murmur
[root@localhost src]$ cp -r ./murmur-static_x86-1.2.19/* /usr/local/murmur/
[root@localhost src]$ cp ./murmur-static_x86-1.2.19/murmur.ini /etc/murmur.ini
```

on créé les compte, groupe et dossier pour les logs:

```bash
[root@localhost src]$ groupadd -r murmur
[root@localhost src]$ useradd -r -g murmur -m -d /var/lib/murmur -s /sbin/nologin murmur
[root@localhost src]$ mkdir /var/log/murmur
[root@localhost src]$ chown murmur:murmur /var/log/murmur
[root@localhost src]$ chmod 0770 /var/log/murmur
```

### Configuration du logiciel

#### Configuration du service

Nous editon le fichier murmur.ini , nous spécifions la base de données le fichier de log er le fichier pid.

```bash
[root@localhost src]$ cp /etc/murmur.ini /etc/murmur.ini.orig
[root@localhost src]$ vi /etc/murmur.ini
[root@localhost src]$ diff /etc/murmur.ini /etc/murmur.ini.orig
16,18c16
< database=/var/lib/murmur/murmur.sqlite
< logfile=/var/log/murmur/murmur.log
< pidfile=/var/run/murmur/murmur.pid
---
> database=
```

la commande diff montre ce qui à été modifié sur le fichier

#### Création d’une unité systemd

```bash
[root@localhost src]$ vi /etc/systemd/system/murmur.service
[root@localhost src]$ cat /etc/systemd/system/murmur.service
[Unit]
Description=Mumble Server (Murmur)
Requires=network-online.target
After=network-online.target mariadb.service time-sync.target

[Service]
User=murmur
Type=forking
ExecStart=/usr/local/murmur/murmur.x86 -ini /etc/murmur.ini
PIDFile=/var/run/murmur/murmur.pid
ExecReload=/bin/kill -s HUP $MAINPID

[Install]
WantedBy=multi-user.target
```

Gestion d'un dossier temporraire systemd (/var/run/murmur) :

```bash
[root@localhost src]$ vi /etc/tmpfiles.d/murmur.conf
[root@localhost src]$ cat /etc/tmpfiles.d/murmur.conf
d /var/run/murmur 775 murmur murmur
[root@localhost src]$ systemd-tmpfiles --create /etc/tmpfiles.d/murmur.conf
[root@localhost src]$
```

#### Gestion de la rotation de log

l'outil logrotate effectue une rotation automatique des fichier de log afin que ceux-ci ne grossisent pas indéfiniment, l'intégration d'un service qui génère des log sur une distribution necessite d'ajouter un fichier de configuration logrotate

```bash
[root@localhost src]$ vi /etc/logrotate.d/murmur
[root@localhost src]$ cat /etc/logrotate.d/murmur
/var/log/murmur/*log {
    su murmur murmur
    dateext
    rotate 4
    missingok
    notifempty
    sharedscripts
    delaycompress
    postrotate
        /bin/systemctl reload murmur.service > /dev/null 2>/dev/null || true
    endscript
}
[root@localhost src]$
```

### résultats de l'installation

Demarrez le service :

```bash
[root@localhost src]$ systemctl daemon-reload
[root@localhost src]$ systemctl enable murmur.service
Created symlink from /etc/systemd/system/multi-user.target.wants/murmur.service to /etc/systemd/system/murmur.service.
[root@localhost src]$ systemctl start murmur.service
[root@localhost src]$
```

Verifiez :

```bash
[root@localhost src]$ ps -ef | grep murmur | grep -v grep
murmur   12393     1  0 22:06 ?        00:00:00 /usr/local/murmur/murmur.x86 -ini /etc/murmur.ini
[root@localhost src]$ cat /var/run/murmur/murmur.pid
12393
[root@localhost src]$ tail -4 /var/log/murmur/murmur.log
<W>2018-02-28 22:15:09.350 1 => Server listening on [::]:64738
<W>2018-02-28 22:15:09.357 1 => Announcing server via bonjour
<W>2018-02-28 22:15:09.364 1 => Not registering server as public
<W>2018-02-28 22:15:09.364 Object::connect: No such slot MurmurDBus::userTextMessage(const User *, const TextMessage &)
[root@localhost src]$
```

Le service à bien démarré.

Pour testez , Installez un client mumble et connectez vous à votre serveur mumble.

Penssez à désactiver selinux et a arrêter le firewall :

```bash
[root@localhost src]$ setenforce 0
[root@localhost src]$ systemctl stop firewalld
```

## cas 2 : installation d'une version spécifique de python

Python est un langage de dévelopement open source très aprécicié. il existe un grand nombre de contributeurs qui apporte de nouvelles bibliothèque logiciel au langage. Celui-ci évolue donc rapidement.

A partir d'une centos 7 minimale fraîchement installée.

### préambule

Validez la version existante de python

```bash
[root@localhost ~]$ type python
python is hashed (/usr/bin/python)
[root@localhost ~]$ python --version
Python 2.7.5
```

C'est une version trèèès ancienne : [final countdown](https://pythonclock.org/) (et pourtant encore utilisée)

Creation de répertoires pour les sources, récupération des sources d'une version plus récente et extraction de celle-ci

```bash
$ sudo mkdir /opt/src
$ sudo wget https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tgz -O /opt/src/Python-3.7.3.tgz
$ cd /opt/src
$ sudo tar zxvf Python-3.7.3.tgz ; cd Python-3.7.3
…/…
```

### Installation des pré-requis

Installation d'outils (compilateur linkeur configurateur perl rpm-build etc...) et **des biblothèques nécessaire à python**.

```bash
$ sudo yum groupinstall "Development tools"
$ sudo yum install wget zlib-devel libffi-devel
...
```

Groupinstall permet d'inetaller tout un groupe de logiciels, vous noterez la quantité de package et la présence du langage perl et de tout un tas de bibliothèques associée à celui-ci.

### Compilation

Configuration de la compilation

```bash
$ ./configure --help
`configure' configures python 3.7 to adapt to many kinds of systems.

Usage: ./configure [OPTION]... [VAR=VALUE]...

To assign environment variables (e.g., CC, CFLAGS...), specify them as
VAR=VALUE.  See below for descriptions of some of the useful variables.

Defaults for the options are specified in brackets.

Configuration:
  -h, --help              display this help and exit
      --help=short        display options specific to this package
      --help=recursive    display the short help of all the included packages
.../...
```

On notera les options suivantes : Celles permettant de définir là ou le composant du logiciel sera installé

```bash
Installation directories:
  --prefix=PREFIX         install architecture-independent files in PREFIX
                          [/usr/local]
  --exec-prefix=EPREFIX   install architecture-dependent files in EPREFIX
                          [PREFIX]

By default, `make install' will install all the files in
`/usr/local/bin', `/usr/local/lib' etc.  You can specify
an installation prefix other than `/usr/local' using `--prefix',
for instance `--prefix=$HOME'.

For better control, use the options below.

Fine tuning of the installation directories:
  --bindir=DIR            user executables [EPREFIX/bin]
  --sbindir=DIR           system admin executables [EPREFIX/sbin]
  --libexecdir=DIR        program executables [EPREFIX/libexec]
…/…
```

Nous gardons les options par défault mais on active les optimisations  (Dans d'autre cas on dois changer certaines options):

```bash
$ ./configure --enable-optimizations
…/…
```

ici on valide la capacité du système à compiler python3 et on configure le code source, certaines variable sont alors valorisée.

Compilation :

```bash
$ make
…/…
```

Enfin l'installation

```bash
$ sudo make install
…/…
```

On netois le code source en vue d'une autre compilation :

```bash
$ sudo make clean
…/…
$
```

#### validation

finalisation et test

```bash
$ ln -s /usr/local/bin/python3 /usr/bin/python3
$ python3 --version
Python 3.7.3
```
