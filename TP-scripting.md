# Evaluation scripting

Suivez précisément les consignes, c'est important.

Vous devez realiser un script 'passeplats' et le livrer dans un dépot git. Vous créez donc un dépot git dédié à cette évaluation.

A chaque étape vous livrez un script fonctionnel (vous devez donc le tester).

En debut d'évaluation, vous me faites un message discord :
prenom.nom@ynov.com:http://url-du-dépot-gt

## Spécifications

A chacune des ses exécutions, le script doit récupèrer l'ensemble des fichiers qu'il trouve dans le dossier /tmp/in, les compresser avec gzip et les déposer dans le dossier /tmp/out

Simple! faites simple car ce n'est pas fini.

Vous fixer cette version dans le dépot git:

```bash
git add lescript; git commit -m version1 ; git push
```

Ce script refusera de s'exécuter avec le code retour 22 si une autre exécution est déjà en cours. On peu utiliser le fait de déposer un fichier 'lock' dans le dossier /tmp/out, s'il existe, c'est qu'une exécution est déja en cours, donc stop avec le code retour 22.

```bash
git add lescript; git commit -m version2 ; git push
```

Une fois réalisé vous ajouterez une gestion d'erreur :

Le script dois founir un code retour non nul en cas de d'impossibilité de réaliser l'opération pour une raison ou pour une autre sur l'un des fichier ou sur sur tous. Il traitera tout ce qu'il peu traiter.

```bash
git add lescript; git commit -m version3 ; git push
```

Par ailleurs le script alimentera un fichier de log dans le dossier out, il listera tout les fichier traité ou non.

```bash
git add lescript; git commit -m version4 ; git push
```

Voilà, vous envoyez le message
