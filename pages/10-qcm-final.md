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

# Question 2

Quelle permission est représentée par le chiffre 7 en octal ?

A) `r--`

B) `rw-`

C) `rwx`

D) `r-x`

---

# Question 3

Quel utilisateur a l'UID 0 ?

A) `admin`

B) `root`

C) `system`

D) `sudo`

---

# Question 4

Quelle commande permet de changer le propriétaire d'un fichier ?

A) `chmod`

B) `chown`

C) `chgrp`

D) `usermod`

---

# Question 5

Quel signal envoie la commande `kill` par défaut ?

A) SIGKILL (9)

B) SIGTERM (15)

C) SIGHUP (1)

D) SIGSTOP (19)

---

# Question 6

Quel fichier contient les mots de passe chiffrés ?

A) `/etc/passwd`

B) `/etc/shadow`

C) `/etc/group`

D) `/etc/security`

---

# Question 7

Quelle est la commande pour voir les processus en temps réel ?

A) `ps`

B) `top`

C) `pgrep`

D) `kill`

---

# Question 8

Quel est le PID du premier processus du système ?

A) 0

B) 1

C) 10

D) 100

---

# Question 9

Quelle commande permet de monter un système de fichiers ?

A) `mount`

B) `umount`

C) `fsck`

D) `df`

---

# Question 10

Où sont stockées les configurations de montage automatique au boot ?

A) `/etc/mount`

B) `/etc/fstab`

C) `/etc/filesystems`

D) `/boot/mount.conf`

---

# Question 11

Quelle commande permet de voir l'utilisation du disque par répertoire ?

A) `df`

B) `du`

C) `fdisk`

D) `lsblk`

---

# Question 12

Quelle adresse IP est réservée pour localhost ?

A) `192.168.1.1`

B) `10.0.0.1`

C) `127.0.0.1`

D) `0.0.0.0`

---

# Question 13

Quel port utilise le protocole SSH par défaut ?

A) 21

B) 22

C) 23

D) 80

---

# Question 14

Quelle commande permet de voir les connexions réseau actives ?

A) `ping`

B) `netstat` ou `ss`

C) `ifconfig`

D) `route`

---

# Question 15

Quel gestionnaire de paquets est utilisé sur Debian/Ubuntu ?

A) `yum`

B) `dnf`

C) `apt`

D) `pacman`

---

# Question 16

Quelle commande met à jour la liste des paquets disponibles ?

A) `apt upgrade`

B) `apt update`

C) `apt install`

D) `apt search`

---

# Question 17

Quel fichier contient le shebang d'un script bash ?

A) La dernière ligne

B) La première ligne

C) N'importe où dans le fichier

D) Dans un fichier séparé

---

# Question 18

Quelle est la syntaxe correcte pour une boucle for en bash ?

A) `for i in {1..5}; do echo $i; done`

B) `for (i=1; i<=5; i++) { echo $i }`

C) `foreach i in 1..5: echo $i`

D) `for i = 1 to 5 do echo $i end`

---

# Question 19

Quelle commande permet de planifier des tâches récurrentes ?

A) `at`

B) `cron`

C) `systemd`

D) `timer`

---

# Question 20

Dans un crontab, que signifie `0 2 * * *` ?

A) Toutes les 2 minutes

B) À 2h du matin tous les jours

C) Tous les 2 jours à minuit

D) Le 2 de chaque mois

---

# Question 21

Où sont généralement stockés les logs système ?

A) `/log`

B) `/var/log`

C) `/etc/log`

D) `/home/log`

---

# Question 22

Quelle commande permet de voir les logs systemd en temps réel ?

A) `tail -f /var/log/syslog`

B) `journalctl -f`

C) `dmesg -w`

D) `logwatch`

---

# Question 23

Qu'est-ce que SELinux ?

A) Un gestionnaire de paquets

B) Un système de contrôle d'accès obligatoire (MAC)

C) Un éditeur de texte

D) Un protocole réseau

---

# Question 24

Quelle commande permet de désactiver temporairement SELinux ?

A) `selinux disable`

B) `setenforce 0`

C) `systemctl stop selinux`

D) `chmod 000 /etc/selinux`

---

# Question 25

Quel outil bloque les IP après plusieurs tentatives de connexion échouées ?

A) `iptables`

B) `fail2ban`

C) `ufw`

D) `firewalld`

---

# Question 26

Quel est le protocole pour transférer des fichiers de manière sécurisée ?

