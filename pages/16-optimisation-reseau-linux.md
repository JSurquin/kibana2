---
layout: new-section
routeAlias: 'optimisation-reseau-linux'
---

<a name="optimisation-reseau-linux" id="optimisation-reseau-linux"></a>

# 🌐 Module BONUS
## Optimisation Réseau sous Linux

### Comprendre et optimiser la pile réseau

---

# "Optimiser le réseau" sur Linux 🤔

## C'est quoi concrètement ?

**C'est l'ensemble des techniques qui visent à :**

- 🔻 **Réduire la latence**
- 🔺 **Augmenter le débit**
- 🧠 **Réduire la charge CPU**
- 🛡 **Améliorer la stabilité**

---

# "Optimiser le réseau" sur Linux 🤔

## C'est quoi concrètement ? (suite)

**C'est l'ensemble des techniques qui visent à :**

- 📦 **Éviter la perte de paquets**
- ⚙️ **Adapter le comportement du noyau au type d'usage**

---

# En résumé 💡

<div class="text-center text-xl mt-10">

**On ne "rend pas Internet plus rapide"**

**On rend la machine plus efficace pour envoyer, recevoir et traiter le trafic.**

</div>

---

# Les 6 grands piliers 🏛️

## Optimisation réseau sous Linux

1. 🧠 Optimisation de la pile TCP/IP
2. 📡 Optimisation de la carte réseau (NIC)
3. ⚖️ File d'attente & congestion
4. 🔐 Pare-feu & filtrage
5. 🧱 Virtualisation & conteneurs
6. 📊 Supervision et diagnostic

---

# 1️⃣ Pile TCP/IP 🧠

**Basé sur le protocole TCP**

**Objectif :**
- Éviter la congestion
- Mieux gérer les files d'attente
- Accélérer les échanges

**Outils :**
- `sysctl`
- `/proc/sys/net/*`

---

# 1️⃣ Pile TCP/IP - Exemples 🧠

**Exemples de réglages :**

- Taille des buffers d'envoi/réception
- Nombre de connexions simultanées
- Algorithme de congestion (BBR, CUBIC…)

```bash
# Voir les paramètres réseau actuels
sysctl -a | grep net.ipv4
```

---

# 1️⃣ Optimisation TCP - Configuration

```bash
# Éditer la configuration
sudo nano /etc/sysctl.conf
```

```ini
# Augmenter les buffers TCP
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216

# Augmenter le backlog
net.core.somaxconn = 4096
net.ipv4.tcp_max_syn_backlog = 8192
```

---

# 1️⃣ Optimisation TCP - Configuration (suite)

```ini
# Réduire les timeouts
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 300
net.ipv4.tcp_keepalive_probes = 5
net.ipv4.tcp_keepalive_intvl = 15

# Réutiliser les sockets TIME_WAIT
net.ipv4.tcp_tw_reuse = 1

# Algorithme de congestion BBR (2025)
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
```

---

# 1️⃣ Appliquer les changements

```bash
# Appliquer immédiatement
sudo sysctl -p

# Vérifier l'algorithme actif
sysctl net.ipv4.tcp_congestion_control

# Voir les algorithmes disponibles
cat /proc/sys/net/ipv4/tcp_available_congestion_control
```

---

# 2️⃣ Carte réseau (NIC) 📡

**Matériel : Network Interface Card**

**Objectif :**
- Réduire l'overhead CPU
- Améliorer la gestion des interruptions

**Techniques :**
- **Offloading** (TSO, GRO, LRO)
- **Multiqueue**
- **IRQ Affinity**

**Outil principal :**
- `ethtool`

---

# 2️⃣ Offloading avec ethtool

```bash
# Voir la configuration actuelle
sudo ethtool -k eth0

# Activer le TSO (TCP Segmentation Offload)
sudo ethtool -K eth0 tso on

# Activer le GRO (Generic Receive Offload)
sudo ethtool -K eth0 gro on

# Activer le GSO (Generic Segmentation Offload)
sudo ethtool -K eth0 gso on
```

---

# 2️⃣ Statistiques réseau

```bash
# Voir les statistiques de la carte
sudo ethtool -S eth0

# Voir la vitesse et le mode duplex
sudo ethtool eth0

# Régler la taille du ring buffer
sudo ethtool -G eth0 rx 4096 tx 4096
```

---

# 2️⃣ Multiqueue et IRQ Affinity

**Multiqueue** : distribuer le trafic sur plusieurs CPU

```bash
# Voir le nombre de queues
ls /sys/class/net/eth0/queues/

# Voir l'affectation des IRQ
cat /proc/interrupts | grep eth0

# Affecter une IRQ à un CPU spécifique
echo "2" > /proc/irq/125/smp_affinity_list
```

