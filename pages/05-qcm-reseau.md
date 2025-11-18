---

# QCM Module 5 : Réseau ✅

**10 questions sur la configuration et le diagnostic réseau**

---

# Question 1

Quelle adresse IP est localhost ?

A) `192.168.1.1`
B) `127.0.0.1`
C) `10.0.0.1`
D) `0.0.0.0`

---

# Réponse 1

**Bonne réponse : B) `127.0.0.1`**

Toute la plage 127.0.0.0/8 est réservée au loopback.

---

# Question 2

Quel port utilise SSH par défaut ?

A) 21
B) 22
C) 23
D) 80

---

# Réponse 2

**Bonne réponse : B) 22**

Port 22 = SSH, Port 21 = FTP, Port 23 = Telnet, Port 80 = HTTP.

---

# Question 3

Quelle commande teste la connectivité réseau ?

A) `test`
B) `ping`
C) `connect`
D) `check`

---

# Réponse 3

**Bonne réponse : B) `ping`**

`ping google.com` teste si le serveur est joignable.

---

# Question 4

Quelle commande affiche la configuration IP ?

A) `ipconfig`
B) `ifconfig`
C) `ip addr`
D) B et C sont corrects

---

# Réponse 4

**Bonne réponse : D) B et C sont corrects**

`ip addr` est moderne, `ifconfig` est ancien mais toujours utilisé.

---

# Question 5

Quelle commande affiche les connexions actives ?

A) `connections`
B) `netstat` ou `ss`
C) `active`
D) `ports`

---

# Réponse 5

**Bonne réponse : B) `netstat` ou `ss`**

`ss` est la version moderne de `netstat`.

---

# Question 6

Quel outil génère une paire de clés SSH ?

A) `ssh-key`
B) `ssh-keygen`
C) `keygen`
D) `ssh-create`

---

# Réponse 6

**Bonne réponse : B) `ssh-keygen`**

`ssh-keygen -t ed25519` est recommandé en 2025.

---

# Question 7

Où sont stockées les clés SSH ?

A) `/etc/ssh/`
B) `~/.ssh/`
C) `/var/ssh/`
D) `/root/ssh/`

---

# Réponse 7

**Bonne réponse : B) `~/.ssh/`**

Répertoire personnel de chaque utilisateur.

---

# Question 8

Quel pare-feu est simple à utiliser sur Ubuntu ?

A) `iptables`
B) `ufw`
C) `firewalld`
D) `firewall`

---

# Réponse 8

**Bonne réponse : B) `ufw`**

ufw = Uncomplicated Firewall (interface simple pour iptables).

---

# Question 9

Comment copier un fichier via SSH ?

A) `copy`
B) `scp`
C) `ssh-copy`
D) `transfer`

---

# Réponse 9

**Bonne réponse : B) `scp`**

`scp fichier user@serveur:/chemin/`

---

# Question 10

Quelle commande résout un nom de domaine en IP ?

A) `resolve`
B) `nslookup` ou `dig`
C) `dns`
D) `lookup`

---

# Réponse 10

**Bonne réponse : B) `nslookup` ou `dig`**

`dig` est plus moderne et détaillé.

---

# Score du Module 5 📊

Comptez vos bonnes réponses :

- **9-10** : Excellent ! ⭐⭐⭐
- **7-8** : Bien ! ⭐⭐
- **5-6** : À revoir ⭐
- **< 5** : Relire le module 5

---

