---
layout: intro
routeAlias: 'qcm-validation'
---

# QCM de Validation 📝

### Test de validation des connaissances acquises

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Validons nos connaissances <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Instructions du QCM 📋

### Modalités de l'évaluation

**Format de l'évaluation :**
- **50 questions** à choix multiples
- **4 réponses** possibles par question
- **1 seule réponse** correcte par question
- **Durée** : 60 minutes maximum
- **Seuil de réussite** : 70% (35/50)

**Thèmes évalués :**
- Concepts Unix/Linux de base
- Ligne de commande et shell
- Gestion des utilisateurs et permissions
- Système de fichiers
- Réseaux et connectivité
- Processus et services
- Paquets logiciels
- Shell scripting
- Virtualisation et conteneurs
- Docker et Ansible

**Conseils :**
- Lisez attentivement chaque question
- Éliminez les réponses manifestement incorrectes
- Gestionnez votre temps (1 minute par question)
- Relisez vos réponses si possible

---
layout: default
---

# Question 1 - Concepts de base 🐧

**Qu'est-ce qu'Unix ?**

A) Un système d'exploitation créé par Microsoft en 1985
B) Un système d'exploitation créé par AT&T en 1969
C) Un système d'exploitation créé par Apple en 1984
D) Un système d'exploitation créé par IBM en 1970

**Réponse : B**

Unix est un système d'exploitation créé par AT&T en 1969. Il a introduit de nombreux concepts fondamentaux comme la philosophie "tout est un fichier", le multi-utilisateur et le multi-tâches.

---
layout: default
---

# Question 2 - Ligne de commande 💻

**Quelle commande permet de voir le contenu d'un répertoire ?**

A) `cat`
B) `ls`
C) `dir`
D) `show`

**Réponse : B**

La commande `ls` (list) permet de lister le contenu d'un répertoire. `cat` affiche le contenu d'un fichier, `dir` est une commande Windows, et `show` n'existe pas.

---
layout: default
---

# Question 3 - Navigation 📁

**Quelle commande permet de remonter d'un niveau dans l'arborescence ?**

A) `cd /`
B) `cd ..`
C) `cd ~`
D) `cd -`

**Réponse : B**

`cd ..` permet de remonter d'un niveau dans l'arborescence. `cd /` va à la racine, `cd ~` va au répertoire home, et `cd -` va au répertoire précédent.

---
layout: default
---

# Question 4 - Permissions 🔐

**Que signifie la permission 755 sur un fichier ?**

A) Lecture, écriture, exécution pour tous
B) Lecture, écriture pour le propriétaire, lecture et exécution pour les autres
C) Lecture, écriture, exécution pour le propriétaire, lecture et exécution pour le groupe et les autres
D) Lecture, écriture pour le propriétaire et le groupe, lecture pour les autres

**Réponse : C**

755 signifie : 7 (rwx) pour le propriétaire, 5 (r-x) pour le groupe, 5 (r-x) pour les autres.

---
layout: default
---

# Question 5 - Utilisateurs 👥

**Quel est l'UID de l'utilisateur root ?**

A) 1
B) 0
C) 1000
D) 999

**Réponse : B**

L'utilisateur root a toujours l'UID 0. C'est l'utilisateur super-administrateur du système.

---
layout: default
---

# Question 6 - Processus ⚡

**Quelle commande affiche les processus en temps réel ?**

A) `ps`
B) `top`
C) `htop`
D) `process`

**Réponse : B et C**

`top` et `htop` affichent les processus en temps réel. `ps` affiche un instantané, et `process` n'existe pas.

---
layout: default
---

# Question 7 - Réseaux 🌐

**Quelle commande teste la connectivité réseau ?**

A) `telnet`
B) `ping`
C) `netstat`
D) `ifconfig`

**Réponse : B**

`ping` teste la connectivité réseau en envoyant des paquets ICMP. Les autres commandes ont des fonctions différentes.

---
layout: default
---

# Question 8 - Paquets 📦

