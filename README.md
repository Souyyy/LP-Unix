# Compte Rendu TP01

## 1 - Installation Machine virtuelle.

L'installation de la VM avec le disque ISO a rencontré de nombreux problèmes, notamment des plantages récurrents lors du démontage du disque ISO. Après plusieurs tentatives infructueuses, c'est finalement à la cinquième installation que tout a fonctionné correctement. Le problème provenait du GRUB, qui bloquait le démarrage de l'installation. En sélectionnant les critères appropriés pour l'installation de GRUB, le processus s'est déroulé sans encombre.

## 2- Post-Installation.
### SSH
Pour ce faire je me suis connecter à ma VM avec l'utilisateur root, j'ai ensuite executer 
Utilisation des commandes suivantes pour rechercher et installer le paquet SSH, `apt search ssh` `apt install ssh`

-------------

### Nombre de paquets

En executant la commande `dpkg -l | wc -l` on obtient `354 packets`. Cette différence dans le nombre de paquets s'explique par le fait que le TP est assez ancien et que Debian a effectué plusieurs mises à jour.

-------------

### Space Usage

```
root@serveur1:~# df -h
Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
udev               965M       0  965M   0% /dev
tmpfs              197M    564K  197M   1% /run
/dev/sda1          9,1G    1,2G  7,4G  14% /
tmpfs              984M       0  984M   0% /dev/shm
tmpfs              5,0M       0  5,0M   0% /run/lock
/dev/sda3          921M     30M  828M   4% /var/log
/dev/sda2          3,6G     40K  3,4G   1% /tmp
tmpfs              197M       0  197M   0% /run/user/0
```

-------------

### Locales

La commande `echo $LANG` nous renvoie ceci:
```
fr_FR.UTF-8
```

et la commande `env` nous renvoie ceci:
```
root@serveur1:~# env
SHELL=/bin/bash
PWD=/root
LOGNAME=root
XDGSESSION_TYPE=tty
MOTD_SHOWN=pam
HOME=/root
LANG=fr_FR.UTF-8
SSH_CONNECTION=10.0.2.2 54636 10.0.2.15 22
XDG_SESSION_CLASS=user
TERM=xterm-256color
USER=root
SHLVL=1
XDG_SESSION_ID=3
XDG_RUNTIME_DIR=/run/user/0
SSH_CLIENT=10.0.2.2 54636 22
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/0/bus
SSH_TTY=/dev/pts/0
=/usr/bin/env
```

**Explication**

la commande `echo $LANG` affiche la valeur de la variable d'environnement `$LANG`. Cette variable détermine les paramètres linguistiques et régionaux du système. la commande `env` sans arguments permet d'afficher toutes les variables d'environnement actives sur le système.

-------------

### Nom de la machine

La commande `hostname` nous renvoie ceci:

```
root@serveur1:~# hostname
serveur1
```
**Explication**

Cette commande permet d'afficher ou de définir le nom d'hôte (`hostname`) du système. Le nom d'hôte est utilisé pour identifier la machine sur un réseau. Dans cet exemple, la commande retourne `serveur1`, qui est le nom attribué à notre VM.

-------------

### Domaine

La commande `hostname -d` nous renvoie ceci:

```
root@serveur1:~# hostname -d
...
```

**Explication**

D'après le manuel `man`, l'option `-d` de la commande `hostname` permet d'afficher le nom de domaine associé à l'hôte. Dans notre cas, le résultat est vide, car la VM est hébergée sur mon ordinateur portable, et lors de la création de la VM, il n'était pas obligatoire de saisir un nom de domaine.

-------------

### Vérification emplacement depots

La commande `cat /etc/apt/sources.list | grep -v -E '^#|^$'` nous renvoie ceci:

```
root@serveur1:~# cat /etc/apt/sources.list | grep -v -E '^#|^$'
deb http://deb.debian.org/debian/ bookworm main
deb http://security.debian.org/debian-security bookworm-security main
deb http://deb.debian.org/debian/ bookworm-updates main
```

