# La commande `find`

## `man find`

Lisez le man !

### En résumé

#### Synopsis

```bash
find [options] [path] [expresion]
```

* Options : Pour modifier le comportement : debug, gestion des liens symboliques, ordre de parcour etc...
* Path : a partir d'ou chercher (`.` si pas précisé)
* Expression : C'est une suite de filtres de recherche, certains effectuent une action et tous retournent vrai ou faux suivant les cas.

#### Comportement

La commande `find` parcourt l'arborescence de fichiers à partir du [path] défini.  
Elle parcourt un par un les fichiers et dossier présents dans la sous arbo.

L'expression logique est appliquée jusqu'à détermination du statut de l'expression pour ce fichier puis `find` passe au fichier suivant.

L'expression logique est évaluée de gauche à droite. Chaque filtre retourne vrai ou faux sur le fichier évalué. Les filtres sont par défaut liés par le `et` logique ; ainsi dès qu'un filtre renvoit "faux" la suite de l'expression est ignorée car l'ensemble restera faux quoiqu'il arrive (`faux` `et` `vrai` = `faux`).

Certains filtres effectuent une action sur le fichier (exemple `-ls` affiche les attributs du fichier) si aucune action n'est définie, `find` affiche le chemin des fichiers pour lesquels l'expression retourne vrai.

#### Exemple simple

```bash
~$ find . -name 'fichier.txt' -ctime +3
~$ find . -name 'fichier.txt' -ctime -3
./fichier.txt
```

La première commande recherche les fichiers et dossiers dont le nom est fichier.txt **ET** qui à été modifié il y a **plus** de 3 jours : `find` ne retourne rien.

La seconde commande recherche les fichiers dont le nom est fichier.txt **ET** qui a été modifié il y a **moins** de 3 jours.  
Un fichier est trouvé, son chemin relatif est affiché (à partir de `.`)

### Options

* `-depth` : traite les sous-répertoires avant les répertoires
* `-mandepth x` : ne va pas plus profond que x-1 sous-répertoires (`-mindepth` pas moins profond que x sous-rép)
* `-daystart` : utilise 00h00 aujourd'hui comme base de temps pour maintenant
* `-mount` : ne traite pas les autres FS

### Filtres

Les filtres sont composés en général d'un nom de filtre suivi d'un argument.

#### Filtre sur le nom du fichier

* `-name '*patern*'` : retourne vrai si le nom de fichier match la wildcard `*patern*`
* `-iname` : idem mais sans tenir compte de la case
* `-regex '^regex$'` : on utilise les [regex](./definitions.md#regex) plutôt que les wildcards
* `-path` : la pattern concerne le chemin uniquement, pas le nom de fichier
* `-samefile /un/fichier` : est un lien physique vers `/un/fichier` (ou juste le même mais en passant par un autre chemin)

#### Sur les attributs de fichier

`-type {b,c,d,p,f,l,s}` : type de fichiers (`f` : un simple fichier, `d` un dossier, `l` un lien symbolique, etc...) : renvoie vrai si le fichier est de ce type.

`-size nX` : renvoie vrai si le fichier occupe exactement n*X d'espace avec pour X :

* b : 512octet
* c : octet
* w : 2octet
* k : KB
* M : MB
* G : GB

A chaque fois le numérique `n` peut être :

* une valeur N entière pour dire exactement N
* `+N` pour dire plus que N
* `-N` pour dire moins que N

**Dates de** a : acces, c : changement des métadonnées du fichier (de l'inode), m : changement des données du fichier.

`-atime n`, `-ctime n`, `-mtime n`
 n est un nombre de jour, le test retourne vrai si le fichier à été modifié il y a 24 * n heures

`-amin n`, `-cmin n`, `-mmin n`
n est un nombre de minutes.  
exemple : `-4` il y a moins de 4 minutes, `+10` il y a au plus 10 minutes

`-newer /un/fichier` : vrai si modifié plus récement que `/un/fichier`

`-used n` vrai si le dernier accès date de n jours après changement des métadonnées du fichier.

## Les actions

* `-prune` : demande à `find` de ne pas descendre dans la sous-arbo (test toujours vrai)
* `-delete` : supprime le fichier
* `-print` : affiche le chemin du fichier
* `-ls` : liste les attributs du fichier
* `-fprint resultats.txt` : idem -ls mais dans le fichier resultats.txt
* `-printf format` : affiche au format $format$
* `-exec cmd {} \;` : exécute la comande `cmd` pour chaque fichier, le chemin de fichier sera placé à la place de `{}` dans la ligne de commande qui se termine par `\;`
* `-ok cmd {} \;` : idem mais demande la validation
* `-quite` : sort du find maintenant

## Combinaison d'expression logique

Soit les expressions logiques de commande find expr, expr1 et expr2 (une suite de filtre et d'arguments de filtre)

* `(expr)` : groupe toutes les expressions formées dans expr
* `expr1 expr2` : les expressions sont lié par un `et` logique
* `expr1 -o expr2` : les expressions sont lié par un `ou` logique
* `expr1 , expr2` : find traite les 2 expression indépendament au cours de la recherche (ne tien pas compte du résultat de expr1)
* `-not expr` : un non logique

Un petit [TD](TD-find.md) pour finir.