**Quelle commande met à jour la liste des paquets disponibles ?**

A) `apt upgrade`
B) `apt update`
C) `apt install`
D) `apt remove`

**Réponse : B**

`apt update` met à jour la liste des paquets disponibles. `apt upgrade` met à jour les paquets installés.

---
layout: default
---

# Question 9 - Shell Scripting 📝

**Quel est le shebang pour un script bash ?**

A) `#!/bin/sh`
B) `#!/bin/bash`
C) `#!bash`
D) `#!/usr/bin/env bash`

**Réponse : B et D**

`#!/bin/bash` et `#!/usr/bin/env bash` sont tous les deux valides. Le second est plus portable.

---
layout: default
---

# Question 10 - Virtualisation 🖥️

**Qu'est-ce que KVM ?**

A) Un émulateur de machine virtuelle
B) Un hyperviseur de type 1
C) Un outil de conteneurisation
D) Un gestionnaire de paquets

**Réponse : B**

KVM (Kernel-based Virtual Machine) est un hyperviseur de type 1 intégré au noyau Linux.

---
layout: default
---

# Question 11 - Docker 🐳

**Quelle commande construit une image Docker ?**

A) `docker create`
B) `docker build`
C) `docker run`
D) `docker make`

**Réponse : B**

`docker build` construit une image Docker à partir d'un Dockerfile.

---
layout: default
---

# Question 12 - Ansible 🔧

**Quel format utilise Ansible pour ses playbooks ?**

A) JSON
B) YAML
C) XML
D) TOML

**Réponse : B**

Ansible utilise le format YAML pour ses playbooks, ce qui les rend lisibles et structurés.

---
layout: default
---

# Question 13 - Système de fichiers 📂

**Quel répertoire contient les fichiers de configuration système ?**

A) `/bin`
B) `/etc`
C) `/var`
D) `/usr`

**Réponse : B**

Le répertoire `/etc` contient les fichiers de configuration système.

---
layout: default
---

# Question 14 - Services 🔄

**Quelle commande redémarre un service systemd ?**

A) `service restart nom_service`
B) `systemctl restart nom_service`
C) `restart nom_service`
D) `system restart nom_service`

**Réponse : B**

`systemctl restart nom_service` redémarre un service systemd.

---
layout: default
---

# Question 15 - Sécurité 🔒

**Quelle commande configure le pare-feu ufw ?**

A) `iptables`
B) `ufw`
C) `firewall`
D) `security`

**Réponse : B**

`ufw` (Uncomplicated Firewall) est l'outil de configuration du pare-feu sur Ubuntu.

---
layout: default
---

# Question 16 - Monitoring 📊

**Quelle commande affiche l'utilisation de l'espace disque ?**

A) `top`
B) `free`
C) `df`
D) `du`

**Réponse : C**

`df` (disk free) affiche l'utilisation de l'espace disque. `du` affiche l'utilisation par répertoire.

---
layout: default
---

# Question 17 - Logs 📝

**Où se trouvent les logs système sur Ubuntu ?**

A) `/var/log`
B) `/etc/log`
C) `/log`
D) `/sys/log`

**Réponse : A**

Les logs système se trouvent dans `/var/log` sur Ubuntu et la plupart des distributions Linux.

---
layout: default
---

# Question 18 - Variables d'environnement 🌍

**Quelle commande affiche toutes les variables d'environnement ?**

A) `env`
B) `echo $PATH`
C) `set`
D) `variables`

**Réponse : A**

La commande `env` affiche toutes les variables d'environnement.

---
layout: default
---

# Question 19 - Redirection 🔄

**Quel opérateur redirige la sortie d'erreur vers un fichier ?**

A) `>`
B) `>>`
C) `2>`
D) `&>`

**Réponse : C**

`2>` redirige la sortie d'erreur (stderr) vers un fichier.

---
layout: default
---

# Question 20 - Recherche 🔍

**Quelle commande recherche du texte dans des fichiers ?**

