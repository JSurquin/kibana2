---
layout: new-section
routeAlias: 'exercices-pratiques'
---

<a name="exercices-pratiques" id="exercices-pratiques"></a>

# 🎯 Exercices pratiques
## Mise en pratique des connaissances

### Entraînez-vous sur des cas réels

---

# Comment aborder les exercices ? 📝

**Conseils :**

1. **Lisez attentivement** l'énoncé
2. **Décomposez** le problème
3. **Testez** vos commandes dans un environnement sûr
4. **Documentez** votre démarche
5. **Utilisez `man` et `--help`** en cas de doute

**Environnement recommandé :**
- Machine virtuelle (VirtualBox, VMware)
- Conteneur Docker
- WSL sur Windows
- Raspberry Pi

---

# Exercice 1 : Navigation et fichiers 📁

**Objectif :** Maîtriser les commandes de base

**Tâches :**

1. Créer l'arborescence suivante :

```
projet/
├── src/
│   ├── main.py
│   └── utils.py
├── docs/
│   └── README.md
└── tests/
    └── test_main.py
```

2. Créer 3 fichiers texte avec du contenu
3. Trouver tous les fichiers `.py`
4. Afficher les 10 dernières lignes de `main.py`
5. Compter le nombre de lignes dans tous les fichiers

---

# Exercice 1 : Solution 💡

```bash
# 1. Créer l'arborescence
mkdir -p projet/{src,docs,tests}
touch projet/src/{main,utils}.py
touch projet/docs/README.md
touch projet/tests/test_main.py

# 2. Ajouter du contenu
echo "print('Hello')" > projet/src/main.py
echo "def helper(): pass" > projet/src/utils.py
echo "# Documentation" > projet/docs/README.md

# 3. Trouver les fichiers .py
find projet -name "*.py"

# 4. Dernières lignes
tail projet/src/main.py

# 5. Compter les lignes
find projet -type f -exec wc -l {} + | tail -1
```

---

# Exercice 2 : Utilisateurs et permissions 👥

**Objectif :** Gestion des utilisateurs et droits

**Tâches :**

1. Créer 3 utilisateurs : `dev1`, `dev2`, `admin`
2. Créer un groupe `developers`
3. Ajouter `dev1` et `dev2` au groupe
4. Créer `/shared` avec accès groupe `developers`
5. Configurer les permissions : `rwxrwx---`
6. Tester l'accès avec chaque utilisateur

---

# Exercice 2 : Solution 💡

```bash
# 1. Créer les utilisateurs
sudo useradd -m -s /bin/bash dev1
sudo useradd -m -s /bin/bash dev2
sudo useradd -m -s /bin/bash admin
echo "dev1:password1" | sudo chpasswd
echo "dev2:password2" | sudo chpasswd
echo "admin:password3" | sudo chpasswd

# 2. Créer le groupe
sudo groupadd developers

# 3. Ajouter au groupe
sudo usermod -aG developers dev1
sudo usermod -aG developers dev2
```

---

# Exercice 2 : Solution (suite) 💡

```bash
# 4. Créer le répertoire partagé
sudo mkdir /shared
sudo chown root:developers /shared

# 5. Permissions
sudo chmod 770 /shared

# 6. Tester
su - dev1
touch /shared/test1.txt    # Devrait fonctionner
exit

su - admin
touch /shared/test2.txt    # Devrait échouer (pas dans le groupe)
exit
```

---

# Exercice 3 : Gestion des processus ⚙️

**Objectif :** Comprendre et gérer les processus

**Tâches :**

1. Lancer un processus long en arrière-plan
2. Trouver son PID
3. Changer sa priorité (nice)
4. Surveiller son utilisation CPU/mémoire
5. Suspendre puis reprendre le processus
6. Terminer proprement le processus

**Processus suggéré :**

```bash
sleep 300 &
```

---

# Exercice 3 : Solution 💡

```bash
# 1. Lancer en arrière-plan
sleep 300 &

# 2. Trouver le PID
jobs
pgrep sleep
ps aux | grep sleep

# 3. Changer la priorité
PID=$(pgrep sleep)
sudo renice 10 -p $PID

# 4. Surveiller
top -p $PID
# ou
htop  # puis rechercher le processus
```

---

# Exercice 3 : Solution (suite) 💡

