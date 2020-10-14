# Environnements d'exécution Unix Linux

## Variables shell

Une **variable** est un mot qui définit une valeur abstraite. Dans le shell, le mot commence par un caractère alphabétique.  
D'usage, on les définit en minuscules car les variables en majuscules sont celles definies et utilisées par le système.

Lorsque dans le shell on travaille sur une variable :

* soit on lui assigne une valeur.
* soit on traite en fait la valeur qu'elle contient.

--

Pour définir la variable : `var` et lui affecter la valeur : `val`

```bash
alan@al-e6230:~/form/B1/linux1$ var=val
```

---

Pour utiliser une variable et donc traiter sa valeur nous la préfixons par un `$` :

```bash
alan@al-e6230:~/form/B1/linux1$ echo $var
val
```

---

On place parfois le nom de variable entre accolades afin de bien séparer la partie nom de variable d'une autre chaine de caractère :

```bash
alan@al-e6230:~/form/B1/linux1$ echo "${var}eur"
valeur
```

> C'est une **bonne pratique** d'utiliser la notation avec accolades **systématiquement**. Cela permet une relecture et une modification du code plus aisées.

---

Une variable non définie est utilisable et renvoie une valeur nulle : une chaîne de caractère vide.

Pour supprimer la variable `var` :

```bash
alan@al-e6230:~/form/B1/linux1$ unset var

# Affichage de la variable, alors qu'elle est non-définie
alan@al-e6230:~/form/B1/linux1$ echo $var
alan@al-e6230:~/form/B1/linux1$
```

---

Pour bloquer une variable en lecture seule :

```bash
alan@al-e6230:~/form/B1/linux1$ readonly var
alan@al-e6230:~/form/B1/linux1$ unset var
bash: unset: var: cannot unset: readonly variable
```

Celle-ci ne sera ni modifiable, ni suppressible.

---

Une variable n'est pas visible par les *sous processus* de votre shell, c'est à dire les commandes externes que vous passez à votre shell.

Afin de rendre une variable utilisable par des éventuels futurs processus fils , on **l'export** :

```bash
alan@al-e6230:~/form/B1/linux1$ export var
```

## Environnement

L'environnement est un ensemble de variables et fonctions qui résident en mémoire (RAM), les variables et fonctions *exportées* son transmisent aux sous processus (et commandes externes du shell).

La commande `set` permet d'afficher toutes les variables et fonctions présentes dans l’environnement.

La commande `env` ou `printenv` permet d'afficher les variables exportées.

### Environnement d'exécution du shell

Voici quelques variables notables du shell (elles sont prédéfinies) :

`LOGNAME` : le nom du compte utilisateur au login  
`USER` : le nom du compte utilisateur courant  
`UID` : l'identifiant numérique du compte utilisateur  
`HOME` : le répertoire principal de l’utilisateur  
`LANG` : le langage utilisé par le shell  
`PID` : l'identifiant numérique du processus en cours d'exécution  
`PPID` : l'identifiant numérique du processus père du processus courant  
`PWD` : le répertoire courant du shell  
`TERM` : le type de terminal actuellement utilisé  
`COLUMNS` / `LINES` : le nombre de colonnes/lignes du terminal  

#### `PATH`

La variable `PATH` a un rôle très important dans le fonctionnement du shell.  
Elle définit :

* Les répertoires contenant les commandes
* L'ordre de recherche dans ses répertoires

Elle permet d'appeler les commandes externes sans fournir le chemin complet de celles-ci.

> **Attention : il est possible en changeant cette variable de vous faire exécuter de mauvaises commandes.**

### Alias

Les alias permettent le remplacement d'un alias de commande par une commande complexe lors de l'évaluation de la ligne de commande par le shell.

Exemple : `ll` se traduit par la commande `ls -al`

Passée sans arguments, la commande `alias` retourne les alias déjà définis (à l'initialisation du shell)

Ici on déclare l'alias de commande `lr` qui sera traduit par le shell en `ls -altr` :

```bash
prompt$ alias lr='ls -altr'
```

On encadre bien la chaîne de caratères par des simple quotes.

La commande `unalias` permet de supprimer un alias

```bash
prompt$ unalias ll
```

## Configuration de l'environnement

Lors de son initialisation, le shell source des fichiers de configurations afin de définir l'environnement initiale.

Sourcer des fichiers c'est exécuter dans le shell courant toutes les commandes définies dans ces fichiers.

Exemple :

```bash
prompt$ echo $var

prompt$ cat env.sh
var=val
prompt$ . env.sh
ptompt$ echo $var
val
```

Les fichiers de configuration du shell sont standardisé et ils agissent au niveau :

* du système :
  * /etc/profile  
  * /etc/profile.d/*
  * puis en fonction du shell de l'utilisateur :
    * /etc/bash.bashrc  
    * /etc/bash_completion  
    * /etc/bash_completion.d/*  
  * etc...  
  Ces fichier seront donc sourcer pour initialiser l'environement de tout les utilisateurs  
* du compte utilisateur  
  * ~/.profile
  * ~/.bashrc  
  etc... dans son dossier personnel ils sont donc personalisable :  

Exemple de personnalisation :

* La variable PS1 qui définit la forme du prompte peut être personalisée pour disposer par exemple de la branche git en cours  
* ajouter la completion git dans sont shell en suivant la [documentation git](https://git-scm.com/book/fr/v2/Git-dans-d%E2%80%99autres-environnements-Git-dans-Bash)
* definir les couleurs associées aux types de fichiers pour la commande ls
* Définir des alias pour des comande courantes :
 `alias gitnb='git checkout -b'`

un petit [TD](./TD-environment.md), ça vous dit ?