---
layout: intro
routeAlias: 'permissions-securite'
---

# Permissions et Sécurité 🔒

### Gestion avancée des permissions et sécurité système

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Sécurisons le système ensemble <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Système de permissions Unix 🔐

### Comprendre les permissions de base

**Structure des permissions :**
```bash
-rw-r--r-- 1 user group 1234 Jan 1 12:00 fichier
# Type | Owner | Group | Others
```

**Types de permissions :**
- **r** (read) : lecture (4)
- **w** (write) : écriture (2)
- **x** (execute) : exécution (1)

**Pour les fichiers :**
- **r** : lire le contenu
- **w** : modifier le contenu
- **x** : exécuter le fichier

**Pour les répertoires :**
- **r** : lister le contenu
- **w** : créer/supprimer des fichiers
- **x** : accéder au répertoire

---
layout: default
---

# Permissions spéciales 🎯

### Permissions avancées du système

**SUID (Set User ID)**
```bash
chmod u+s fichier              # SUID sur propriétaire
chmod 4755 fichier             # SUID avec permissions 755
# Le fichier s'exécute avec les droits du propriétaire
```

**SGID (Set Group ID)**
```bash
chmod g+s repertoire           # SGID sur groupe
chmod 2755 repertoire          # SGID avec permissions 755
# Les fichiers créés héritent du groupe du répertoire
```

**Sticky Bit**
```bash
chmod +t repertoire            # Sticky bit
chmod 1755 repertoire          # Sticky bit avec permissions 755
# Seul le propriétaire peut supprimer ses fichiers
```

---
layout: default
---

# Commandes de gestion des permissions 🔧

### Modifier les permissions

**chmod (changer mode)**
```bash
chmod 755 fichier              # Permissions numériques
chmod u+x,g-w,o=r fichier      # Permissions symboliques
chmod +x script.sh             # Ajouter exécution
chmod -w fichier.txt           # Retirer écriture
chmod -R 755 repertoire/       # Récursif
```

**chown (changer owner)**
```bash
chown user fichier             # Changer propriétaire
chown user:group fichier       # Changer propriétaire et groupe
chown -R user repertoire/      # Récursif
chown --reference=source dest  # Copier les permissions d'un fichier
```

**chgrp (changer group)**
```bash
chgrp groupe fichier           # Changer groupe
chgrp -R groupe repertoire/    # Récursif
```

---
layout: default
---

# ACL (Access Control Lists) 📋

### Permissions granulaires avancées

**Installation des ACL**
```bash
sudo apt install acl           # Installer les ACL
sudo mount -o acl /dev/sda1 /  # Monter avec support ACL
```

**Commandes ACL**
```bash
setfacl -m u:user:rwx fichier  # Donner permissions à un utilisateur
setfacl -m g:group:r-x fichier # Donner permissions à un groupe
setfacl -x u:user fichier      # Supprimer ACL pour un utilisateur
setfacl -b fichier             # Supprimer toutes les ACL
getfacl fichier                 # Afficher les ACL
```

**Exemples d'utilisation**
```bash
# Donner accès en lecture à un utilisateur spécifique
setfacl -m u:john:r fichier.txt

# Donner accès en écriture à un groupe
setfacl -m g:developers:rw repertoire/

# Permissions par défaut pour les nouveaux fichiers
setfacl -m d:u:john:rwx repertoire/
```

---
layout: default
---

# Sécurité des mots de passe 🔑

### Politiques de sécurité

**Configuration dans /etc/login.defs**
```bash
# Politique de mots de passe
PASS_MAX_DAYS 90              # Expiration après 90 jours
PASS_MIN_DAYS 1               # Minimum 1 jour entre changements
PASS_WARN_AGE 7               # Avertissement 7 jours avant
PASS_MIN_LEN 8                # Longueur minimale 8 caractères
PASS_CHANGE_TRIES 3           # Tentatives de changement
```

**Configuration PAM**
```bash
# /etc/pam.d/common-password
password requisite pam_pwquality.so retry=3 minlen=8 difok=3
password requisite pam_unix.so sha512 shadow nullok
```

**Outils de vérification**
```bash
pwscore mot_de_passe           # Score de qualité du mot de passe
cracklib-check mot_de_passe    # Vérification avec cracklib
```

---
layout: default
---

# Pare-feu et sécurité réseau 🛡️

### Configuration du pare-feu

**ufw (Uncomplicated Firewall)**
```bash
sudo ufw enable                 # Activer le pare-feu
sudo ufw disable                # Désactiver le pare-feu
sudo ufw status                 # Statut du pare-feu
sudo ufw default deny incoming  # Refuser par défaut
sudo ufw default allow outgoing # Autoriser sortant par défaut
```