A) `find`
B) `grep`
C) `search`
D) `locate`

**Réponse : B**

`grep` recherche du texte dans des fichiers. `find` recherche des fichiers, `locate` recherche des fichiers rapidement.

---
layout: default
---

# Question 21 - Compression 📦

**Quelle commande compresse un fichier en tar.gz ?**

A) `tar -czf`
B) `gzip`
C) `zip`
D) `compress`

**Réponse : A**

`tar -czf` crée une archive tar compressée avec gzip.

---
layout: default
---

# Question 22 - Processus en arrière-plan 🔄

**Quel symbole permet d'exécuter une commande en arrière-plan ?**

A) `&`
B) `&&`
C) `||`
D) `;`

**Réponse : A**

Le symbole `&` permet d'exécuter une commande en arrière-plan.

---
layout: default
---

# Question 23 - Signaux 📡

**Quel signal arrête proprement un processus ?**

A) SIGKILL (9)
B) SIGTERM (15)
C) SIGINT (2)
D) SIGSTOP (19)

**Réponse : B**

SIGTERM (15) arrête proprement un processus, permettant une fermeture gracieuse.

---
layout: default
---

# Question 24 - Réseaux avancés 🌐

**Quelle commande affiche les connexions réseau actives ?**

A) `netstat`
B) `ss`
C) `ifconfig`
D) `ip`

**Réponse : A et B**

`netstat` et `ss` affichent les connexions réseau actives. `ss` est plus moderne.

---
layout: default
---

# Question 25 - Docker Compose 🎼

**Quel fichier définit les services Docker Compose ?**

A) `dockerfile`
B) `docker-compose.yml`
C) `compose.yml`
D) `services.yml`

**Réponse : B**

`docker-compose.yml` définit les services Docker Compose.

---
layout: default
---

# Question 26 - Ansible Inventory 📋

**Quel format peut utiliser l'inventory Ansible ?**

A) INI uniquement
B) YAML uniquement
C) INI et YAML
D) JSON uniquement

**Réponse : C**

L'inventory Ansible peut utiliser les formats INI et YAML.

---
layout: default
---

# Question 27 - Permissions spéciales 🎯

**Qu'est-ce que le SUID ?**

A) Set User ID - exécute le fichier avec les droits du propriétaire
B) Set Group ID - exécute le fichier avec les droits du groupe
C) Sticky bit - empêche la suppression par d'autres utilisateurs
D) Aucune des réponses précédentes

**Réponse : A**

SUID (Set User ID) fait qu'un fichier s'exécute avec les droits du propriétaire.

---
layout: default
---

# Question 28 - ACL 📋

**Que sont les ACL ?**

A) Access Control Lists - permissions granulaires
B) Advanced Command Line - interface avancée
C) Application Control Layer - couche de contrôle
D) Aucune des réponses précédentes

**Réponse : A**

ACL (Access Control Lists) permettent des permissions granulaires sur les fichiers.

---
layout: default
---

# Question 29 - Monitoring avancé 📊

**Quel outil surveille les performances système en temps réel ?**

A) `htop`
B) `iotop`
C) `nethogs`
D) Toutes les réponses précédentes

**Réponse : D**

Tous ces outils surveillent différents aspects des performances système.

---
layout: default
---

# Question 30 - Sécurité avancée 🔒

**Qu'est-ce que fail2ban ?**

A) Un pare-feu
B) Un outil de détection d'intrusion
C) Un scanner de vulnérabilités
D) Un outil de chiffrement

**Réponse : B**

fail2ban est un outil de détection d'intrusion qui bloque les tentatives d'attaque.

---
layout: default
---

# Question 31 - Scripts avancés 📝

**Quelle option bash arrête le script sur la première erreur ?**

A) `set -e`
B) `set -x`
C) `set -u`
D) `set -o pipefail`

**Réponse : A**

`set -e` arrête le script sur la première erreur.

---
layout: default
---

# Question 32 - Variables shell 📊

**Quelle variable contient le nombre d'arguments passés au script ?**