A) FTP

B) HTTP

C) SCP ou SFTP

D) Telnet

---

# Question 27

Où sont stockées les clés SSH par défaut ?

A) `/etc/ssh/`

B) `~/.ssh/`

C) `/var/ssh/`

D) `/home/ssh/`

---

# Question 28

Quelle commande génère une paire de clés SSH ?

A) `ssh-key`

B) `ssh-gen`

C) `ssh-keygen`

D) `keygen`

---

# Question 29

Quel serveur web est connu pour ses performances et sa faible consommation mémoire ?

A) Apache

B) Nginx

C) IIS

D) Tomcat

---

# Question 30

Où sont stockés les fichiers de configuration de Nginx sur Debian/Ubuntu ?

A) `/etc/apache2/`

B) `/etc/nginx/`

C) `/var/www/nginx/`

D) `/usr/local/nginx/`

---

# Question 31

Quelle commande teste la configuration de Nginx sans redémarrer ?

A) `nginx test`

B) `nginx -t`

C) `systemctl test nginx`

D) `nginx --check`

---

# Question 32

Quel protocole permet d'obtenir des certificats SSL gratuits avec Let's Encrypt ?

A) HTTP

B) FTP

C) ACME

D) SSL

---

# Question 33

Quelle commande permet de sauvegarder une base MySQL ?

A) `mysql-backup`

B) `mysqldump`

C) `mysql-save`

D) `mysqlexport`

---

# Question 34

Quelle est la différence entre `systemctl start` et `systemctl enable` ?

A) Aucune différence

B) `start` démarre maintenant, `enable` démarre au boot

C) `start` est permanent, `enable` est temporaire

D) `enable` démarre maintenant, `start` au boot

---

# Question 35

Quel fichier contient la configuration principale de systemd ?

A) `/etc/systemd/system.conf`

B) `/etc/systemd/systemd.conf`

C) `/etc/init.d/systemd`

D) `/lib/systemd/systemd.conf`

---

# Question 36

Quelle commande permet de voir les services qui ont échoué ?

A) `systemctl failed`

B) `systemctl --failed`

C) `systemctl list-failed`

D) `systemctl status --failed`

---

# Question 37

Que signifie LVM ?

A) Linux Virtual Memory

B) Logical Volume Manager

C) Linux Volume Management

D) Local Virtual Machine

---

# Question 38

Quelle commande permet de créer un lien symbolique ?

A) `link`

B) `ln -s`

C) `symlink`

D) `mklink`

---

# Question 39

Quelle est la différence entre `>` et `>>` en redirection ?

A) Aucune différence

B) `>` écrase, `>>` ajoute à la fin

C) `>>` écrase, `>` ajoute à la fin

D) `>` est pour stdout, `>>` pour stderr

---

# Question 40

Quelle variable d'environnement contient le répertoire personnel ?

A) `$USER`

B) `$PWD`

C) `$HOME`

D) `$PATH`

---

# Question 41

Comment afficher les 10 premières lignes d'un fichier ?

A) `tail fichier`

B) `head fichier`

C) `cat fichier | head`

D) `top fichier`

---

# Question 42

Quelle commande recherche un fichier par son nom ?

A) `search`

B) `find`

C) `locate`

D) B et C sont corrects

---

# Question 43

Quelle commande permet de compresser un fichier avec gzip ?

A) `gzip fichier`

B) `compress fichier`

C) `zip fichier`

D) `tar fichier`

---

# Question 44

Que fait la commande `chmod +x script.sh` ?

A) Rend le script lisible

B) Rend le script exécutable

C) Rend le script modifiable

D) Supprime le script

---

# Question 45

Quelle commande permet de voir l'espace disque disponible ?

A) `du`

B) `df`

C) `disk`

D) `free`

---

# Question 46

Quel est le répertoire temporaire standard ?

A) `/temp`

B) `/tmp`

C) `/var/tmp`

D) B et C sont corrects

---

# Question 47

Quelle commande affiche les messages du noyau ?

A) `kernel`

B) `dmesg`

C) `syslog`

D) `messages`

---

# Question 48

Quelle est la commande pour redémarrer le système ?

A) `restart`

B) `reboot`

C) `shutdown -r now`

D) B et C sont corrects

---

# Question 49

Qu'est-ce qu'un daemon ?

A) Un virus

B) Un processus en arrière-plan

C) Un fichier de configuration

