# TP Variables et environnement

## Variables

* Créez une variable `rototo` et assignez lui la valeur `"Blurps"`
* Affichez la valeur de la variable `rototo`
* Affichez les valeurs courantes associées aux variables :
  * LOGNAME
  * COLUMNS
  * LINES
* Redimentionez votre fenêtre terminal et réaffichez les valeurs de COLUMNS et LINES

## Export

Vous allez ajouter à votre installation ubuntu la langue française et les pages du man en français.

```bash
prompt$ sudo apt-get install language-pack-fr manpages-fr
```

Consultez le `man`, il est toujours en anglais.

La langue française et le `man` en français sont bien installés mais pas utilisés.

Dans votre terminal, exportez la variable `LANG` avec la valeur `"fr_FR.UTF-8"`

```bash
prompt$ export LANG=fr_FR.UTF-8
```

Consultez le man de nouveau. La variable `LANG` exportée est maintenant utilisée par la commande `man` afin de chercher la page de manuel en français.

## `PATH`

Lancez un sous shell :

```bash
prompt$ bash
```

Votre shell à donc lancé un second shell `bash` ; vous êtes maintenant dans une nouvelle session shell.

* Affichez votre `PATH` :

```bash
prompt$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

* Videz la variable `PATH`

```bash
prompt$ PATH=""
```

* Essayez une commande puis quittez le second shell :

```bash
prompt$ ls fichier.txt
prompt$ /bin/ls fichier.txt
fichier.txt
bash: ls: No such file or directory
prompt$ exit
```

Le shell ne retrouve plus la commande `/bin/ls` si on ne précise pas son chemin. Après avoir quitté le second shell votre variable `PATH` est retrouvée.

## Alias

* listez les alias définis
* lesquels sont remarquables selon vous ?

## Configuration

Mettez en place ce qu'il faut pour avoir un pompte et une complétions adapté à une utilisation intensive de git.