**Règles ufw**
```bash
sudo ufw allow ssh              # Autoriser SSH
sudo ufw allow 80/tcp           # Autoriser HTTP
sudo ufw allow 443/tcp          # Autoriser HTTPS
sudo ufw deny 22                # Refuser SSH
sudo ufw allow from 192.168.1.0/24  # Autoriser un réseau
```

**iptables (pare-feu avancé)**
```bash
sudo iptables -L               # Lister les règles
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT  # Autoriser HTTP
sudo iptables -A INPUT -p tcp --dport 22 -j DROP    # Refuser SSH
```

---
layout: default
---

# Sécurité des services 🔧

### Sécurisation des services système

**systemctl et sécurité**
```bash
systemctl mask service          # Masquer un service (empêcher le démarrage)
systemctl unmask service        # Démasquer un service
systemctl disable service       # Désactiver au démarrage
systemctl enable service        # Activer au démarrage
```

**Configuration des services**
```bash
# /etc/systemd/system/service.service
[Unit]
Description=Mon Service
After=network.target

[Service]
Type=simple
User=service_user
Group=service_group
ExecStart=/chemin/vers/service
Restart=always

[Install]
WantedBy=multi-user.target
```

**Sécurisation SSH**
```bash
# /etc/ssh/sshd_config
Port 2222                       # Changer le port
PermitRootLogin no              # Interdire root
PasswordAuthentication no        # Désactiver les mots de passe
PubkeyAuthentication yes        # Activer les clés
AllowUsers user1 user2          # Utilisateurs autorisés
```

---
layout: default
---

# Audit et surveillance 📊

### Surveiller la sécurité du système

