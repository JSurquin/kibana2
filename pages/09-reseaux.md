---
layout: intro
routeAlias: 'reseaux'
---

# Réseaux et Connectivité 🌐

### Configuration et gestion des réseaux Linux

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Configurons les réseaux ensemble <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Concepts de base 🌍

### Architecture réseau Linux

**Interfaces réseau :**
- **eth0, enp0s3** : Interface Ethernet
- **wlan0, wlp2s0** : Interface WiFi
- **lo** : Interface loopback (127.0.0.1)
- **docker0, veth*** : Interfaces virtuelles Docker

**Adressage IP :**
- **IPv4** : 192.168.1.100/24
- **IPv6** : 2001:db8::1/64
- **Masque de sous-réseau** : 255.255.255.0
- **Passerelle** : 192.168.1.1

**Configuration réseau :**
- **DHCP** : Attribution automatique
- **Statique** : Configuration manuelle
- **Bonding** : Agrégation d'interfaces
- **Bridging** : Pont réseau

---
layout: default
---

# Commandes de base réseau 🔧

### Outils de diagnostic réseau

**ip (remplace ifconfig)**
```bash
ip addr show                   # Afficher les adresses IP
ip link show                   # Afficher les interfaces
ip route show                  # Afficher la table de routage
ip addr add 192.168.1.100/24 dev eth0  # Ajouter une IP
ip link set eth0 up            # Activer une interface
ip link set eth0 down          # Désactiver une interface
```

**ifconfig (obsolète mais encore utilisé)**
```bash
ifconfig                       # Afficher toutes les interfaces
ifconfig eth0                  # Interface spécifique
ifconfig eth0 192.168.1.100 netmask 255.255.255.0  # Configurer
ifconfig eth0 up               # Activer
ifconfig eth0 down             # Désactiver
```

**nmcli (NetworkManager CLI)**
```bash
nmcli device status            # Statut des interfaces
nmcli connection show          # Connexions configurées
nmcli device wifi list         # Réseaux WiFi disponibles
```

---
layout: default
---

# Configuration réseau statique ⚙️

### Configuration manuelle des interfaces

**Configuration avec ip :**
```bash
# Configurer une interface
ip addr add 192.168.1.100/24 dev eth0
ip route add default via 192.168.1.1 dev eth0

# Configurer DNS
echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "nameserver 8.8.4.4" >> /etc/resolv.conf
```

**Configuration avec netplan (Ubuntu) :**
```yaml
# /etc/netplan/01-netcfg.yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

**Application de la configuration :**
```bash
sudo netplan apply             # Appliquer la configuration
sudo netplan try               # Tester la configuration
```

---
layout: default
---

# Configuration DHCP 🌐

### Attribution automatique d'adresses

**Configuration DHCP client :**
```bash
# Demander une adresse DHCP
dhclient eth0                  # Client DHCP classique
dhcpcd eth0                    # Client DHCP alternatif

# Configuration automatique
systemctl enable systemd-networkd
systemctl enable systemd-resolved
```

**Configuration avec NetworkManager :**
```bash
# Activer DHCP sur une interface
nmcli connection modify "Wired connection 1" ipv4.method auto

# Voir la configuration DHCP
nmcli connection show "Wired connection 1"
```

**Serveur DHCP (dnsmasq) :**
```bash
# Installation
sudo apt install dnsmasq

