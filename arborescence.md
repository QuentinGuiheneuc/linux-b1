# Arborescence Unix

## Présentation

L'ensemble des fichiers sont organisés dans une [arborescence](definitions.md#arborescence) de fichiers.

L'organisation des fichiers sous Unix respecte la norme [FHS](./normes.md#FHS)

La **racine** ("*root*") de l'arborescence est nommée "`/`".

## Denomination des fichiers et dossiers

### Affichage des fichiers

La commande `ls` permet de lister les principaux attributs des fichiers, dossiers, si aucun fichier n'est passé en paramètre `ls` liste alors le contenu du dossier courant:

```bash
ls -al /home/alan/form/B1/fichier.txt
-rw-rw-r-- 1 alan alan 4 mars  17 14:05 fichier.txt
```

Passée avec les options `-a` et `-l` on peu retrouver :

* le premier caratère indique le type de fichier `-` ici c'est un simple fichier. (d pour un dossier etc...)
* `rw-rw-r--` les [droits](./droits.md) sur le fichier
* `1` : le nombre de références existantes vers ce fichier (liens physique)
* `alan` : L'utilisateur propriétaire du fichier
* ̀`alan` : Le groupe propriétaire du fichier
* `4` : Une estimation de sa taille en octet
* `Apr  13 23:05` :  La date de dernière modification du fichier
* `fichier.txt` : Le chemin relatif vers ce qui est listé

### Chemin absolu

Tout fichier peut être adressé **à partir de la racine** de l'arborescence en listant les différents dossiers qui mènent à lui. C'est le **chemin absolu**. **Il commence par `/`.**

Exemple:

```bash
/home/alan/form/B1/fichier.txt
```

### Chemins relatifs

* l'utilisateur se situe dans le répertoire courant nommé **CWD** (*current working directory*) ou "`.`"
* le dossier parent du dossier courant peu être nommé "`..`"

Il est alors possible de définir un **chemin relatif** au dossier courant vers tout fichier.

Exemples:

```bash
../B2/notes-finales.txt
```

Depuis le dossier `/home/alan/cours-linux/B1` ce chemin réprésente le fichier `/home/alan/cours-linux/B2/notes-finales.txt`.

Il existe une multitude de chemins relatifs possibles permettant, à partir d'un dossier défini, d'atteindre un fichier donné.

**On utilisera principalement les chemins absolus.**

### wildcards

Il est possible d'utiliser des joker (wildcard) dans la dénomination des fichiers

* `*` Signifie n'importe quel chaine de caratères
* `?` Signifie un caractère alphanumérique quelquonque
* `[a-f]` Signifie toutes les lettres de l'alaphabet entre a et f inclues
* `{*.pdf,*.jpg}` Signifie une liste de noms ou de  wildcards

voir le man pour plus de détails :

```bash
alan@al-e6230:~/form/B1/linux1$ man 7 glob
```

### Liens ou raccourci

#### Liens symboliques

Les liens symbolique sont des fichiers spéciaux qui  contiennent un chemin relatif ou absolu vers un fichier ou dossier existant ou non.
Ils sont créés avec la commande `ln` et l'option `-s`

```bash
alan@al-e6230:~/form/B1/linux1$ ln -s ../../B1/linux1/fichier.txt lien
alan@al-e6230:~/form/B1/linux1$ ls -al fichier.txt lien
-rw-rw-r-- 1 alan alan 4098 Apr 17 14:22 fichier.txt
lrwxrwxrwx 1 alan alan   27 Apr 17 14:45 lien -> ../../B1/linux1/fichier.txt
```

On a créer le lien `lien` qui pointe vers le fichier ../../B1/linux1/fichier.txt

> *On préfèrera encore une fois les chemins absolus sauf si cela n'est pas pertinent*

#### Liens physiques

Un lien physique est une simple référence vers un fichier existant. Il permet de faire apparaitre un fichier plusieurs fois dans une arborescence.
Ils sont créés avec la commande `ln`

```bash
alan@al-e6230:~/form/B1/linux1$ ln ../../B1/linux1/fichier.txt lien-physique
alan@al-e6230:~/form/B1/linux1$ ls -al fichier.txt lien-physique
-rw-rw-r-- 2 alan alan 4098 Apr 17 14:22 fichier.txt
-rw-rw-r-- 2 alan alan 4098 Apr 17 14:22 lien-physique
```

Le nombre de référence est maintenant à `2` sur les deux entrées du dossier courant, nous avons deux noms donc deux chemins différents pour le même fichier, la donnée n'existe qu'une seul fois.

> *Les liens physique sont soumis à une restriction importante : les deux références doivent être sur le même système de fichier. Nous verons cette notion un peu plus tard dans ce cours, comprenez ici par système de fichier le même disque ou le même volume de données*

## Les principaux sous répertoires de `/`

### `/etc`

Les fichiers de configuration du système et des applications installées sur le système.

### `/var`

Les données "chaudes", "en mouvement" du système, **Var**iable.

Avec :

* `/var/log` : Les logs et fichiers de traces
* `/var/spool` : Les files d'attentes
* `/var/local` : Le `/var` spécifique à ce host (associé à /usr/local)
* `/var/opt` : Le `/var` de l'optionnel
* `/var/www` : La racine de l'arborescence web

### `/tmp`

Espace de fichiers temporaire sur le système.

### `/bin`

Les commandes de base du système.

### `/lib`

Contient les principales [bibliothèques partagées](./definitions.md#bibliothèques).

### `/sbin`

Les commandes d'administration du système (**S**ystem **bin**).

### `/usr`

Le dossier d'installation des applications.

Sa sous-arbo est la suivante :

```bash
$ tree -d -L 1 /usr/
/usr
├── bin
├── games
├── include
├── lib
├── local
├── sbin
├── share
└── src
```

**On y retrouve `bin` `sbin` `lib`**

**/usr/share** : l'espace partagé par les application

et **/usr/local/** qui est l'équivalent du /usr d'une distribution mais pour la machine locale ; ce qui sera spécifique à ce host :

```bash
$ tree -d -L 1 /usr/local/
/usr/local/
├── bin
├── etc
├── games
├── include
├── lib
├── man -> share/man
├── sbin
├── share
└── src
```

**On y retrouve `bin` `sbin` `lib` `share` `src`** exactement comme `/usr`

### `/home`

Ce dossier contient les répertoires personnels (*home dir*) des utilisateurs.

### `/root`

Le dossier de l'utilisateur `root`.

### `/opt`

L'aborescence optionelle : ce que vous voulez.

### `/boot`

Les fichiers nécessaires au démarrage (boot).

### `/dev`

Les fichiers périphériques (devices). Ces fichiers représentent le matériel.

### `/proc`

Un dossier du kernel représentant les processus (sous Linux uniquement).

### `/media`

Le dossier de montage des médias amovibles.

### `/mnt`

Le dossier de montage temporaire (`/media` historique).

### `/srv`

Dossier courant contenant les fichiers Servis par le système (exemple les arborescences servies par une serveur FTP).

## En conclusion

Cette hierarchie standard n'est pas à apprendre ; en revanche il faut la connaitre.

le squelette de dossiers : **`/etc`, `/var`, `/tmp`, `/bin`, `sbin`, `/lib` et `/src`** est couramment utilisé dans différents environnements notamment dans des arborescences logiciels indépendantes du système.