**auditd (système d'audit)**
```bash
sudo apt install auditd        # Installer auditd
sudo systemctl enable auditd   # Activer au démarrage
sudo systemctl start auditd    # Démarrer le service
```

**Règles d'audit**
```bash
# Surveiller les accès aux fichiers sensibles
auditctl -w /etc/passwd -p wa -k passwd_changes
auditctl -w /etc/shadow -p wa -k shadow_changes
auditctl -w /etc/sudoers -p wa -k sudoers_changes

# Surveiller les connexions
auditctl -w /var/log/auth.log -p wa -k auth_changes
```

**Consultation des logs d'audit**
```bash
ausearch -k passwd_changes      # Rechercher par clé
ausearch -f /etc/passwd         # Rechercher par fichier
aureport --summary              # Rapport résumé
```

---
layout: default
---

# Chiffrement et intégrité 🔐

### Protection des données

**Chiffrement des disques**
```bash
# Chiffrement LUKS
sudo cryptsetup luksFormat /dev/sda1
sudo cryptsetup luksOpen /dev/sda1 encrypted_disk
sudo mkfs.ext4 /dev/mapper/encrypted_disk
sudo mount /dev/mapper/encrypted_disk /mnt
```

**Chiffrement des fichiers**
```bash
# GPG pour chiffrer des fichiers
gpg -c fichier.txt              # Chiffrer avec mot de passe
gpg -e -r destinataire fichier.txt  # Chiffrer pour un destinataire
gpg -d fichier.txt.gpg          # Déchiffrer
```

**Vérification d'intégrité**
```bash
# Calculer des checksums
md5sum fichier                  # MD5 (obsolète)
sha256sum fichier               # SHA256 (recommandé)
sha512sum fichier               # SHA512 (très sécurisé)

# Vérifier l'intégrité
echo "checksum fichier" | sha256sum -c
```

---
layout: default
---

# Sécurité des conteneurs 🐳

### Sécurisation Docker

**Docker avec sécurité**
```bash
# Exécuter un conteneur en mode non-privilégié
docker run --user 1000:1000 image

# Limiter les capacités
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE image

# Monter en lecture seule
docker run -v /host/path:/container/path:ro image
```

**Dockerfile sécurisé**
```dockerfile
# Utiliser un utilisateur non-root
FROM ubuntu:20.04
RUN groupadd -r appuser && useradd -r -g appuser appuser
USER appuser
WORKDIR /app
COPY --chown=appuser:appuser . .
```

**Scan de sécurité**
```bash
# Scanner une image pour les vulnérabilités
docker scan image_name
trivy image image_name
```

---
layout: default
---

# Sécurité des applications web 🌐

### Sécurisation des services web

**Apache avec sécurité**
```apache
# /etc/apache2/sites-available/secure-site.conf
<VirtualHost *:80>
    ServerName example.com
    DocumentRoot /var/www/html
    
    # Désactiver les informations serveur
    ServerTokens Prod
    ServerSignature Off
    
    # Headers de sécurité
    Header always set X-Content-Type-Options nosniff
    Header always set X-Frame-Options DENY
    Header always set X-XSS-Protection "1; mode=block"
    
    # Limiter les méthodes HTTP
    <LimitExcept GET POST>
        Deny from all
    </LimitExcept>
</VirtualHost>
```

**Nginx avec sécurité**
```nginx
# /etc/nginx/sites-available/secure-site
server {
    listen 80;
    server_name example.com;
    
    # Headers de sécurité
    add_header X-Content-Type-Options nosniff;
    add_header X-Frame-Options DENY;
    add_header X-XSS-Protection "1; mode=block";
    
    # Limiter la taille des requêtes
    client_max_body_size 10M;
}
```

---
layout: default
---

# Monitoring de sécurité 📈

### Surveillance en temps réel

**Logs de sécurité**
```bash
# Surveiller les logs d'authentification
tail -f /var/log/auth.log

# Surveiller les tentatives de connexion SSH
grep "Failed password" /var/log/auth.log

# Surveiller les accès sudo
grep "sudo:" /var/log/auth.log
```

**Outils de monitoring**
```bash
# fail2ban pour bloquer les attaques
sudo apt install fail2ban
sudo systemctl enable fail2ban
sudo systemctl start fail2ban

# Configuration fail2ban
sudo nano /etc/fail2ban/jail.local
```

**Alertes de sécurité**
```bash
# Script de surveillance
#!/bin/bash
# Vérifier les connexions SSH suspectes
grep "Failed password" /var/log/auth.log | \
  awk '{print $1,$2,$3}' | \
  sort | uniq -c | \
  awk '$1 > 5 {print "ALERTE: Trop de tentatives échouées pour " $2 " " $3 " " $4}'
```

---
layout: default
---

# Sauvegarde sécurisée 💾

### Stratégies de sauvegarde sécurisées

**Sauvegarde chiffrée**
```bash
# Sauvegarde avec chiffrement
tar -czf - /chemin/a/sauvegarder | \
  gpg -e -r backup@example.com > backup_$(date +%Y%m%d).tar.gz.gpg

# Sauvegarde incrémentale sécurisée
rsync -av --delete --chmod=600 \
  --exclude='*.tmp' \
  /source/ /destination/
```

**Rotation des sauvegardes**
```bash
# Script de rotation
#!/bin/bash
# Garder 7 sauvegardes quotidiennes, 4 hebdomadaires, 12 mensuelles
find /backup/daily -name "*.tar.gz" -mtime +7 -delete
find /backup/weekly -name "*.tar.gz" -mtime +28 -delete
find /backup/monthly -name "*.tar.gz" -mtime +365 -delete
```

**Vérification des sauvegardes**
```bash
# Tester la restauration
tar -tzf backup.tar.gz | head -10
gpg -d backup.tar.gz.gpg | tar -tz | head -10
```

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique de la sécurité

**Exercice 1 : Permissions avancées**
```bash
# Créer un répertoire partagé sécurisé
sudo mkdir /shared
sudo chown root:developers /shared
sudo chmod 775 /shared
sudo chmod g+s /shared

# Tester les permissions
touch /shared/test.txt
ls -la /shared/
```

**Exercice 2 : Configuration ufw**
```bash
# Configurer un pare-feu basique
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw enable
sudo ufw status
```

**Exercice 3 : Audit de sécurité**
```bash
# Vérifier les permissions sensibles
find /etc -perm -4000 -ls
find /etc -perm -2000 -ls
find /etc -perm -1000 -ls

# Vérifier les fichiers sans propriétaire
find / -nouser -ls
find / -nogroup -ls
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour la sécurité

**Principes de base**
- Principe du moindre privilège
- Défense en profondeur
- Surveillance continue
- Mises à jour régulières
- Sauvegardes sécurisées

**Sécurité réseau**
- Utiliser des pare-feu
- Chiffrer les communications
- Limiter les services exposés
- Surveiller le trafic réseau
- Utiliser des VPN

**Sécurité système**
- Désactiver les services inutiles
- Utiliser des mots de passe forts
- Limiter les accès root
- Surveiller les logs
- Maintenir les systèmes à jour

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Processus et services** système
2. **Réseaux et connectivité**
3. **Paquets logiciels** et gestion
4. **Shell scripting** de base
5. **Virtualisation** et conteneurs

**Préparation :**
- Testez les permissions avancées
- Configurez un pare-feu basique
- Familiarisez-vous avec les ACL
- Pratiquez la surveillance de sécurité

---
src: './pages/08-processus-services.md'
--- 