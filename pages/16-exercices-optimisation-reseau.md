---
layout: exercices
---

# Exercices - Optimisation Réseau Linux (BONUS) 🎯

**Module BONUS : Mise en pratique**

---

# Exercice 1 : Optimiser TCP avec sysctl 🧠

**Objectif :** Configurer les paramètres TCP pour haute performance

**Consignes :**

1. Voir les paramètres TCP actuels
2. Éditer /etc/sysctl.conf
3. Augmenter les buffers TCP (rmem/wmem)
4. Augmenter somaxconn à 4096
5. Activer BBR si disponible
6. Appliquer les changements
7. Vérifier l'application

**Durée :** 15 minutes

---

# Exercice 1 : Solution 💡

```bash
# 1. Voir paramètres actuels
sysctl -a | grep -E "net.core.rmem_max|net.ipv4.tcp_congestion"

# 2. Éditer
sudo nano /etc/sysctl.conf

# 3-5. Ajouter:
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216
net.core.somaxconn = 4096
net.ipv4.tcp_max_syn_backlog = 8192
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr

# 6. Appliquer
sudo sysctl -p
```

---

# Exercice 1 : Solution (suite) 💡

```bash
# 7. Vérifier
sysctl net.ipv4.tcp_congestion_control
sysctl net.core.somaxconn

# Vérifier les algorithmes disponibles
cat /proc/sys/net/ipv4/tcp_available_congestion_control
```

---

# Exercice 2 : Optimiser la carte réseau 📡

**Objectif :** Activer les optimisations matérielles avec ethtool

**Consignes :**

1. Identifier votre interface réseau (ip a)
2. Voir la configuration actuelle avec ethtool
3. Activer TSO (TCP Segmentation Offload)
4. Activer GRO (Generic Receive Offload)
5. Activer GSO (Generic Segmentation Offload)
6. Vérifier l'activation
7. Rendre permanent dans /etc/network/interfaces ou systemd

**Durée :** 15 minutes

---

# Exercice 2 : Solution 💡

```bash
# 1. Identifier l'interface
ip addr show
# Exemple: eth0, ens33, enp0s3...

# 2. Voir config actuelle
sudo ethtool -k eth0 | grep -E "tcp-segmentation|generic-receive"

# 3-5. Activer les offloads
sudo ethtool -K eth0 tso on
sudo ethtool -K eth0 gro on
sudo ethtool -K eth0 gso on
```

---

# Exercice 2 : Solution (suite) 💡

```bash
# 6. Vérifier
sudo ethtool -k eth0 | grep -E "tcp-segmentation|generic-receive|generic-segmentation"

# 7. Rendre permanent (systemd)
sudo nano /etc/systemd/network/10-eth0.link
```

---

# Exercice 2 : Configuration systemd 💡

Contenu du fichier `/etc/systemd/network/10-eth0.link` :

```ini
[Match]
MACAddress=XX:XX:XX:XX:XX:XX

[Link]
TCPSegmentationOffload=yes
GenericReceiveOffload=yes
GenericSegmentationOffload=yes
```

---

# Exercice 3 : Configurer fq_codel ⚖️

**Objectif :** Éliminer le bufferbloat avec fq_codel

**Consignes :**

1. Voir la discipline de file d'attente actuelle
2. Installer fq_codel sur l'interface principale
3. Vérifier l'installation
4. Tester la latence avec ping avant/après
5. Rendre permanent

**Durée :** 15 minutes

---

# Exercice 3 : Solution 💡

```bash
# 1. Voir la discipline actuelle
tc qdisc show dev eth0

# 2. Installer fq_codel
sudo tc qdisc replace dev eth0 root fq_codel

# 3. Vérifier
tc qdisc show dev eth0
# Devrait afficher: qdisc fq_codel ...

# 4. Tester la latence (en charge)
ping -c 10 8.8.8.8
# Faire un téléchargement simultanément
# wget http://speedtest.tele2.net/100MB.zip
# Observer si la latence reste stable
```

