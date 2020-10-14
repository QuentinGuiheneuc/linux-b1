# TD utilisation du bash

TD beaucoup trop simple a mon goût, mais bon.

## la base

### `id`

Pour **id**entity

```bash
~$ id
uid=1000(alan) gid=1000(alan) groups=1000(alan),27(sudo),125(wireshark)
```

La commande vous retourne votre identité et les groupes auxquels vous appartenez.

### `pwd`

Pour **P**athname of the current **W**orking **D**irectory ou **P**rint **W**orking **D**irectory

```bash

~$ pwd
/home/alan
```

### `mkdir`

Pour **m**a**k**e **dir**ectory

Créez deux dossiers `tmp` et `tmp2`

```bash
~$ mkdir tmp
~$ mkdir /home/alan/tmp2
```

L'argument `tmp` est un chemin relatif au répertoire courant, l'argument `/home/alan/tmp2` est un chemin absolu qui ne fonctionne que si `/home/alan` existe, d'où l'intérêt des chemins relatifs.

### `cd`

Pour **C**hange **D**irectory, permet de changer de dossier courrant

```bash
~$ cd tmp
~/tmp$
```

`cd /le/chemin` permet d'aller dans le dossier pointé par /le/chemin.  

`cd` seul permet de revenir dans son *home* (son dossier personnel).

```bash
~$ cd tmp
~/tmp$ cd
~$
```

`cd -` permet de revenir au dossier précendent

```bash
~$ cd tmp
~/tmp$ cd ~/tmp2
~/tmp2$ cd -
~/tmp$ cd -
~/tmp2$  
```

### `touch`

Permet de **touch**er un fichier et donc gérer sa date de dernière modification ; s'il n'existe pas il est alors créé.

```bash
~/tmp$ touch fichier
~/tmp$ ls
fichier
```

### `echo`

Permet d'écrire sur sa sortie standard ce qui lui est transmis en argument.

```bash
~/tmp$ echo "hello world"
hello world
~/tmp$ echo "hello world" >> fichier
```

Avec `>>` on redirige la sortie vers un fichier, le résultat est donc écrit dans le fichier.

### `cat`

Permet de concaténer sur sa sortie les fichiers listés en argument.

```bash
~/tmp$ cat fichier
hello world
```

Ici la sortie est votre terminal, le contenu est alors affiché.

### `stat`

Fournit les méta-données d'un fichier.

```bash
~/tmp$ stat fichier
  File: `fichier'
  Size: 8          Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d Inode: 8800390     Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/    alan)   Gid: ( 1000/    alan)
Access: 2017-01-30 22:41:17.069795532 +0100
Modify: 2017-01-30 22:41:17.069795532 +0100
Change: 2017-01-30 22:41:17.169797490 +0100
 Birth: -
~/tmp$
```

## Navigation

### Chemins relatifs et absolus

Les chemins de fichiers et dossiers commençant par `/` sont dits absolus car il représente le chemin vers le fichier à partir de la racine. Les autres chemins sont relatifs au répertoire courant : `pwd`

Le chemin relatif dépend donc de l'en droit ou l'on se situe actuelement :

* Depuis mon home cd tmp vas dans le dosier /home/alan/tmp

  ```bash
  ~$ cd tmp
  ~/tmp$
  ```

* Depuis la racine je vaois dans le dossier /tmp

  ```bash
  ~$ cd /
  /$ cd tmp
  /tmp$
  ```

Le chemin absolue est lui valable quelque soit le dossier courant.

```bash
~/tmp$ cd /home/alan/tmp2
```

> Par souci d'efficacité et sauf en cas de besoin précis en terme de chemin relatif ; Je passe toute mes commandes avec un chemin absolu tout en restant dans mon répertoire personnel (mon home); cela me permet de réutiliser l'historique des commandes accessible avec les flèches haut et bas ou sur lequel je peux faire une recherche avec les touches [ctrl]+r et de rediriger mes sorties vers des fichiers dans mon dossier perso.

### `tree`

La commande tree permet de représenter une sous arborescence sur un terminal :

```bash
~$ tree .
.
├── tmp
└── tmp2
```

En revanche une sous arborescence peu être trés profonde
(l'option -p de mkdir permet de créé toute l'arbo, donc ici les 3 dossiers d'un coup)

```bash
~$ mkdir -p tmp/subtmp/subsubtmp/subsubsubtmp
~$ tree .
.
├── tmp
│   └── subtmp
│       └── subsubtmp
│           └── subsubsubtmp
└── tmp2

5 directories, 0 files
```

Auquel cas on peu souhaiter limite la profondeur de la commande tree avec l'option `-L`

```bash
~$ tree . -L 2
.
├── tmp
│   └── subtmp
└── tmp2