D) Un utilisateur système

---

# Question 50

Quelle est la meilleure pratique pour travailler en tant qu'administrateur ?

A) Toujours se connecter en root

B) Utiliser sudo pour les tâches admin

C) Désactiver root

D) Partager le mot de passe root

---

# 📝 RÉPONSES

### Les réponses sont sur les slides suivantes

**Ne trichez pas ! 😉**

---

# Réponses 1-5

**Réponse 1 : B) `pwd`**
- `pwd` : Print Working Directory
- `ls` : liste les fichiers
- `cd` : change de répertoire

**Réponse 2 : C) `rwx`**
- 7 = 4 (read) + 2 (write) + 1 (execute)
- 6 = rw-, 5 = r-x, 4 = r--

**Réponse 3 : B) `root`**
- L'utilisateur `root` a toujours l'UID 0
- C'est le super-utilisateur avec tous les privilèges

**Réponse 4 : B) `chown`**
- `chown` : change owner (propriétaire)
- `chmod` : change les permissions

**Réponse 5 : B) SIGTERM (15)**
- SIGTERM permet au processus de se terminer proprement
- SIGKILL (9) termine immédiatement

---

# Réponses 6-10

**Réponse 6 : B) `/etc/shadow`**
- `/etc/shadow` : mots de passe chiffrés (accessible uniquement par root)
- `/etc/passwd` : informations utilisateurs

**Réponse 7 : B) `top`**
- `top` : affichage en temps réel des processus
- `ps` : snapshot à un instant T

**Réponse 8 : B) 1**
- Le PID 1 est toujours `init` ou `systemd`
- C'est le processus parent de tous les autres

**Réponse 9 : A) `mount`**
- `mount` : monte un système de fichiers
- `umount` : démonte

**Réponse 10 : B) `/etc/fstab`**
- `/etc/fstab` : File System TABle
- Contient les montages automatiques au démarrage

---

# Réponses 11-15

**Réponse 11 : B) `du`**
- `du` : Disk Usage (par fichier/répertoire)
- `df` : Disk Free (par partition)

**Réponse 12 : C) `127.0.0.1`**
- `127.0.0.1` est l'adresse de loopback (localhost)
- `::1` est l'équivalent IPv6

**Réponse 13 : B) 22**
- Port 22 : SSH (Secure Shell)
- Port 21 : FTP, Port 23 : Telnet

**Réponse 14 : B) `netstat` ou `ss`**
- `ss` : Socket Statistics (moderne)
- `netstat` : ancien mais toujours utilisé

**Réponse 15 : C) `apt`**
- `apt` : Advanced Package Tool (Debian/Ubuntu)
- `dnf` : RHEL/Fedora

---

# Réponses 16-20

**Réponse 16 : B) `apt update`**
- `apt update` : met à jour la **liste** des paquets
- `apt upgrade` : installe les **mises à jour**

**Réponse 17 : B) La première ligne**
- Le shebang est `#!/bin/bash`
- Doit être sur la première ligne

**Réponse 18 : A) `for i in {1..5}; do echo $i; done`**
- Syntaxe Bash standard
- `do` et `done` encadrent le corps

**Réponse 19 : B) `cron`**
- `cron` : tâches récurrentes
- `at` : tâche ponctuelle unique

**Réponse 20 : B) À 2h du matin tous les jours**
- Format cron : `minute heure jour mois jour_semaine`
- `0 2 * * *` = minute 0, heure 2, tous les jours

---

# Réponses 21-25

**Réponse 21 : B) `/var/log`**
- `/var/log` : répertoire standard des logs
- Fichiers : `syslog`, `auth.log`, `kern.log`

**Réponse 22 : B) `journalctl -f`**
- `journalctl -f` : follow (comme `tail -f`)
- Affiche les logs systemd en temps réel

**Réponse 23 : B) Un système de contrôle d'accès obligatoire (MAC)**
- SELinux = Security-Enhanced Linux
- Trois modes : enforcing, permissive, disabled

**Réponse 24 : B) `setenforce 0`**
- `setenforce 0` : passe en mode permissive
- `setenforce 1` : passe en mode enforcing

**Réponse 25 : B) `fail2ban`**
- `fail2ban` : bannit les IP après X échecs
- Protection contre le brute-force

---

# Réponses 26-30

**Réponse 26 : C) SCP ou SFTP**
- SCP : Secure Copy (basé sur SSH)
- SFTP : SSH File Transfer Protocol