---

# Exercice 3 : Solution (suite) 💡

```bash
# 5. Rendre permanent
sudo nano /etc/rc.local
# Ajouter avant exit 0 :
# #!/bin/bash
# tc qdisc replace dev eth0 root fq_codel

# Rendre exécutable
sudo chmod +x /etc/rc.local
```

---

# Exercice 4 : Configurer nftables 🔐

**Objectif :** Mettre en place un pare-feu performant

**Consignes :**

1. Installer nftables
2. Créer une configuration de base
3. Autoriser localhost sans filtrage
4. Autoriser SSH, HTTP, HTTPS
5. Drop par défaut le reste
6. Activer le service
7. Tester les connexions

**Durée :** 20 minutes

---

# Exercice 4 : Solution 💡

```bash
# 1. Installer
sudo apt install nftables -y

# 2. Éditer la configuration
sudo nano /etc/nftables.conf
```

---

# Exercice 4 : Solution (config) 💡

```bash
#!/usr/sbin/nft -f

flush ruleset

table inet filter {
    chain input {
        type filter hook input priority 0; policy drop;
        
        # Loopback
        iif lo accept
        
        # Connection tracking
        ct state established,related accept
        ct state invalid drop
        
        # SSH
        tcp dport 22 accept
        
        # HTTP/HTTPS
        tcp dport { 80, 443 } accept
        
        # ICMP (ping)
        icmp type echo-request accept
    }
    
    chain forward {
        type filter hook forward priority 0; policy drop;
    }
    
    chain output {
        type filter hook output priority 0; policy accept;
    }
}
```

---

# Exercice 4 : Solution (activation) 💡

```bash
# 6. Activer
sudo systemctl enable --now nftables

# Recharger la config
sudo nft -f /etc/nftables.conf

# Vérifier
sudo nft list ruleset

# 7. Tester
# SSH doit fonctionner
ssh user@localhost

# HTTP doit fonctionner
curl http://localhost

# Autres ports doivent être bloqués
telnet localhost 3306
# Connection refused ou timeout
```

---

# Exercice 5 : Tester avec iperf3 📊

**Objectif :** Mesurer le débit réseau avant/après optimisations

**Consignes :**

1. Installer iperf3 sur 2 machines (ou 2 VMs)
2. Mesurer le débit AVANT optimisations
3. Appliquer les optimisations TCP (exercice 1)
4. Appliquer les optimisations NIC (exercice 2)
5. Mesurer le débit APRÈS optimisations
6. Comparer les résultats
7. Documenter le gain

**Durée :** 20 minutes

---

# Exercice 5 : Solution 💡

```bash
# 1. Installer sur les 2 machines
sudo apt install iperf3 -y

# 2. Machine A (serveur)
iperf3 -s

# Machine B (client) - Test AVANT
iperf3 -c 192.168.1.100 -t 30
# Noter: [ ID] Interval   Transfer  Bitrate

# 3-4. Appliquer les optimisations (ex 1 et 2)
# Sur les deux machines !

# 5. Machine A (serveur)
iperf3 -s
```

---

# Exercice 5 : Solution (suite) 💡

```bash
# Machine B (client) - Test APRÈS
iperf3 -c 192.168.1.100 -t 30
# Noter: [ ID] Interval   Transfer  Bitrate

# Test avec plusieurs connexions parallèles
iperf3 -c 192.168.1.100 -P 10 -t 30

# 6-7. Comparer
# Exemple:
# AVANT: 800 Mbits/sec
# APRÈS: 940 Mbits/sec
# GAIN: +17.5%
```

---

# Exercice 6 : Optimiser Docker réseau 🐋

**Objectif :** Améliorer les performances réseau des conteneurs

**Consignes :**

