# Historique Linux/Unix

## Avant tout, il y a eu Alan Turing

Bon ok il y a eu d'autres trucs avant mais c'est plutôt anecdotique ([la pascaline](https://fr.wikipedia.org/wiki/Pascaline)).

Outre son excellent prénom, Alan est considéré comme le fondateur de la science "informatique", connu notamment pour avoir créé la machine capable de déchiffrer les messages de l'armée allemande pendant la seconde guerre mondiale.  
Il est le père de l'algorithmique grâce à son modèle de **machine de Turing** définissant le concept d'algorithme.

*A cette époque, la machine est la fonctionnalité.*

[wikipedia: machine de turing](https://fr.wikipedia.org/wiki/Machine_de_Turing)

## Les premiers systèmes d'exploitation

Dans les années 50, l'objectif est d'avoir une machine qui peut offrir plusieurs fonctionnalités. Le matériel est très très coûteux et peu performant (~1/2 Mhz). Le concept de système d'exploitation permet d'étendre les fonctionalités offertes par le matériel.
Les systèmes d'exploitation sont très liés aux machines qui les exécutent : [OS/360](https://fr.wikipedia.org/wiki/OS/360)(IBM), [GECOS](https://fr.wikipedia.org/wiki/General_Comprehensive_Operating_System) (GE), [Multics](https://fr.wikipedia.org/wiki/Multics)(MIT, bell, GE).

*On distingue la fonctionnalité (un programme) de la machine (le matériel et son système d'exloitation).*

## Il y a eu Apollo 11

Ok, ok, l'homme a marché sur la Lune. Mais avant ça, la femme [Margaret Hamilton](https://fr.wikipedia.org/wiki/Margaret_Hamilton_(scientifique)) a inventé le concept d'ingénierie logicielle et est une actrice très importante de son fondemenent. On lui doit beaucoup sur la qualité logicielle (principe de **test de bout en bout**), les concepts de programmation asynchrone et la gestion des priorités.

*On distingue alors précisément l'ingénierie de la partie matérielle (figée) de la partie logicielle (modifiable) d'une machine informatique.*

## Et enfin, Unix

**1969** : Ken Thompson développe Unics, c'est un projet voulant simplifier Multics, encore une fois codé en langage machine et donc lié au matériel qui l'exécute : un PDP-7.
Brian Kernighan (L'inventeur de awk) trouve le nom Unix à partir d'un jeux de mot (multics/unics).

**1971** : Dennis Ritchie Invente le **langage C** afin de pouvoir réécrire Unix et qu'il puisse être **porté sur plusieurs solution matériel.**
Brian Kernighan co-écrit le premier livre sur le C avec Denis Ritchie.

**1973** : Première publication sur Unix (Dennis R., Kenneth T.)
*“the success of UNIX is largely due to the fact that it was not designed to meet any predefined objectives.“*

**1975** : AT&T distribue UNIX V5 complet avec son code source dans les universités à des fins éducatives. Kenneth Thompson enseigne à l'UCB.

**1977** : Bill Joy (inventeur de vi et de Sun Microsystem) étudiant à l'UCB réalise la **première Berkeley Software Distribution (BSD)** à base de noyaud AT&T UNIX V6 qui est mis à disposition des entreprises (expansion des Unix).

.../...

**1983** : Première intégration de TCP/IP dans la version 4.2BSD.  
**1984** : Lancement du **[projet GNU](https://fr.wikipedia.org/wiki/GNU)** par monsieur Richard Matthew Stallman (RMS).  
**1985** : RMS crée la **[Free Software Foundation](https://fr.wikipedia.org/wiki/Free_Software_Foundation)**.  
**1989** : Première distribution BSD libre : Networking Release 1 et naissance de la **licence GPL** version 1.

### Copyleft et logiciel libre

Le [copyleft](https://fr.wikipedia.org/wiki/Copyleft)(all right reversed) est inventé sur la base d'un jeu de mot sur "copyright" (*all right reserved*).  
Le principe : il est permis de copier, d'utiliser, de modifier et redistribuer le produit sous licence copyleft et en aucun cas de restreindre ses droits. Ainsi tout produit issu d'un copyleft devient *de facto* lui-même accessible en copyleft.

**1991** : Linus Torvalds commence à écrire un noyau pour PC

> I'm doing a (free) operating system (just a hobby, won't be big and professional like gnu) for 386(486) AT clones
>
> -- *Linus Torvalds*

Au départ le projet était sans grande prétention (comme unics).

**1992** : version 0.12 du kernel est disponible sous licence GPL

**1996** : Citation de torvalds

> "Making Linux GPL'd was definitely the best thing I ever did."
>
> -- *Linus Torvalds*

## Pour conclure

* L'automatisation du traitement de l'information est une science jeune.
* Dès les début d'unix les ingénieurs et chercheurs collaborraient et partageaient leurs avancées dans une démarche scientifique (publications).
* Des projets qui étaient au départ sans prétention mais ambitieux (Unics, C, linux) ont révolutionné le monde de l'IT.
* La mise en oeuvre de standards est le ciment de la collaboration.
* Les logicels libres et l'open source sont des vecteurs très importants de l'évolution de l'IT.
* Les chercheurs en informatique font des jeux de mots.

[la suite : présentation linux](./presentation_Linux.md)
