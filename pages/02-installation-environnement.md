---
layout: intro
routeAlias: 'installation-environnement'
---

# Installation et Environnement ⚙️

### Configuration de votre environnement Unix/Linux

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Configurons votre environnement <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Choix de la distribution 🐧

### Quelle distribution choisir ?

**Ubuntu (Recommandée pour débutants)**
- Facile d'installation
- Large communauté
- Documentation abondante
- Support LTS (5 ans)

**CentOS/Rocky Linux**
- Stabilité entreprise
- Compatible Red Hat
- Idéal pour serveurs
- Support communautaire

**Debian**
- Très stable
- Libre et open source
- Base d'Ubuntu
- Sécurité renforcée

---
layout: default
---

# Méthodes d'installation 💻

### Options disponibles

**Machine virtuelle (Recommandée)**
- VirtualBox (gratuit)
- VMware Workstation
- Hyper-V (Windows)
- Sécurité et isolation

**Dual boot**
- Partage du disque dur
- Choix au démarrage
- Performance native
- Risque de perte de données

**WSL (Windows)**
- Sous-système Linux
- Intégration Windows
- Performance limitée
- Compatibilité partielle

---
layout: default
---

# Installation d'Ubuntu 🚀

### Guide étape par étape

**Étape 1 : Téléchargement**
```bash
# Télécharger Ubuntu Desktop
wget https://releases.ubuntu.com/22.04/ubuntu-22.04.3-desktop-amd64.iso
```

**Étape 2 : Création de la VM**
- Ouvrir VirtualBox
- Nouvelle machine virtuelle
- Allouer 4GB RAM minimum
- Créer un disque virtuel de 20GB

**Étape 3 : Installation**
- Démarrer la VM
- Suivre l'assistant d'installation
- Choisir "Effacer le disque"
- Créer un utilisateur

---
layout: default
---

# Configuration post-installation ⚙️

### Optimisation du système

**Mise à jour du système**
```bash
sudo apt update
sudo apt upgrade -y
```

**Installation des outils essentiels**
```bash
sudo apt install -y \
  curl \
  wget \
  git \
  vim \
  htop \
  tree \
  build-essential
```

**Configuration du shell**
```bash
# Vérifier le shell par défaut
echo $SHELL

# Installer zsh si nécessaire
sudo apt install zsh
```

---
layout: default
---

# Environnement de développement 🛠️

### Outils recommandés

**Éditeur de code**
```bash
# Installation de VS Code
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list
sudo apt update
sudo apt install code
```

**Terminal amélioré**
```bash
# Installation de Oh My Zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---
layout: default
---

# Configuration du terminal 🖥️

### Personnalisation avancée

**Thème et plugins**
```bash
# Éditer la configuration zsh
nano ~/.zshrc

# Thème recommandé
ZSH_THEME="agnoster"

# Plugins utiles
plugins=(
  git
  docker
  kubectl
  ansible
  history
  zsh-autosuggestions
)
```

**Alias utiles**
```bash
# Ajouter dans ~/.zshrc
alias ll='ls -la'
alias la='ls -A'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'
```

---
layout: default
---

# Gestion des paquets 📦

### Utilisation d'apt

**Recherche de paquets**
```bash
# Rechercher un paquet
apt search nom_du_paquet

# Informations sur un paquet
apt show nom_du_paquet
```

**Installation et suppression**
```bash
# Installer un paquet
sudo apt install nom_du_paquet

# Supprimer un paquet
sudo apt remove nom_du_paquet

# Supprimer complètement
sudo apt purge nom_du_paquet
```

**Mise à jour**
```bash
# Mettre à jour la liste
sudo apt update

# Mettre à jour les paquets
sudo apt upgrade
```

---
layout: default
---

# Gestion des services 🔧

### systemctl et systemd

**Commandes de base**
```bash
# Démarrer un service
sudo systemctl start nom_service

# Arrêter un service
sudo systemctl stop nom_service

# Redémarrer un service
sudo systemctl restart nom_service

# Statut d'un service
sudo systemctl status nom_service
```

**Services au démarrage**
```bash
# Activer un service
sudo systemctl enable nom_service

# Désactiver un service
sudo systemctl disable nom_service