```bash
# 5. Suspendre et reprendre
kill -STOP $PID
ps aux | grep sleep  # État T
kill -CONT $PID
ps aux | grep sleep  # État S ou R

# 6. Terminer proprement
kill $PID
# Vérifier
ps aux | grep sleep

# Si toujours présent, forcer
kill -9 $PID
```

---

# Exercice 4 : Disques et partitions 💾

**Objectif :** Gérer le stockage

**Tâches :**

1. Créer un fichier image de 100Mo
2. Créer une partition dessus
3. Formater en ext4
4. Monter dans `/mnt/test`
5. Créer des fichiers dedans
6. Vérifier l'utilisation
7. Démonter proprement

**⚠️ Exercice à faire dans une VM !**

---

# Exercice 4 : Solution 💡

```bash
# 1. Créer un fichier image
dd if=/dev/zero of=disk.img bs=1M count=100

# 2. Créer une partition (ou utiliser directement)
# Pour cet exercice, on utilise directement le fichier

# 3. Formater
sudo mkfs.ext4 disk.img

# 4. Créer le point de montage et monter
sudo mkdir -p /mnt/test
sudo mount -o loop disk.img /mnt/test
```

---

# Exercice 4 : Solution (suite) 💡

```bash
# 5. Créer des fichiers
sudo touch /mnt/test/fichier{1..5}.txt
echo "Contenu test" | sudo tee /mnt/test/fichier1.txt

# 6. Vérifier l'utilisation
df -h /mnt/test
du -sh /mnt/test/*

# 7. Démonter
sudo umount /mnt/test

# Nettoyer
rm disk.img
```

---

# Exercice 5 : Configuration réseau 🌐

**Objectif :** Configurer et diagnostiquer le réseau

**Tâches :**

1. Afficher la configuration IP actuelle
2. Identifier la passerelle par défaut
3. Tester la connectivité vers `8.8.8.8`
4. Résoudre `google.com` en IP
5. Tracer la route vers `cloudflare.com`
6. Voir les ports en écoute sur la machine
7. Configurer une entrée dans `/etc/hosts`

---

# Exercice 5 : Solution 💡

```bash
# 1. Configuration IP
ip addr show
# ou
ifconfig

# 2. Passerelle
ip route show default
# ou
route -n

# 3. Test de connectivité
ping -c 4 8.8.8.8

# 4. Résolution DNS
dig google.com +short
# ou
nslookup google.com
```

---

# Exercice 5 : Solution (suite) 💡

```bash
# 5. Trace route
traceroute cloudflare.com
# ou
traceroute -I cloudflare.com  # avec ICMP

# 6. Ports en écoute
sudo ss -tulnp
# ou
sudo netstat -tulnp

# 7. Entrée hosts
echo "192.168.1.100  monserveur.local" | sudo tee -a /etc/hosts
ping monserveur.local
```

---

# Exercice 6 : Script de sauvegarde 💾

**Objectif :** Créer un script de sauvegarde automatique

**Spécifications :**

1. Sauvegarder `/home/user/documents`
2. Nom : `backup_YYYYMMDD_HHMMSS.tar.gz`
3. Destination : `/backup`
4. Supprimer les sauvegardes > 7 jours
5. Logger les actions dans un fichier
6. Envoyer un email en cas d'erreur

---

# Exercice 6 : Solution 💡

```bash
#!/bin/bash
set -euo pipefail

# Configuration
SOURCE="/home/$USER/documents"
DESTINATION="/backup"
DATE=$(date +%Y%m%d_%H%M%S)
ARCHIVE="backup_${DATE}.tar.gz"
LOGFILE="/var/log/backup.log"

# Fonction de log
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOGFILE"
}

# Vérifications
if [ ! -d "$SOURCE" ]; then
    log "ERREUR: $SOURCE n'existe pas"
    exit 1
fi
```

---

# Exercice 6 : Solution (suite) 💡

```bash
# Créer le répertoire de destination
mkdir -p "$DESTINATION"

# Sauvegarde
log "Début de la sauvegarde de $SOURCE"
if tar -czf "${DESTINATION}/${ARCHIVE}" "$SOURCE" 2>&1 | tee -a "$LOGFILE"; then
    log "Sauvegarde réussie: ${ARCHIVE}"
    SIZE=$(du -h "${DESTINATION}/${ARCHIVE}" | cut -f1)
    log "Taille: $SIZE"
else
    log "ERREUR lors de la sauvegarde"
    echo "Erreur de sauvegarde" | mail -s "Backup Failed" admin@example.com
    exit 1
fi

# Nettoyage (> 7 jours)
log "Nettoyage des anciennes sauvegardes"
find "$DESTINATION" -name "backup_*.tar.gz" -mtime +7 -delete
log "Fin de la sauvegarde"
```