# Configuration /etc/dnsmasq.conf
interface=eth0
dhcp-range=192.168.1.50,192.168.1.150,12h
dhcp-option=3,192.168.1.1
dhcp-option=6,8.8.8.8,8.8.4.4
```

---
layout: default
---

# Diagnostic réseau 🔍

### Outils de test et diagnostic

**ping (test de connectivité)**
```bash
ping google.com                # Test de connectivité
ping -c 4 google.com          # 4 pings seulement
ping -i 2 google.com          # Intervalle de 2 secondes
ping -s 1000 google.com       # Taille de paquet 1000 bytes
ping -t 64 google.com         # TTL de 64
```

**traceroute (tracer le chemin)**
```bash
traceroute google.com          # Tracer le chemin
traceroute -n google.com       # Sans résolution DNS
traceroute -I google.com       # Utiliser ICMP
mtr google.com                 # Traceroute en temps réel
```

**nslookup et dig (DNS)**
```bash
nslookup google.com            # Résolution DNS
nslookup 8.8.8.8              # Résolution inverse
dig google.com                 # Requête DNS détaillée
dig +short google.com          # Réponse courte
```

---
layout: default
---

# Monitoring réseau 📊

### Surveillance du trafic réseau

**netstat (statistiques réseau)**
```bash
netstat -tuln                  # Ports en écoute
netstat -i                     # Interfaces réseau
netstat -r                     # Table de routage
netstat -s                     # Statistiques
netstat -an | grep ESTABLISHED # Connexions établies
```

**ss (Socket Statistics)**
```bash
ss -tuln                       # Ports en écoute (moderne)
ss -i                          # Informations détaillées
ss -s                          # Statistiques
ss -t state established        # Connexions établies
```

**iftop (monitoring en temps réel)**
```bash
iftop                          # Trafic réseau en temps réel
iftop -i eth0                  # Interface spécifique
iftop -P                       # Afficher les ports
```

---
layout: default
---

# Pare-feu et sécurité réseau 🛡️

### Configuration des pare-feu

**ufw (Uncomplicated Firewall)**
```bash
sudo ufw enable                # Activer le pare-feu
sudo ufw status                # Statut du pare-feu
sudo ufw allow ssh             # Autoriser SSH
sudo ufw allow 80/tcp          # Autoriser HTTP
sudo ufw deny 22               # Refuser SSH
sudo ufw allow from 192.168.1.0/24  # Autoriser un réseau
```

**iptables (pare-feu avancé)**
```bash
# Lister les règles
sudo iptables -L
sudo iptables -L -n -v

# Ajouter des règles
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j DROP
sudo iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT

# Sauvegarder les règles
sudo iptables-save > /etc/iptables/rules.v4
```

**nftables (nouveau pare-feu)**
```bash
# Créer une table
sudo nft add table inet filter

# Créer une chaîne
sudo nft add chain inet filter input { type filter hook input priority 0 \; }

# Ajouter une règle
sudo nft add rule inet filter input tcp dport 80 accept
```

---
layout: default
---

# VPN et tunnels 🔐

### Configuration de tunnels sécurisés

**OpenVPN**
```bash
# Installation
sudo apt install openvpn

# Configuration client
sudo openvpn --config client.ovpn

# Configuration serveur
sudo openvpn --config server.conf
```

**WireGuard (VPN moderne)**
```bash
# Installation
sudo apt install wireguard

# Générer les clés
wg genkey | tee privatekey | wg pubkey > publickey

# Configuration interface
sudo ip link add wg0 type wireguard
sudo ip addr add 10.0.0.1/24 dev wg0
sudo wg set wg0 private-key < privatekey
sudo ip link set wg0 up
```

**SSH Tunnel**
```bash
# Tunnel local
ssh -L 8080:localhost:80 user@remote

# Tunnel distant
ssh -R 8080:localhost:80 user@remote

# Tunnel SOCKS
ssh -D 1080 user@remote
```

---
layout: default
---

# Services réseau 🌐

### Configuration des services réseau

**SSH (Secure Shell)**
```bash
# Installation
sudo apt install openssh-server

# Configuration /etc/ssh/sshd_config
Port 2222                      # Changer le port
PermitRootLogin no             # Interdire root
PasswordAuthentication no       # Désactiver les mots de passe
PubkeyAuthentication yes       # Activer les clés
AllowUsers user1 user2         # Utilisateurs autorisés

# Redémarrer SSH
sudo systemctl restart ssh
```

**Nginx (serveur web)**
```bash
# Installation
sudo apt install nginx

# Configuration /etc/nginx/sites-available/default
server {
    listen 80;
    server_name example.com;
    root /var/www/html;
    index index.html;
}

# Activer le site
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
sudo nginx -t                  # Tester la configuration
sudo systemctl restart nginx
```

---
layout: default
---

# DNS et résolution de noms 📝

### Configuration DNS

**Configuration DNS client :**
```bash
# Fichier /etc/resolv.conf
nameserver 8.8.8.8
nameserver 8.8.4.4
search example.com

# Configuration systemd-resolved
sudo systemctl enable systemd-resolved
sudo systemctl start systemd-resolved
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

**Serveur DNS local (dnsmasq) :**
```bash
# Configuration /etc/dnsmasq.conf
interface=eth0
bind-interfaces
domain=example.com
dhcp-range=192.168.1.50,192.168.1.150,12h
server=8.8.8.8
server=8.8.4.4
```

