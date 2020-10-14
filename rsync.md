# rsync

la commande `rsync` est un outils de synchronisation d'arborescances il permet de remettre en synchro une arborescance (la destination) par raport à une autre (la source) en local ou avec la source ou(exclusif) la destination distante. la synchronisation distante peu s'appuyer sur le protocole ssh ou sur le protocole `rsync`

rsync est donc une commande et un protocole de communication réseaux.

## Utilisation

> $ rsync Options Les-Sources La-Destination

Les-Sources : Une liste de fichiers ou d'aborescances locales ou distantes. Dans le cas d'une source distante la destination sera focément locale.
La-Destination : Un dossier local dans lequel on vien synchronier les arbo sources ou un dossier distant (Seulement si toutes les sources sont locales).

dans la cas d'un objet distant, il peu être :

* "Un chemin ssh" : user@host:/le/dossier
* "Un chemin rsync" : user@hosts::canal/le/dossier (canal est une nom de flux de synchro sur le protocole rsync)

L'aide de la commande (option -h) est plutot explicite et presque exhaustif :  

```bash
prompt$ rsync -h
rsync  version 3.1.1  protocol version 31
Copyright (C) 1996-2014 by Andrew Tridgell, Wayne Davison, and others.
Web site: http://rsync.samba.org/
Capabilities:
    64-bit files, 64-bit inums, 64-bit timestamps, 64-bit long ints,
    socketpairs, hardlinks, symlinks, IPv6, batchfiles, inplace,
    append, ACLs, xattrs, iconv, symtimes, prealloc

rsync comes with ABSOLUTELY NO WARRANTY.  This is free software, and you
are welcome to redistribute it under certain conditions.  See the GNU
General Public Licence for details.

rsync is a file transfer program capable of efficient remote update
via a fast differencing algorithm.

Usage: rsync [OPTION]... SRC [SRC]... DEST
  or   rsync [OPTION]... SRC [SRC]... [USER@]HOST:DEST
  or   rsync [OPTION]... SRC [SRC]... [USER@]HOST::DEST
  or   rsync [OPTION]... SRC [SRC]... rsync://[USER@]HOST[:PORT]/DEST
  or   rsync [OPTION]... [USER@]HOST:SRC [DEST]
  or   rsync [OPTION]... [USER@]HOST::SRC [DEST]
  or   rsync [OPTION]... rsync://[USER@]HOST[:PORT]/SRC [DEST]
The ':' usages connect via remote shell, while '::' & 'rsync://' usages connect
to an rsync daemon, and require SRC or DEST to start with a module name.

Options
 -v, --verbose               increase verbosity
     --info=FLAGS            fine-grained informational verbosity
     --debug=FLAGS           fine-grained debug verbosity
     --msgs2stderr           special output handling for debugging
 -q, --quiet                 suppress non-error messages
     --no-motd               suppress daemon-mode MOTD (see manpage caveat)
 -c, --checksum              skip based on checksum, not mod-time & size
 -a, --archive               archive mode; equals -rlptgoD (no -H,-A,-X)

.../...
```

Exemple d'usage :

```bash
prompt$ sudo rsync -av /var/log/ /tmp/logcopy
sending incremental file list
created directory /tmp/logcopy
./
Xorg.0.log
.../...
```

**Attention à la syntaxe sur le source** `/var/log` et `/var/log/` définissent deux choses différentes `/var/log` signifie le dossier log et tout ce qu'il contiens `/var/log/` ne désigne que ce qu'il contiens.

Ici nous synchronisons tout le contenue du dossier /var/log/ et nous le plaçon dans le dossier /tmp/logcopy.

Si je ré-exécute la même commande un peu plus tard :

```bash
prompt$ sudo rsync -av /var/log/ /tmp/logcopy
sending incremental file list
auth.log
kern.log
syslog

sent 19,858 bytes  received 85 bytes  39,886.00 bytes/sec
total size is 6,465,409  speedup is 324.19
```

Seul les fichiers qui ont été modifiés depuis la dernière synchro sont resynchronisés. Par défaut rsync utilise les dates de changements de fichier et controle la taille de ceux-ci pour déterminer si celui-ci doit être resynchroniser ou pas, on peu moyenant une consomation cpu plus importante valider l'égalité des fichier grace au calcul d'une somme de controle sur ceux-ci option `--checksum`.

## Server rsync

`rsync` peu être lancé comme un [Daemon](./definitions.md#Daemon). Pour ce faire on le lance avec l'option `--daemon`.

```bash
prompt$ rsync --daemon --help
rsync  version 3.1.1  protocol version 31
Copyright (C) 1996-2014 by Andrew Tridgell, Wayne Davison, and others.
Web site: http://rsync.samba.org/
Capabilities:
    64-bit files, 64-bit inums, 64-bit timestamps, 64-bit long ints,
    socketpairs, hardlinks, symlinks, IPv6, batchfiles, inplace,
    append, ACLs, xattrs, iconv, symtimes, prealloc

rsync comes with ABSOLUTELY NO WARRANTY.  This is free software, and you
are welcome to redistribute it under certain conditions.  See the GNU
General Public Licence for details.

Usage: rsync --daemon [OPTION]...
     --address=ADDRESS       bind to the specified address
     --bwlimit=RATE          limit socket I/O bandwidth
     --config=FILE           specify alternate rsyncd.conf file
 -M, --dparam=OVERRIDE       override global daemon config parameter
     --no-detach             do not detach from the parent
     --port=PORT             listen on alternate port number
     --log-file=FILE         override the "log file" setting
     --log-file-format=FMT   override the "log format" setting
     --sockopts=OPTIONS      specify custom TCP options
 -v, --verbose               increase verbosity
 -4, --ipv4                  prefer IPv4
 -6, --ipv6                  prefer IPv6
     --help                  show this help screen

If you were not trying to invoke rsync as a daemon, avoid using any of the
daemon-specific rsync options.  See also the rsyncd.conf(5) man page.
```

Nous utiliseront en général un fichier de configuration du daemon `/etc/rsyncd.conf` pour préciser les options et les canaux de synchronisation

[TD de mise en oeuvre d'un serveur de sauvegarde rsync](./TD-rsync-daemon.md)