**Explication**

D'après le manuel `man`, la commande `cat` permet de concatener des fichiers et les afficher sur la sortie standard et la commande `grep` permet d'afficher les lignes correspondant à un motif donnée. 

Dans notre cas, la commande permet d'afficher le contenu du fichier `sources.list` en filtrant certaines lignes grâce aux options `-v -E '^#|^$'`. Ces options permettent d'exclure les lignes qui commencent par un `#` et d'exclure les lignes vides.

-------------

### passwd/shadow

La commande `cat /etc/shadow | grep -vE ':\*:|:!\*:'` nous renvoie ceci:

```
root@serveur1:~# cat /etc/shadow | grep -vE ':*:|:!*:'
root:$y$j9T$Jl4/OujF5cROHZWgCQI21.$jf5hJyGuacvYsJsQgUL5UX015p2go7xOTUlWpaUK6EC:19998:0:99999:7:::
messagebus:!:19998::::::
avahi-autoipd:!:19998::::::
sshd:!:19998::::::
```

**Explication**

D'après le manuel `man`, la commande `cat` permet de concatener des fichiers et les afficher sur la sortie standard et la commande `grep` permet d'afficher les lignes correspondant à un motif donnée. 

Dans notre cas, la commande affiche le contenu du fichier `/etc/shadow` en filtrant certaines lignes à l'aide des options `grep -vE ':*:|:!*:'`, où `:*:` désigne les lignes sans mot de passe et `:!:` indique les comptes désactivés ; ainsi, `grep -vE` supprime les lignes correspondantes, ce qui permet de montrer uniquement les comptes ayant un mot de passe actif, comme celui de root, ou des comptes système

-------------

### Compte utilisateurs

La commande `cat /etc/passwd | grep -vE 'nologin|sync'`
Nous renvoie ceci:
``` 
root@serveur1:~# cat /etc/passwd | grep -vE 'nologin|sync'
root:x:0:0:root:/root:/bin/bash
```
**Explication**

La commande `cat /etc/passwd` affiche le contenu du fichier `/etc/passwd`, qui contient des informations sur tous les utilisateurs du système, tandis que le symbole `|` redirige la sortie de `cat` comme entrée de `grep`, un outil utilisé pour rechercher des lignes correspondant à un motif spécifique ; l'option `-v` inverse la recherche pour exclure les lignes qui correspondent au motif, et `-E` permet d'utiliser des expressions régulières étendues pour spécifier plusieurs motifs (ici, nologin ou sync).