**Test de résolution DNS :**
```bash
# Test avec dig
dig @8.8.8.8 google.com
dig +short google.com
dig MX google.com

# Test avec nslookup
nslookup google.com 8.8.8.8
```

---
layout: default
---

# Agrégation et bonding 🔗

### Agrégation d'interfaces réseau

**Bonding (agrégation d'interfaces)**
```bash
# Configuration /etc/modules-load.d/bonding.conf
bonding

# Configuration /etc/sysconfig/network-scripts/ifcfg-bond0
DEVICE=bond0
TYPE=Bond
NAME=bond0
BONDING_OPTS="mode=1 miimon=100"
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
```

**Configuration des interfaces esclaves :**
```bash
# /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
TYPE=Ethernet
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=no

# /etc/sysconfig/network-scripts/ifcfg-eth1
DEVICE=eth1
TYPE=Ethernet
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=no
```

---
layout: default
---

# Monitoring avancé 📈

### Outils de surveillance réseau

**tcpdump (capture de paquets)**
```bash
# Capturer le trafic
sudo tcpdump -i eth0
sudo tcpdump -i eth0 port 80
sudo tcpdump -i eth0 host 192.168.1.100
sudo tcpdump -i eth0 -w capture.pcap

# Filtres avancés
sudo tcpdump -i eth0 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```

**Wireshark (analyseur de paquets)**
```bash
# Installation
sudo apt install wireshark

# Capture en ligne de commande
sudo tshark -i eth0 -w capture.pcap
sudo tshark -i eth0 -f "port 80" -w http.pcap
```

**nethogs (utilisation par processus)**
```bash
# Installation
sudo apt install nethogs

# Utilisation
sudo nethogs eth0
sudo nethogs -t eth0
```

---
layout: default
---

# Configuration IPv6 🌐

### Support IPv6

**Configuration IPv6 automatique :**
```bash
# Activer IPv6
echo 'net.ipv6.conf.all.disable_ipv6 = 0' >> /etc/sysctl.conf
sysctl -p

# Configuration avec netplan
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp6: true
      addresses:
        - 2001:db8::1/64
```

**Test IPv6 :**
```bash
# Test de connectivité IPv6
ping6 google.com
ping6 -c 4 google.com

# Résolution DNS IPv6
dig AAAA google.com
nslookup -type=AAAA google.com
```

**Désactiver IPv6 (si nécessaire) :**
```bash
# Désactiver IPv6
echo 'net.ipv6.conf.all.disable_ipv6 = 1' >> /etc/sysctl.conf
echo 'net.ipv6.conf.default.disable_ipv6 = 1' >> /etc/sysctl.conf
echo 'net.ipv6.conf.lo.disable_ipv6 = 1' >> /etc/sysctl.conf
sysctl -p
```

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique

**Exercice 1 : Configuration réseau basique**
```bash
# Configurer une interface en statique
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up
sudo ip route add default via 192.168.1.1 dev eth0

# Tester la connectivité
ping -c 4 192.168.1.1
ping -c 4 google.com
```

**Exercice 2 : Diagnostic réseau**
```bash
# Analyser la configuration réseau
ip addr show
ip route show
cat /etc/resolv.conf

# Tester la connectivité
traceroute google.com
nslookup google.com
```

**Exercice 3 : Configuration ufw**
```bash
# Configurer un pare-feu basique
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw enable
sudo ufw status
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour la gestion réseau

**Sécurité**
- Utiliser des pare-feu appropriés
- Désactiver les services inutiles
- Surveiller le trafic réseau
- Utiliser des VPN pour les connexions distantes
- Maintenir les systèmes à jour

**Performance**
- Optimiser les interfaces réseau
- Utiliser l'agrégation d'interfaces
- Surveiller la bande passante
- Configurer des DNS rapides
- Utiliser des caches DNS locaux

**Maintenance**
- Documenter les configurations
- Faire des sauvegardes des configurations
- Surveiller les logs réseau
- Tester régulièrement la connectivité
- Maintenir une documentation réseau

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Paquets logiciels** et gestion
2. **Shell scripting** de base
3. **Virtualisation** et conteneurs
4. **Docker** introduction
5. **Ansible** introduction

**Préparation :**
- Testez les commandes réseau de base
- Configurez un pare-feu simple
- Familiarisez-vous avec les outils de diagnostic
- Pratiquez la configuration d'interfaces

---
src: './pages/10-paquets-logiciels.md'
---