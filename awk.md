# awk

Awk est un langage de programmation dédié au traitement de fichier texte représentant un tableaux de valeurs. Il est extrémement puissant et consomme que très peu de ressources.

## Principe

On défini un séparateur de champs par example le caractère ':' puis on exécute l'ensemble des lignes du programme awk sur les lignes du fichier à traiter. Les lignes sont traité une par une. Ainsi un fichier de plusieurs Giga de données sera traité avec une très faible consomation mémoire.

### Variables

Une chaine de caratère quelquonque est une variable, on lui assigne une valeur avec le symbole `=` puis on l'utilise par son nom.
exemple :

```bash
var="value" ; print(var)
```

> la chaine de caratère `value` n'est pas une variable mais bien une chaine de caratère car entourée de `"`

il est aussi possible d'utiliser des variables définissant une liste de valeurs:

```bash
tab[i]=var
```

ici, la variable tableau `tab` à l'indice `i` aura la valeur `var`

Il existe des variables spécifique au langage :

* FS pour Fields séparator : le séparateur de champs du tableau sa valeur par défaut représente un espace, une tabulation ou une succession d'espace ou de tabulation.
* NF pour Number of Field contiendra le nombre de colonne de la ligne courante
* NR pour Number of Record cotiendra le numéro de ligne en cours
* $0 contiendra toute la ligne en cours de traitement
* chacune des valeurs des colonnes du tableau sera contenu dans les variables $1, $2, $3, ... $(NF-1), $NF (le chiffre représente alors le numéro de colonne).

### Le programme

Chacune des lignes du programme ou chacune des instructions sont de la forme :

```bash
Condition { Action [; Action ] } ;
```

> le `;` est le séparateur d'instruction ou d'action

Si la condition est vrai alors les Actions qui la suive sont exécutés. On peu comprendre : si `Condition` alors `Action`
Si la condition est ommise alors elle est vrai

#### Les Conditions

Les conditions sont principalement des tests sur des variables

* `$1==root` la première colonne contiens la chaine de caratère `root`
* `$4>32000` la 4ieme colonne contiens un entier supérieur à 32000
* `NR==1` nous traitons actuellement la première ligne
* `NF==8` cette ligne à 8 colonnes
* `NR!=1 && NF==8` ce n'est pas la première ligne et elle contiens 8 colonnes
* `$8~/^bash/` la 8ieme colonne contiens l'expression régulière "^bash"

> Le double égal : `==` est le test d'égalité il est courant de se tromper avec une simple affectation de variable `=`.
> La double espèrluette `&&` est le ET logique?
> Le double pipe `||` est le OU logique.
> Des regex ! \o/

il existe deux conditions spécifiques :

* BEGIN c'est en général la première ligne du programme awk, les actions sont exécutées avant le traitement de la première ligne du fichier
* END c'est la dernière ligne du programme awk, les actions sont exécutées à la fin du traitement du fichier

#### Les actions

les Actions peuvent être :

* Des assignation de variables `max=$1`
* Des calculs arithmétique `nb=($4+$5)/max
* Un affichage en sortie standard : print(NR" : "$1" ... "NF" champs")

Les actions peuvent faire appels aux fonctions interne de awk : 

* print("voilà") : affiche sur la sortie standard la chaine "voilà"
* substr(chaine,2,length(chaine)-1) : est interprèté comme la sous chaine de "chaine" qui commence aux deusiemme caratère et de longeur : la longeur de la chaine moins 1 :  (haine)
* length(chaine) : retourne la longeure de la chaine "chaine" : 6
* gsub("cherche","remplace",var) ne retourne rien mais modifie la chaine contenue dans la variable var en remplcant toute les accurance de "cherche" par "remplace"
* sub("cherche","remplace",var) effectue la même chose mais ne remplace que la première occurence
* exit : sort du programe
* getline : passe à la ligne suivante sans recommancer l'exécution
* next : passe à la ligne suivante en recommançant l'exécution

ou utiliser les structure de controle courantes

```bash
if (condition) { Action ; Action }
```

```bash
while (condition) { Action ; Action }
```

```bash
for (index in tableau) { print "le tableau contien l'index "index" qui représente la valeur "tableau[index] }
```

> A oui, awk maitrise les tables de hachages, l'index peut être une chaine de caractère \o/

### Exemple de commande simple awk

Affiche le 4ieme champs du fichier /etc/passwd :

```bash
awk -F: '{print $4}' /etc/passwd
```

> l'option -F permet de définir le séparateur de champs pour le traitement

Retourne la valeur maximum du 4ieme champs du fichier

```bash
awk -F: '$4>max{max=$4}; END {print max}' /etc/passwd
```

Retourne une ligne par colonne du fichier csv en affichant le contenu de la colonne (en général le nom de la colonne dans les fichier csv)

```bash
awk -F, 'NR==1 { for (i=1;i<=NF;i++) print i" : "$i ; exit}' fichier.csv
```

Stop tout les processus bash qui ne sont pas exécuté sous l'identité "alan" : (on génère les commandes kill avec awk que l'on envois dans un shell root)

```bash
ps -e -o pid,user,comm | awk '$2!="alan" && $3=="bash" {print "kill "$1}' | sudo bash
```

> la concaténation de chaine de caratère est implicite

Il est aussi possible de definir et utiliser des fonctions, pour aller plus loin consultez ce [memento en français sur awk](https://www.shellunix.com/awk.html) (il y a d'autre memento sympa là). Pour une question rapide, utilisez le man.