La ligne retournée contient les informations suivantes pour l'utilisateur root :
**Nom d'utilisateur :** `root`
**Mot de passe :** `x` (signifie que le mot de passe est dans `/etc/shadow`)
**UID/GID :** `0` (root a les identifiants les plus élevés)
**Commentaire :** `root` (description de l'utilisateur)
**Répertoire personnel :** `/root`
**Shell :** `/bin/bash` (le shell utilisé par défaut)
Cela montre que seul l'utilisateur root a un accès complet avec un shell.

-------------

### La commande fdisk

La commande `fdisk -l` et `fdisk -x`
Nous renvoie respectivement ceci:

`fdisk -l`
```
root@serveur1:~# fdisk -l
Disque /dev/sda : 20 GiB, 21474836480 octets, 41943040 secteurs
Modèle de disque : VBOX HARDDISK   
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : gpt
Identifiant de disque : DF4F006B-0847-40CD-9744-2A0EC0357024

Périphérique    Début      Fin Secteurs Taille Type
/dev/sda1        2048 19531775 19529728   9,3G Système de fichiers Linux
/dev/sda2    19531776 27344895  7813120   3,7G Système de fichiers Linux
/dev/sda3    27344896 29298687  1953792   954M Système de fichiers Linux
/dev/sda4    29298688 41940991 12642304     6G Partition d'échange Linux
```

`fdisk -x`

```
root@serveur1:~# fdisk -x
Disque /dev/sda : 20 GiB, 21474836480 octets, 41943040 secteurs
Modèle de disque : VBOX HARDDISK   
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : gpt
Identifiant de disque: DF4F006B-0847-40CD-9744-2A0EC0357024
Premier LBA utilisable: 34
Dernier LBA utilisable: 41943006
LBA alternatif: 41943039
LBA de départ des entrées de partition: 2
Entrées de partitions allouées: 128
LBA de fin des entrées de partition: 33

Périphérique    Début      Fin Secteurs Type-UUID                            UUID                                 Nom          Attr.
/dev/sda1        2048 19531775 19529728 0FC63DAF-8483-4772-8E79-3D69D8477DE4 FC5BE491-38E7-43A6-963C-A7945B2D8754 la racine    
/dev/sda2    19531776 27344895  7813120 0FC63DAF-8483-4772-8E79-3D69D8477DE4 E85E36D6-4526-496C-891A-040ED2DB1705 espace tempo 
/dev/sda3    27344896 29298687  1953792 0FC63DAF-8483-4772-8E79-3D69D8477DE4 5D43002F-D70E-4CB8-82F7-64B654A86750 les logs     
/dev/sda4    29298688 41940991 12642304 0657FD6D-A4AB-43C4-84E5-0933C84B4F4F 690522A4-DD91-473D-8193-37C599585FE0 ma swap      
```

**Explication**

Selon la commande `man`, `fdisk` est un utilitaire utilisé pour manipuler les tables de partitions d'un disque dur. Le paramètre `-l` permet de lister toutes les partitions et les tables de partitions des disques disponibles sur le système. De plus, le paramètre `-x` permet d'afficher des informations détaillées supplémentaires sur les partitions, comme les cylindres, les têtes et les secteurs (ce paramètre peut ne pas être disponible sur toutes les versions de fdisk).

-------------

### La commande : df -h

Cette commande nous renvoie ceci:

```
root@serveur1:~# df -h
Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
udev               965M       0  965M   0% /dev
tmpfs              197M    564K  197M   1% /run
/dev/sda1          9,1G    1,2G  7,4G  14% /
tmpfs              984M       0  984M   0% /dev/shm
tmpfs              5,0M       0  5,0M   0% /run/lock
/dev/sda3          921M     30M  828M   4% /var/log
/dev/sda2          3,6G     40K  3,4G   1% /tmp
tmpfs              197M       0  197M   0% /run/user/0
```

**Explication**

D'après la commande `man`, la commande `df` permet d'indiquer l'espace occupé par les systèmes de fichiers, le paramètre `-h` _(`--human-readable`)_ permet d'afficher les tailles en puissances de 1024. De façon à ce que la taille des fichiers soit facile à comprendre. 


-------------

## Aller plus loin
### installation automatique

Le **Preseed** est un outil qui permet d'automatiser l'installation de Debian et de ses dérivés. Il permet de créer des fichiers de réponse, qui contiennent toutes les informations nécessaires pour effectuer une installation sans interaction de l'utilisateur. Cela signifie que les utilisateurs n'ont pas à répondre à chaque question pendant le processus d'installation, ce qui rend l'installation plus rapide et moins sujette à des erreurs humaines. Il est particulièrement utile pour des déploiements à grande échelle.

### Rescue mode

Pour changer le mot de passe root sans y être connecté, il faut lancer une clé bootable et ouvrir le terminal. Ensuite il faut identifiez et montez la partition contenant l'installation.

Par la suite, il faut ouvrir le fichier  ou se trouve le document /shadow pour remplacer le hachage du mot de passe root par un nouveau hachage ou l'on connait le mot de passe en exécutant passwd root. Ensuite il faut enregistrez les modifications, démontez la partition et redémarrez le système. 

-------------

## Source

https://polprog.net/blog/passwd/

https://www.ma-no.org/en/software/operating-systems/reset-linux-root-password-without-knowing-the-password

https://www.debian.org/

https://linuxfr.org/forums/

https://forum.ubuntu-fr.org/

