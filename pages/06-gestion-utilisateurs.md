---
layout: intro
routeAlias: 'gestion-utilisateurs'
---

# Gestion des utilisateurs 👥

### Administration des comptes utilisateurs et groupes

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Gérons les utilisateurs ensemble <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Concepts de base 🔐

### Utilisateurs et groupes dans Unix/Linux

**Utilisateur :**
- Compte d'accès au système
- Identifié par un nom d'utilisateur (username)
- Possède un UID (User ID) unique
- A un répertoire home personnel

**Groupe :**
- Collection d'utilisateurs
- Identifié par un nom de groupe
- Possède un GID (Group ID) unique
- Permet de partager des ressources

**Types d'utilisateurs :**
- **root** : Super utilisateur (UID 0)
- **Utilisateurs système** : Services et daemons
- **Utilisateurs normaux** : Utilisateurs finaux

---
layout: default
---

# Fichiers de configuration 📄

### Stockage des informations utilisateurs

**/etc/passwd**
```bash
# Format : username:password:UID:GID:comment:home:shell
root:x:0:0:root:/root:/bin/bash
user1:x:1000:1000:User One:/home/user1:/bin/bash
```

**/etc/shadow**
```bash
# Mots de passe chiffrés
root:$6$...:18000:0:99999:7:::
user1:$6$...:18000:0:99999:7:::
```

**/etc/group**
```bash
# Format : groupname:password:GID:members
root:x:0:
user1:x:1000:user1
sudo:x:27:user1
```

**/etc/gshadow**
```bash
# Informations de sécurité des groupes
root:!::
user1:!::
sudo:!:user1
```

---
layout: default
---

# Commandes de gestion des utilisateurs 👤

### Créer et gérer les utilisateurs

**useradd (ajouter un utilisateur)**
```bash
useradd nom_utilisateur              # Créer un utilisateur
useradd -m nom_utilisateur           # Créer avec répertoire home
useradd -s /bin/bash nom_utilisateur # Spécifier le shell
useradd -u 1001 nom_utilisateur      # Spécifier l'UID
useradd -g groupe nom_utilisateur    # Groupe principal
useradd -G groupe1,groupe2 nom_utilisateur # Groupes secondaires
```

**usermod (modifier un utilisateur)**
```bash
usermod -s /bin/zsh nom_utilisateur  # Changer le shell
usermod -d /nouveau/home nom_utilisateur # Changer le home
usermod -L nom_utilisateur           # Verrouiller le compte
usermod -U nom_utilisateur           # Déverrouiller le compte
```

---
layout: default
---

# Commandes de gestion des utilisateurs 👤

### Supprimer et gérer les mots de passe

**userdel (supprimer un utilisateur)**
```bash
userdel nom_utilisateur              # Supprimer l'utilisateur
userdel -r nom_utilisateur           # Supprimer avec le home
userdel -f nom_utilisateur           # Forcer la suppression
```

**passwd (gérer les mots de passe)**
```bash
passwd                              # Changer son mot de passe
passwd nom_utilisateur              # Changer le mot de passe d'un utilisateur
passwd -l nom_utilisateur           # Verrouiller le compte
passwd -u nom_utilisateur           # Déverrouiller le compte
passwd -e nom_utilisateur           # Forcer le changement au prochain login
```