A) `$0`
B) `$#`
C) `$@`
D) `$*`

**Réponse : B**

`$#` contient le nombre d'arguments passés au script.

---
layout: default
---

# Question 33 - Boucles shell 🔄

**Quelle boucle exécute le code tant qu'une condition est vraie ?**

A) `for`
B) `while`
C) `until`
D) `loop`

**Réponse : B**

La boucle `while` exécute le code tant qu'une condition est vraie.

---
layout: default
---

# Question 34 - Fonctions shell 📦

**Comment déclare-t-on une fonction en bash ?**

A) `function nom()`
B) `nom()`
C) `def nom()`
D) A et B

**Réponse : D**

On peut déclarer une fonction avec `function nom()` ou `nom()`.

---
layout: default
---

# Question 35 - Gestion d'erreurs ⚠️

**Quelle commande vérifie le code de retour de la dernière commande ?**

A) `$?`
B) `$!`
C) `$$`
D) `$0`

**Réponse : A**

`$?` contient le code de retour de la dernière commande exécutée.

---
layout: default
---

# Question 36 - Docker avancé 🐳

**Qu'est-ce qu'un multi-stage build ?**

A) Un build avec plusieurs étapes d'optimisation
B) Un build qui utilise plusieurs images de base
C) Un build qui crée plusieurs images
D) Aucune des réponses précédentes

**Réponse : A**

Un multi-stage build utilise plusieurs étapes pour optimiser la taille de l'image finale.

---
layout: default
---

# Question 37 - Volumes Docker 💾

**Quel type de volume est géré par Docker ?**

A) Volume nommé
B) Bind mount
C) Volume anonyme
D) Toutes les réponses précédentes

**Réponse : D**

Docker gère tous ces types de volumes.

---
layout: default
---

# Question 38 - Réseaux Docker 🌐

**Quel type de réseau Docker isole complètement le conteneur ?**

A) Bridge
B) Host
C) None
D) Overlay

**Réponse : C**

Le réseau `none` isole complètement le conteneur du réseau.

---
layout: default
---

# Question 39 - Ansible Vault 🔐

**Quel outil chiffre les données sensibles dans Ansible ?**

A) `ansible-encrypt`
B) `ansible-vault`
C) `ansible-secure`
D) `ansible-crypt`

**Réponse : B**

`ansible-vault` chiffre les données sensibles dans Ansible.

---
layout: default
---

# Question 40 - Rôles Ansible 🔄

**Quel répertoire contient les tâches d'un rôle Ansible ?**

A) `tasks/`
B) `files/`
C) `templates/`
D) `vars/`

**Réponse : A**

Le répertoire `tasks/` contient les tâches d'un rôle Ansible.

---
layout: default
---

# Question 41 - Templates Jinja2 📝

**Quel langage utilise Ansible pour ses templates ?**

A) ERB
B) Jinja2
C) Handlebars
D) Mustache

**Réponse : B**

Ansible utilise Jinja2 pour ses templates.

---
layout: default
---

# Question 42 - Conditionals Ansible 🔄

**Quelle directive Ansible exécute une tâche sous condition ?**

A) `if`
B) `when`
C) `condition`
D) `if_condition`

**Réponse : B**

La directive `when` exécute une tâche sous condition.

---
layout: default
---

# Question 43 - Loops Ansible 🔄

**Quelle directive Ansible répète une tâche ?**

A) `loop`
B) `repeat`
C) `for`
D) `iterate`

**Réponse : A**

La directive `loop` répète une tâche sur une liste d'éléments.

---
layout: default
---

# Question 44 - Handlers Ansible 📢

**Qu'est-ce qu'un handler Ansible ?**

A) Une tâche qui s'exécute automatiquement
B) Une tâche qui s'exécute seulement si appelée
C) Une tâche qui s'exécute en arrière-plan
D) Une tâche qui s'exécute en parallèle

**Réponse : B**

Un handler est une tâche qui s'exécute seulement si elle est appelée par `notify`.

