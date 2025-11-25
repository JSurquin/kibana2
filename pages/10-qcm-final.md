---
layout: new-section
routeAlias: 'qcm-final'
---

<a name="qcm-final" id="qcm-final"></a>

# ✅ QCM de validation finale
## Évaluation des connaissances Unix/Linux

### Testez vos compétences !

---

# Instructions 📝

**Format du QCM :**
- 50 questions
- 4 réponses possibles par question
- 1 seule bonne réponse
- Durée recommandée : 60 minutes

**Notation :**
- 80% et + : Excellent ⭐⭐⭐
- 60-79% : Bien ⭐⭐
- 40-59% : Moyen ⭐
- Moins de 40% : À revoir

**Conseil :** Lisez attentivement chaque question !

---

# Question 1

Quelle commande permet de voir le répertoire courant ?

A) `ls`

B) `pwd`

C) `cd`

D) `whereis`

---

# Réponse 1

**Bonne réponse : B) `pwd`**

**Explication :**
- `pwd` : Print Working Directory
- `ls` : liste les fichiers
- `cd` : change de répertoire
- `whereis` : localise un binaire

---

# Question 2

Quelle permission est représentée par le chiffre 7 en octal ?

A) `r--`

B) `rw-`

C) `rwx`

D) `r-x`

---

# Réponse 2

**Bonne réponse : C) `rwx`**

**Explication :**
- 7 = 4 (read) + 2 (write) + 1 (execute)
- 6 = rw-
- 5 = r-x
- 4 = r--

---

# Question 3

Quel utilisateur a l'UID 0 ?

A) `admin`

B) `root`

C) `system`

D) `sudo`

---

# Réponse 3

**Bonne réponse : B) `root`**

**Explication :**
- L'utilisateur `root` a toujours l'UID 0
- C'est le super-utilisateur avec tous les privilèges
- Les UIDs 1-999 sont réservés aux comptes système
- Les UIDs 1000+ sont pour les utilisateurs normaux

---

# Question 4

Quelle commande permet de changer le propriétaire d'un fichier ?

A) `chmod`

B) `chown`

C) `chgrp`

D) `usermod`

---

# Réponse 4

**Bonne réponse : B) `chown`**

**Explication :**
- `chown` : change owner (propriétaire)
- `chmod` : change les permissions
- `chgrp` : change le groupe uniquement
- `usermod` : modifie un utilisateur

---

# Question 5

Quel signal envoie la commande `kill` par défaut ?

A) SIGKILL (9)

B) SIGTERM (15)

C) SIGHUP (1)

D) SIGSTOP (19)

---

# Réponse 5

**Bonne réponse : B) SIGTERM (15)**

**Explication :**
- SIGTERM permet au processus de se terminer proprement
- SIGKILL (9) termine immédiatement sans nettoyage
- Toujours essayer SIGTERM avant SIGKILL
- `kill -9` doit être utilisé en dernier recours

---

# Question 6

Quel fichier contient les mots de passe chiffrés ?

A) `/etc/passwd`

B) `/etc/shadow`

C) `/etc/group`

D) `/etc/security`

---

# Réponse 6

**Bonne réponse : B) `/etc/shadow`**

**Explication :**
- `/etc/shadow` : mots de passe chiffrés (accessible uniquement par root)
- `/etc/passwd` : informations utilisateurs (mais plus les mots de passe)
- `/etc/group` : informations sur les groupes
- Le 'x' dans passwd indique que le mot de passe est dans shadow

---

# Question 7

Quelle est la commande pour voir les processus en temps réel ?

A) `ps`

B) `top`

C) `pgrep`

D) `kill`

---

# Réponse 7

**Bonne réponse : B) `top`**

**Explication :**
- `top` : affichage en temps réel des processus
- `ps` : snapshot des processus à un instant T
- `pgrep` : recherche de processus par nom
- `htop` est une alternative moderne à `top`

---

# Question 8

Quel est le PID du premier processus du système ?

A) 0

B) 1

C) 10

D) 100

---

# Réponse 8

**Bonne réponse : B) 1**

**Explication :**
- Le PID 1 est toujours `init` ou `systemd`
- C'est le processus parent de tous les autres
- Tous les processus descendent de PID 1
- Si PID 1 crash, le système crash

---

# Question 9

Quelle commande permet de monter un système de fichiers ?

A) `mount`

B) `umount`

C) `fsck`

D) `df`

---

# Réponse 9

