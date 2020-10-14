# TD utilisateurs

## comptes et droits sudo

* Créez un compte utilisateur morty

* Definissez lui un mot de passe

* Donnez lui le droit sudo de passer la commande `id` uniquement en tant que root

* prenez son identité avec la commande `su - morty`

* validez qu'il peu utiliser la commande id en tant que root et pas d'autres commandes

## clone de root

* Editez le fichier /etc/passwd pour remplacer l'id de l'utilisateur anssi créé par 0

* Depuis votre compte prenez son identité avec la commande `su - morty` et en saisissant sont mot de passe

* Tappez la commande id : que constatez vous ?

Répondez à la fameuse question philosophique : **"Mais pourquoi ?"**
Vous n'avez pas deux heures.
