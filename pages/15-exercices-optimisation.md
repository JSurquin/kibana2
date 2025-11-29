---
layout: exercices
---

# Exercices - Optimisation & Performances (BONUS) 🎯

**Module 6 : Mise en pratique**

---

# Exercice 1 : Optimiser la mémoire 💾

**Objectif :** Configurer swappiness

**Consignes :**

1. Vérifier la valeur actuelle de swappiness
2. La passer à 10 temporairement
3. Vérifier le changement
4. Rendre la modification permanente dans /etc/sysctl.conf
5. Recharger la configuration

**Durée :** 10 minutes

---

# Exercice 1 : Solution 💡

```bash
# 1. Valeur actuelle
sysctl vm.swappiness
# ou
cat /proc/sys/vm/swappiness

# 2. Modifier temporairement
sudo sysctl vm.swappiness=10

# 3. Vérifier
sysctl vm.swappiness

# 4. Permanent
echo "vm.swappiness=10" | sudo tee -a /etc/sysctl.conf

# 5. Recharger
sudo sysctl -p

# Vérification finale
sysctl vm.swappiness
```

---

# Exercice 2 : Augmenter les limites fichiers 📂

**Objectif :** Passer la limite à 65535 fichiers ouverts

**Consignes :**

1. Vérifier la limite actuelle : `ulimit -n`
2. Éditer /etc/security/limits.conf
3. Ajouter les lignes pour * soft/hard nofile
4. Redémarrer la session (logout/login)
5. Vérifier la nouvelle limite

**Durée :** 10 minutes

---

# Exercice 2 : Solution 💡

```bash
# 1. Limite actuelle
ulimit -n

# 2. Éditer limits.conf
sudo nano /etc/security/limits.conf

# 3. Ajouter à la fin:
* soft nofile 65535
* hard nofile 65535

# 4. Enregistrer et fermer

# 5. Logout puis login
exit
# Se reconnecter

# 6. Vérifier
ulimit -n
# Devrait afficher 65535
```

---

# Exercice 3 : Optimiser Nginx 🚀

**Objectif :** Activer gzip et ajuster les workers

**Consignes :**

1. Éditer /etc/nginx/nginx.conf
2. Configurer worker_processes à auto
3. Activer gzip avec types appropriés
4. Tester la configuration
5. Recharger Nginx
6. Vérifier avec curl que gzip est actif

**Durée :** 15 minutes

---

# Exercice 3 : Solution 💡

```bash
# 1. Éditer la config
sudo nano /etc/nginx/nginx.conf

# 2-3. Modifier:
user www-data;
worker_processes auto;

events {
    worker_connections 1024;
}

http {
    # Gzip
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_min_length 1000;
    
    # ... reste de la config
}

# 4. Tester
sudo nginx -t

# 5. Recharger
sudo systemctl reload nginx

# 6. Vérifier gzip
curl -H "Accept-Encoding: gzip" -I http://localhost
# Chercher: Content-Encoding: gzip
```

---

# Exercice 4 : Tuned (Red Hat/CentOS uniquement) 🎚️

**Objectif :** Appliquer un profil d'optimisation

**Prérequis :** RHEL/CentOS/AlmaLinux/Rocky

**Consignes :**

1. Installer tuned
2. Lister les profils disponibles
3. Voir le profil actif
4. Appliquer le profil throughput-performance
5. Vérifier l'application

**Durée :** 10 minutes

---

# Exercice 4 : Solution 💡

```bash
# 1. Installation
sudo dnf install tuned -y

# Démarrer
sudo systemctl enable --now tuned

# 2. Lister les profils
tuned-adm list

# 3. Profil actif
tuned-adm active

# 4. Appliquer le profil
sudo tuned-adm profile throughput-performance

# 5. Vérifier
tuned-adm active
# Devrait afficher: Current active profile: throughput-performance

# Voir les changements appliqués
tuned-adm profile_info
```

---

# Exercice 5 : Optimiser MySQL avec MySQLTuner 🗄️