**Bonne réponse : A) `mount`**

**Explication :**
- `mount` : monte un système de fichiers
- `umount` : démonte (attention, pas "unmount")
- `fsck` : vérifie l'intégrité du système de fichiers
- `df` : affiche l'espace disque utilisé

---

# Question 10

Où sont stockées les configurations de montage automatique au boot ?

A) `/etc/mount`

B) `/etc/fstab`

C) `/etc/filesystems`

D) `/boot/mount.conf`

---

# Réponse 10

**Bonne réponse : B) `/etc/fstab`**

**Explication :**
- `/etc/fstab` : File System TABle
- Contient les montages automatiques au démarrage
- Format : device, mountpoint, fstype, options, dump, pass
- Toujours tester avec `mount -a` avant de rebooter

---

# Question 11

Quelle commande permet de voir l'utilisation du disque par répertoire ?

A) `df`

B) `du`

C) `fdisk`

D) `lsblk`

---

# Réponse 11

**Bonne réponse : B) `du`**

**Explication :**
- `du` : Disk Usage (par fichier/répertoire)
- `df` : Disk Free (par partition)
- `fdisk` : gestion des partitions
- `lsblk` : liste les périphériques bloc

---

# Question 12

Quelle adresse IP est réservée pour localhost ?

A) `192.168.1.1`

B) `10.0.0.1`

C) `127.0.0.1`

D) `0.0.0.0`

---

# Réponse 12

**Bonne réponse : C) `127.0.0.1`**

**Explication :**
- `127.0.0.1` est l'adresse de loopback (localhost)
- Toute la plage `127.0.0.0/8` est réservée au loopback
- Utilisée pour les communications internes à la machine
- `::1` est l'équivalent IPv6

---

# Question 13

Quel port utilise le protocole SSH par défaut ?

A) 21

B) 22

C) 23

D) 80

---

# Réponse 13

**Bonne réponse : B) 22**

**Explication :**
- Port 22 : SSH (Secure Shell)
- Port 21 : FTP
- Port 23 : Telnet (non sécurisé)
- Port 80 : HTTP

---

# Question 14

Quelle commande permet de voir les connexions réseau actives ?

A) `ping`

B) `netstat` ou `ss`

C) `ifconfig`

D) `route`

---

# Réponse 14

**Bonne réponse : B) `netstat` ou `ss`**

**Explication :**
- `ss` : Socket Statistics (moderne)
- `netstat` : ancien mais toujours utilisé
- `ping` : teste la connectivité
- `ifconfig` : configuration des interfaces

---

# Question 15

Quel gestionnaire de paquets est utilisé sur Debian/Ubuntu ?

A) `yum`

B) `dnf`

C) `apt`

D) `pacman`

---

# Réponse 15

**Bonne réponse : C) `apt`**

**Explication :**
- `apt` : Advanced Package Tool (Debian/Ubuntu)
- `dnf` : Dandified Yum (RHEL/Fedora)
- `yum` : ancien gestionnaire Red Hat
- `pacman` : Arch Linux

---

# Question 16

Quelle commande met à jour la liste des paquets disponibles ?

A) `apt upgrade`

B) `apt update`

C) `apt install`

D) `apt search`

---

# Réponse 16

**Bonne réponse : B) `apt update`**

**Explication :**
- `apt update` : met à jour la **liste** des paquets
- `apt upgrade` : installe les **mises à jour** des paquets
- Toujours faire `update` avant `upgrade`
- Comme consulter le catalogue avant d'acheter

---

# Question 17

Quel fichier contient le shebang d'un script bash ?

A) La dernière ligne

B) La première ligne

C) N'importe où dans le fichier

D) Dans un fichier séparé

---

# Réponse 17

**Bonne réponse : B) La première ligne**

**Explication :**
- Le shebang est `#!/bin/bash`
- Doit être sur la première ligne du fichier
- Indique quel interpréteur utiliser
- Obligatoire pour rendre le script exécutable

---

# Question 18

Quelle est la syntaxe correcte pour une boucle for en bash ?

A) `for i in {1..5}; do echo $i; done`

B) `for (i=1; i<=5; i++) { echo $i }`

C) `foreach i in 1..5: echo $i`

D) `for i = 1 to 5 do echo $i end`

---

# Réponse 18

**Bonne réponse : A) `for i in {1..5}; do echo $i; done`**