1. Lancer un conteneur Nginx en mode bridge (défaut)
2. Tester avec ab (Apache Bench)
3. Relancer en mode host
4. Tester à nouveau
5. Comparer les performances
6. Ajuster le MTU Docker si nécessaire

**Durée :** 20 minutes

---

# Exercice 6 : Solution 💡

```bash
# 1. Mode bridge (défaut)
docker run -d -p 8080:80 --name nginx-bridge nginx

# 2. Tester
sudo apt install apache2-utils -y
ab -n 10000 -c 100 http://localhost:8080/
# Noter: Requests per second

# Arrêter
docker stop nginx-bridge && docker rm nginx-bridge

# 3. Mode host
docker run -d --network host --name nginx-host nginx
```

---

# Exercice 6 : Solution (suite) 💡

```bash
# 4. Tester à nouveau
ab -n 10000 -c 100 http://localhost/
# Noter: Requests per second

# 5. Comparer
# Mode bridge: ~5000 req/sec (exemple)
# Mode host: ~8000 req/sec (exemple)
# Gain: +60%

# 6. Ajuster MTU
sudo nano /etc/docker/daemon.json
# Ajouter: { "mtu": 1450 }

# Redémarrer Docker
sudo systemctl restart docker
```

---

# Exercice 7 : Monitoring réseau en temps réel 📡

**Objectif :** Surveiller le trafic réseau

**Consignes :**

1. Installer iftop
2. Surveiller eth0 en temps réel
3. Installer et utiliser ss pour voir les connexions
4. Utiliser tcpdump pour capturer le trafic HTTP
5. Analyser une capture avec Wireshark (optionnel)

**Durée :** 15 minutes

---

# Exercice 7 : Solution 💡

```bash
# 1-2. Installer et utiliser iftop
sudo apt install iftop -y
sudo iftop -i eth0 -P -n
# -P: afficher les ports
# -n: ne pas résoudre les noms
# Appuyer sur 'q' pour quitter

# 3. Utiliser ss
# Voir toutes les connexions TCP
ss -tan

# Voir les processus écoutant
sudo ss -tlnp

# Statistiques résumées
ss -s
```

---

# Exercice 7 : Solution (suite) 💡

```bash
# 4. Capturer avec tcpdump
# Capturer le trafic HTTP pendant 60 secondes
sudo tcpdump -i eth0 port 80 -w capture.pcap -c 1000

# Voir le contenu (ASCII)
sudo tcpdump -i eth0 port 80 -A

# Filtrer par IP
sudo tcpdump -i eth0 host 192.168.1.100

# 5. Analyser avec Wireshark (sur poste local)
# Copier capture.pcap vers le poste local
# Ouvrir avec: wireshark capture.pcap
```

---

# Projet BONUS : Serveur web haute performance 🚀

**Objectif :** Configurer un serveur Nginx optimisé pour haute charge

**Étapes complètes :**

1. Installer Nginx
2. Appliquer TOUTES les optimisations système (TCP, NIC, fq_codel)
3. Optimiser Nginx (workers, keepalive, buffers)
4. Configurer nftables
5. Benchmark avec ab : 100 000 requêtes, 500 connexions simultanées
6. Objectif : > 10 000 req/sec

**Durée :** 45 minutes

---

# Projet BONUS : Solution (1/4) 💡

```bash
# 1. Installer Nginx
sudo apt update
sudo apt install nginx -y

# 2. Optimisations système
sudo nano /etc/sysctl.conf
# Ajouter les paramètres TCP ci-dessous
```

---

# Projet BONUS : Configuration sysctl 💡

Contenu à ajouter dans `/etc/sysctl.conf` :

```ini
# TCP
net.core.rmem_max = 268435456
net.core.wmem_max = 268435456
net.ipv4.tcp_rmem = 4096 87380 134217728
net.ipv4.tcp_wmem = 4096 65536 134217728
net.core.somaxconn = 65535
net.ipv4.tcp_max_syn_backlog = 20480
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_tw_reuse = 1
```

