---
layout: new-section
---

<a name="optimisation-performances" id="optimisation-performances"></a>

# ⚡ Module 6 (BONUS)
## Optimisation & Performances

### Aller plus loin dans l'optimisation système

---

#### Quand optimiser ? 🤔

<div class="text-xs">

**Ne pas optimiser prématurément !**

Citation célèbre :
> "Premature optimization is the root of all evil" - Donald Knuth

**Optimiser SEULEMENT si :**
- Problème de performance identifié
- Goulet d'étranglement localisé
- Impact business réel
- Données de monitoring disponibles

**Démarche :**
1. **Mesurer** (monitoring)
2. **Identifier** le problème
3. **Optimiser** la zone critique
4. **Re-mesurer** l'impact

</div>

---

# sysctl : tuning du noyau ⚙️

**sysctl** : modifier les paramètres du noyau Linux à chaud

```bash
# Voir tous les paramètres
sysctl -a

# Voir un paramètre spécifique
sysctl vm.swappiness

# Modifier temporairement
sudo sysctl vm.swappiness=10

# Appliquer depuis /etc/sysctl.conf
sudo sysctl -p
```

**Modifications temporaires** = perdues au redémarrage  
**Modifications permanentes** = dans `/etc/sysctl.conf`

---

# Optimisation mémoire : swappiness 💾

**vm.swappiness** : tendance à utiliser le swap (0-100)

- **100** : swap agressif (par défaut anciennes versions)
- **60** : valeur par défaut moderne
- **10** : swap minimal (serveurs avec beaucoup de RAM)
- **0** : pas de swap sauf urgence absolue

**Pour un serveur avec 8 Go+ de RAM :**

```bash
# Temporaire
sudo sysctl vm.swappiness=10

# Permanent
echo "vm.swappiness=10" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

---

# Optimisation réseau : paramètres TCP 🌐

**Pour un serveur web haute charge :**

```bash
sudo nano /etc/sysctl.conf
```

```ini
# Augmenter la file d'attente de connexions
net.core.somaxconn = 4096

# Augmenter les backlog SYN
net.ipv4.tcp_max_syn_backlog = 8096

# Réduire le timeout FIN_WAIT
net.ipv4.tcp_fin_timeout = 30

# Activer la réutilisation des sockets TIME_WAIT
net.ipv4.tcp_tw_reuse = 1

# Augmenter les buffers réseau
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
```

```bash
sudo sysctl -p
```

---

# Limites fichiers ouverts 📂

**Problème courant** : "Too many open files"

```bash
# Voir la limite actuelle
ulimit -n

# Augmenter temporairement
ulimit -n 65535
```

**Permanent : `/etc/security/limits.conf`**

```
* soft nofile 65535
* hard nofile 65535
```

**Pour systemd services :** dans le fichier .service

```ini
[Service]
LimitNOFILE=65535
```

---

# Tuned : profiles d'optimisation automatiques 🎚️

**tuned** : daemon qui applique des profils d'optimisation (Red Hat)

```bash
# Installer
sudo dnf install tuned

# Démarrer
sudo systemctl enable --now tuned

# Voir les profils disponibles
tuned-adm list

# Profils courants:
# - throughput-performance : serveurs haute performance
# - latency-performance : faible latence
# - network-latency : optimisé réseau
# - virtual-guest : VM invitée
```

---

# Utiliser tuned 🔧

```bash
# Voir le profil actif
tuned-adm active

# Appliquer un profil
sudo tuned-adm profile throughput-performance

# Recommandation automatique
sudo tuned-adm recommend

# Désactiver
sudo tuned-adm off
```

**Avantage** : optimisation automatique sans éditer sysctl manuellement !

---

# Nginx : optimisations de base 🚀

**Fichier : `/etc/nginx/nginx.conf`**

```nginx
# Processus = nombre de CPUs
worker_processes auto;

# Connexions par worker
events {
    worker_connections 1024;
    use epoll;
}

http {
    # Keepalive
    keepalive_timeout 65;
    keepalive_requests 100;
    
    # Compression
    gzip on;
    gzip_types text/plain text/css application/json application/javascript;
    gzip_min_length 1000;
}
```

---

# Nginx : cache FastCGI (PHP) 💨

**Accélérer PHP avec cache FastCGI :**

```nginx
# Dans http
fastcgi_cache_path /var/cache/nginx levels=1:2 keys_zone=APPCACHE:100m inactive=60m;
fastcgi_cache_key "$scheme$request_method$host$request_uri";

