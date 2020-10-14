# Le manuel

La commande `man` permet de consulter le manuel d'une commande externe.  
La commande suivante fournit **le manuel du manuel.**

```(bash)
alan@al-e6230:~/form/linux1$ man man
```

Les pages de manuel sont trés complètes et contiennent beaucoup d'informations. Vous devez savoir lire le manuel.

## Chapitres du `man`

Le manuel est divisé en 9 sections, chaque section traitant d'un type d'aide :

* Section 1 Les programmes exécutables ou commandes externes du shell
**-** *pour les utilisateurs*
* Section 2 Appels système (les fonctions fournies par le noyau)
**-** *pour les dev Unix*
* Section 3 Appels de bibliothèques (fonctions fournies par les bibliothèques des programmes)
**-** *pour les dev Unix*
* Section 4 Fichiers spéciaux (situés généralement dans /dev)
* Section 5 Formats des fichiers et conventions. Par exemple `/etc/passwd`
**-** *pour les adminsys*
* Section 6 Jeux
* Section 7 Divers  
* Section 8 Commandes de gestion du système
**-** *pour les adminsys*
* Section 9 Sous-­programmes du noyau

## Consultation

La consultation du manuel se fait au travers d'une commande `more`.

On se déplace avec les flèches ou les raccourcis :

* *u* pour Up
* *f* pour Forward (ou la barre d'espace)
* *G* pour aller à la fin du fichier
* *g* pour retourner au début

Il est aussi possible de faire des recherches dans la page du manuel :

* La touche “/” du clavier permet de saisir la chaîne à rechercher.
* La touche “n” permet de chercher l'occurrence suivante
* La touche ”N” permet de revenir à l’occurrence précédente

On quitte avec le raccourci *q* (*Quit*).

---

Exemple de page de man :

```(bash)
MAN(1)                                  Manual pager utils                                 MAN(1)

NAME
       man - an interface to the on-line reference manuals

SYNOPSIS
       man  [-C  file]  [-d]  [-D]  [--warnings[=warnings]]  [-R  encoding]  [-L locale] [-m sys‐
       tem[,...]] [-M path] [-S list] [-e extension] [-i|-I] [--regex|--wildcard]  [--names-only]
       [-a]  [-u]  [--no-subpages]  [-P  pager] [-r prompt] [-7] [-E encoding] [--no-hyphenation]
       [--no-justification] [-p string] [-t] [-T[device]] [-H[browser]] [-X[dpi]] [-Z] [[section]
       page ...] ...
       man -k [apropos options] regexp ...
       man -K [-w|-W] [-S list] [-i|-I] [--regex] [section] term ...
       man -f [whatis options] page ...
      .../...
DESCRIPTION
       man is the system's manual pager.  Each page argument given to man is normally the name of
       a program, utility or function.  The manual page associated with each of  these  arguments
       .../...

.../...
```

En haut le nom de la page du manuel avec la section concernée entre parenthèses : MAN(1)

Puis la commande est présentée très brièvement.

Suit le synopsis définissant les syntaxes d'usage de la commande :

* Chaque ligne est un cas d'usage avec les options et arguments
  * lorsqu'entre crochets, ils sont facultatifs
  * ils sont en général expliqués par la suite dans le `man`.
* le symbole `|` signifie un “OU” logique,
  * dans l'exemple montré au dessus, l'option `-i` peut être aussi appelée `-I`
* **le synopsis semble complexe car il est exhaustif, *il faut s'habituer à cette syntaxe*.**

Enfin le manuel :

* Une description détaillée (DESCRIPTION)
* L'explication des options (OPTIONS)
* Souvant des exemples d'usage (EXEMPLE)
* Les fichiers associés (FILE)
* Les pages de manuels liés à celle-ci (SEE ALSO)

## Recherche

Il est possible de faire une **recherche dans le manuel**, la commande suivante recherche [l'expression régulière](./definitions.md#regex) “^passwd$” dans les noms des pages du manuel :

```(bash)
alan@al-e6230:~/form/B1/linux1$ man -k --regex "^passwd$"
passwd (1)           - change user password
passwd (1ssl)        - compute password hashes
passwd (5)           - the password file
```

Ici elle nous retourne simplement la liste des pages du manuel (et la section associée) dont le nom est "passwd".
