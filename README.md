# Cours GNU/linux promo 2019

Les cours sont écrit en markdown et disponibles dans ce dépot sous [licence Creative Commons](./LICENCES.md) CC-by-nc-sa 4.0  (c)Alan Simon *alsim*

Ils sont bien sur destinés aux débutants et toutes les remarques sont les bienvenues.

## Chapitre 1, Installation et utilisation

### Introduction

* Quelques éléments de contexte [historique](./historique.md) d'Unix et Linux
* Une [présentation](./presentation_Linux.md) des distribution GNU/Linux
* Une [première installation](./TD-install-ubuntu.md) sous virtualbox d'une Ubuntu

### Utilisation du shell

* Le '[shell](./shell.md)'
* Le '[man](./man.md)'
* Les principales [commandes](./commands.md) et un [TD](./TD-commands.md)
* L'[arborescence](./arborescence.md) de fichiers
* Les commandes [filtres](./filtres.md) et encore un [TD](./TD-filtres.md)
* L'[environnement](./environment-Unix.md) d'execution du shell et un [TD](./TD-environment.md)
* Comment rechercher des fichiers dans une arborescence avec la commande '[find](./find.md)' puis un [TD](TD-find.md)

### Utilisation Avancée

* Le client [ssh](./ssh-client.md)  
* La gestion des [processus](./processus.md)
* La planification des taches: [cron](./cron.md)
* L'outil de synchronisation [rsync](./rsync.md)
* [scripting bash](./scripting.md) et son [TD](./TD-scripting.md)
* Edition de fichier avec [vi et sed](./vi.md)  
* Le tableur en ligne de commande : [awk](./awk.md)  

Evaluation Scripting : [TP evalué](TP-scripting.md)

## Chapitre 2, administration serveur

### Les utilisateurs et les droits

* Gestion des comptes [utilisateurs](./utilisateurs.md) et un [TD](./TD-utilisateurs.md)
* Gestion des [Droits](./droits.md) sur les fichiers et encore un [TD](./TD-droits.md)

### Installation / Configuration

* [Installation et configuration d'une centOS 7](./TD-install-centos.md)
* [Configuration DNS](./using-dns.md)
* présentation des [bibiothèque partagés](./shared-libs.md)
* [gestion de package logiciel](./packages.md) et un [TD](./TD-install-soft.md) sur les installation logiciels
* [gestion des mails](./using-mail.md) et un [TD](./TD-mail-system.md)

### Gestion du stockage

* le stockage physique et les [partitions](./partitionning.md)
* Cours sur les [Les filesystèmes](./filesystem.md) et un [TD](./TD-filesystem.md) sur le sujet
* Cours sur le Logical Volume Manager [LVM](./lvm.md) et un [TD](./TD-lvm.md) sur le sujet

### Troubleshooting

* [Principe du troubleshooting](./troubleshoot.md)
* La [Syslog](./syslog.md) et un [TD](./TD-syslog.md)
* Comprendre le [Boot](./boot.md)