---
layout: default
---

# Question 45 - Fact Gathering 📊

**Quelle commande Ansible collecte les informations système ?**

A) `ansible-facts`
B) `ansible setup`
C) `ansible gather`
D) `ansible collect`

**Réponse : B**

`ansible setup` collecte les informations système (facts).

---
layout: default
---

# Question 46 - Ansible Galaxy 🌌

**Quel outil gère les rôles Ansible ?**

A) `ansible-role`
B) `ansible-galaxy`
C) `ansible-manager`
D) `ansible-hub`

**Réponse : B**

`ansible-galaxy` gère les rôles Ansible.

---
layout: default
---

# Question 47 - Debug Ansible 🔍

**Quelle option affiche les détails d'exécution Ansible ?**

A) `-v`
B) `-d`
C) `-x`
D) `-t`

**Réponse : A**

L'option `-v` (verbose) affiche les détails d'exécution Ansible.

---
layout: default
---

# Question 48 - Check Mode Ansible ✅

**Quelle option teste un playbook sans l'exécuter ?**

A) `--test`
B) `--check`
C) `--dry-run`
D) `--simulate`

**Réponse : B**

L'option `--check` teste un playbook sans l'exécuter (dry-run).

---
layout: default
---

# Question 49 - Limites Ansible 🎯

**Quelle option limite l'exécution à certains hôtes ?**

A) `--limit`
B) `--hosts`
C) `--target`
D) `--filter`

**Réponse : A**

L'option `--limit` limite l'exécution à certains hôtes.

---
layout: default
---

# Question 50 - Bonnes pratiques 💡

**Quelle est la meilleure pratique pour les mots de passe dans Ansible ?**

A) Les écrire en clair dans les playbooks
B) Utiliser Ansible Vault
C) Les stocker dans des fichiers texte
D) Les utiliser directement en ligne de commande

**Réponse : B**

Ansible Vault est la meilleure pratique pour gérer les mots de passe de manière sécurisée.

---
layout: default
---

# Résultats et Évaluation 📊

### Analyse des résultats

**Critères de réussite :**
- **Excellent** : 45-50 réponses correctes (90-100%)
- **Très bien** : 40-44 réponses correctes (80-89%)
- **Bien** : 35-39 réponses correctes (70-79%)
- **Satisfaisant** : 30-34 réponses correctes (60-69%)
- **À améliorer** : Moins de 30 réponses correctes (<60%)

**Thèmes à approfondir selon les erreurs :**
- **Commandes de base** : Revoir la ligne de commande
- **Permissions** : Étudier les ACL et permissions spéciales
- **Réseaux** : Approfondir la configuration réseau
- **Scripts** : Pratiquer le shell scripting
- **Docker** : Revoir les concepts de conteneurisation
- **Ansible** : S'exercer sur les playbooks et rôles

---
layout: default
---

# Correction et Explications 📚

### Réponses détaillées

**Questions 1-10 : Concepts de base**
- Unix/Linux : Histoire et philosophie
- Commandes essentielles : ls, cd, pwd
- Navigation : Chemins relatifs et absolus
- Permissions : Système octal et symbolique
- Utilisateurs : UID, GID, gestion des comptes

**Questions 11-20 : Système avancé**
- Processus : ps, top, htop, kill
- Réseaux : ping, netstat, ss, ip
- Paquets : apt, dpkg, gestion des dépendances
- Scripts : Shebang, variables, structures
- Virtualisation : KVM, QEMU, conteneurs

**Questions 21-30 : Outils et sécurité**
- Compression : tar, gzip, bzip2
- Recherche : find, grep, locate
- Monitoring : htop, iotop, nethogs
- Sécurité : ufw, fail2ban, audit
- Logs : /var/log, journalctl

---
layout: default
---

# Correction et Explications 📚

### Réponses détaillées (suite)

