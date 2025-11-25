---

# 🎯 Exercices pratiques - Module 7

**Configuration et diagnostic réseau**

---

# Exercice 1 : Diagnostic réseau de base 🔍

**Objectif :** Diagnostiquer la connectivité

**Consignes :**

1. Affichez votre adresse IP
2. Affichez la passerelle par défaut
3. Testez la connectivité vers `8.8.8.8`
4. Testez la résolution DNS vers `google.com`
5. Tracez la route vers `cloudflare.com`
6. Affichez les connexions actives

---

# Solution Exercice 1 💡

```bash
# 1. Adresse IP
ip addr show
# ou
hostname -I
# ou
ifconfig  # si installé

# 2. Passerelle
ip route show default
# ou
route -n | grep ^0.0.0.0

# 3. Test connectivité
ping -c 4 8.8.8.8

# 4. Test DNS
ping -c 4 google.com
# ou
nslookup google.com
# ou
dig google.com +short

# 5. Trace route
traceroute cloudflare.com
# ou avec ICMP
sudo traceroute -I cloudflare.com

# 6. Connexions actives
ss -tunap
# ou
sudo netstat -tunap
```

---

# Exercice 2 : Configuration SSH 🔐

**Objectif :** Configurer et sécuriser SSH

**Consignes :**

1. Générez une paire de clés SSH (ed25519)
2. Affichez votre clé publique
3. Copiez votre clé vers `localhost` (test)
4. Connectez-vous sans mot de passe
5. Changez le port SSH en 2222 (config)
6. Désactivez l'authentification par mot de passe

---

# Solution Exercice 2 💡

```bash
# 1. Générer clés
ssh-keygen -t ed25519 -C "votre@email.com"
# Appuyez sur Entrée pour accepter les valeurs par défaut

# 2. Afficher clé publique
cat ~/.ssh/id_ed25519.pub

# 3. Copier vers localhost
ssh-copy-id localhost
# ou manuellement :
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

# 4. Connexion sans mot de passe
ssh localhost
# Devrait fonctionner directement !

# 5 & 6. Configuration SSH (sur serveur distant)
sudo nano /etc/ssh/sshd_config

# Modifier :
# Port 2222
# PasswordAuthentication no
# PubkeyAuthentication yes
# PermitRootLogin no

# Tester la config
sudo sshd -t

# Redémarrer
sudo systemctl restart sshd

# Se connecter avec nouveau port
ssh -p 2222 user@server
```

---

# Exercice 3 : Pare-feu avec UFW 🔥

**Objectif :** Configurer un pare-feu simple

**Consignes :**

1. Installez et activez UFW
2. Autorisez SSH (port 22)
3. Autorisez HTTP et HTTPS
4. Bloquez le ping (ICMP)
5. Affichez les règles
6. Testez les règles

---

# Solution Exercice 3 💡

```bash
# 1. Installer et activer
sudo apt install ufw
sudo ufw enable

# 2. Autoriser SSH (IMPORTANT : avant d'activer !)
sudo ufw allow 22/tcp
# ou
sudo ufw allow ssh

# 3. HTTP et HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
# ou
sudo ufw allow 'Nginx Full'

# 4. Bloquer ping
sudo ufw deny proto icmp

# 5. Afficher règles
sudo ufw status verbose
sudo ufw status numbered

# 6. Tester
# Depuis une autre machine
ping votre_ip  # Devrait être bloqué
curl http://votre_ip  # Devrait fonctionner

# Supprimer une règle (par numéro)
sudo ufw delete 3

# Désactiver temporairement
sudo ufw disable
```

---

# Exercice 4 : Transfert de fichiers 📤

**Objectif :** Transférer des fichiers de manière sécurisée

**Consignes :**

1. Créez un fichier de test de 10 Mo
2. Transférez-le vers `/tmp` sur localhost avec `scp`
3. Transférez un répertoire complet avec `scp -r`
4. Utilisez `rsync` pour synchroniser un répertoire
5. Comparez les temps de transfert

---

# Solution Exercice 4 💡

