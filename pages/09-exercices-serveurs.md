---

# 🎯 Exercices pratiques - Module 10

**Services serveurs**

---

# Exercice 1 : Serveur web Nginx 🌐

**Objectif :** Installer et configurer Nginx

**Consignes :**

1. Installez Nginx
2. Créez 2 sites : `site1.local` et `site2.local`
3. Configurez 2 virtual hosts
4. Ajoutez du contenu HTML
5. Testez les deux sites
6. Consultez les logs d'accès

---

# Solution Exercice 1 💡

```bash
# 1. Installer
sudo apt update
sudo apt install nginx -y

# 2 & 3. Site 1
sudo mkdir -p /var/www/site1
sudo tee /var/www/site1/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head><title>Site 1</title></head>
<body style="background: #4CAF50; color: white;">
  <h1>Bienvenue sur Site 1 🎯</h1>
</body>
</html>
EOF

sudo tee /etc/nginx/sites-available/site1 << 'EOF'
server {
    listen 80;
    server_name site1.local;
    root /var/www/site1;
    index index.html;
}
EOF

# Site 2
sudo mkdir -p /var/www/site2
sudo tee /var/www/site2/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head><title>Site 2</title></head>
<body style="background: #2196F3; color: white;">
  <h1>Bienvenue sur Site 2 🚀</h1>
</body>
</html>
EOF

sudo tee /etc/nginx/sites-available/site2 << 'EOF'
server {
    listen 80;
    server_name site2.local;
    root /var/www/site2;
    index index.html;
}
EOF

# Activer
sudo ln -s /etc/nginx/sites-available/site1 /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/site2 /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

# Ajouter au hosts
echo "127.0.0.1 site1.local site2.local" | sudo tee -a /etc/hosts

# 5. Tester
curl http://site1.local
curl http://site2.local

# 6. Logs
sudo tail -f /var/log/nginx/access.log
```

---

# Exercice 2 : Reverse Proxy 🔄

**Objectif :** Configurer Nginx comme reverse proxy

**Consignes :**

1. Lancez 2 serveurs Python sur ports 8001 et 8002
2. Configurez Nginx pour proxy les requêtes
3. Testez le load balancing
4. Ajoutez des headers personnalisés
5. Configurez le cache

---

# Solution Exercice 2 💡

```bash
# 1. Serveurs Python
cat > /tmp/server1.py << 'EOF'
from http.server import HTTPServer, BaseHTTPRequestHandler

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.end_headers()
        self.wfile.write(b'Response from Server 1')

HTTPServer(('', 8001), Handler).serve_forever()
EOF

cat > /tmp/server2.py << 'EOF'
from http.server import HTTPServer, BaseHTTPRequestHandler

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.end_headers()
        self.wfile.write(b'Response from Server 2')

HTTPServer(('', 8002), Handler).serve_forever()
EOF

# Lancer en arrière-plan
python3 /tmp/server1.py &
python3 /tmp/server2.py &

# 2 & 3. Configuration Nginx
sudo tee /etc/nginx/sites-available/proxy << 'EOF'
upstream backend {
    server localhost:8001;
    server localhost:8002;
}

server {
    listen 80;
    server_name proxy.local;
    
    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        
        # Cache
        proxy_cache_valid 200 10m;
        proxy_cache_bypass $http_pragma $http_authorization;
    }
}
EOF

sudo ln -s /etc/nginx/sites-available/proxy /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

echo "127.0.0.1 proxy.local" | sudo tee -a /etc/hosts

# Tester (load balancing round-robin)
for i in {1..10}; do
    curl http://proxy.local
    echo
done

# Nettoyer
killall python3
```

---

# Exercice 3 : Base de données MySQL 🗄️

**Objectif :** Installer et sécuriser MySQL

**Consignes :**

1. Installez MySQL
2. Exécutez la sécurisation initiale
3. Créez une base de données `webapp`
4. Créez un utilisateur avec droits limités
5. Testez la connexion
6. Créez une sauvegarde

---

# Solution Exercice 3 💡

```bash
# 1. Installer
sudo apt install mysql-server -y

# 2. Sécurisation
sudo mysql_secure_installation
# - Mot de passe root
# - Supprimer utilisateurs anonymes : Oui
# - Interdire connexion root à distance : Oui
# - Supprimer base test : Oui
# - Recharger privilèges : Oui

# 3 & 4. Créer base et utilisateur
sudo mysql << 'EOF'
CREATE DATABASE webapp;
CREATE USER 'webapp_user'@'localhost' IDENTIFIED BY 'SecurePass123!';
GRANT SELECT, INSERT, UPDATE, DELETE ON webapp.* TO 'webapp_user'@'localhost';
FLUSH PRIVILEGES;

-- Créer une table test
USE webapp;
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100)
);

INSERT INTO users (username, email) VALUES
('alice', 'alice@example.com'),
('bob', 'bob@example.com');
EOF

# 5. Tester
mysql -u webapp_user -p'SecurePass123!' -e "SELECT * FROM webapp.users;"

# 6. Sauvegarde
sudo mysqldump -u root webapp > /tmp/webapp_backup.sql

# Restaurer (si besoin)
# mysql -u root webapp < /tmp/webapp_backup.sql

# Script de sauvegarde automatique
cat > /usr/local/bin/mysql_backup.sh << 'EOF'
#!/bin/bash
BACKUP_DIR="/backup/mysql"
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p $BACKUP_DIR
mysqldump --all-databases | gzip > $BACKUP_DIR/all_dbs_${DATE}.sql.gz

# Garder seulement 7 jours
find $BACKUP_DIR -name "*.sql.gz" -mtime +7 -delete
EOF

chmod +x /usr/local/bin/mysql_backup.sh
```

