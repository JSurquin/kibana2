---

# QCM Module 7 : Réseau ✅

**20 questions sur la configuration et le diagnostic réseau**

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

Où sont stockées les clés SSH de l'utilisateur ?

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

# Question 11

Que signifie la notation CIDR /24 ?

A) 24 machines dans le réseau

B) Masque 255.255.255.0 (256 adresses)

C) 24 ports ouverts

D) 24 sous-réseaux

---

# Question 12

Quelle option SSH crée un tunnel local (Local Forwarding) ?

A) `-R`

B) `-L`

C) `-D`

D) `-T`

---

# Question 13

Quel fichier contient les serveurs DNS sur Linux ?

A) `/etc/dns.conf`

B) `/etc/resolv.conf`

C) `/etc/network/dns`

D) `/etc/dns/servers`

---

# Question 14

Quelle commande synchronise des fichiers via SSH avec reprise ?

A) `scp`

B) `sftp`

C) `rsync`

D) `ssh-sync`

---

# Question 15

Quel algorithme de clé SSH est recommandé en 2025 ?

A) RSA 1024

B) DSA

C) Ed25519

D) ECDSA 256

---

# Question 16

Que fait la commande `traceroute` ?

A) Teste la vitesse de connexion

B) Affiche le chemin réseau vers un hôte

C) Liste les interfaces réseau

D) Scanne les ports ouverts

---

# Question 17

Quel outil configure le réseau sur Ubuntu moderne (22.04+) ?

A) ifconfig

B) network-scripts

C) Netplan

D) wicked

---

# Question 18

Quel fichier permet de définir des alias DNS locaux ?

A) `/etc/hosts`

B) `/etc/aliases`

C) `/etc/dns/local`

D) `/etc/hostname`

---

# Question 19

Quelle est la plage de ports "bien connus" (well-known) ?

A) 1-100

B) 0-1023

C) 1024-49151

D) 49152-65535

---

# Question 20

Quel service attribue automatiquement des IP aux clients ?

A) DNS

B) NAT

C) DHCP

D) ARP

---

# Réponses du Module 7 📝 (1/4)

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

# Réponses du Module 7 📝 (2/4)

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

# Réponses du Module 7 📝 (3/4)

**Réponse 11 :** B) Masque 255.255.255.0 (256 adresses)
- /24 signifie 24 bits pour le réseau, 8 bits pour les hôtes (2^8 = 256)

**Réponse 12 :** B) `-L`
- `-L` = Local forwarding (ramène un port distant vers local)
- `-R` = Remote forwarding, `-D` = Dynamic (proxy SOCKS)

**Réponse 13 :** B) `/etc/resolv.conf`
- Contient les serveurs DNS avec `nameserver 8.8.8.8`

**Réponse 14 :** C) `rsync`
- `rsync -avz` synchronise avec compression et reprise en cas d'interruption

**Réponse 15 :** C) Ed25519
- Ed25519 est le plus moderne, sûr et performant. RSA 4096 reste acceptable.

---

# Réponses du Module 7 📝 (4/4)

**Réponse 16 :** B) Affiche le chemin réseau vers un hôte
- Montre chaque routeur traversé entre vous et la destination

**Réponse 17 :** C) Netplan
- Netplan est l'outil standard sur Ubuntu 18.04+ pour configurer le réseau

**Réponse 18 :** A) `/etc/hosts`
- Permet de définir des correspondances nom → IP localement

**Réponse 19 :** B) 0-1023
- Well-known ports (réservés aux services système), nécessitent root

**Réponse 20 :** C) DHCP
- Dynamic Host Configuration Protocol : attribution automatique d'IP, masque, gateway, DNS

---

# Score du Module 7 📊

Comptez vos bonnes réponses :

- **18-20** : Expert réseau ! ⭐⭐⭐
- **14-17** : Très bien ! ⭐⭐
- **10-13** : Bien, à consolider ⭐
- **< 10** : Relire le module 7
