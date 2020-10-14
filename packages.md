# Gestion de packages

## Présentation

Une [distribution](./presentation_Linux.md#Une) est une suite logiciel qui intègre un système d'exploitation et un ensemble de logiciel intégré pour celui-ci. **Les packages sont les unités de livraison des logiciels intégrés de la distribution.**

Un package est un fichier contenant :

* l'ensemble des fichiers associé au logiciels
* les scriptes permettant leur installation
* les fichiers permettant leur intégration sur le système et d'éventuel autre logiciel.

Exemple : l'installation du serveur ftp vsftpd du dépot EPEL sur une centos contient un fichier de config pour logrotate et un fichier d'intégration sur pam (Plugable Authentification Module) :

```bash
$ rpm -ql vsftpd | grep /etc
/etc/logrotate.d/vsftpd
/etc/pam.d/vsftpd
/etc/vsftpd
/etc/vsftpd/ftpusers
/etc/vsftpd/user_list
/etc/vsftpd/vsftpd.conf
/etc/vsftpd/vsftpd_conf_migrate.sh
```

## Format des packages

Deux format principaux **rpm** et **deb** il y en a d'autres qui ne seront pas étudié dans ce document.

### Packages rpm

rpm : RedHat Package Manager

#### Nomenclature

Exemple :

util-linux-ng-2.17.2-12.7.el6.i686.rpm

Package : util-linux-ng
Version : 2.17.2
Release : 12.7.el6
Architecture  : i686

#### Structure

C'est une structure spécifique lisible avec l'outil `rpm`, elle contiens :

* Une entête de fichier
* Des Signatures cryptographique
* Un header de package : les métadonnée du package
* L'archive cpio compressé des fichiers à installer

#### Commandes de gestion des packages rpm

* Liste des packages installé : `rpm -qa`  
* Contenu d'un package installé : `rpm -ql $PKG`  
  à tester sur les packages  
  * coreutils  
  * util-linux
* Contenu d'un fichier package : `rpm -qpl $pkg-file`  
* Package depuis un fichier : `rpm -qf $FILE`  
  à tester sur les fichiers :
  * /etc/services
  * /sbin/ip
* Vérification d'un package : `rpm -V $PKG`  
  taille md5sum etc...  
* Consultation des Metadonnées d'un package :  
`rpm -q --info openssh-clients`
* Consultation des Scripts pre/post install : `rpm -q --scripts openssh-server`

### Packages debian

#### Nomenclature

exemple : apache_1.3.31-6_i386
Nom du package : apache
Version : 1.3.31-6
Architecture : i386

#### Structure

C'est une archive ar contenant :

* Debian-binary : contiens la version du packaging
* data.tar.gz : archive des fichiers à installer/désinstallé du système
* control.tar.gz : fichiers de controle lié à la gestion du package :
  * Conffile : liste des fichiers de config
  * Control : info sur le package
  * Md5sums : checksum du contenu de data.gz
  * Config : script de configuration
  * Scripts de pre ou post installation et suppresion
    * preinst
    * prerm
    * postinst
    * postrm

[td oscultation d'un package debian](TD-debian-package-content.md)

#### Commandes de gestion des packages debian

* Liste des packages installé : `dpkg -l`  
  à utiliser avec un grep
* Contenu d'un package installé : `dpkg -L $PKG`  
  à utiliser avec un grep
* Contenu d'un package : `dpkg -c $PKG`
  à tester sur les packages :
  * coreutil
  * util-linux
  * netbase
* Package owner d'un fichier : `dpkg -S $FILE`
  à tester sur les fichiers :
  * /etc/services
  * /bin/ip
  * /sbin/ifconfig
* reconfiguration d'un package installé : `dpkg --reconfigure`
  Sauvegarde des fichiers de configuration locale, execution de la configuration et la post installation.
* `dpkg --repack` : reconstruit un package à    partir de fichiers locaux (sauvegarde du produit et de sa conf)

## Dépendances

Certains logiciel nécessite que d'autre logiciels soit aussi installés, par exemple une application web nécessitera qu'un serveur web soit déja installé, cette relation s'appele une dépendance.

D'autre logiciels peuvent être incopatible entre eux du fait de leur intégration. Exemple : deux serveurs web configuré pour écouté sur le port 80 ne peuvent pas être lancés en même temps. Etant intégrer pour démarrer automatiquement ils sont incompatibles.

Ainsi pour installer un package il faut parfois necessairement installer ou désinstaller d'autre packages. Les éditeurs de distribution proposent en général des dépot de packages et un outil de gestions des dépots de package et des dépendances.

aussi lorsque que l'on utilise un tel outil pour installer un logiciel cet outil étudira les dépendance et proposera d'installer l'ensemble des package necessaire au bon fonctionnement du logiciel.

### yum : YellowDog Update Manager

C'est l'outils de gestion des dépots de RedHat :

Installation d'un package et de ses dépendances : `yum install $PKG`

Supression d'un package : `yum remove $PKG`

Mise à jour système : `yum update`

#### Dépots de package rpm

Certains logiciels proposent d'ajouter tout un depot de packages intégré pour une distribution.
On augmente alors la distribution avec ce dépot
Tecniquement un fichier d"crivant le répository est ajouté dans /etc/yum.repos.d/*

Dans ce fichier on viens definir des canaux. Un canal est en fait un ensemble cohérent packages sur un dépots accessible via un URL.

```bash
 grep -v "^#" /etc/yum.repos.d/CentOS-Base.repo | grep . | head -5
[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

Ici une clef permet de valider la signature numérique des packets

### apt

C'est l'outils de gestion des dépots de package pour les distribution debian et associé  

Installation : `apt-get install $PKG`  

Supression : `apt-get remove $PKG` / `apt-get --purge remove $PKG`  

Mise à jour de la base locale de package : `apt-get update` (resynchronisation de l'index des packages : /var/lib/dpkg/available)  

Mise à jour système `apt-get upgrade`

#### Dépots de package debian

voir /etc/apt/sources.list et les fichier /etc/apt/sources.list.d/*

chaque fichier deffini des canaux :

* Un Type de canal : deb ou deb-src (packages compilé ou package de code source à compiler)
* L'Url : la racine de dépot
* des Tag : distribution / type de package main contrib non-free