---

# Exercice 6 : Planification 📅

**Ajouter au crontab pour exécution quotidienne à 2h :**

```bash
# Éditer le crontab
crontab -e

# Ajouter la ligne
0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1
```

**Ou avec systemd timer :**

```bash
# /etc/systemd/system/backup.service
[Unit]
Description=Sauvegarde quotidienne

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh
```

```bash
# /etc/systemd/system/backup.timer
[Unit]
Description=Timer pour sauvegarde

[Timer]
OnCalendar=daily
OnCalendar=02:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

---

# Exercice 7 : Sécurisation SSH 🔒

**Objectif :** Sécuriser l'accès SSH

**Tâches :**

1. Générer une paire de clés SSH
2. Copier la clé publique sur le serveur
3. Désactiver l'authentification par mot de passe
4. Changer le port SSH (2222)
5. Configurer fail2ban pour SSH
6. Tester la connexion

**⚠️ Attention à ne pas vous bloquer !**

---

# Exercice 7 : Solution 💡

```bash
# 1. Générer les clés (sur le client)
ssh-keygen -t ed25519 -C "admin@example.com"

# 2. Copier la clé
ssh-copy-id user@serveur
# ou
ssh-copy-id -p 2222 user@serveur  # si port changé

# 3-4. Configuration SSH (sur le serveur)
sudo nano /etc/ssh/sshd_config
```

```
Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

```bash
# Vérifier la config
sudo sshd -t

# Redémarrer SSH
sudo systemctl restart sshd
```

---

# Exercice 7 : Solution (suite) 💡

```bash
# 5. Configurer fail2ban
sudo apt install fail2ban
sudo nano /etc/fail2ban/jail.local
```

```ini
[sshd]
enabled = true
port = 2222
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600
findtime = 600
```

```bash
sudo systemctl restart fail2ban

# 6. Tester la connexion
ssh -p 2222 user@serveur
```

---

# Exercice 8 : Serveur web LEMP 🌐

**Objectif :** Installer une stack LEMP

**LEMP** : Linux + Nginx + MySQL + PHP

**Tâches :**

1. Installer Nginx
2. Installer MySQL/MariaDB
3. Installer PHP-FPM
4. Créer un virtual host
5. Créer une base de données
6. Tester avec un script PHP

---

# Exercice 8 : Solution 💡

```bash
# 1. Installer Nginx
sudo apt update
sudo apt install nginx
sudo systemctl enable --now nginx

# 2. Installer MariaDB
sudo apt install mariadb-server
sudo mysql_secure_installation

# 3. Installer PHP-FPM
sudo apt install php-fpm php-mysql php-cli php-curl php-gd

# 4. Créer le virtual host
sudo nano /etc/nginx/sites-available/monsite
```

---

# Exercice 8 : Solution (suite) 💡

```nginx
server {
    listen 80;
    server_name monsite.local;
    root /var/www/monsite;
    index index.php index.html;

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

```bash
# Activer le site
sudo ln -s /etc/nginx/sites-available/monsite /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

---

# Exercice 8 : Solution (suite) 💡

```bash
# Créer le répertoire et contenu
sudo mkdir -p /var/www/monsite
sudo chown -R www-data:www-data /var/www/monsite

# 5. Créer la base de données
sudo mysql
```

```sql
CREATE DATABASE monsite_db;
CREATE USER 'monsite_user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON monsite_db.* TO 'monsite_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

# Exercice 8 : Solution (suite) 💡

```bash
# 6. Script PHP de test
sudo nano /var/www/monsite/test.php
```

```php
<?php
$conn = new mysqli('localhost', 'monsite_user', 'password', 'monsite_db');

if ($conn->connect_error) {
    die("Connexion échouée: " . $conn->connect_error);
}

echo "Connexion réussie à la base de données!<br>";
echo "Version PHP: " . phpversion();
?>
```

```bash
# Ajouter au /etc/hosts
echo "127.0.0.1 monsite.local" | sudo tee -a /etc/hosts

# Tester
curl http://monsite.local/test.php
```

---

# Exercice 9 : Monitoring système 📊

**Objectif :** Créer un script de monitoring

**Le script doit vérifier :**

1. Utilisation CPU > 80%
2. Utilisation mémoire > 80%
3. Espace disque > 80%
4. Services critiques actifs (nginx, mysql)
5. Envoyer une alerte si problème

---

# Exercice 9 : Solution 💡

```bash
#!/bin/bash
set -euo pipefail

