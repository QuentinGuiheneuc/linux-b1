# Les commandes filtres

Certaines commandes agissent comme des filtres, elles reçoivent de la données en entrée et va founir une sortie filtrée, `grep` est un bon exemple, `grep chaine fichier` lis le fichier et ne garde que les lignes qui contiennent la chaine de caratères `chaine`.

En redirigeant la sortie d'une commande dans une autre commande au travers du caractère `|` (un tube ou pipe en anglais) on utilise alors la seconde commande comme un filtre :

```bash
$ cat /etc/passwd | grep alan
alan:x:1000:1000:alan,,,:/home/alan:/bin/bash
```

## `head` La tète

Ne conserve que le début du fichier, on peu choisir le **nombre** de lignes en utilisant ce nombre comme une option.

```bash
$ head -1 /etc/passwd
root:x:0:0:root:/root:/bin/bash
```

## `tail` La queue

Ne conserve que la fin du fichier, on peut aussi choisir le **nombre** de lignes en utilisant ce nombre comme une option. il est possible de surveiller l'évolution de la fin d'un fichier avec l'option -f

## `grep` LE filtre

C'est le filtre par excellence il ne garde que les lignes qui "matchent" une wildcard ou une [expression régulière.](./definitions.md#regex)  

options principales (à connaitre car bien pratique)

* `-v` : versus , supprime les ligne qui "matchent" plutot que de les garder
* ̀`-o` : only matching part, ne garde pas toute la ligne mais seulement la partie qui "matche"
* `-e` : utilise les [expressions régulière](./definitions.md#regex)  
* `-A n` : conserve aussi les `n` lignes Après la ligne qui "matche" (A pour After)
* `-B n` : conserve aussi les `n` lignes qui précède la ligne qui "matche"
* `-q` : n'affiche aucune ligne il se contente de sortir avec le code retour 0 si une occurence a été trouvée et différent de 0 si elle n'a pas été trouvée (on vera la notion de code retour plus loin dans ce cours)

## `cut` : comme son nom l'indique il découpe

```bash
~$ grep alan /etc/passwd | cut -d: -f6
/home/alan
```

Avec le **D**élimiteur de champs `:` on ne garde que le champ (**F**ield) 6

En consultant le man 5 passwd vous saurez que le 6ieme champs du fichier password est le home-dir de l'utilisateur

Encore une fois le man de la commande cut (man 1 cut) vous proposera plein d'options possible de découpages.

Exemple :

```bash
~$ grep alan /etc/passwd | cut -c1-2,12-28
al:1000:alan,,,:/ho
```

Option `-c` pour caractères de 1 à 2 puis de 12 à 28

## `sort` : pour trier

```bash

~$ cut -d: -f4 /etc/passwd | sort -n
.../...
~$
```

Ici on trie en ordre numérique `-n` la sortie du filtre `cut` retournant le 4ieme champs du fichier passwd (les gids)

## `uniq` : pour dé-doublonner

Cette commande nécéssite que les doublons se suivent donc on l'utilise en sortie d'un tri

Avec l'option -c on va compter les doublons

```bash

~$ cut -d: -f4 /etc/passwd | sort -n | uniq
.../...
~$  
```

## `tr` : pour translate

Permet de translater un ensemble de caratère

on remplace le o en 0 les l en 1 les Z en 2 etc...

```bash
~$ echo nan mais rien en fait | tr “olzeasbtg” “012345679”
n4n m4i5 ri3n 3n f4i7
```

Dans ce [TD](./TD-filtres.md) vous verrez un exemple de commande filtre combinée puis vous en ferrez quelques unes.
