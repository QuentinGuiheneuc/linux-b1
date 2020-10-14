# Mail système

a partir d'une nouvelle VM CentOS

## Installez et utilisez la commande mail

### install

```bash
$ yum install mailx
.../...
```

### Utilisation

Envoyer un mail a root avec la commande mail :

```bash
[root@localhost ~]$ echo test | mail -s test root@localhost
[root@localhost ~]$ mail
Heirloom Mail version 12.4 7/29/08.  Type ? for help.
"/var/spool/mail/root": 1 message 1 new
>N  1 root                  Sun Feb 28 18:51  18/608   "test"
& [touche entrer]
From root@localhost.localdomain  Sun Feb 28 18:51:43 2016
Return-Path: <root@localhost.localdomain>
X-Original-To: root@localhost
Delivered-To: root@localhost.localdomain
Date: Sun, 28 Feb 2016 18:51:43 +0100
To: root@localhost.localdomain
Subject: test
User-Agent: Heirloom mailx 12.4 7/29/08
Content-Type: text/plain; charset=us-ascii
From: root@localhost.localdomain (root)
Status: R

test

& q
Held 1 message in /var/spool/mail/root
[root@localhost ~]$
```

### Exercices

Lisez la man si vous avez des questions, utilizez “quit” ou “q” pour quitter l'outil mail

* Avec la commande mail envoyez vous un mail depuis le host. (sur une de vos adresses mail perso)
* Avec la commande mail envoyez un mail à root avec comme émeteur votre compte de messagerie personnel (ou comment définir le from)
* Ouvrez la boite mail de root (commande mail), visualiser les mails (h), positionez vous sur le mail envoyé depuis votre mail perso (3 si ce mail est en ligne 3) répondez à ce mail (respond et terminez le texte par un retour chariot, un point puis un autre retour chariot)
* Transferez un mail de votre boite mail vers votre compte de messagerie
* Supprimez tout les mails de la mailbox de root

## Configurez Postfix

### Instanciation du host

Renommez le host en mail.lab.local (commande `hostnamectl set-hostname mail.lab.local` et ajout du nom dans `/etc/hosts`)
Spécifiez l'ip hostonly pour le nom mail.lab.local  

### Configurez postfix pour servir le host

Vous copiez le fichier /etc/postfix/main.cf vers /etc/postfix/main.cf.orig puis vous l'éditez afin d'obtenir les différence suivantes :

```bash
[root@mail ~]$ diff /etc/postfix/main.cf /etc/postfix/main.cf.orig
77d76
< myhostname = mail.lab.local
84c83
< mydomain = lab.local
---
> #mydomain = domain.tld
117c116
< inet_interfaces = localhost,$myhostname
---
> inet_interfaces = localhost
[root@mail ~]$ service postfix restart
Shutting down postfix:                                     [  OK  ]
Starting postfix:                                          [  OK  ]
```

### Le relayage  

#### par ip source

Dans la configuration on accepte le relayage pour certaines ip sources :

```bash
[root@mail ~]$ diff /etc/postfix/main.cf /etc/postfix/main.cf.orig
77d76
< myhostname = mail.lab.local
84c83
< mydomain = lab.local
---
> #mydomain = domain.tld
117c116
< inet_interfaces = localhost,$myhostname
---
> inet_interfaces = localhost
266c265
< mynetworks = $config_directory/mynetworks
---
> #mynetworks = $config_directory/mynetworks
[root@mail ~]$ cat /etc/postfix/mynetworks
192.168.56.0/24
```

Ici le serveur deviens un `mail relay` pour les serveurs du réseaux 192.168.56.0/24 .

#### par destination

En activant l'option relay_domains, on accepte les mails entrants uniquement pour certain domaine de destinations :  

```bash
[root@mail ~]$ diff /etc/postfix/main.cf /etc/postfix/main.cf.orig
…/…
297c296
< relay_domains = $mydestination
---
> #relay_domains = $mydestination
```

#### Testez le relayage

directement avec la commande telnet on applique manuelement les commandes du protocole SMTP : HELO, MAIL FROM:, RCPT TO:

```bash
[root@mail ~]$ telnet localhost 25
Trying ::1...
Connected to localhost.
Escape character is '^]'.
220 mail.lab.local ESMTP Postfix
HELO local
250 mail.lab.local
MAIL FROM:root@localhost
503 5.5.1 Error: nested MAIL command
RCPT TO:alan.simon@free.fr
554 5.7.1 <alan.simon@free.fr>: Relay access denied
quit
221 2.0.0 Bye
Connection closed by foreign host.
```

le serveur refuse maintenant le relayage de mails qui ne sont pas pour le domaine lab.local

### le relayhost

Il est possible de définir un serveur de relay SMTP à utiliser dans tout les cas

```bash
[root@mail ~]$ grep “^relayhost =” /etc/postfix/main.cf
relayhost = [192.168.56.60]
```

Tout les mails seront alors transmis pour relay au serveur SMTP 192.168.56.60 (ne pas le mettre en place).

### le masquerading

Cela consiste en la réduction du nom domaine de messagerie pour ne concerver que ses parties les plus à droite :

```bash
[root@mail ~]$ diff /etc/postfix/main.cf /etc/postfix/main.cf.orig 
…/...
356d354
< masquerade_domains = $mydomain
```

Ceci réécrit tout les domaines des mail envoyé any.thing.lab.local en lab.local on peu placer plusieurs entrées à la suite la première qui correspond sera alors appliquée

un simple test : (utilisez votre adresse)

```bash
[root@mail ~]$ echo test | mail -s test alan.simon@free.fr
```

et regardez quel est l'adresse émetrice.