ALERT_EMAIL="admin@example.com"
ALERT=false

# CPU
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d'%' -f1)
if (( $(echo "$CPU_USAGE > 80" | bc -l) )); then
    echo "ALERTE: CPU à ${CPU_USAGE}%"
    ALERT=true
fi

# Mémoire
MEM_USAGE=$(free | grep Mem | awk '{print ($3/$2) * 100.0}')
if (( $(echo "$MEM_USAGE > 80" | bc -l) )); then
    echo "ALERTE: Mémoire à ${MEM_USAGE}%"
    ALERT=true
fi
```

---

# Exercice 9 : Solution (suite) 💡

```bash
# Disque
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}' | tr -d '%')
if [ "$DISK_USAGE" -gt 80 ]; then
    echo "ALERTE: Disque à ${DISK_USAGE}%"
    ALERT=true
fi

# Services
for service in nginx mysql; do
    if ! systemctl is-active --quiet $service; then
        echo "ALERTE: $service est arrêté"
        ALERT=true
    fi
done

# Envoyer alerte
if [ "$ALERT" = true ]; then
    echo "Problème détecté sur $(hostname)" | \
        mail -s "Alerte Monitoring" $ALERT_EMAIL
fi
```

---

# Exercice 10 : Projet complet 🎯

**Objectif :** Déployer une application complète

**Scénario :**

Vous devez déployer un blog WordPress avec :
- Nginx comme serveur web
- MySQL pour la base de données
- PHP-FPM
- SSL avec Let's Encrypt
- Sauvegarde automatique quotidienne
- Monitoring
- Fail2ban

**Durée estimée :** 2-3 heures

---

# Exercice 10 : Étapes 📋

1. **Préparation système**
   - Mise à jour
   - Configuration firewall
   - Création utilisateur

2. **Installation stack LEMP**
   - Nginx
   - MySQL
   - PHP-FPM

3. **Téléchargement WordPress**
   - Dernière version
   - Configuration base de données
   - Permissions

---

# Exercice 10 : Étapes (suite) 📋

4. **Configuration Nginx**
   - Virtual host
   - SSL Let's Encrypt
   - Redirection HTTP → HTTPS

5. **Sécurisation**
   - fail2ban
   - Durcissement SSH
   - Permissions fichiers

6. **Automatisation**
   - Script de sauvegarde
   - Cron quotidien
   - Script de monitoring

---

# Exercice 10 : Solution guidée 💡

**Trop long pour une slide !**

**Ressources :**
- Documentation WordPress
- Documentation Nginx
- DigitalOcean Tutorials
- Linode Guides

**Conseils :**
- Documenter chaque étape
- Tester après chaque modification
- Faire des sauvegardes avant modifications importantes
- Utiliser une VM pour tester

---

# Ressources supplémentaires 📚

**Sites web :**
- [Linux Journey](https://linuxjourney.com/)
- [OverTheWire](https://overthewire.org/wargames/) - Jeux d'apprentissage
- [Explain Shell](https://explainshell.com/) - Explications commandes

**Livres :**
- "The Linux Command Line" - William Shotts
- "UNIX and Linux System Administration Handbook"
- "Linux Bible"

**YouTube :**
- NetworkChuck
- LearnLinuxTV
- TechWorld with Nana

---

# Conseils pour pratiquer 💪

1. **Installer Linux en VM**
   - VirtualBox gratuit
   - Tester sans risque

2. **Utiliser WSL sur Windows**
   - Pratique au quotidien

3. **Raspberry Pi**
   - Serveur personnel
   - Projets home automation

4. **Contribuer à l'open source**
   - GitHub
   - Apprendre des autres

---

# Certification et évaluation 📜

**Prochaine étape :**
- QCM de validation
- Évaluation des compétences acquises
- Retour sur les points à améliorer

**Certifications recommandées :**
- LPIC-1 (Linux Professional Institute)
- CompTIA Linux+
- RHCSA (Red Hat)

**Continuer à apprendre :**
- Linux est un voyage, pas une destination
- Toujours de nouvelles choses à découvrir
- La communauté est votre meilleure ressource

---
layout: default
---

# Bon courage pour les exercices ! 💪

Prenez votre temps, testez, expérimentez !

L'apprentissage se fait par la pratique.

