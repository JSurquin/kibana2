---

# QCM Module 8 : Sécurité système ✅

**10 questions sur la sécurisation du système**

---

# Question 1

Qu'est-ce que SELinux ?

A) Un gestionnaire de paquets
B) Un système de contrôle d'accès obligatoire (MAC)
C) Un pare-feu
D) Un éditeur de texte

---

# Réponse 1

**Bonne réponse : B) Un système de contrôle d'accès obligatoire (MAC)**

SELinux ajoute une couche de sécurité au-delà des permissions Unix.

---

# Question 2

Quel outil bloque les IP après plusieurs échecs de connexion ?

A) `iptables`
B) `fail2ban`
C) `ufw`
D) `firewalld`

---

# Réponse 2

**Bonne réponse : B) `fail2ban`**

Protection contre le brute-force.

---

# Question 3

Où sont stockés les mots de passe chiffrés ?

A) `/etc/passwd`
B) `/etc/shadow`
C) `/etc/security`
D) `/etc/passwords`

---

# Réponse 3

**Bonne réponse : B) `/etc/shadow`**

Accessible uniquement par root.

---

# Question 4

Quelle commande désactive temporairement SELinux ?

A) `selinux disable`
B) `setenforce 0`
C) `systemctl stop selinux`
D) `disable selinux`

---

# Réponse 4

**Bonne réponse : B) `setenforce 0`**

0 = permissive, 1 = enforcing.

---

# Question 5

Quel fichier configure sudo ?

A) `/etc/sudo`
B) `/etc/sudoers`
C) `/etc/sudo.conf`
D) `/etc/security/sudo`

---

# Réponse 5

**Bonne réponse : B) `/etc/sudoers`**

À éditer UNIQUEMENT avec `visudo`.

---

# Question 6

Qu'est-ce que fail2ban surveille ?

A) Les performances système
B) Les logs pour détecter des tentatives d'intrusion
C) L'espace disque
D) Les processus

---

# Réponse 6

**Bonne réponse : B) Les logs pour détecter des tentatives d'intrusion**

Bannit les IPs suspectes temporairement.

---

# Question 7

Quelle commande affiche les tentatives de connexion échouées ?

A) `lastlog`
B) `last`
C) `lastb`
D) `faillog`

---

# Réponse 7

**Bonne réponse : C) `lastb`**

`last` affiche les connexions réussies.

---

# Question 8

Quelle est la meilleure pratique pour SSH ?

A) Utiliser des mots de passe forts
B) Utiliser des clés SSH
C) Changer le port
D) Tout ce qui précède

---

# Réponse 8

**Bonne réponse : D) Tout ce qui précède**

Defense en profondeur : plusieurs couches de sécurité.

---

# Question 9

Que fait `auditd` ?

A) Optimise les performances
B) Audite les actions système
C) Nettoie le système
D) Sauvegarde les fichiers

---

# Réponse 9

**Bonne réponse : B) Audite les actions système**

Enregistre qui fait quoi et quand.

---

# Question 10

Comment chiffrer une partition avec Linux ?

A) `encrypt`
B) `LUKS`
C) `cryptfs`
D) `secure`

---

# Réponse 10

**Bonne réponse : B) `LUKS`**

LUKS = Linux Unified Key Setup.

---

# Score du Module 8 📊

Comptez vos bonnes réponses :

- **9-10** : Excellent ! ⭐⭐⭐
- **7-8** : Bien ! ⭐⭐
- **5-6** : À revoir ⭐
- **< 5** : Relire le module 8

---