**Questions 31-40 : Scripts et automatisation**
- Variables shell : $0, $#, $@, $?
- Structures de contrôle : if, for, while
- Fonctions : Déclaration et utilisation
- Gestion d'erreurs : set -e, trap
- Redirection : >, >>, 2>, &>

**Questions 41-50 : Docker et Ansible**
- Docker : Images, conteneurs, volumes, réseaux
- Docker Compose : Services, orchestration
- Ansible : Playbooks, rôles, inventory
- Ansible Vault : Chiffrement des secrets
- Bonnes pratiques : Sécurité, performance

**Points clés à retenir :**
- Comprendre les concepts fondamentaux
- Pratiquer régulièrement les commandes
- Sécuriser les environnements
- Automatiser les tâches répétitives
- Maintenir une veille technologique

---
layout: default
---

# Ressources de Formation Continue 📚

### Développement professionnel

**Certifications recommandées :**
- **Linux Foundation** : LFCS, LFCE
- **Red Hat** : RHCSA, RHCE
- **Docker** : Docker Certified Associate
- **Ansible** : Red Hat Certified Specialist

**Livres de référence :**
- "The Linux Command Line" - William Shotts
- "UNIX and Linux System Administration Handbook"
- "Docker in Action" - Jeff Nickoloff
- "Ansible: Up and Running" - Lorin Hochstein

**Ressources en ligne :**
- **Documentation officielle** : man pages, --help
- **Tutoriels** : DigitalOcean, Linode, AWS
- **Communautés** : Stack Overflow, Reddit r/linux
- **Podcasts** : Linux Unplugged, The Linux Action Show

---
layout: default
---

# Prochaines Étapes 🎯

### Évolution professionnelle

**Compétences à développer :**
- **Kubernetes** : Orchestration de conteneurs
- **Terraform** : Infrastructure as Code
- **CI/CD** : Jenkins, GitLab CI, GitHub Actions
- **Cloud** : AWS, Azure, Google Cloud
- **Monitoring** : Prometheus, Grafana, ELK Stack

**Projets pratiques :**
- **Infrastructure personnelle** : Serveur NAS, VPN
- **Contributions open source** : GitHub, GitLab
- **Blog technique** : Partager ses connaissances
- **Meetups** : Rencontrer la communauté

**Formation continue :**
- **Cours en ligne** : Coursera, edX, Linux Academy
- **Conférences** : FOSDEM, LinuxCon, DockerCon
- **Labs virtuels** : Katacoda, Play with Docker
- **Certifications** : Préparer les examens officiels

---
layout: default
---

# Conclusion et Perspectives 🌟

### Bilan de la formation

**Acquis de compétences :**
- ✅ Maîtrise de la ligne de commande Linux
- ✅ Gestion des utilisateurs et permissions
- ✅ Configuration réseau et services
- ✅ Automatisation avec scripts shell
- ✅ Conteneurisation avec Docker
- ✅ Infrastructure as Code avec Ansible
- ✅ Projet final intégrateur

**Compétences transversales :**
- 🔍 Résolution de problèmes
- 📚 Documentation et partage
- 🔒 Sécurité et bonnes pratiques
- 🤝 Travail en équipe
- 📊 Monitoring et observabilité

**Perspectives professionnelles :**
- **Administrateur système** Linux
- **DevOps Engineer**
- **Site Reliability Engineer (SRE)**
- **Cloud Engineer**
- **Infrastructure Engineer**

---
layout: default
---

# Félicitations ! 🎉

### Formation Unix/Linux - Initiation terminée

**Vous avez maintenant :**
- Une solide base en administration Linux
- Des compétences en automatisation
- Une expérience pratique avec Docker et Ansible
- Un projet complet d'infrastructure
- Une validation de vos connaissances

**Continuez à :**
- Pratiquer régulièrement
- Suivre l'évolution technologique
- Participer à la communauté
- Développer vos compétences
- Préparer vos certifications

**Merci d'avoir suivi cette formation !**
**Bonne continuation dans votre parcours Linux ! 🐧**

---
src: './pages/01-introduction-unix-linux.md'
--- 