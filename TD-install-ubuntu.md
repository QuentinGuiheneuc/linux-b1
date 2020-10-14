# Installation Ubuntu desktop

## Environnement virtual box

Une installation doit se faire sur une machine. Oui c'est évident.
Nous allons utiliser une machine virtuelle et donc l'outil VirtualBox.

### Configuration Virtual Box

Il faudra configurer virtualBox pour avoir une interface host-only disponible :

<details><summary>Détails</summary>

Allez dans les globals tools / Network :
![net](./images/vb-1.png)

Ajoutez et/ou editez un réseaux hostonly :
![hostonly](./images/vb-2.png)

Précisez bien les spécifications de votre réseau virtuel en activant le DHCP et en précisant la plage d'adresses DHCP à partir de xx.xx.xx.101 (et non pas xx.xx.xx.1) : 
![dhcp](./images/vb-3.png)

</details>

### Configuration de la VM

Configurez une VM ubuntu avec :

* Mémoire : 2048MB (min)
* CPU : 1 coeur
* un disque : 10GB
* 2 cartes réseau
  * NAT (standard)
  * Host only
* pas de lecteur de disquette

Suivez bien les étapes définies ci dessous.

<details><summary>étapes</summary>

* Dans les machine tools cliquez sur new en haut à gauche
![new](./images/vb-4.png)

* Définissez le nom de votre VM son type et la version Ubuntu
![definition](./images/vb-5.png)

* Précisez la quantité de mémoire
![mem](./images/vb-6.png)

* Créez un disque virtuel
![disk](./images/vb-7.png)

* De type **V**irtualBox **D**isk **I**mage
![disk-type](./images/vb-8.png)

* En stockage dynamique (le fichier disque qui contient les données écrites sur le disque s'agrandit au fur et à mesure)
![](./images/vb-9.png)

* De 10GB
![10GB](./images/vb-10.png)

* On édite la VM
![edit](./images/vb-11.png)

* On désactive le lecteur de disquette ça existe plus ça !
![floppy](./images/vb-12.png)

* Un core
![core](./images/vb-13.png)

* On s'assure que le vt est actif (le nested paging peu poser des problèmes si votre laptop le gère mal).
![vtx](./images/vb-14.png)

* Activez la seconde carte réseau en host-only
![nethostonly](./images/vb-15.png)

* Activez le buffer copier-coller bidirectionnel
![clipboard](./images/vb-16.png)

* insèrer un cd-rom virtuel (une iso)
![iso](./images/vb-17.png)

* Sélectionez l'image `iso` d'installation
![iso-bis](./images/vb-18.png)

</details>

### Installation ubuntu

Effectuez une installation standard :

* On va faire du "Next Next Next" : Cela veux dire lire le texte et choisir les options standards et évidentes
* Attention à comprendre ce que vous faites à chaque étape.
En particulier :
  * Ne pas télécharger les updates pendant l'install (vous le ferez plus tard)
  * Installer les logiciels tier (cela ne coûte pas grand chose)
  * Utiliser des noms explicites : (votre prénom pour le compte c'est simple et facile)
  * **Notez bien de votre mot de passe**

#### Suivez bien les détails ci-dessous

<details><summary>détails</summary>

* Démmarez la VM.
![start](./images/vb-19.png)

* Notez bien les messages VirtualBox lisez les, comprenez les, puis cachez les définitivement.
![hide-messages](./images/vb-20.png)

* Une installation en anglais bien sûr !
![english](./images/vb-21.png)

* Mais avec le clavier qui **VOUS** convient.
![kbd](./images/vb-22.png)

* installation Minimale et avec les logiciels tiers et **SANS** installer les mises à jour (vous le ferez plus tard).
![min install](./images/vb-23.png)

* On écrasera le disque (c'est un disque virtuel).
![partitionning](./images/vb-24.png)

* Oui on confirme.
![goon](./images/vb-25.png)

* On est en France.
![location](./images/vb-26.png)

* Un compte, un nom de host un mot de passe **A RETENIR**.
![loginpass](./images/vb-27.png)

* On attends un peu.
![wait](./images/vb-28.png)

* Puis on reboot.
![reboot](./images/vb-29.png)

</details>

### Utilisation

Connectez-vous !

![firstpage](./images/vb-30.png)

Une petite présentation vous acceuille, suivez là, prenez le temps.

## Conclusion

Bah, rien de complexe.  
En bas à gauche les applications, vous rechercherez "term" et vous lancerez aussi un terminal.

Nous allons parler du [shell](./shell.md).
