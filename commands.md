# Utilisation du shell

Cette page fournit l'essentiel à savoir sur l'utilisation de la ligne de commande en fournissant des exemples d'usage. Chaque commande est bien plus complète que ce qui est présenté.

Ce qu'il faut retenir ici : Le nom et l'objectif de chacune des commandes (philosophie : faire une chose et la faire bien). Lorsque vous aurez un besoin plus précis, vous lirez le [`man`](./man.md) pour trouver les options qui correspondent à votre besoin.

## Dénomination

* `un/chemin/relatif` : on nomme le fichier en mode *relatif* en fonction du répertoire courant : `.` (`un/chemin/relatif` est équivalent à `./un/chemin/relatif`)
* `/un/chemin/absolu` : on nomme le fichier en mode *absolu* en fonction de la racine de l'arborescence `/`  

**L'utilisation des chemin absolu est préférable.**

On notera les racourcis suivants :

* `.` : le répertoire courant
* `..` : le répertoire parent
* `~` : votre répertoire personnel (home)
* `*` : une chaîne de caractères quelconque
* `?` : un caractère quelconque

## Principales commandes

### Navigation

* ̀`pwd` : indique le répertoire courant.
  * *Print Working Directory*
* `ls /un/répertoire`  : liste le contenu du répertoire `/un/répertoire`
  * *LiSt*
* `cd`  : permet de changer de réperoire
  * *Change Directory*
  * `cd /un/répertoire` : permet de se déplacer dans /un/répertoire
  * `cd` (sans argument): permet de retourner dans son répertoire personnel

* `tree -L 3 /un/répertoire` : permet de visualiser la sous-arborescence commençant par /un/répertoire (en se limitant à 3 niveaux de sous-répertoires)

Sous `bash` : `↑` permet de rappeler une commande déjà passée, et `[ctrl]r` : permet de rechercher un chaîne dans l'historique des commandes déjà passées.

> L'utilisation de **chemins absolus** dans ses lignes de commandes permet notamment de rappeler les commandes de l'historique sans se soucier du dossier dans lequel on était lorsqu'on a passé la commande.

### Commandes de consultation

* `head fic1` : Affiche les 10 première lignes du fichier fic1
* `tail fic1` : affiche les 10 dernière lignes du fichier fic1
* `cat fic1 fic2` : Renvoie sur la sortie la concaténation des fichiers fic1 et fic2
(note : bzcat et zcat font de même mais décompresse le fichier à la volée).
* `more fic1` : Affiche sur la sortie le contenu du fichier fic1 page par page.
* `less fic1` : Affiche sur la sortie le contenu du fichier fic1 page par page.
(note : /chaine permet de rechercher la chaîne de caractères chaine)
* `wc -l fic1` compte le nombre de ligne d'un fichier
* `grep une-chaine /un/fichier` : Retourne sur la sortie toutes les lignes de /un/fichier qui contiennent "une-chaine".
* `file /un/fichier` : Retourne le format de fichier de /un/fichier après analyse de son entête.
* `type une-commande` : Retourne comment le shell va interpréter une-commande.
* `diff fic1 fic2` : compare les fichier fic1 et fic2 et retourne les lignes différentes de chacun des fichiers.
* `nl fic1` : retourne les lignes du fichier fic1 en les préfixants par un numéro de ligne.

### Gestion de fichiers

* `mkdir ~/un/répertoire` : crée ~/un/répertoire
* `rmdir ~/un/répertoire` : supprime ~/un/répertoire (s’il est vide)
* `touch ./un/fichier` : crée un fichier ou change sa date de modification
* `mv ~/un/fichier un-autre` : déplace/renomme le fichier ~/un/fichier vers ~/un-autre
* `cp ~/un/fichier ~/un/autre` : copie le fichier un/fichier vers un/autre
* `cp ~/un/fichier un/autre un/répertoire` : copie les fichiers un/fichier et un/autre vers un/répertoire (notez : l'option -p préserve les droits sur le fichier `cp -p`)
* `rm ~/un/fichier` : supprime le fichier ~/un/fichie (notez : l'option -rf permet de forcer la suppression récursive d'une sous-arborescence) **Attention la suppression est définitive**

### Archivage et compression

* `tar cvf une/archive.tar des fichiers à mettre dedans` : crée une/archive.tar contenant les fichiers et sous-arborescences : `./des`, `./fichiers`, `./a`, `./mettre` et `./dedans`
* `tar xvf une/archive.tar` : extrait le contenu de ./une/archive.tar dans le répertoire courant
* `tar xvf une/archive.tar un-fichier`: extrait un-fichier d'une/archive.tar
(note : les options c, x, v et f signifient **C**reate e**X**tract **V**erbose **F**ile)
* `tar xvf -` : extrait une archive lue sur l'entrée standard. Exemple : `bzcat une-archive.tar.bz2 | tar xvf -`
* `gzip un/fichier` : compresse le fichier un/fichier et le remplace par le fichier compressé un/fichier.gz
* `bzip2 un/fichier` : idem au format bz2 avec le fichier un/fichier.bz2
* `gunzip un/fichier.gz` : décompresse le fichier
* `bunzip2 un fichier.bz2` : avec le format bz2
* `tar zxvf une/archive.tgz` : décompresse au format gzip puis extrait l'archive
* `tar jcvf une/archive.tar.bz2 liste fichiers` : crée l'archive compressée au format bz2

## Gestion des sorties

* `une-commande >> un/fichier` : redirige la sortie standard d'une-commande vers un/fichier
* `une-commande > un/fichier` : redirige la sortie standard d'une-commande vers un/fichier en écrasant le contenu existant
* `une-commande 2> un/fichier` : écrase ou crée un/fichier et y écrit la sortie d'erreur d'une-commande
* `une-commande >> un/fichier 2>&1` : ajoute à la fin d'un/fichier la sortie standard (>>) et redirige la sortie d'erreur vers la sortie standard (2>&1)
* `une-commande | une-autre-commande` : redirige la sortie standard d'une-commande vers l'entrée d'une-autre-commande

Avec cette dernière redirection : la sortie d'une commande vers l'entrée d'une autre la seconde commande pourras être utilisée comme un [filtre](./filtres.md)

Faisons un petit [TD](./TD-commands.md)
