# TD scripting

## Le jeux du plus ou moins

Faites le jeux du plus ou moins en bash.

La variable $RANDOM vous retourne un nouvel entier aléatoir entre 0 et 32765 à chaque fois qu'elle est consulté

Sauvegarder un nombre aléatoire entre 0 et 1000 puis proposez à l'utilisateur de le retrouver en faisant des propositions. à chacune des proposition votre scripte répondra si le nombre chercher est plus grand ou plus petit que celui proposer.

Une fois trouver votre scripte retournera à l'utilisateur le nombre d'itérations effectué avant d'avoir retrouver la valeur cherchée.

Votre script gérera les cas inatendu :

* pas de valeur saisie
* valeur non souhaitée (une chaîne plutot qu'un numérique)
* etc...

## Un outils de sauvegarde

Faite un script de sauvegarde (commande tar) d'une sous arborescence passé en argument en effectuant une sauvegarde "full" le vendredi et des sauvegardes incrémentals les autres jours (vous n'utiliserez pas l'option --update de la commande tar mais plutot un commande find)

## youtube-dl

Installez l'outil en ligne de commande youtube-dl. [doc ubuntu](https://doc.ubuntu-fr.org/youtube-dl)

Faites un script qui prend en argument une url youtube et qui récupère la musique lié à la vidéo dont l'url est passée en argument.