**Explication :**
- Syntaxe Bash standard
- `do` et `done` encadrent le corps de la boucle
- Les accolades ne sont pas utilisées en Bash
- La syntaxe C-style existe mais différente : `for ((i=1; i<=5; i++))`

---

# Question 19

Quelle commande permet de planifier des tâches récurrentes ?

A) `at`

B) `cron`

C) `systemd`

D) `timer`

---

# Réponse 19

**Bonne réponse : B) `cron`**

**Explication :**
- `cron` : tâches récurrentes (quotidiennes, hebdomadaires, etc.)
- `at` : tâche ponctuelle unique
- `systemd timers` : alternative moderne à cron
- `crontab -e` pour éditer les tâches cron

---

# Question 20

Dans un crontab, que signifie `0 2 * * *` ?

A) Toutes les 2 minutes

B) À 2h du matin tous les jours

C) Tous les 2 jours à minuit

D) Le 2 de chaque mois

---

# Réponse 20

**Bonne réponse : B) À 2h du matin tous les jours**

**Explication :**
- Format cron : `minute heure jour mois jour_semaine`
- `0 2 * * *` = minute 0, heure 2, tous les jours
- `*` signifie "tous"
- `*/5` signifie "toutes les 5 unités"

---

# Question 21

Où sont généralement stockés les logs système ?

A) `/log`

B) `/var/log`

C) `/etc/log`

D) `/home/log`

---

# Réponse 21

**Bonne réponse : B) `/var/log`**

**Explication :**
- `/var/log` : répertoire standard des logs
- Fichiers importants : `syslog`, `auth.log`, `kern.log`
- `journalctl` pour les logs systemd
- Les logs sont rotatés par `logrotate`

---

# Question 22

Quelle commande permet de voir les logs systemd en temps réel ?

A) `tail -f /var/log/syslog`

B) `journalctl -f`

C) `dmesg -w`

D) `logwatch`

---

# Réponse 22

**Bonne réponse : B) `journalctl -f`**

**Explication :**
- `journalctl -f` : follow (comme `tail -f`)
- Affiche les logs systemd en temps réel
- `journalctl -u service` pour un service spécifique
- `-f` est l'équivalent de `--follow`

---

# Question 23

Qu'est-ce que SELinux ?

A) Un gestionnaire de paquets

B) Un système de contrôle d'accès obligatoire (MAC)

C) Un éditeur de texte

D) Un protocole réseau

---

# Réponse 23

**Bonne réponse : B) Un système de contrôle d'accès obligatoire (MAC)**

**Explication :**
- SELinux = Security-Enhanced Linux
- Ajoute une couche de sécurité au-delà des permissions Unix
- Trois modes : enforcing, permissive, disabled
- Par défaut sur RHEL/CentOS/Fedora

---

# Question 24

Quelle commande permet de désactiver temporairement SELinux ?

A) `selinux disable`

B) `setenforce 0`

C) `systemctl stop selinux`

D) `chmod 000 /etc/selinux`

---

# Réponse 24

**Bonne réponse : B) `setenforce 0`**

**Explication :**
- `setenforce 0` : passe en mode permissive
- `setenforce 1` : passe en mode enforcing
- Temporaire (jusqu'au reboot)
- Pour désactiver définitivement : éditer `/etc/selinux/config`

---

# Question 25

Quel outil bloque les IP après plusieurs tentatives de connexion échouées ?

A) `iptables`

B) `fail2ban`

C) `ufw`

D) `firewalld`

---

# Réponse 25

**Bonne réponse : B) `fail2ban`**

**Explication :**
- `fail2ban` : bannit les IP après X échecs
- Protection contre le brute-force
- Configurable dans `/etc/fail2ban/jail.conf`
- Fonctionne avec SSH, HTTP, FTP, etc.

---

# Question 26

Quel est le protocole pour transférer des fichiers de manière sécurisée ?

A) FTP

B) HTTP

C) SCP ou SFTP

D) Telnet

---

# Réponse 26

**Bonne réponse : C) SCP ou SFTP**

**Explication :**
- SCP : Secure Copy (basé sur SSH)
- SFTP : SSH File Transfer Protocol
- FTP est non chiffré (mot de passe en clair)
- `rsync` est aussi une bonne option avec SSH

---

# Question 27

Où sont stockées les clés SSH par défaut ?

A) `/etc/ssh/`

B) `~/.ssh/`

C) `/var/ssh/`

D) `/home/ssh/`

---

# Réponse 27

