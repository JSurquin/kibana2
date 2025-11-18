---

# QCM Module 2 : Gestion utilisateurs ✅

**10 questions sur les utilisateurs, groupes et permissions**

---

# Question 1

Quel fichier contient les informations des utilisateurs ?

A) `/etc/users`
B) `/etc/passwd`
C) `/etc/shadow`
D) `/etc/accounts`

---

# Réponse 1

**Bonne réponse : B) `/etc/passwd`**

`/etc/passwd` contient les informations utilisateurs (sauf les mots de passe qui sont dans `/etc/shadow`).

---

# Question 2

Quelle commande crée un nouvel utilisateur ?

A) `adduser` ou `useradd`
B) `newuser`
C) `createuser`
D) `mkuser`

---

# Réponse 2

**Bonne réponse : A) `adduser` ou `useradd`**

`useradd` est bas niveau, `adduser` est interactif (Debian/Ubuntu).

---

# Question 3

Que signifie `chmod 644 fichier.txt` ?

A) rw-r--r--
B) rwxr--r--
C) rw-rw-r--
D) r--r--r--

---

# Réponse 3

**Bonne réponse : A) rw-r--r--**

6 = rw- (propriétaire), 4 = r-- (groupe), 4 = r-- (autres).

---

# Question 4

Quelle option de `usermod` ajoute un utilisateur à un groupe ?

A) `-g`
B) `-G`
C) `-aG`
D) `-a`

---

# Réponse 4

**Bonne réponse : C) `-aG`**

`-aG` (append Group) ajoute sans supprimer les groupes existants.

---

# Question 5

Que fait la commande `sudo` ?

A) Connecte en tant que root
B) Exécute une commande avec les privilèges root
C) Crée un nouvel utilisateur
D) Change de groupe

---

# Réponse 5

**Bonne réponse : B) Exécute une commande avec les privilèges root**

`sudo` permet d'élever les privilèges temporairement.

---

# Question 6

Quel fichier configure les droits sudo ?

A) `/etc/sudo`
B) `/etc/sudoers`
C) `/etc/sudo.conf`
D) `/etc/security/sudo`

---

# Réponse 6

**Bonne réponse : B) `/etc/sudoers`**

À éditer UNIQUEMENT avec `visudo`.

---

# Question 7

Que fait `umask 0022` ?

A) Définit les permissions par défaut des nouveaux fichiers
B) Masque les fichiers cachés
C) Crypte les fichiers
D) Change le propriétaire

---

# Réponse 7

**Bonne réponse : A) Définit les permissions par défaut des nouveaux fichiers**

umask soustrait des permissions : 666 - 022 = 644 pour les fichiers.

---

# Question 8

Comment changer le groupe d'un fichier ?

A) `chmod`
B) `chown`
C) `chgrp`
D) B et C sont corrects

---

# Réponse 8

**Bonne réponse : D) B et C sont corrects**

`chgrp groupe fichier` ou `chown :groupe fichier`.

---

# Question 9

Que fait la commande `passwd` ?

A) Affiche les mots de passe
B) Change le mot de passe
C) Crypte les fichiers
D) Liste les utilisateurs

---

# Réponse 9

**Bonne réponse : B) Change le mot de passe**

`passwd` change votre mot de passe, `sudo passwd user` change celui d'un autre utilisateur.

---

# Question 10

Quelle commande liste les groupes d'un utilisateur ?

A) `groups utilisateur`
B) `id utilisateur`
C) `whoami`
D) A et B sont corrects

---

# Réponse 10

**Bonne réponse : D) A et B sont corrects**

`groups` et `id` affichent les appartenances aux groupes.

---

# Score du Module 2 📊

Comptez vos bonnes réponses :

- **9-10** : Excellent ! ⭐⭐⭐
- **7-8** : Bien ! ⭐⭐
- **5-6** : À revoir ⭐
- **< 5** : Relire le module 2

---