**chage (changer les informations d'expiration)**
```bash
chage -l nom_utilisateur            # Lister les informations
chage -M 90 nom_utilisateur         # Expiration dans 90 jours
chage -W 7 nom_utilisateur          # Avertissement 7 jours avant
```

---
layout: default
---

# Commandes de gestion des groupes 👥

### Créer et gérer les groupes

**groupadd (ajouter un groupe)**
```bash
groupadd nom_groupe                  # Créer un groupe
groupadd -g 1001 nom_groupe         # Spécifier le GID
groupadd -r nom_groupe              # Groupe système
```

**groupmod (modifier un groupe)**
```bash
groupmod -n nouveau_nom ancien_nom  # Renommer un groupe
groupmod -g 1002 nom_groupe         # Changer le GID
```

**groupdel (supprimer un groupe)**
```bash
groupdel nom_groupe                  # Supprimer un groupe
```

**gpasswd (gérer les membres du groupe)**
```bash
gpasswd -a utilisateur groupe       # Ajouter un membre
gpasswd -d utilisateur groupe       # Supprimer un membre
gpasswd -M user1,user2,user3 groupe # Définir la liste des membres
```

---
layout: default
---

# Commandes d'information 📊

### Obtenir des informations sur les utilisateurs

**id (identité de l'utilisateur)**
```bash
id                                # Mon identité
id nom_utilisateur                # Identité d'un utilisateur
id -u nom_utilisateur             # Seulement l'UID
id -g nom_utilisateur             # Seulement le GID principal
id -G nom_utilisateur             # Tous les groupes
id -n nom_utilisateur             # Noms au lieu des IDs
```

**who et w**
```bash
who                               # Utilisateurs connectés
who am i                          # Mon utilisateur
w                                 # Utilisateurs et leurs processus
w -h                              # Sans en-tête
```

**last et lastlog**
```bash
last                              # Historique des connexions
last nom_utilisateur              # Connexions d'un utilisateur
lastlog                           # Dernière connexion de tous les utilisateurs
lastlog -u nom_utilisateur        # Dernière connexion d'un utilisateur
```

---
layout: default
---

# Gestion des permissions avancées 🔐

### sudo et su

**sudo (Substitute User DO)**
```bash
sudo commande                      # Exécuter une commande en tant que root
sudo -u utilisateur commande       # Exécuter en tant qu'un autre utilisateur
sudo -i                           # Shell root interactif
sudo -s                           # Shell root non-interactif
sudo -l                           # Lister les permissions sudo
```

**su (Switch User)**
```bash
su                                # Changer vers root
su -                              # Changer vers root avec environnement
su utilisateur                    # Changer vers un utilisateur
su - utilisateur                  # Changer avec environnement
```

**Configuration sudo (/etc/sudoers)**
```bash
# Syntaxe : utilisateur hôte=(utilisateur_cible) commandes
user1 ALL=(ALL) ALL              # Toutes les commandes
user1 ALL=(ALL) NOPASSWD: ALL    # Sans mot de passe
user1 ALL=(ALL) /usr/bin/apt     # Seulement apt
%sudo ALL=(ALL) ALL              # Groupe sudo
```

---
layout: default
---

# Sécurité des utilisateurs 🛡️

### Bonnes pratiques de sécurité

**Politique de mots de passe**
```bash
# Configuration dans /etc/login.defs
PASS_MAX_DAYS 90                 # Expiration après 90 jours
PASS_MIN_DAYS 1                  # Minimum 1 jour entre changements
PASS_WARN_AGE 7                  # Avertissement 7 jours avant
PASS_MIN_LEN 8                   # Longueur minimale 8 caractères
```

**Verrouillage de comptes**
```bash
passwd -l utilisateur             # Verrouiller un compte
usermod -L utilisateur            # Alternative
usermod -e 1 utilisateur          # Expiration immédiate
```

**Surveillance des connexions**
```bash
last                              # Historique des connexions
lastb                             # Tentatives de connexion échouées
faillog -a                        # Échecs de connexion
```

---
layout: default
---

# Gestion des quotas 📊

### Limiter l'utilisation de l'espace disque

**Configuration des quotas**
```bash
# Activer les quotas sur le système de fichiers
mount -o usrquota,grpquota /dev/sda1 /home

# Initialiser les quotas
quotacheck -cugm /home
quotaon -av

# Éditer les quotas pour un utilisateur
edquota -u utilisateur
```

**Commandes de quota**
```bash
quota -u utilisateur              # Voir les quotas utilisateur
quota -g groupe                   # Voir les quotas groupe
repquota -a                       # Rapport global
warnquota                         # Envoyer des avertissements
```

**Limites configurables**
```bash
# Dans edquota
Filesystem  blocks  soft  hard  inodes  soft  hard
/dev/sda1   1000    2000  3000    100    200   300
# blocks = utilisation actuelle
# soft = limite douce
# hard = limite dure
```

---
layout: default
---

# Gestion des profils utilisateur 👤

### Personnalisation des environnements

**Fichiers de profil**
```bash
~/.bashrc                        # Configuration bash pour chaque shell
~/.bash_profile                   # Configuration bash pour login
~/.profile                        # Configuration générale
~/.bash_logout                    # Actions à la déconnexion
```

**Variables d'environnement**
```bash
export PATH=$PATH:/chemin/ajoute  # Ajouter au PATH
export EDITOR=vim                 # Éditeur par défaut
export LANG=fr_FR.UTF-8          # Langue
export HISTSIZE=1000              # Taille de l'historique
```

**Alias et fonctions**
```bash
# Dans ~/.bashrc
alias ll='ls -la'
alias la='ls -A'
alias ..='cd ..'
alias h='history'

# Fonctions personnalisées
backup() {
    tar -czf backup_$(date +%Y%m%d).tar.gz "$1"
}
```

---
layout: default
---

# Gestion des clés SSH 🔑

### Authentification par clés

**Générer une paire de clés**
```bash
ssh-keygen -t rsa -b 4096         # Clé RSA 4096 bits
ssh-keygen -t ed25519              # Clé Ed25519 (recommandée)
ssh-keygen -f ~/.ssh/id_rsa        # Spécifier le fichier
```

**Copier la clé publique**
```bash
ssh-copy-id utilisateur@serveur    # Copier automatiquement
cat ~/.ssh/id_rsa.pub | ssh utilisateur@serveur "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

**Configuration SSH**
```bash
# Dans ~/.ssh/config
Host serveur
    HostName 192.168.1.100
    User utilisateur
    Port 22
    IdentityFile ~/.ssh/id_rsa
```

**Permissions sécurisées**
```bash
chmod 700 ~/.ssh                   # Répertoire SSH
chmod 600 ~/.ssh/id_rsa            # Clé privée
chmod 644 ~/.ssh/id_rsa.pub        # Clé publique
chmod 600 ~/.ssh/authorized_keys   # Clés autorisées
```

---
layout: default
---

# Gestion des utilisateurs système 🔧

### Utilisateurs pour les services

**Créer un utilisateur système**
```bash
useradd -r -s /bin/false service_user  # Utilisateur système sans shell
useradd -r -d /var/lib/service service_user  # Répertoire spécifique
```

**Utilisateurs courants**
```bash
# Utilisateurs système typiques
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
mysql:x:127:127:MySQL Server:/var/lib/mysql:/bin/false
postgres:x:128:128:PostgreSQL:/var/lib/postgresql:/bin/bash
```

**Gestion des permissions de service**
```bash
# Donner l'accès à un service
usermod -aG service_group utilisateur
chown -R service_user:service_group /chemin/service
chmod 755 /chemin/service
```

---
layout: default
---

# Monitoring des utilisateurs 📈

### Surveiller l'activité utilisateur

**Commandes de surveillance**
```bash
who                               # Utilisateurs connectés
w                                 # Utilisateurs et processus
ps aux | grep utilisateur          # Processus d'un utilisateur
lsof -u utilisateur               # Fichiers ouverts par un utilisateur
```

**Logs de connexion**
```bash
tail -f /var/log/auth.log         # Suivre les connexions
grep "session opened" /var/log/auth.log  # Sessions ouvertes
grep "session closed" /var/log/auth.log  # Sessions fermées
```

**Surveillance des ressources**
```bash
du -sh /home/*                    # Utilisation disque par utilisateur
quota -u utilisateur              # Quotas utilisateur
ps -eo user,pid,ppid,cmd --sort=-%mem | head  # Top mémoire par utilisateur
```

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique de la gestion utilisateurs

**Exercice 1 : Créer des utilisateurs**
```bash
# Créer un utilisateur avec répertoire home
sudo useradd -m -s /bin/bash john
sudo passwd john

# Créer un groupe
sudo groupadd developers

# Ajouter l'utilisateur au groupe
sudo usermod -aG developers john
```

**Exercice 2 : Gestion des permissions**
```bash
# Créer un répertoire partagé
sudo mkdir /shared
sudo chown root:developers /shared
sudo chmod 775 /shared

# Tester l'accès
su - john
cd /shared
touch test.txt
```

**Exercice 3 : Configuration sudo**
```bash
# Ajouter un utilisateur au groupe sudo
sudo usermod -aG sudo john

# Tester sudo
su - john
sudo apt update
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour la gestion des utilisateurs

**Sécurité**
- Utiliser des mots de passe forts
- Limiter les privilèges sudo
- Surveiller les connexions
- Verrouiller les comptes inactifs
- Utiliser l'authentification par clés SSH

**Organisation**
- Créer des groupes logiques
- Utiliser des conventions de nommage
- Documenter les permissions
- Faire des sauvegardes des configurations

**Maintenance**
- Réviser régulièrement les permissions
- Nettoyer les comptes inactifs
- Mettre à jour les mots de passe
- Surveiller l'utilisation des ressources

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Permissions et sécurité** avancées
2. **Processus et services** système
3. **Réseaux et connectivité**
4. **Paquets logiciels** et gestion
5. **Shell scripting** de base

**Préparation :**
- Pratiquez la création d'utilisateurs
- Familiarisez-vous avec sudo
- Testez les permissions de groupe

---
src: './pages/07-permissions-securite.md'
--- 