**Réponse 27 : B) `~/.ssh/`**
- `~/.ssh/` : répertoire personnel de l'utilisateur
- `id_rsa`, `id_ed25519` : clés privées

**Réponse 28 : C) `ssh-keygen`**
- `ssh-keygen -t ed25519` : recommandé en 2025
- Génère une paire de clés

**Réponse 29 : B) Nginx**
- Nginx : architecture asynchrone, très performant
- Apache : plus ancien, multi-processus

**Réponse 30 : B) `/etc/nginx/`**
- `/etc/nginx/nginx.conf` : config principale
- `/etc/nginx/sites-available/` : sites disponibles

---

# Réponses 31-35

**Réponse 31 : B) `nginx -t`**
- `nginx -t` : test de syntaxe
- Toujours tester avant de recharger

**Réponse 32 : C) ACME**
- ACME : Automatic Certificate Management Environment
- Utilisé par Certbot (Let's Encrypt)

**Réponse 33 : B) `mysqldump`**
- `mysqldump` : export d'une base en SQL
- `mysqldump -u root -p database > backup.sql`

**Réponse 34 : B) `start` démarre maintenant, `enable` démarre au boot**
- `start` : démarre immédiatement
- `enable` : active le démarrage au boot

**Réponse 35 : A) `/etc/systemd/system.conf`**
- Configuration globale de systemd
- Services dans `/etc/systemd/system/`

---

# Réponses 36-40

**Réponse 36 : B) `systemctl --failed`**
- Liste tous les services en état "failed"
- Utile pour le dépannage

**Réponse 37 : B) Logical Volume Manager**
- LVM : gestion flexible du stockage
- Structure : PV → VG → LV

**Réponse 38 : B) `ln -s`**
- `ln -s source destination` : lien symbolique
- `ln source destination` : lien dur

**Réponse 39 : B) `>` écrase, `>>` ajoute à la fin**
- `>` : redirection (écrase)
- `>>` : redirection en ajout (append)

**Réponse 40 : C) `$HOME`**
- `$HOME` : répertoire personnel (`/home/user`)
- `$USER` : nom de l'utilisateur

---

# Réponses 41-45

**Réponse 41 : B) `head fichier`**
- `head` : 10 premières lignes par défaut
- `tail` : 10 dernières lignes

**Réponse 42 : D) B et C sont corrects**
- `find` : recherche en temps réel
- `locate` : recherche dans une base de données

**Réponse 43 : A) `gzip fichier`**
- `gzip` : compression (crée `fichier.gz`)
- `gunzip` : décompression

**Réponse 44 : B) Rend le script exécutable**
- `+x` : ajoute le droit d'exécution
- Nécessaire pour lancer `./script.sh`

**Réponse 45 : B) `df`**
- `df` : Disk Free (par partition)
- `df -h` : format lisible

---

# Réponses 46-50

**Réponse 46 : D) B et C sont corrects**
- `/tmp` : effacé au redémarrage
- `/var/tmp` : persistant entre redémarrages

**Réponse 47 : B) `dmesg`**
- `dmesg` : affiche le buffer du noyau
- Utile pour diagnostiquer les problèmes matériels

**Réponse 48 : D) B et C sont corrects**
- `reboot` : redémarre immédiatement
- `shutdown -r now` : idem

**Réponse 49 : B) Un processus en arrière-plan**
- Daemon : service en arrière-plan
- Exemples : `sshd`, `nginx`, `cron`

**Réponse 50 : B) Utiliser sudo pour les tâches admin**
- Principe du moindre privilège
- `sudo` offre la traçabilité

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

#### Points à réviser selon votre score 📚

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

#### Félicitations ! 🎉

<div class="text-xs">

**Vous avez complété la formation Unix/Linux - Initiation !**

**Compétences acquises :**

- ✅ Utilisation de la ligne de commande
- ✅ Administration système
- ✅ Gestion réseau
- ✅ Scripting et automatisation
- ✅ Sécurité
- ✅ Services serveurs

**Prochaines étapes :**
- Pratiquer régulièrement
- Mettre en place un projet personnel
- Envisager une certification (LPIC-1, RHCSA)
- Continuer à apprendre !

</div>

---
layout: default
---

# Merci et bon courage ! 💪

**La communauté Linux est là pour vous aider !**

Continuez à apprendre, pratiquer et partager.

L'open source est une aventure collaborative.