# Dans location ~ \.php$
fastcgi_cache APPCACHE;
fastcgi_cache_valid 200 60m;
fastcgi_cache_bypass $http_pragma $http_authorization;
add_header X-Cache-Status $upstream_cache_status;
```

**Résultat** : PHP exécuté 1x, puis servi depuis le cache pendant 60min

---

# MySQL : optimisations essentielles 🗄️

**Fichier : `/etc/mysql/my.cnf` ou `/etc/mysql/mysql.conf.d/mysqld.cnf`**

```ini
[mysqld]
# Buffer pool = 70-80% de la RAM disponible
innodb_buffer_pool_size = 2G

# Logs
innodb_log_file_size = 256M
innodb_flush_log_at_trx_commit = 2

# Connexions
max_connections = 200

# Query cache (si MySQL < 8.0)
query_cache_size = 64M
query_cache_type = 1
```

```bash
sudo systemctl restart mysql
```

---

# MySQL : identifier les requêtes lentes 🐌

**Activer le slow query log :**

```bash
sudo nano /etc/mysql/my.cnf
```

```ini
[mysqld]
slow_query_log = 1
slow_query_log_file = /var/log/mysql/slow.log
long_query_time = 2
log_queries_not_using_indexes = 1
```

```bash
sudo systemctl restart mysql

# Analyser les requêtes lentes
sudo mysqldumpslow /var/log/mysql/slow.log
```

---

# MySQLTuner : script d'analyse 📊

**MySQLTuner** : script qui analyse votre MySQL et recommande des optimisations

```bash
# Télécharger
wget https://raw.githubusercontent.com/major/MySQLTuner-perl/master/mysqltuner.pl
chmod +x mysqltuner.pl

# Exécuter
sudo ./mysqltuner.pl

# Le script affiche:
# - Statistiques actuelles
# - Recommandations d'optimisation
# - Paramètres à ajuster
```

**Suivre les recommandations** en ajustant my.cnf

---

# Cache applicatif : Redis ⚡

**Redis** : cache en mémoire clé-valeur

```bash
# Installer
sudo apt install redis-server

# Configurer en mémoire uniquement
sudo nano /etc/redis/redis.conf
# maxmemory 256mb
# maxmemory-policy allkeys-lru

# Redémarrer
sudo systemctl restart redis-server
```

**Utilisation** : cache de sessions, résultats de requêtes DB, pages HTML...

**Exemple en PHP :** remplacer les sessions fichier par Redis

---

#### Checklist optimisation serveur web ✅

<div class="text-xs">

**Pour un serveur web classique (Nginx + PHP + MySQL) :**

□ **Système**
- vm.swappiness = 10
- Limites fichiers ouverts = 65535
- Profile tuned : throughput-performance

□ **Nginx**
- worker_processes auto
- gzip activé
- keepalive configuré
- Cache FastCGI pour PHP

□ **MySQL**
- innodb_buffer_pool_size ajusté (70% RAM)
- Slow query log activé
- MySQLTuner exécuté

</div>

---

#### Récapitulatif du Module 6 (BONUS) ✅

<div class="text-xs">

**Ce que vous avez appris :**

- ✅ Tuning mémoire (swappiness)
- ✅ Optimisation réseau (TCP)
- ✅ Limites fichiers ouverts
- ✅ Profiles tuned automatiques
- ✅ Optimisation Nginx (workers, gzip, cache)
- ✅ Optimisation MySQL (buffer pool, slow queries)
- ✅ MySQLTuner pour l'analyse
- ✅ Cache Redis

**Important** : toujours mesurer avant/après !

</div>

---

# Félicitations ! 🎉

Vous avez complété la formation **Administration Serveur Linux** !

**Compétences acquises :**
- Supervision système (top, htop, iotop, sar)
- Monitoring avancé (Zabbix)
- Optimisation performances (sysctl, tuned)
- Configuration NTP/DHCP

**Prochaines étapes :**
- Pratiquer sur des serveurs réels
- Explorer d'autres outils (Prometheus, Ansible)
- Se former aux conteneurs (Docker, Kubernetes)