---

# Projet BONUS : Solution (2/4) 💡

Ajouter aussi dans `/etc/sysctl.conf` :

```ini
# Congestion
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
```

---

# Projet BONUS : Appliquer les optimisations 💡

```bash
# Appliquer sysctl
sudo sysctl -p

# Optimiser NIC
sudo ethtool -K eth0 tso on gro on gso on

# fq_codel
sudo tc qdisc replace dev eth0 root fq_codel
```

---

# Projet BONUS : Solution (3/4) 💡

```bash
# 3. Optimiser Nginx
sudo nano /etc/nginx/nginx.conf
# Utiliser la configuration des slides suivantes
```

---

# Projet BONUS : Configuration Nginx (partie 1) 💡

```nginx
user www-data;
worker_processes auto;
worker_rlimit_nofile 100000;
pid /run/nginx.pid;

events {
    worker_connections 10000;
    use epoll;
    multi_accept on;
}

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    keepalive_requests 10000;
    types_hash_max_size 2048;
```

---

# Projet BONUS : Configuration Nginx (partie 2) 💡

```nginx
    # Gzip
    gzip on;
    gzip_vary on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml text/javascript;
    
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    
    access_log off;
    error_log /var/log/nginx/error.log crit;
    
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

---

# Projet BONUS : Tester et appliquer 💡

```bash
# Tester la configuration
sudo nginx -t

# Recharger Nginx
sudo systemctl reload nginx
```

---

# Projet BONUS : Benchmark final 💡

```bash
# 4. nftables (voir exercice 4)

# 5. Benchmark final
ab -n 100000 -c 500 http://localhost/

# Résultats attendus:
# Requests per second: > 10000 [#/sec]
# Time per request: < 50 [ms] (mean)
# Failed requests: 0

# Si échec, vérifier:
ulimit -n  # Doit être >= 65535
sudo systemctl status nginx
dmesg | tail  # Erreurs système
```

---

# Checklist optimisation réseau complète ✅

<div class="text-sm">

**Système optimisé si :**

□ **TCP/IP** : Buffers élargis, BBR activé, backlog augmenté  
□ **NIC** : TSO/GRO/GSO activés  
□ **Queue** : fq_codel ou cake installé  
□ **Pare-feu** : nftables optimisé  
□ **Conteneurs** : MTU ajusté, mode réseau optimal  
□ **Monitoring** : iperf3, ss, iftop maîtrisés  
□ **Benchmarks** : avant/après documentés

</div>

---

# Récapitulatif des exercices ✅

**Compétences acquises :**

✅ Tuning TCP/IP avec sysctl  
✅ Optimisation carte réseau (ethtool)  
✅ Gestion des files d'attente (tc, fq_codel)  
✅ Pare-feu moderne (nftables)  
✅ Tests de performance (iperf3, ab)  
✅ Optimisation conteneurs Docker  
✅ Monitoring réseau (iftop, ss, tcpdump)

---

# Récapitulatif des exercices ✅

**Gains typiques observés :**

- 🚀 +15-20% débit réseau (TCP optimisé)
- 🎯 -40% latence sous charge (fq_codel)
- 💪 +60% performance conteneurs (mode host)
- ⚡ +30% requêtes/sec serveur web

**Important :** Toujours mesurer avant/après !

---
layout: default
---

# Félicitations ! 🎉

Vous maîtrisez maintenant l'optimisation réseau avancée sous Linux !

**Pour aller plus loin :**
- eBPF et XDP (eXpress Data Path)
- DPDK (Data Plane Development Kit)
- Load balancing avancé (HAProxy, Traefik)
- Tuning kernel avancé (IRQ affinity, NUMA)
- Réseau haute performance 10G/40G/100G

**Ressources :**
- kernel.org/doc/Documentation/networking/
- brendangregg.com/blog/ (performance)
- netdevconf.org (conférences)