**Objectif :** Analyser et optimiser MySQL

**Consignes :**

1. Télécharger MySQLTuner
2. Rendre exécutable
3. Exécuter le script
4. Noter les recommandations principales
5. Appliquer 2-3 recommandations dans my.cnf
6. Redémarrer MySQL
7. Relancer MySQLTuner pour voir l'amélioration

**Durée :** 20 minutes

---

# Exercice 5 : Solution 💡

```bash
# 1-2. Télécharger et rendre exécutable
wget https://raw.githubusercontent.com/major/MySQLTuner-perl/master/mysqltuner.pl
chmod +x mysqltuner.pl

# 3. Exécuter
sudo ./mysqltuner.pl

# Le script demande les identifiants MySQL root
# Puis affiche les recommandations

# 4. Exemple de recommandations:
# - Augmenter innodb_buffer_pool_size
# - Ajuster max_connections
# - Activer slow_query_log

# 5. Appliquer
sudo nano /etc/mysql/my.cnf

# Ajouter/modifier:
[mysqld]
innodb_buffer_pool_size = 1G
max_connections = 200
slow_query_log = 1
slow_query_log_file = /var/log/mysql/slow.log

# 6. Redémarrer
sudo systemctl restart mysql

# 7. Re-tester
sudo ./mysqltuner.pl
# Les warnings devraient avoir diminué
```

---

# Projet BONUS : Benchmark avant/après ⚡

**Objectif :** Mesurer l'impact des optimisations

**Outils :** Apache Bench (ab)

**Étapes :**

1. Installer Apache Bench : `sudo apt install apache2-utils`
2. Tester Nginx AVANT optimisations :
   ```bash
   ab -n 1000 -c 10 http://localhost/
   ```
3. Noter le "Requests per second"
4. Appliquer les optimisations Nginx (exercice 3)
5. Re-tester APRÈS :
   ```bash
   ab -n 1000 -c 10 http://localhost/
   ```
6. Comparer les résultats

**Objectif :** Gain de +20% en requests/sec

---

# Projet BONUS : Solution 💡

```bash
# 1. Installer Apache Bench
sudo apt install apache2-utils -y

# 2. Benchmark AVANT
ab -n 1000 -c 10 http://localhost/
# Noter: Requests per second: XXX [#/sec]

# 3. Optimiser Nginx (voir exercice 3)
# - worker_processes auto
# - gzip on
# - keepalive_timeout 65

# 4. Benchmark APRÈS
ab -n 1000 -c 10 http://localhost/
# Noter: Requests per second: YYY [#/sec]

# 5. Calculer le gain
# Gain % = ((YYY - XXX) / XXX) * 100

# Exemple:
# Avant: 500 req/sec
# Après: 650 req/sec
# Gain: +30%
```

---

# Checklist optimisation complète ✅

**Système optimisé si :**

□ vm.swappiness = 10  
□ Limites fichiers = 65535  
□ Paramètres réseau TCP ajustés (somaxconn, syn_backlog)  
□ Nginx : worker_processes auto + gzip  
□ MySQL : buffer_pool ajusté + slow_query activé  
□ MySQLTuner exécuté et recommandations appliquées  
□ Benchmarks avant/après documentés

---

# Récapitulatif des exercices BONUS ✅

**Compétences acquises :**

✅ Configuration sysctl (swappiness, réseau)

✅ Limites fichiers ouverts

✅ Optimisation Nginx (workers, gzip)

✅ Profils tuned (Red Hat)

✅ Analyse MySQL avec MySQLTuner

✅ Benchmarking avec Apache Bench

**Important :** Toujours mesurer l'impact réel des optimisations !

---
layout: default
---

# Bravo ! 🎉

Vous connaissez maintenant les bases de l'optimisation système.

**Pour aller plus loin :**
- Profiling avancé (perf, flamegraphs)
- Cache distribué (Redis, Memcached, Varnish)
- Load balancing et haute disponibilité
- Monitoring APM (Application Performance Monitoring)