**Bonne réponse : B) `~/.ssh/`**

**Explication :**
- `~/.ssh/` : répertoire personnel de l'utilisateur
- `id_rsa` ou `id_ed25519` : clé privée
- `id_rsa.pub` ou `id_ed25519.pub` : clé publique
- `authorized_keys` : clés publiques autorisées

---

# Question 28

Quelle commande génère une paire de clés SSH ?

A) `ssh-key`

B) `ssh-gen`

C) `ssh-keygen`

D) `keygen`

---

# Réponse 28

**Bonne réponse : C) `ssh-keygen`**

**Explication :**
- `ssh-keygen -t ed25519` : recommandé en 2025
- `ssh-keygen -t rsa -b 4096` : aussi valide
- Génère une paire de clés (privée + publique)
- Toujours protéger la clé privée !

---

# Question 29

Quel serveur web est connu pour ses performances et sa faible consommation mémoire ?

A) Apache

B) Nginx

C) IIS

D) Tomcat

---

# Réponse 29

**Bonne réponse : B) Nginx**

**Explication :**
- Nginx : architecture asynchrone, très performant
- Apache : plus ancien, architecture multi-processus
- Nginx souvent utilisé comme reverse proxy
- Les deux sont excellents selon les cas d'usage

---

# Question 30

Où sont stockés les fichiers de configuration de Nginx sur Debian/Ubuntu ?

A) `/etc/apache2/`

B) `/etc/nginx/`

C) `/var/www/nginx/`

D) `/usr/local/nginx/`

---

# Réponse 30

**Bonne réponse : B) `/etc/nginx/`**

**Explication :**
- `/etc/nginx/nginx.conf` : config principale
- `/etc/nginx/sites-available/` : sites disponibles
- `/etc/nginx/sites-enabled/` : sites actifs
- `/var/www/html/` : répertoire web par défaut

---

# Question 31

Quelle commande teste la configuration de Nginx sans redémarrer ?

A) `nginx test`

B) `nginx -t`

C) `systemctl test nginx`

D) `nginx --check`

---

# Réponse 31

**Bonne réponse : B) `nginx -t`**

**Explication :**
- `nginx -t` : test de syntaxe
- Valide la configuration sans appliquer
- Toujours tester avant de recharger
- `nginx -T` affiche la config complète

---

# Question 32

Quel protocole permet d'obtenir des certificats SSL gratuits avec Let's Encrypt ?

A) HTTP

B) FTP

C) ACME

D) SSL

---

# Réponse 32

**Bonne réponse : C) ACME**

**Explication :**
- ACME : Automatic Certificate Management Environment
- Utilisé par Certbot (client Let's Encrypt)
- Renouvellement automatique tous les 90 jours
- Gratuit et automatisé

---

# Question 33

Quelle commande permet de sauvegarder une base MySQL ?

A) `mysql-backup`

B) `mysqldump`

C) `mysql-save`

D) `mysqlexport`

---

# Réponse 33

**Bonne réponse : B) `mysqldump`**

**Explication :**
- `mysqldump` : export d'une base en SQL
- `mysqldump -u root -p database > backup.sql`
- Pour restaurer : `mysql -u root -p database < backup.sql`
- Toujours sauvegarder avant les mises à jour !

---

# Question 34

Quelle est la différence entre `systemctl start` et `systemctl enable` ?

A) Aucune différence

B) `start` démarre maintenant, `enable` démarre au boot

C) `start` est permanent, `enable` est temporaire

D) `enable` démarre maintenant, `start` au boot

---

# Réponse 34

**Bonne réponse : B) `start` démarre maintenant, `enable` démarre au boot**

**Explication :**
- `start` : démarre le service immédiatement
- `enable` : active le démarrage automatique au boot
- `enable --now` : les deux en une commande
- `disable` : désactive le démarrage au boot

---

# Question 35

Quel fichier contient la configuration principale de systemd ?

A) `/etc/systemd/system.conf`

B) `/etc/systemd/systemd.conf`

C) `/etc/init.d/systemd`

D) `/lib/systemd/systemd.conf`

---

# Réponse 35

**Bonne réponse : A) `/etc/systemd/system.conf`**

**Explication :**
- Configuration globale de systemd
- Les services sont dans `/etc/systemd/system/`
- Les services système dans `/lib/systemd/system/`
- Utiliser `systemctl` pour gérer les services

---

# Question 36

Quelle commande permet de voir les services qui ont échoué ?