---

# Exercice 4 : SSL avec Let's Encrypt 🔒

**Objectif :** Sécuriser un site avec HTTPS

**Consignes :**

1. Installez Certbot
2. Obtenez un certificat (domaine requis ou test local)
3. Configurez Nginx pour HTTPS
4. Forcez la redirection HTTP → HTTPS
5. Testez le certificat
6. Configurez le renouvellement automatique

---

# Solution Exercice 4 💡

```bash
# 1. Installer Certbot
sudo apt install certbot python3-certbot-nginx -y

# 2. Obtenir certificat (avec domaine réel)
sudo certbot --nginx -d example.com -d www.example.com

# Pour test local (certificat auto-signé)
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/nginx-selfsigned.key \
    -out /etc/ssl/certs/nginx-selfsigned.crt \
    -subj "/C=FR/ST=France/L=Paris/O=Test/CN=site1.local"

# 3 & 4. Config HTTPS
sudo tee /etc/nginx/sites-available/site1-ssl << 'EOF'
server {
    listen 80;
    server_name site1.local;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name site1.local;
    
    ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
    
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    
    root /var/www/site1;
    index index.html;
}
EOF

sudo ln -sf /etc/nginx/sites-available/site1-ssl /etc/nginx/sites-enabled/site1
sudo nginx -t
sudo systemctl reload nginx

# 5. Tester
curl -k https://site1.local

# 6. Renouvellement automatique (Let's Encrypt)
sudo certbot renew --dry-run

# Ajouter au cron
echo "0 3 * * * certbot renew --quiet" | sudo crontab -
```

---

# Exercice 5 : Monitoring avec Prometheus & Node Exporter 📊

**Objectif :** Mettre en place du monitoring

**Consignes :**

1. Installez Node Exporter
2. Configurez-le comme service
3. Testez les métriques
4. Créez un script de monitoring custom
5. Exportez des métriques personnalisées

---

# Solution Exercice 5 💡

```bash
# 1. Télécharger Node Exporter
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
tar xvfz node_exporter-1.7.0.linux-amd64.tar.gz
sudo mv node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/
sudo useradd -rs /bin/false node_exporter

# 2. Service systemd
sudo tee /etc/systemd/system/node_exporter.service << 'EOF'
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter

# 3. Tester
curl http://localhost:9100/metrics

# Métriques intéressantes
curl http://localhost:9100/metrics | grep node_cpu
curl http://localhost:9100/metrics | grep node_memory

# 4 & 5. Métriques custom
cat > /var/lib/node_exporter/textfile_collector/custom.prom << 'EOF'
# HELP custom_disk_usage Custom disk usage metric
# TYPE custom_disk_usage gauge
custom_disk_usage{path="/"} 75.5

# HELP custom_service_status Custom service status
# TYPE custom_service_status gauge
custom_service_status{service="nginx"} 1
custom_service_status{service="mysql"} 1
EOF

# Script de mise à jour des métriques
cat > /usr/local/bin/update_custom_metrics.sh << 'EOF'
#!/bin/bash
METRICS_FILE="/var/lib/node_exporter/textfile_collector/custom.prom"

# Disk usage
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}' | tr -d '%')

# Services status
NGINX_STATUS=$(systemctl is-active nginx && echo 1 || echo 0)
MYSQL_STATUS=$(systemctl is-active mysql && echo 1 || echo 0)

cat > $METRICS_FILE << ENDOFMETRICS
# HELP custom_disk_usage Custom disk usage metric
# TYPE custom_disk_usage gauge
custom_disk_usage{path="/"} $DISK_USAGE

# HELP custom_service_status Custom service status
# TYPE custom_service_status gauge
custom_service_status{service="nginx"} $NGINX_STATUS
custom_service_status{service="mysql"} $MYSQL_STATUS
ENDOFMETRICS
EOF

chmod +x /usr/local/bin/update_custom_metrics.sh

# Exécuter toutes les minutes
echo "* * * * * /usr/local/bin/update_custom_metrics.sh" | crontab -
```

---

# Exercice bonus : Stack complète Docker 🐳

**Objectif :** Déployer une stack avec Docker Compose

**Créez un `docker-compose.yml` avec :**
- Nginx
- PHP-FPM
- MySQL
- Redis
- PHPMyAdmin

---

# Solution Exercice bonus 💡

```yaml
# docker-compose.yml
version: '3.8'

services:
  nginx:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./html:/var/www/html
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - php

  php:
    image: php:8.2-fpm
    volumes:
      - ./html:/var/www/html
    environment:
      - MYSQL_HOST=mysql
      - REDIS_HOST=redis

  mysql:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: webapp
      MYSQL_USER: user
      MYSQL_PASSWORD: userpass
    volumes:
      - mysql_data:/var/lib/mysql

  redis:
    image: redis:latest

  phpmyadmin:
    image: phpmyadmin:latest
    ports:
      - "8080:80"
    environment:
      PMA_HOST: mysql
      PMA_USER: root
      PMA_PASSWORD: rootpass

volumes:
  mysql_data:
```

**Démarrer :**

```bash
docker-compose up -d
docker-compose ps
docker-compose logs -f
```

---

# Points clés à retenir 📌

**Nginx :**
- Virtual hosts pour multi-sites
- Reverse proxy pour applications
- SSL/TLS obligatoire en production

**Bases de données :**
- Sauvegardes régulières
- Utilisateurs avec droits minimaux
- Sécurisation post-installation

**Monitoring :**
- Métriques essentielles
- Alertes automatiques
- Logs centralisés

