# Identité Unix

## Objectif

La séparation des privilèges, la traçabilité des actions et la non répudiation sont nécessaire lorsqu'on souhaites garantir un certain niveau de service.
Ainsi chaque "intervenant" sur un système doit agir sous son identité qui lui est propre.

Les utilisateurs, les applications et les administrateurs interviendront sous leur propre identité.

Une identité Unix est définie par un uid(user id), un gid (l'id du groupe primaire de l'utilisateur) et un ensemble de groupes secondaires.

## Gestion des utilisateurs et groupes

### Commandes

Ces commandes sont des commandes d'administrations elles nécessitent donc d'être exécuté sous l'identité root (donc au travers d'un sudo: `sudo commande`)

* `useradd` permet de créer un utilisateur.
* `userdel` permet de supprimer un utilisateur
* `usermod -G` permet de modifier la liste des groupes secondaires d'un utilisateur
* `groupadd` permet de créer un groupe
* `groupdel` permet de supprimer un groupe
* `passwd user` permet de changer le mot de passe de l'utilisateur user

> En vous aidant du man, créez le compte test1 ayant pour groupe primaire le group test et definissez sont mot de passe.

## Fichiers de références

La norme posix définie les fichier de référence pour les utilisateurs et groupes

* /etc/passwd : les utilisateurs
* /etc/shadow : les mot de passe date de changement
* /etc/group : les groupes
* /etc/shells : les shells de connexion

### `/etc/passwd`

C'est un fichier tableau dont le séparateur de champs est le caractère `:`

```bash
$ head -2 /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

Il contient :

* Le login du compte
* Le password (historiquement il est maintenant dans le fichier shadow)
* L'UID
* Le groupe primaire : GID
* Le champs gecos (historique)
* Le home dir (répertoire de connexion)
* Le shell de connexion

### `/etc/group`

C'est aussi un fichier tableau dont le séparateur de champs est le caractère `:`

```bash
$ head -2 /etc/group
root:x:0:
daemon:x:1:
```

Il contient :

* Le nom du groupe
* Le mot de passe du groupe (pas vraiment utilisé)
* Le gid
* La liste des login membres séparé par des virgules

### Le skel dir

Les skeldir sont des répertoirs définissants le contenu initial du home utilisateur à sa création. Son contenu est copié lors de la création d'un compte dans le home-dir de l'utilisateur :

```bash
$ ls -al /etc/skel/
total 40
drwxr-xr-x   3 root root  4096 juil.  8  2017 .
drwxr-xr-x 144 root root 12288 mars  23 17:30 ..
-rw-r--r--   1 root root   220 sept.  1  2015 .bash_logout
-rw-r--r--   1 root root  3771 sept.  1  2015 .bashrc
drwxr-xr-x   3 root root  4096 févr. 15  2017 .config
-rw-r--r--   1 root root   655 juin  24  2016 .profile
-rw-r--r--   1 root root  1600 janv.  9  2016 .Xdefaults
-rw-r--r--   1 root root    14 janv.  9  2016 .xscreensaver
```

## changement d'identité

### `su`

La commande su (Pour Switch User) permet de changer l'identité courante. Le systeme demande alors le mot de passe de l'utilisateur cible (le mot de passe ne s'affiche pas) et ouvre un shell sous cette nouvelle identité.

Si on ommet de préciser l'utilisateur, alors l'utilisateur cible est root.
Si on ajoute un caratère `-` entre `su` et le login on charge alors l'environement de l'utilisateur cible (.profile etc...)

```bash
$ su - leo
Password:
leo@al-e6230:~$ id
uid=1001(leo) gid=1001(leo) groups=1001(leo),130(docker)
leo@al-e6230:~$ exit
$
```

### `sudo`

La commande `sudo` permet de changer d'identité sans connaitre le mot de passe de l'utilisateur cible et seulement pendant l'exécution de la commande qui suis. Cet outil est en général configuré pour demander le mot de passe de l'utilisateur qui passe la commande mais cela n'est pas obligatoir.
Enfin `sudo` permet d'enregistrer une trace de chaque commande ainsi passé.

```bash
$ sudo -u leo id
[sudo] password for alan:
uid=1001(leo) gid=1001(leo) groups=1001(leo),130(docker)
$ id
uid=1000(alan) gid=1000(alan) groups=1000(alan),4(adm)...
$ grep leo /var/log/auth.log
Mar 26 21:21:34 al-e6230 sudo:     alan : TTY=pts/4 ; PWD=/home/alan/form/B1/linux1 ; USER=leo ; COMMAND=/usr/bin/id
```

Cette command est très fréquemment utilisée pour prendre les droits root pour effectuer une tache d'administration cependant elle peu aussi être utilisé pour prendre une autre identité.

#### configuration sudo

La commande `visudo` permet d'éditer les droits sudo défini dans le fichier `/etc/sudoers`. Le `man 5 sudoers` est plutot complexe à appréhender.

Pour faire simple, dans la pratique :
On défini des Alias pour definir des classes de
host : Host_Alias
utilisateur de sudo : User_Alias
identité utilisable : Runas_Alias
commandes : Cmnd_Alias

Exemple :

```bash
Runas_Alias     OP = root, operator, %adm
```

L'alias OP vaudra pour les compte root opérator et les membre du groupe adm

```bash
Cmnd_Alias      SHELLS = /bin/sh, /bin/bash,\
                         /usr/local/bin/zsh
Cmnd_Alias      SU = /usr/bin/su
Cmnd_Alias      STD = ALL, !SU, !SHELL
```

Puis on défini des règle en utilisant ces alias :

> **Users OnHost=(Runas_user) option:cmd, option: cmd**

Avec les Options :
PASSWD : l'utilisateur devra saisir sont mot de passe
NOPASSWD : Lutilisateur ne saisira pas son mot de passe
NOEXEC : bloque les échapements, évite

Exemple :

```bash
%alan   ALL=(OP) NOPASSWD:STD
```

Les membres du groupe Alan sur tout les hosts peu prendre les identités OP (définies plus haut) pour passer les commandes STD (définies plus haut) sans avoir à saisir son mot de passe

> cet exemple est bien sur une mauvaise idée on peu asser facilement lancer un shell root malgré la restriction mise en place sur le bash, sh et su

Ou :

```bash
exploit    ALL=(root) PASSWD:ALL
```

Permettre au compte exploit de passer toutes commandes root après avoir passé son mot de passe