---

# 3️⃣ File d'attente & congestion ⚖️

**Objectif :**
- Éviter le **bufferbloat**
- Donner la priorité au trafic important

**Algorithmes recommandés (2025) :**
- `fq` (Fair Queue)
- `fq_codel` (Fair Queue Controlled Delay)
- `cake` (Common Applications Kept Enhanced)

**Outil principal :**
- `tc` (Traffic Control)

---

# 3️⃣ Qu'est-ce que le bufferbloat ? 🤔

**Bufferbloat** = latence excessive causée par des buffers trop grands

**Symptômes :**
- Latence élevée quand le réseau est chargé
- Jeux en ligne qui "lag" pendant un téléchargement
- Vidéoconférence saccadée

**Solution :**
Utiliser un algorithme moderne comme `fq_codel` ou `cake`

---

# 3️⃣ Configurer fq_codel

```bash
# Voir la discipline actuelle
tc qdisc show dev eth0

# Installer fq_codel (moderne, par défaut sur kernel récent)
sudo tc qdisc replace dev eth0 root fq_codel
```

---

# 3️⃣ Configurer cake (plus avancé)

```bash
# Installer cake (peut nécessiter modules kernel)
sudo tc qdisc replace dev eth0 root cake bandwidth 100Mbit

# Avec priorité pour le trafic interactif
sudo tc qdisc replace dev eth0 root cake bandwidth 100Mbit besteffort

# Voir les statistiques cake
tc -s qdisc show dev eth0
```

---

# 4️⃣ Pare-feu & filtrage 🔐

**Objectif :**
Sécurité sans tuer les performances

**Technologies (2025) :**
- `iptables` (classique)
- **`nftables`** (moderne, recommandé)

---

# 4️⃣ Optimisations pare-feu

**Bonnes pratiques :**

- ✅ Éviter les règles inutiles
- ✅ Ordre des règles optimal (plus fréquentes en premier)
- ✅ Bypass pour les flux internes (localhost)
- ✅ Utiliser les ensembles (ipset) pour les listes d'IP
- ✅ Activer connection tracking sélectivement

---

# 4️⃣ Exemple optimisé avec nftables

```bash
# Installer nftables
sudo apt install nftables

# Éditer la configuration
sudo nano /etc/nftables.conf
```

---

# 4️⃣ Configuration nftables optimisée

```bash
#!/usr/sbin/nft -f

flush ruleset

table inet filter {
    chain input {
        type filter hook input priority 0; policy drop;
        
        # Accepter loopback sans filtrage
        iif lo accept
        
        # Connection tracking
        ct state established,related accept
        ct state invalid drop
        
        # SSH
        tcp dport 22 accept
        
        # HTTP/HTTPS
        tcp dport { 80, 443 } accept
    }
}
```

---

# 4️⃣ Appliquer nftables

```bash
# Activer nftables
sudo systemctl enable --now nftables

# Recharger la configuration
sudo nft -f /etc/nftables.conf

# Voir les règles actives
sudo nft list ruleset
```

---

# 5️⃣ Virtualisation & conteneurs 🧱

**Avec Docker / Podman**

**Problèmes fréquents :**
- NAT (Network Address Translation)
- Bridges virtuels
- Surcoût réseau

**Optimisations possibles :**
- Réseau `host`
- `macvlan`
- Bypass du bridge

---

# 5️⃣ Docker : mode réseau host

```bash
# Lancer un conteneur en mode host
docker run --network host nginx

# Plus de NAT, performances maximales
# ATTENTION : le conteneur partage le réseau de l'hôte
```

---

# 5️⃣ Docker : ajuster le MTU

```bash
# Éditer le daemon Docker
sudo nano /etc/docker/daemon.json
```

```json
{
  "mtu": 1450,
  "default-address-pools": [
    {
      "base": "172.80.0.0/16",
      "size": 24
    }
  ]
}
```

---

# 5️⃣ Redémarrer Docker

```bash
# Redémarrer le daemon
sudo systemctl restart docker

# Vérifier le MTU
docker network inspect bridge | grep Mtu
```

---

# 5️⃣ Podman : réseau slirp4netns vs netavark

**Podman 2025 utilise `netavark` par défaut (plus performant)**

```bash
# Vérifier le backend réseau
podman info | grep -i network

# Mode host avec Podman
podman run --network host nginx

# Créer un réseau macvlan
podman network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 macvlan-net
```

---

# 6️⃣ Supervision et diagnostic 📊

**Principe fondamental :**

