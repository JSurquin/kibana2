---

# QCM Module 9 : Sécurité système ✅

**10 questions sur la sécurisation du système**

---

# Question 1

Qu'est-ce que SELinux ?

A) Un gestionnaire de paquets
B) Un système de contrôle d'accès obligatoire (MAC)
C) Un pare-feu
D) Un éditeur de texte

---

# Question 2

Quel outil bloque les IP après plusieurs échecs de connexion ?

A) `iptables`
B) `fail2ban`
C) `ufw`
D) `firewalld`

---

# Question 3

Où sont stockés les mots de passe chiffrés ?

A) `/etc/passwd`
B) `/etc/shadow`
C) `/etc/security`
D) `/etc/passwords`

---

# Question 4

Quelle commande désactive temporairement SELinux ?

A) `selinux disable`
B) `setenforce 0`
C) `systemctl stop selinux`
D) `disable selinux`

---

# Question 5

Quel fichier configure sudo ?

A) `/etc/sudo`
B) `/etc/sudoers`
C) `/etc/sudo.conf`
D) `/etc/security/sudo`

---

# Question 6

Qu'est-ce que fail2ban surveille ?

A) Les performances système
B) Les logs pour détecter des tentatives d'intrusion
C) L'espace disque
D) Les processus

---

# Question 7

Quelle commande affiche les tentatives de connexion échouées ?

A) `lastlog`
B) `last`
C) `lastb`
D) `faillog`

---

# Question 8

Quelle est la meilleure pratique pour SSH ?

A) Utiliser des mots de passe forts
B) Utiliser des clés SSH
C) Changer le port
D) Tout ce qui précède

---

# Question 9

Que fait `auditd` ?

A) Optimise les performances
B) Audite les actions système
C) Nettoie le système
D) Sauvegarde les fichiers

---

# Question 10

Comment chiffrer une partition avec Linux ?

A) `encrypt`
B) `LUKS`
C) `cryptfs`
D) `secure`

---

# Réponses du Module 9 📝

**Réponse 1 :** B) Un système de contrôle d'accès obligatoire (MAC)
- SELinux ajoute une couche de sécurité au-delà des permissions Unix

**Réponse 2 :** B) `fail2ban`
- Protection contre le brute-force

**Réponse 3 :** B) `/etc/shadow`
- Accessible uniquement par root

**Réponse 4 :** B) `setenforce 0`
- 0 = permissive, 1 = enforcing

**Réponse 5 :** B) `/etc/sudoers`
- À éditer UNIQUEMENT avec `visudo`

---

# Réponses du Module 9 (suite) 📝

**Réponse 6 :** B) Les logs pour détecter des tentatives d'intrusion
- Bannit les IPs suspectes temporairement

**Réponse 7 :** C) `lastb`
- `last` affiche les connexions réussies

**Réponse 8 :** D) Tout ce qui précède
- Defense en profondeur : plusieurs couches de sécurité

**Réponse 9 :** B) Audite les actions système
- Enregistre qui fait quoi et quand

**Réponse 10 :** B) `LUKS`
- LUKS = Linux Unified Key Setup

---

# Score du Module 9 📊

Comptez vos bonnes réponses :

- **9-10** : Excellent ! ⭐⭐⭐
- **7-8** : Bien ! ⭐⭐
- **5-6** : À revoir ⭐
- **< 5** : Relire le module 8

---