3 directories, 0 files
```

## consultation

### `less`

Permet de consulter un fichier page par page avec :  
`g` ou `G` début et fin de fichier, `u`/`d` pour up et down (fonctionne avec les flèches et la barre d'espace)  
`/chaine` : recherche `chaine` dans l'ensemble du fichier et `n` (next) pour la prochaine occurrence `N` l'occurence précendente

```bash
~$ less /var/log/messages
.../...
```

Essayez les raccourcis suivants :

* `G` vous arrivez à la fin du fichier
* `g` vous revenez au début du fichier
* `/net` pour rechercher le mot `net` puis `n` ou `N` pour aller d'occurence en occurence, vers le bas ou vers le haut

### `grep`

Permet de filtrer un fichier en ne gardant que les lignes qui contiennent une expression régulière.

```bash
~$ grep net /var/log/messages
Jan 30 20:50:34 e4310 kernel: [99132.800761] ipheth 2-1.3:4.2: Apple iPhone USB Ethernet device attached
.../...
```

> Cette commande est utilisée tout le temp !

### `head`

Permet de ne récupérer que le haut d'un fichier.

```bash
~$ head -1 /etc/passwd
root:x:0:0:root:/root:/bin/bash
```

### `tail`

Permet de ne récupérer que la fin d'un fichier.

```bash
~$ tail -2 /var/log/messages
Jan 31 14:05:05 e4310 kernel: [161095.431254] cfg80211:     (5735000 KHz - 5835000 KHz @ 80000 KHz), (N/A, 2000 mBm)
Jan 31 14:05:05 e4310 kernel: [161095.431256] cfg80211:     (57240000 KHz - 63720000 KHz @ 2160000 KHz), (N/A, 0 mBm)
```

Option `-2` pour les 2 dernières lignes seulement.

`tail -f` permet de surveiller la fin d'un fichier (voir la fin du fichier évoluer. On l'utilise régulièrement pour surveiller l'évolution d'un fichier de log).

## Manipulation de fichiers

### `cp`

Copie de fichier.

```bash
~$ cp ~/tmp/fichier ~/tmp2/copiefichier
~$ ls -l tm*/*fic*
-rw-r--r-- 1 alan alan 12 Jan 30 23:15 tmp/fichier
-rw-r--r-- 1 alan alan 12 Jan 30 23:16 tmp2/copiefichier
~$
```

Ici on utilise des `*` en argument, ce sont des [wildcards](./arborescence.md#wildcard)

Cela est traduit par le shell en remplaçant la chaîne de caractères par tous les chemins existants possibles.

* `*` Signifie n'importe quelle chaîne de caratères, même vide
* `?` Signifie un caratère alphanumérique quelconque

Copie de plusieurs fichiers vers un dossier.

```bash
~$ mkdir tmp3 tmp4
~$ cp tmp/fichier tmp2/fichier2 tmp3
~$ cp tmp/fichier tmp2/fichier2 tmp4
~$ ls -l tmp?/fic*
-rw-r--r-- 1 alan alan 12 Jan 30 23:16 tmp2/fichier2
-rw-r--r-- 1 alan alan 12 Jan 30 23:16 tmp3/fichier
-rw-r--r-- 1 alan alan 12 Jan 30 23:16 tmp3/fichier2
-rw-r--r-- 1 alan alan 12 Jan 30 23:16 tmp4/fichier
-rw-r--r-- 1 alan alan 12 Jan 30 23:16 tmp4/fichier2
```

### `mv`

Déplacement/renommage de fichier.

```bash
~$ mv ~/tmp2/copiefichier ~/tmp2/fichier2
~$ ls -l tm*/fic*
-rw-r--r-- 1 alan alan 12 Jan 30 23:15 tmp/fichier
-rw-r--r-- 1 alan alan 12 Jan 30 23:16 tmp2/fichier2
~$
```

### `rm`

Pour **R**e**M**ove permet de supprimer des fichiers ou toute une arbo **de façon définitive**. L'option `-rf` pour **R**ecursive et **F**orce.

```bash
~$ rm -rf tmp4
~$ ls tmp4
ls: cannot access tmp4: No such file or directory
```

### `tar`

Pour **T**ape **AR**chiver permet de faire un fichier contenant une archive de plusieurs fichiers ou d'une arborescence.

#### Créer une archive

```bash
~$ tar zcvf backup.tar.gz tmp?/fic*
tmp2/fichier2
tmp3/fichier
tmp3/fichier2
~$
```

* `tar` : la commande
* `zcvf` : les options :: **Z**:compress, **C**:Create, **V**:Verbose, ***F*:fichier : créer un fichier d'archive compressé en listant son contenu
* `backup.tar.gz` : argument : l'archive à créer
* `tmp?/fic*` : arguments : les fichiers à archiver

#### Lister le contenu

L'option T pour **T**able **O**f **C**ontent (toc) : liste le contenu de l'archive compressée.

```bash
~$ tar ztvf backup.tar.gz
tmp2/fichier2
tmp3/fichier
tmp3/fichier2
~$
```

#### Restauration

`tar` permet aussi de restaurer une archive.

Tout d'abord, on supprime définitivement le dossier original :

```bash
~$ rm -rf tmp3
```

Puis on restore depuis l'archive :

```bash
~$ tar zxvf backup.tar.gz tmp3/
tmp3/fichier
tmp3/fichier2
~$ ls tmp3/*
tmp3/fichier  tmp3/fichier2
```

## Le man

* Recherchez dans les pages du `man` et répondez aux questions :
  * A quoi sert l'option `-a` de la commande `ls` ?
  * Quelle option faut-il passer à la commande `ls` afin d'afficher la date de dernier accès au fichier plutôt que la date de dernière modification du fichier ?
  * Comment je peux changer la date de dernière modification d'un fichier à une date future ? (comme ça pour rigoler un peu)
* Prouvez, par une suite de commandes, que cela est bien le cas.
