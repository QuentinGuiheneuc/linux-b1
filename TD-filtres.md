# TD Les commandes filtres

## Exemple de combinaison de filtres

Le fichier /etc/fstab défini les filesystèmes **connue**, les disque et partition formaté si vous préférez.

```bash
~$ grep -v "^#" /etc/fstab | tr -s " \t" ";" | cut -d\; -f3
```

Ici, `grep` supprime les ligne 'commentaires', `tr -s` remplace les répétition d'espace ou de tabulation par un seul `;`, `cut` récupère le 3ieme champ ainsi défini.

Pour les motivés, `awk` fait ça d'un seul coup, `sed` devrais le faire aussi mais je suis plus awk que sed. Savoir utiliser ces commandes font partie de la maitrise du shell.

```bash
~$ awk '$0!~/#/ {print $3}' /etc/fstab
```

## TP challenges

### le premier

Le fichier /etc/passwd défini les utilisateurs, le 4ieme champ contien l'id de groupe le gid.

Trouvez, en une seule ligne de commande utilisant des filtres combinés, quel est le gid le plus utilisé.

### Le deuz'

* Le fichier /etc/group defini les groupes connu du système
* dans une ligne de commande on peu substitué une partie de la ligne de commande par le résultat d'une ligne commande $(commande)
Exemple:

```bash
echo mon uid est $(grep alan /etc/passwd | cut -d: -f3 | head -1)
```

En utilisant la réponse à la première question à l'intérieur d'une pre-execution : $( commande ), affichez en une seule commande le nom du groupe associé au gid le plus utilisé.