A) `systemctl failed`

B) `systemctl --failed`

C) `systemctl list-failed`

D) `systemctl status --failed`

---

# Réponse 36

**Bonne réponse : B) `systemctl --failed`**

**Explication :**
- Liste tous les services en état "failed"
- Utile pour le dépannage
- `systemctl status service` pour plus de détails
- `journalctl -u service` pour les logs

---

# Question 37

Que signifie LVM ?

A) Linux Virtual Memory

B) Logical Volume Manager

C) Linux Volume Management

D) Local Virtual Machine

---

# Réponse 37

**Bonne réponse : B) Logical Volume Manager**

**Explication :**
- LVM : gestion flexible du stockage
- Permet de redimensionner les partitions à chaud
- Snapshots possibles
- Structure : PV → VG → LV

---

# Question 38

Quelle commande permet de créer un lien symbolique ?

A) `link`

B) `ln -s`

C) `symlink`

D) `mklink`

---

# Réponse 38

**Bonne réponse : B) `ln -s`**

**Explication :**
- `ln -s source destination` : lien symbolique
- `ln source destination` : lien dur (sans `-s`)
- Les liens symboliques peuvent traverser les partitions
- Les liens durs partagent le même inode

---

# Question 39

Quelle est la différence entre `>` et `>>` en redirection ?

A) Aucune différence

B) `>` écrase, `>>` ajoute à la fin

C) `>>` écrase, `>` ajoute à la fin

D) `>` est pour stdout, `>>` pour stderr

---

# Réponse 39

**Bonne réponse : B) `>` écrase, `>>` ajoute à la fin**

**Explication :**
- `>` : redirection (écrase le fichier)
- `>>` : redirection en ajout (append)
- `2>` : redirection de stderr
- `&>` : redirection de stdout et stderr

---

# Question 40

Quelle variable d'environnement contient le répertoire personnel ?

A) `$USER`

B) `$PWD`

C) `$HOME`

D) `$PATH`

---

# Réponse 40

**Bonne réponse : C) `$HOME`**

**Explication :**
- `$HOME` : répertoire personnel (`/home/user`)
- `$USER` : nom de l'utilisateur
- `$PWD` : répertoire courant
- `$PATH` : chemins de recherche des exécutables

---

# Question 41

Comment afficher les 10 premières lignes d'un fichier ?

A) `tail fichier`

B) `head fichier`

C) `cat fichier | head`

D) `top fichier`

---

# Réponse 41

**Bonne réponse : B) `head fichier`**

**Explication :**
- `head` : 10 premières lignes par défaut
- `head -n 5` : 5 premières lignes
- `tail` : 10 dernières lignes
- `tail -n 20` : 20 dernières lignes

---

# Question 42

Quelle commande recherche un fichier par son nom ?

A) `search`

B) `find`

C) `locate`

D) B et C sont corrects

---

# Réponse 42

**Bonne réponse : D) B et C sont corrects**

**Explication :**
- `find` : recherche en temps réel dans le système
- `locate` : recherche dans une base de données (plus rapide)
- `find /chemin -name "fichier.txt"`
- `updatedb` pour mettre à jour la base de `locate`

---

# Question 43

Quelle commande permet de compresser un fichier avec gzip ?

A) `gzip fichier`

B) `compress fichier`

C) `zip fichier`

D) `tar fichier`

---

# Réponse 43

**Bonne réponse : A) `gzip fichier`**

**Explication :**
- `gzip` : compression (crée `fichier.gz`)
- `gunzip` : décompression
- `tar -czf` : tar + gzip en une commande
- `bzip2` et `xz` sont d'autres algorithmes de compression

---

# Question 44

Que fait la commande `chmod +x script.sh` ?

A) Rend le script lisible

B) Rend le script exécutable

C) Rend le script modifiable

D) Supprime le script

---

# Réponse 44

**Bonne réponse : B) Rend le script exécutable**

**Explication :**
- `+x` : ajoute le droit d'exécution
- Nécessaire pour lancer `./script.sh`
- `chmod 755 script.sh` : alternative numérique
- Sans `x`, bash doit être appelé explicitement

---

# Question 45

Quelle commande permet de voir l'espace disque disponible ?

A) `du`

B) `df`

C) `disk`

D) `free`

---

# Réponse 45

**Bonne réponse : B) `df`**

**Explication :**
- `df` : Disk Free (par partition)
- `df -h` : format lisible (human-readable)
- `du` : Disk Usage (par fichier/répertoire)
- `free` : affiche la mémoire RAM

