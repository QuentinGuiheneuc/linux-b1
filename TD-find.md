# TD Find

## exercices

Avec la commande find , retrouvez :

* Tous les fichiers de votre home modifiés depuis moins de 30 minutes.

* Tous les fichiers n'apartenant pas à root du dossier /tmp modifié il y a moins de 30 minutes.

* Tous les fichiers de /var de plus de 30MB.

* Tous les fichier de plus de 30MB accèdé depuis moins de 5 minutes.

Pour chacun des exercices précédent vérifiez le résultats. Quelle méthode utilisez vous ?

## Attention challenge

* Retrouvez tout sous répertoir de /var et contenant plus de 50Mo de données

* `du` : disk utility, avec les argument `-sm` Permet de faire une estimation en MB de la taille des fichiers présent dans une sous arborescence

Si find n'est pas suffisant on pourra utiliser le `|` et `xargs`
`xargs` : Passe la commande définies dans ses arguments en lui transmettant toutes les lignes reçu sur l'entrée standard en arguments
Exemple:

```bash
$ seq 1 3 | xargs echo
1 2 3
```

La commande `seq` affiche une séquence de 1 à 3 affiché sur trois lignes, xargs les translate en trois arguments passé à la commande echo echo
