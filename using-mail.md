# Les mails sous linux

## Présentation

Le mail à été inventé dans les annèes 60 permettant de se laisser des messages d'un compte unix vers un autre sur le même hosts.

Dans les annèes 70 le premier mail inter hosts sur l'arpanet est emis 2 années plus tard les mails représente 75% du traffique réseaux.

Chaque compte utilisateur unix peu donc disposer d'une boite mail locale.

## Fonctionement

L'adresse mail définie un compte de messagerie sur un domaine `user@domaine` l'envois d'un mail est fait par un serveur smtp (Simple Mail Transfert Protocol) qui interroge le service [DNS](./using-dns.md) pour connaitre les serveurs SMTP du domaine (enregistrement MX sur le domaine) et lui transmet le mail. ce dernier dépose alors le mail dans la boite mail associée.

Lorsqu'un mail est envoyé a un compte sans domaine il est alors à destination du compte unix défini sur le host local.

## Utilisation

La commande mailx (package mailx sous CentOS) permet à la fois de consulter la boite mail de votre compte et d'envoyer un mail.

Exemple d'envois :

```bash
echo coucou | mail -s sujetdumail alan.simon@free.fr
```

Pour la consultation, la commande mail sans argument ouvre une session interactive `help` vous fournira l'aide c'est plutot simpl;e à utiliser.

## Configuration SMTP

Le serveur smtp peu être configurer pour :

* être un serveur d'envois de mail : il contact directement le serveur de reception du domaine et lui transmet le mail. (c'est la configuration par défaut sous centOS)
* Utiliser un autre serveur d'envois pour tous les mails qu'il doit envoyer (il utilise alors un serveur de relayage : un relay host)
* être un serveur de relayage
* être à l'écoute de connexion pour recevoir les mails pour un domaine.

Pour un simple système il conviens de configurer la première ou la deusiemme option avec eventuelement du marsquerading : on change le domaine de l'émèteur pour omettre le nom du host dans le domaine
exemple : root@lab.local plutot que root@host.lab.local

## les serveurs SMTP

* Sendmail est le serveur historique sous unix  
* Postfix est une référence professionel mais il n'est pas sous licence GNU.  
* Exim est le serveur SMTP libre.  

Sous CentOS c'est postfix sous Debian c'est exim.

Dans ce [TD](./TD-mail-system.md) vous configurer postfix sur un centos.