---

# Question 46

Quel est le répertoire temporaire standard ?

A) `/temp`

B) `/tmp`

C) `/var/tmp`

D) B et C sont corrects

---

# Réponse 46

**Bonne réponse : D) B et C sont corrects**

**Explication :**
- `/tmp` : effacé au redémarrage
- `/var/tmp` : persistant entre redémarrages
- Tous les utilisateurs y ont accès (sticky bit)
- Nettoyé périodiquement par le système

---

# Question 47

Quelle commande affiche les messages du noyau ?

A) `kernel`

B) `dmesg`

C) `syslog`

D) `messages`

---

# Réponse 47

**Bonne réponse : B) `dmesg`**

**Explication :**
- `dmesg` : affiche le buffer du noyau
- Utile pour diagnostiquer les problèmes matériels
- `dmesg | grep error` : chercher les erreurs
- `journalctl -k` : équivalent systemd

---

# Question 48

Quelle est la commande pour redémarrer le système ?

A) `restart`

B) `reboot`

C) `shutdown -r now`

D) B et C sont corrects

---

# Réponse 48

**Bonne réponse : D) B et C sont corrects**

**Explication :**
- `reboot` : redémarre immédiatement
- `shutdown -r now` : idem (plus verbeux)
- `systemctl reboot` : méthode systemd
- `shutdown -r +10` : redémarre dans 10 minutes

---

# Question 49

Qu'est-ce qu'un daemon ?

A) Un virus

B) Un processus en arrière-plan

C) Un fichier de configuration

D) Un utilisateur système

---

# Réponse 49

**Bonne réponse : B) Un processus en arrière-plan**

**Explication :**
- Daemon : service qui tourne en arrière-plan
- Exemples : `sshd`, `nginx`, `cron`
- Souvent démarrés au boot
- Gérés par systemd ou init

---

# Question 50

Quelle est la meilleure pratique pour travailler en tant qu'administrateur ?

A) Toujours se connecter en root

B) Utiliser sudo pour les tâches admin

C) Désactiver root

D) Partager le mot de passe root

---

# Réponse 50

**Bonne réponse : B) Utiliser sudo pour les tâches admin**

**Explication :**
- Principe du moindre privilège
- `sudo` offre la traçabilité (logs)
- Ne jamais travailler en root directement
- Limiter les privilèges sudo aux commandes nécessaires

---

# Correction du QCM 🎓

**Comptez vos bonnes réponses :**

- **40-50 points (80%+)** : Excellent ! ⭐⭐⭐
  Vous maîtrisez Unix/Linux !

- **30-39 points (60-79%)** : Bien ! ⭐⭐
  Bonnes bases, quelques révisions recommandées

- **20-29 points (40-59%)** : Moyen ⭐
  Revoir les modules où vous avez des lacunes

- **< 20 points (< 40%)** : À revoir
  Reprenez la formation depuis le début

---

# Points à réviser selon votre score 📚

**Si vous avez raté les questions 1-10 :**
- Revoir le Module 1 (Introduction)
- Commandes de base
- Système de fichiers

**Si vous avez raté les questions 11-20 :**
- Revoir les Modules 2-4
- Utilisateurs, permissions
- Processus, stockage

**Si vous avez raté les questions 21-30 :**
- Revoir les Modules 5-6
- Réseau, pare-feu
- Paquets, maintenance

---

# Points à réviser (suite) 📚

**Si vous avez raté les questions 31-40 :**
- Revoir les Modules 7-8
- Scripts, automatisation
- Sécurité système

**Si vous avez raté les questions 41-50 :**
- Revoir le Module 9
- Services serveurs
- Bonnes pratiques

---

# Félicitations ! 🎉

**Vous avez complété la formation Unix/Linux - Initiation !**

**Compétences acquises :**

✅ Utilisation de la ligne de commande

✅ Administration système

✅ Gestion réseau

✅ Scripting et automatisation

✅ Sécurité

✅ Services serveurs

**Prochaines étapes :**
- Pratiquer régulièrement
- Mettre en place un projet personnel
- Envisager une certification (LPIC-1, RHCSA)
- Continuer à apprendre !

---
layout: default
---

# Merci et bon courage ! 💪

**La communauté Linux est là pour vous aider !**

Continuez à apprendre, pratiquer et partager.

L'open source est une aventure collaborative.

