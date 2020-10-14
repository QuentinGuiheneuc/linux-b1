# troubleshooting

C'est notre activité principale : résoudre les incidents et problemes.

Rappels : résoudre un incident consiste à remettre le service en fonction, traiter le probleme consiste à cherche la cause de l'incident et de proposer des solution à long terme.

## Anomalie courante et connues

- Service arreté
- Saturation de ressources : réseaux disque cpu ram
- regression suite à un changement

## Principe de traitement d'une anomalie inconnue

### Qualification

Il s'agit de definir précisément quel est l'anomalie rencontré, __vérifier qu'il s'agit bien d'une anomalie__ 'le service à-t-il déja été fonctionnel, puis lorsque cela est possible de trouver une méthode de reproduction de l'anomalie.

### Analyse

Collecte d'informations et des traces en suivant le protocole de reproduction de l'anomalie ou l'horodatage de l'anomalie déclarée.

En reprenant les principes de fonctionnement du service impacté et validant point par point les éléments rencontrés. Pour le réseaux : les couches du modèle OSI.

### Correction

La correction repect les principe de la gestion des changement en production, dans l'objectif de maitriser l'impact sur la production.

après avoir valider la non regression dans un environment hors production, vous vous preparrer à exécuter l'opération en production.

Celle-ci sera planifier en concertation avec la maitrise d'ouvrage.
