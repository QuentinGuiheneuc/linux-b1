# VI

## Présentation

C'est l'éditeur de texte universel sous unix :

* Il est léger et ultra puissant  
* Il est présent sur toutes les distributions unix et linux
* Il a été conçu pour l'utilisation d'un terminal, le clavier était alors rudimentaire (pas de pavé numérique ou de flèche de déplacement).

## Invocation

Ouverture d'un fichier :

```bash
vi fichier
```

Récupèration d'un fichier mal fermé :

```bash
vi -r fichier
```

Ouverture de plusieurs fichiers :

```bash
vi fichier1 fichier2
```

on utilisera par la suite le mode commande pour basculer en édition de fichier en fichier (:n pour next et :N pour precedent)

## Il y a trois modes d'interaction

### Navigation

Pour se déplacer dans le texte et utiliser les commandes simple.
On entre ou on retourne dans ce mode avec la touche `[esc]`, c'est le mode par défaut de vi.
Pour de se déplacer dans le texte on utilisera les caratère :

* `h`: un caractère à gauche
* `j`: un caractère à bas
* `k`: un caractère à haut
* `l`: un caractère à droite
* `w` / `b` : aller au prochain (b pour backward donc précédent) mot
* `0` ou `^` : aller au début de la ligne
* `$` : aller en fin de ligne
* `G` : aller en fin de fichier
* `gg` : aller au début du fichier

On utilisera aussi dans ce mode les racourci de traitement du texte (supression copier coller etc...)

* `x` : Pour supprimer un caractère
* `j` : Pour supprimer la fin de ligne (joindre la ligne suivante)
* `dd` / `3dd` / `d3d`: supprimer la (les 3) ligne(s)
* `d3w` : delete 3 words
* `D` : supprime jusque la fin de la ligne
* `yy` / `7yy` / `y7y`: copier 'yank' la (les 7) ligne(s)
* `y4w` : yank 4 words
* `p` : coller après la position courante (paste)
  
Enfin on pourra basculer dans les deux autres mode
  
### Insertion

Pour la saisie du texte

Depuis le mode navigation en passant une commande d'insertion :

* `i` / `a` : insertion avant/après le cuseur
* `I` / `A` : insertion en debut/fin de ligne
* `o` / `O` : ouvre une nouvelle ligne en mode insertion après/avant la ligne courante
* `cw` / `c7w` : change word : supprime le mot courant (ou le courant et les 6 suivants) et passe en mode insertion pour le(s) remplacer.
* `c/toto` : change tout jusqua ce que tu trouve le mot 'toto'

### Commande

Pour le passage d'une commande de traitement.

Depuis le mode navigation après la saisie du caratère `:` on peu saisir une commande de traitement (c'est a peu près une commande `sed`)

* `:w` : write/sauvegarder
* `:q` : quitter
* `:wq` / `:x` : sauvegarder et quiter
* `:n` / `:N` : passer au fichier suivant / précédent
* `:r chemin/vers/un/fichier` : ajoute le contenu du fichier nommé à partir de la ligne courante (read)
* `:w chemin/vers/un/fichier` : sauvegarde une copie dans le fichier nommé
* `:! ls` : passe la commande ls
* `r !ls` : ajoute dans le fichier le résultat de la commande ls

#### Commandes avancée

Syntaxe : `départ,ArrivéeActionArgument`

Exemple : `4,12m24`
Pour les lignes de 4 à 12 ; déplacer (m pour move) le bloc à la ligne 24

de Départ à arrivée : On défini un groupe de ligne avec pour départ ou arrivé :

* X, un numéro de ligne
* rien : pour la ligne courante
* +/-X : la Xieme ligne après ou avant la ligne courante

Commandes :

* m pour déplacé l'argument est alors la ligne cible
* d pour supprimer
* co pour copier l'argument est alors la ligne cible
* s search/replace de la commande sed : s/search/replace/g ou s!regex!replace!g
* w sauvegarde cette partie dans le fichier nommé par l'argument
Exemples:
* `:,+4s/search/replace/g` de la ligne courante et les 4 suivantes remplace `search` par `replace` ; pour toute les occurences (si on ommet le `g` seul la première occurence `search` de la ligne sera remplacée)
* `:-4,co+6` copie 6 lignes plus bas les bloc compris composé des 4 lignes précédantes et la ligne courante

## Sed

Sed est un éditeur de fichier non intéractif. Il execute un certain nombre de commande d'édition sur le fichier passé en arguement ou sur ce qu'il reçois en entrée standard.
Par défaut (sans les options -n ou -i) il affiche tout ce qu'il reçois en entrée.

### invocation

```bash
sed [-n] [[-e] commande] [-f fichier de commandes] [[-i] fichier]
```

* `-i` pour 'inplace' on **modifie directement** le fichier Attention cette commande modifie les données sur disque.
* `-n` on n'affiche pas ce qui est en entrée mais seulement ce qui doit être produit en sortie (commande p pour print)
* `-e 'comande'` : on peu passer plusieurs commandes en une seule invocation elle sont toutes définie avec l'option -e
* `-f fichier` : l'ensemble des commande sont spécifié dans le fichier de commande sed (une ligne par commande)

les commandes sont les mêmes que celles du mode commande de vi sans utiliser le caractère `:` propre à vi. On pourra ajouter `p` pour print

### exemple

* simple

  ```bash
  $ sed 's!:!::::!g' /etc/group | head -2
  root::::x::::0::::
  daemon::::x::::1::::
  ```
  
  on remplace chacune des occurences de `:` par `::::`

* via un fichier :

  ```bash
  $ sed -n -f sed-cmd-file /etc/group | head -4
  sudo ; gid 27 ; members -> alan
  $ cat sed-cmd-file
  s!:! ;!
  s!x:\([0-9]*\)! gid \1 ;!
  s!:\(.*\)$! members -> \1!
  21p
  ```

  successivement :  
  on remplace le premier `:` par `;`  
  on remplace `x:un-nombre` par `gid un-nombre ;`  
  on remplace `:une-chaine` par `members -> une-chaine`  
  on affiche que la ligne 21  
