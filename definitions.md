# Quelques définition

## Programmes informatiques

Un programme informatique est un ensemble d'instructions destiné à être exécuté par un ordinateur. Il peut exister sous forme de code source ou en tant que logiciel exécutable.

[wikipedia:programme informatique](https://fr.wikipedia.org/wiki/Programme_informatique)

### Logiciel / Software

C'est un ensemble d'instructions ou de séquences d'instructions exécutable sur du matériel(hardware) informatique.

[wikipedia:logiciel](https://fr.wikipedia.org/wiki/Logiciel)

### Code source

Le code source est la version lisible et interprétable par un être hummain du programme, contrairement au logiciel lui même qui est exécuté ou interprété par une machine.

[wikipedia:code source](https://fr.wikipedia.org/wiki/Code_source)

### Application

Une appli, c'est un ou plusieurs logiciel(s) permettant de réaliser un ensemble de tâches cohérentes dans un domaine particulier.
Exemples :

* un éditeur de texte
* un navigateur web
* un jeu vidéo

[wikipedia:application](https://fr.wikipedia.org/wiki/Application_(informatique))

## L'open source

L'open source est un principe qui impose lors de la fourniture d'un programme de fournir aussi un accès à son code source.

[wikipedia:open source](https://fr.wikipedia.org/wiki/Open_source)

## Système d'exploitation

C'est l'ensemble des couches logicielles faisant interface entre les utilisateurs ou les applications et les ressources matérielles.

[wikipedia:système d'exploitation](https://fr.wikipedia.org/wiki/Syst%C3%A8me_d%27exploitation)

## Le kernel

Le noyau de système d’exploitation est **LE** programme qui gère les ressources matérielle comme les disques, les écrans et le clavier mais surtout la mémoire et le temps processeur alloué aux autres programmes.

[wikipedia: noyau de système d'exploitation](https://fr.wikipedia.org/wiki/Noyau_de_syst%C3%A8me_d%27exploitation)

## Arborescence

C'est une méthode d'organisation hiérarchique de la donnée dans laquelle chaque élément :

* peut être le parent de plusieurs autres éléments;
* dispose au maximum d'un seul parent.
La **racine** est le point de départ de l'arborescence ; c'est aussi le seul élément qui n'a pas de parent.

[wikipedia:arborescence](https://fr.wikipedia.org/wiki/Arborescence)

## Bibliothèques partagées

Ce sont des morceaux de logiciel utilisables par plusieurs autres logiciels ; Une bibliothèque de fonction et d'algorythmes qui peuvent être appelés par d'autres programmes

Exemple:

* `zlib.so` est une bibliothèques de fonctions utilisant l'algorithme de compression décompression "deflate"
* `libacl.so` est une bibliothèque contenant des fonctions pour manipuler les droits POSIX (les droits `rwx` sur les fichiers principalement)

Exemple:

* `zlib.so` est une bibliothèques de fonctions utilisant l'algorithme de compression décompression "deflate"
* `libacl.so` est une bibliothèque contenant des fonctions pour manipuler les droits POSIX (les droits `rwx` sur les fichiers principalement)

## Compilation

Opération qui consiste en la construction de logiciel (exécutable) à partir du code source (lisible)

[wikipedia:compilation](https://fr.wikipedia.org/wiki/Compilateur)

## regex (expressions régulières)

Courament utilisée en programmation une expression régulière est une chaine de caratères définissant exactement tout un ensemble de chaines de caratères

Exemple : `\.jpg$` représente toute les chaines de caratères qui finissent pars ".jpg"

[wikipedia: regex](https://fr.wikipedia.org/wiki/regex)  
[regex101 : un site sympa sur les regex](https://regex101.com/)

## syscall : appel systeme

Un appels système consiste en la sollicitation du kernel pour accèder aux ressources, lorsqu'un programme ouvre un fichier il le fait via un appel système.

[wikipedia: apels système](https://fr.wikipedia.org/wiki/Appel_syst%C3%A8me)

## swap

La swap est espace de stockage de donnée physique utilisé pour stocker temporairement de la données 'chaude' initialement prévue pour être stocké en mémoire vive. L'utilisation de la swap permet au kernel du système d'exploitation de libérer temporairement de la memoire vive afin d'optimiser son usage.

[wikipedia: swap](https://fr.wikipedia.org/wiki/Espace_d%27%C3%A9change)

## NTP

Le "Network Time Protocole" est un protocole réseaux de synchronisation temporel. Il s'apuis sur une arboressance de serveur de temps et permet de garder votre système synchronisé avec ces serveurs

[wikipedia: ntp](https://fr.wikipedia.org/wiki/Network_Time_Protocol)

## Daemon

un daemon est un processus qui fonctionne en mode server. Il n'es pas rataché a une session utilisateur mais directement à `init` le premier processus lancé par le kernel. Il fourni un service système. Exemple : le processus sshd est un daemon.