<div class="text-center text-2xl mt-10">

**On n'optimise jamais sans mesurer !**

</div>

---

# 6️⃣ Outils essentiels

**Commandes de base :**

- `ip` : configuration réseau moderne
- `ss` : statistiques sockets (remplace netstat)
- `iftop` : monitoring bande passante en temps réel
- `iperf3` : tests de débit
- `tcpdump` : capture de paquets

---

# 6️⃣ Utiliser ip

```bash
# Afficher toutes les interfaces
ip addr show

# Afficher les statistiques
ip -s link show eth0

# Afficher la table de routage
ip route show

# Afficher les voisins (ARP)
ip neigh show
```

---

# 6️⃣ Utiliser ss

```bash
# Afficher toutes les connexions TCP
ss -tan

# Afficher les statistiques résumées
ss -s

# Connexions établies avec détails
ss -tiep

# Processus écoutant sur les ports
sudo ss -tlnp
```

---

# 6️⃣ Utiliser iftop

```bash
# Installer iftop
sudo apt install iftop

# Lancer iftop
sudo iftop -i eth0

# Options utiles:
# -P : afficher les ports
# -n : ne pas résoudre les noms
sudo iftop -i eth0 -P -n
```

---

# 6️⃣ Utiliser iperf3

```bash
# Installer iperf3
sudo apt install iperf3

# Serveur (machine A)
iperf3 -s

# Client (machine B) - test TCP
iperf3 -c 192.168.1.100

# Test UDP avec 1 Gbps
iperf3 -c 192.168.1.100 -u -b 1G

# Test parallèle (10 connexions)
iperf3 -c 192.168.1.100 -P 10
```

---

# 6️⃣ Utiliser tcpdump

```bash
# Capturer sur eth0
sudo tcpdump -i eth0

# Capturer et sauvegarder
sudo tcpdump -i eth0 -w capture.pcap

# Filtrer par port
sudo tcpdump -i eth0 port 80

# Filtrer par IP
sudo tcpdump -i eth0 host 192.168.1.100

# Voir le contenu des paquets (ASCII)
sudo tcpdump -i eth0 -A port 80
```

---

# Cas pratique : Serveur web 🖥

## Optimisations typiques

**Pour Nginx haute performance :**

- TCP backlog augmenté
- KeepAlive optimisé
- Buffers TCP élargis
- Multi-core network queues

---

# Cas pratique : Serveur web (config)

```bash
# Optimisations système
sudo nano /etc/sysctl.conf
```

```ini
# Backlog élevé
net.core.somaxconn = 65535
net.ipv4.tcp_max_syn_backlog = 20480

# Buffers
net.core.rmem_max = 268435456
net.core.wmem_max = 268435456
net.ipv4.tcp_rmem = 4096 87380 134217728
net.ipv4.tcp_wmem = 4096 65536 134217728
```

---

# Cas pratique : Serveur web (nginx)

```nginx
# /etc/nginx/nginx.conf
user www-data;
worker_processes auto;
worker_rlimit_nofile 65535;

events {
    worker_connections 4096;
    use epoll;
    multi_accept on;
}

http {
    keepalive_timeout 65;
    keepalive_requests 1000;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
}
```

---

# Cas pratique : Conteneurs 📦

## Problèmes courants

- Latence ajoutée par le bridge Docker
- NAT lent sur forte charge
- MTU inadapté

---

# Cas pratique : Conteneurs (solutions)

**Solutions :**

1. **Réseau host** pour performances maximales
2. **Ajustement MTU** selon le réseau
3. **Désactivation iptables automatique** si non nécessaire
4. **Utiliser macvlan** pour intégration directe

```bash
# Désactiver iptables Docker
sudo nano /etc/docker/daemon.json
```

```json
{
  "iptables": false,
  "ip-forward": false
}
```

---

# Cas pratique : Jeux en ligne 🎮

## Objectifs

- ⬇️ **Latence minimale**
- ⬇️ **Jitter réduit**

**Actions recommandées :**

- Utiliser `fq_codel` ou `cake`
- Priorité au trafic UDP
- Désactivation du bufferbloat

---

# Cas pratique : Jeux (configuration)

```bash
# Installer cake avec priorité jeux
sudo tc qdisc replace dev eth0 root cake bandwidth 100Mbit diffserv3

# Priorité UDP (gaming)
sudo iptables -t mangle -A POSTROUTING -p udp -j DSCP --set-dscp-class EF
```

---

# Cas pratique : Stockage réseau ☁️

## NFS / Samba

**Objectifs :**
- Débit maximal
- TCP window large
- Jumbo Frames (si supporté)