# Lister les services
sudo systemctl list-units --type=service
```

---
layout: default
---

# Monitoring système 📊

### Outils de surveillance

**htop (interactif)**
```bash
# Installation
sudo apt install htop

# Utilisation
htop
```

**Commandes système**
```bash
# Utilisation CPU et mémoire
top

# Utilisation disque
df -h

# Utilisation mémoire
free -h

# Processus
ps aux
```

**Monitoring réseau**
```bash
# Connexions réseau
netstat -tuln

# Interface réseau
ip addr show

# Test de connectivité
ping google.com
```

---
layout: default
---

# Sécurité de base 🔒

### Configuration initiale

**Pare-feu (ufw)**
```bash
# Installation
sudo apt install ufw

# Activer le pare-feu
sudo ufw enable

# Autoriser SSH
sudo ufw allow ssh

# Statut
sudo ufw status
```

**Mise à jour automatique**
```bash
# Installation
sudo apt install unattended-upgrades

# Configuration
sudo dpkg-reconfigure unattended-upgrades
```

**Utilisateur sudo**
```bash
# Ajouter un utilisateur au groupe sudo
sudo usermod -aG sudo nom_utilisateur

# Vérifier les groupes
groups nom_utilisateur
```

---
layout: default
---

# Sauvegarde et restauration 💾

### Stratégies de sauvegarde

**Sauvegarde des fichiers**
```bash
# Créer une archive
tar -czf backup_$(date +%Y%m%d).tar.gz /chemin/a/sauvegarder

# Restaurer
tar -xzf backup_20250101.tar.gz -C /destination
```

**Sauvegarde système**
```bash
# Cloner le système
sudo dd if=/dev/sda of=/dev/sdb bs=4M status=progress

# Image système
sudo dd if=/dev/sda | gzip > system_backup.img.gz
```

**Sauvegarde incrémentale**
```bash
# Utiliser rsync
rsync -av --delete /source/ /destination/
```

---
layout: default
---

# Virtualisation locale 🖥️

### Outils de virtualisation

**VirtualBox**
```bash
# Installation
sudo apt install virtualbox

# Extension pack
sudo apt install virtualbox-ext-pack
```

**Docker**
```bash
# Installation
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Ajouter l'utilisateur au groupe docker
sudo usermod -aG docker $USER
```

**KVM/QEMU**
```bash
# Vérifier la virtualisation
egrep -c '(vmx|svm)' /proc/cpuinfo

# Installation
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
```

---
layout: default
---

# Configuration réseau 🌐

### Réseau local et internet

**Configuration IP statique**
```bash
# Éditer la configuration
sudo nano /etc/netplan/01-netcfg.yaml

# Exemple de configuration
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

**Test de connectivité**
```bash
# Test ping
ping -c 4 google.com

# Test DNS
nslookup google.com

# Test de port
telnet google.com 80
```

---
layout: default
---

# Troubleshooting 🔧

### Résolution de problèmes courants

**Problème de démarrage**
```bash
# Mode recovery
# Redémarrer et appuyer sur 'e' au démarrage
# Ajouter 'single' à la ligne linux

# Réparation du système de fichiers
sudo fsck /dev/sda1
```

**Problème de réseau**
```bash
# Redémarrer le service réseau
sudo systemctl restart NetworkManager

# Vérifier les interfaces
ip link show

# Test de connectivité
ping -c 4 8.8.8.8
```

**Problème de permissions**
```bash
# Corriger les permissions
sudo chown -R user:group /chemin

# Corriger les permissions de fichiers
sudo chmod 644 /fichier
```

---
layout: default
---

# Ressources utiles 📚

### Documentation et aide

**Documentation officielle**
- ubuntu.com/documentation
- debian.org/doc
- redhat.com/documentation

**Communauté**
- askubuntu.com
- stackoverflow.com
- reddit.com/r/linux

**Outils en ligne**
- linuxjourney.com
- linuxcommand.org
- tldp.org

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Premiers pas** avec la ligne de commande
2. **Système de fichiers** et navigation
3. **Commandes de base** et manipulation
4. **Gestion des utilisateurs** et permissions
5. **Processus et services** système

**Préparation :**
- Assurez-vous que votre système est à jour
- Familiarisez-vous avec le terminal
- Testez les commandes de base

---
src: './pages/03-premiers-pas.md'
--- 