```bash
# 1. Créer fichier de test
dd if=/dev/zero of=/tmp/test_10mo.bin bs=1M count=10

# 2. Transférer avec scp
time scp /tmp/test_10mo.bin localhost:/tmp/test_copy.bin

# 3. Répertoire complet
mkdir -p /tmp/source/{dir1,dir2,dir3}
touch /tmp/source/dir{1,2,3}/file{1..5}.txt
time scp -r /tmp/source localhost:/tmp/dest

# 4. Rsync (plus efficace)
time rsync -avz /tmp/source/ localhost:/tmp/dest_rsync/

# Modifier un fichier
echo "modifié" >> /tmp/source/dir1/file1.txt

# Re-synchroniser (seulement les changements)
time rsync -avz /tmp/source/ localhost:/tmp/dest_rsync/

# 5. Comparer
# rsync est plus rapide pour les mises à jour !

# Rsync avec progression
rsync -avz --progress /tmp/source/ localhost:/tmp/dest2/
```

---

# Exercice 5 : Serveur web simple 🌐

**Objectif :** Mettre en place un serveur web basique

**Consignes :**

1. Installez nginx
2. Créez une page HTML simple
3. Configurez un virtual host
4. Testez avec curl et navigateur
5. Ajoutez une entrée `/etc/hosts`
6. Vérifiez les logs d'accès

---

# Solution Exercice 5 💡

```bash
# 1. Installer nginx
sudo apt update
sudo apt install nginx

# 2. Page HTML
sudo mkdir -p /var/www/monsite
sudo tee /var/www/monsite/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head><title>Mon Site</title></head>
<body>
  <h1>Bienvenue sur mon site !</h1>
  <p>Ceci est une page de test.</p>
</body>
</html>
EOF

# 3. Virtual host
sudo tee /etc/nginx/sites-available/monsite << 'EOF'
server {
    listen 80;
    server_name monsite.local;
    root /var/www/monsite;
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
EOF

# Activer
sudo ln -s /etc/nginx/sites-available/monsite /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

# 5. /etc/hosts
echo "127.0.0.1 monsite.local" | sudo tee -a /etc/hosts

# 4. Tester
curl http://monsite.local
firefox http://monsite.local &

# 6. Logs
sudo tail -f /var/log/nginx/access.log
```

---

# Exercice bonus : Script de monitoring réseau 📊

**Objectif :** Surveiller la connectivité

**Créez un script qui :**
- Teste la connectivité vers plusieurs serveurs
- Mesure la latence
- Enregistre les résultats
- Alerte en cas de problème

---

# Solution Exercice bonus 💡

```bash
#!/bin/bash
# Script de monitoring réseau

SERVERS=("8.8.8.8" "1.1.1.1" "google.com" "github.com")
LOG_FILE="/var/log/network_monitor.log"
ALERT_THRESHOLD=200  # ms

echo "===== Monitoring $(date) =====" | tee -a $LOG_FILE

for server in "${SERVERS[@]}"; do
    echo -n "Test de $server... " | tee -a $LOG_FILE
    
    # Ping 3 fois
    result=$(ping -c 3 -W 2 $server 2>/dev/null | grep "avg" | \
             awk -F'/' '{print $5}')
    
    if [ -z "$result" ]; then
        echo "❌ ÉCHEC" | tee -a $LOG_FILE
        logger -t NET_MONITOR "ALERTE: $server injoignable"
    else
        latency=${result%.*}  # Arrondir
        echo "✅ OK (${latency}ms)" | tee -a $LOG_FILE
        
        # Alerte si latence élevée
        if [ $latency -gt $ALERT_THRESHOLD ]; then
            echo "  ⚠️  Latence élevée!" | tee -a $LOG_FILE
            logger -t NET_MONITOR "WARNING: $server latence ${latency}ms"
        fi
    fi
    
    sleep 1
done

echo "============================" | tee -a $LOG_FILE
```

**Automatiser avec cron :**

```bash
# Toutes les 5 minutes
*/5 * * * * /usr/local/bin/network_monitor.sh
```

---

# Points clés à retenir 📌

**Diagnostic :**
- `ping` : connectivité
- `traceroute` : chemin
- `dig`/`nslookup` : DNS
- `ss`/`netstat` : connexions

**SSH :**
- Clés publiques/privées
- `ssh-keygen`, `ssh-copy-id`
- Configuration dans `/etc/ssh/sshd_config`

**Pare-feu :**
- UFW pour simplicité
- `allow`/`deny` pour les règles
- Toujours autoriser SSH avant d'activer !
