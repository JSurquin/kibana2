---

# QCM Module 7 : Réseau ✅

**10 questions sur la configuration et le diagnostic réseau**

---

# Question 1

Quelle adresse IP est localhost ?

A) `192.168.1.1`

B) `127.0.0.1`

C) `10.0.0.1`

D) `0.0.0.0`

---

# Question 2

Quel port utilise SSH par défaut ?

A) 21

B) 22

C) 23

D) 80

---

# Question 3

Quelle commande teste la connectivité réseau ?

A) `test`

B) `ping`

C) `connect`

D) `check`

---

# Question 4

Quelle commande affiche la configuration IP ?

A) `ipconfig`

B) `ifconfig`

C) `ip addr`

D) B et C sont corrects

---

# Question 5

Quelle commande affiche les connexions actives ?

A) `connections`

B) `netstat` ou `ss`

C) `active`

D) `ports`

---

# Question 6

Quel outil génère une paire de clés SSH ?

A) `ssh-key`

B) `ssh-keygen`

C) `keygen`

D) `ssh-create`

---

# Question 7

Où sont stockées les clés SSH ?

A) `/etc/ssh/`

B) `~/.ssh/`

C) `/var/ssh/`

D) `/root/ssh/`

---

# Question 8

Quel pare-feu est simple à utiliser sur Ubuntu ?

A) `iptables`

B) `ufw`

C) `firewalld`

D) `firewall`

---

# Question 9

Comment copier un fichier via SSH ?

A) `copy`

B) `scp`

C) `ssh-copy`

D) `transfer`

---

# Question 10

Quelle commande résout un nom de domaine en IP ?

A) `resolve`

B) `nslookup` ou `dig`

C) `dns`

D) `lookup`

---

# Réponses du Module 7 📝

**Réponse 1 :** B) `127.0.0.1`
- Toute la plage 127.0.0.0/8 est réservée au loopback

**Réponse 2 :** B) 22
- Port 22 = SSH, Port 21 = FTP, Port 23 = Telnet, Port 80 = HTTP

**Réponse 3 :** B) `ping`
- `ping google.com` teste si le serveur est joignable

**Réponse 4 :** D) B et C sont corrects
- `ip addr` est moderne, `ifconfig` est ancien mais toujours utilisé

**Réponse 5 :** B) `netstat` ou `ss`
- `ss` est la version moderne de `netstat`

---

# Réponses du Module 7 (suite) 📝

**Réponse 6 :** B) `ssh-keygen`
- `ssh-keygen -t ed25519` est recommandé en 2025

**Réponse 7 :** B) `~/.ssh/`
- Répertoire personnel de chaque utilisateur

**Réponse 8 :** B) `ufw`
- ufw = Uncomplicated Firewall (interface simple pour iptables)

**Réponse 9 :** B) `scp`
- `scp fichier user@serveur:/chemin/`

**Réponse 10 :** B) `nslookup` ou `dig`
- `dig` est plus moderne et détaillé

---

# Score du Module 7 📊

Comptez vos bonnes réponses :

- **9-10** : Excellent ! ⭐⭐⭐
- **7-8** : Bien ! ⭐⭐
- **5-6** : À revoir ⭐
- **< 5** : Relire le module 5

---