---

# Cas pratique : Stockage réseau (config)

```bash
# Activer Jumbo Frames (MTU 9000)
sudo ip link set eth0 mtu 9000

# Optimisations TCP pour gros transferts
sudo sysctl -w net.ipv4.tcp_window_scaling=1
sudo sysctl -w net.core.rmem_max=536870912
sudo sysctl -w net.core.wmem_max=536870912

# Configuration NFS optimisée
# /etc/exports
/data 192.168.1.0/24(rw,sync,no_subtree_check,no_root_squash)
```

---

# Cas pratique : Stockage réseau (montage)

```bash
# Montage NFS optimisé
sudo mount -t nfs -o \
  rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,tcp \
  192.168.1.100:/data /mnt/nfs
```

---

# Plan de cours complet 🎓

## Chapitre 1 : Ce qu'on cherche à optimiser

- Débit
- Latence
- CPU
- Stabilité
- Sécurité

---

# Plan de cours complet 🎓

## Chapitre 2 : Architecture réseau Linux

- Pile TCP/IP
- Carte réseau
- Files d'attente
- Routage
- Pare-feu

---

# Plan de cours complet 🎓

## Chapitre 3 : Outils de base

- `ip`
- `ss`
- `ethtool`
- `iperf3`
- `tc`

---

# Plan de cours complet 🎓

## Chapitre 4 : Optimisation TCP avec sysctl

- Buffers
- Connexions
- Congestion

---

# Plan de cours complet 🎓

## Chapitre 5 : Optimisation carte réseau

- Offload
- Interruptions
- Multiqueue

---

# Plan de cours complet 🎓

## Chapitre 6 : Cas des conteneurs

- Bridge vs Host
- NAT vs direct
- MTU

---

# Plan de cours complet 🎓

## Chapitre 7 : Mesure & validation

- Tests avant / après
- Graphes
- Interprétation

---

# Checklist d'optimisation complète ✅

<div class="text-sm">

**□ Pile TCP/IP**
- Buffers élargis (rmem/wmem)
- Algorithme BBR activé
- Backlog augmenté
- Réutilisation TIME_WAIT

**□ Carte réseau**
- Offloading activé (TSO, GRO)
- Ring buffers optimisés
- Multiqueue configuré

</div>

---

# Checklist d'optimisation complète ✅

<div class="text-sm">

**□ File d'attente**
- fq_codel ou cake installé
- Bufferbloat éliminé

**□ Pare-feu**
- nftables ou iptables optimisé
- Règles ordonnées efficacement
- Connection tracking optimisé

</div>

---

# Checklist d'optimisation complète ✅

<div class="text-sm">

**□ Conteneurs**
- MTU ajusté
- Mode réseau adapté
- NAT optimisé ou bypassé

**□ Supervision**
- iperf3 testé régulièrement
- ss et ip utilisés pour diagnostic
- tcpdump pour troubleshooting

</div>

---

# Récapitulatif du module 🎯

**Ce que vous avez appris :**

- ✅ Les 6 piliers de l'optimisation réseau
- ✅ Tuning TCP/IP avec sysctl
- ✅ Optimisation carte réseau (ethtool)
- ✅ Gestion des files d'attente (tc, fq_codel, cake)
- ✅ Optimisation pare-feu (nftables)

---

# Récapitulatif du module 🎯

**Ce que vous avez appris (suite) :**

- ✅ Optimisation conteneurs (Docker/Podman)
- ✅ Outils de diagnostic (ip, ss, iftop, iperf3, tcpdump)
- ✅ Cas pratiques (serveur web, jeux, stockage)

---

# Points clés à retenir 💡

<div class="text-center mt-10">

1. **Toujours mesurer avant d'optimiser**
2. **Optimiser selon le cas d'usage**
3. **Tester après chaque modification**
4. **Documenter les changements**
5. **Ne pas optimiser prématurément**

</div>

---

# Ressources complémentaires 📚

**Documentation officielle :**
- `man sysctl`
- `man tc`
- `man ethtool`
- `man iperf3`

**Ressources en ligne :**
- kernel.org/doc/Documentation/networking/
- nftables.org
- netfilter.org

---

# Félicitations ! 🎉

Vous maîtrisez maintenant l'optimisation réseau sous Linux !

**Compétences acquises :**
- Tuning avancé de la pile TCP/IP
- Optimisation matérielle
- Gestion des performances réseau
- Diagnostic et troubleshooting

**Prochaines étapes :**
- Pratiquer sur des serveurs réels
- Explorer eBPF pour l'optimisation avancée
- Se former à XDP (eXpress Data Path)
