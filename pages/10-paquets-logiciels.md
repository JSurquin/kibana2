---
layout: intro
routeAlias: 'paquets-logiciels'
---

# Paquets Logiciels 📦

### Gestion des paquets et logiciels Linux

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Gérons les paquets ensemble <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Concepts de base 📚

### Qu'est-ce qu'un paquet ?

**Définition :**
- Archive contenant un logiciel
- Métadonnées (dépendances, version, etc.)
- Scripts d'installation/désinstallation
- Géré par un gestionnaire de paquets

**Types de paquets :**
- **Paquets binaires** : Logiciels compilés
- **Paquets sources** : Code source à compiler
- **Paquets de développement** : Headers et libs
- **Paquets de documentation** : Manuels et docs

**Gestionnaires courants :**
- **apt** (Debian/Ubuntu)
- **yum/dnf** (Red Hat/CentOS)
- **pacman** (Arch Linux)
- **zypper** (openSUSE)

---
layout: default
---

# Gestionnaire APT 🔧

### Gestion des paquets Debian/Ubuntu

**Mise à jour des listes :**
```bash
sudo apt update                 # Mettre à jour la liste des paquets
sudo apt upgrade               # Mettre à jour les paquets installés
sudo apt full-upgrade          # Mise à jour complète (peut changer de paquets)
sudo apt dist-upgrade          # Mise à jour de distribution
```

**Installation et suppression :**
```bash
sudo apt install nom_paquet    # Installer un paquet
sudo apt install paquet1 paquet2  # Installer plusieurs paquets
sudo apt remove nom_paquet     # Supprimer un paquet
sudo apt purge nom_paquet      # Supprimer complètement
sudo apt autoremove            # Supprimer les dépendances inutiles
```

**Recherche et information :**
```bash
apt search nom_paquet          # Rechercher un paquet
apt show nom_paquet            # Informations sur un paquet
apt list --installed           # Paquets installés
apt list --upgradable          # Paquets à mettre à jour
```

---
layout: default
---

# Commandes APT avancées 🚀

### Fonctionnalités avancées

**Gestion des dépôts :**
```bash
# Ajouter un dépôt
sudo add-apt-repository ppa:nom/ppa
sudo apt update

# Supprimer un dépôt
sudo add-apt-repository --remove ppa:nom/ppa

# Lister les dépôts
cat /etc/apt/sources.list
ls /etc/apt/sources.list.d/
```

**Nettoyage et maintenance :**
```bash
sudo apt clean                 # Nettoyer le cache
sudo apt autoclean             # Nettoyer les paquets obsolètes
sudo apt autoremove            # Supprimer les dépendances orphelines
sudo apt autoremove --purge    # Supprimer avec configuration
```

**Résolution de problèmes :**
```bash
sudo apt --fix-broken install  # Réparer les dépendances cassées
sudo dpkg --configure -a       # Reconfigurer les paquets
sudo apt install -f            # Forcer l'installation
```

---
layout: default
---

# Gestionnaire DNF/YUM 📦

### Gestion des paquets Red Hat/CentOS

**Commandes de base :**
```bash
sudo dnf update               # Mettre à jour le système
sudo dnf install nom_paquet   # Installer un paquet
sudo dnf remove nom_paquet    # Supprimer un paquet
sudo dnf search nom_paquet    # Rechercher un paquet
sudo dnf info nom_paquet      # Informations sur un paquet
```

**Gestion des dépôts :**
```bash
sudo dnf repolist             # Lister les dépôts
sudo dnf repolist enabled     # Dépôts activés
sudo dnf repolist disabled    # Dépôts désactivés
sudo dnf config-manager --add-repo URL  # Ajouter un dépôt
```

**Nettoyage :**
```bash
sudo dnf clean all            # Nettoyer le cache
sudo dnf autoremove           # Supprimer les dépendances inutiles
sudo dnf autoremove --duplicates  # Supprimer les doublons
```

---
layout: default
---

# Gestionnaire Pacman 🏗️

### Gestion des paquets Arch Linux

**Commandes de base :**
```bash
sudo pacman -S nom_paquet     # Installer un paquet
sudo pacman -R nom_paquet     # Supprimer un paquet
sudo pacman -Rs nom_paquet    # Supprimer avec dépendances
sudo pacman -Syu              # Mettre à jour le système
sudo pacman -Ss nom_paquet    # Rechercher un paquet
```

**Gestion des dépôts :**
```bash
sudo pacman -Sy               # Synchroniser les dépôts
sudo pacman -Syy              # Forcer la synchronisation
sudo pacman -Q                # Lister les paquets installés
sudo pacman -Qq               # Liste simple
```

**AUR (Arch User Repository) :**
```bash
# Avec yay (AUR helper)
yay -S nom_paquet             # Installer depuis AUR
yay -R nom_paquet             # Supprimer un paquet AUR
yay -Ss nom_paquet            # Rechercher dans AUR
```

---
layout: default
---

# Gestion des dépendances 🔗

### Comprendre les dépendances

**Types de dépendances :**
```bash
# Dépendances requises
Depends: libc6 (>= 2.17)

# Dépendances recommandées
Recommends: python3

# Dépendances suggérées
Suggests: python3-pip

# Dépendances en conflit
Conflicts: old-package

# Dépendances de remplacement
Replaces: old-package
```

**Commandes de diagnostic :**
```bash
# Voir les dépendances d'un paquet
apt show nom_paquet | grep -E "(Depends|Recommends|Suggests)"

# Voir les paquets qui dépendent d'un paquet
apt rdepends nom_paquet

# Voir les dépendances cassées
apt check
```

**Résolution de conflits :**
```bash
# Forcer l'installation malgré les conflits
sudo apt install nom_paquet --force-yes

# Résoudre les conflits automatiquement
sudo apt install nom_paquet --fix-broken
```

---
layout: default
---

# Paquets de développement 🛠️

### Outils de développement

**Paquets de compilation :**
```bash
# Outils de base
sudo apt install build-essential
sudo apt install gcc g++ make

# Bibliothèques de développement
sudo apt install libssl-dev
sudo apt install libcurl4-openssl-dev
sudo apt install python3-dev

# Outils de debug
sudo apt install gdb
sudo apt install valgrind
sudo apt install strace
```

**Environnements de développement :**
```bash
# Python
sudo apt install python3 python3-pip python3-venv

# Node.js
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install nodejs

# Java
sudo apt install openjdk-11-jdk
sudo apt install maven
```

---
layout: default
---

# Gestion des paquets sources 📝

### Compilation depuis les sources

**Téléchargement et extraction :**
```bash
# Télécharger les sources
wget https://example.com/software-1.0.tar.gz
tar -xzf software-1.0.tar.gz
cd software-1.0
```

**Configuration et compilation :**
```bash
# Configuration
./configure --prefix=/usr/local
make
sudo make install

# Ou avec cmake
mkdir build && cd build
cmake ..
make
sudo make install
```

**Désinstallation :**
```bash
# Si le paquet a un Makefile
sudo make uninstall

# Sinon, supprimer manuellement
sudo rm -rf /usr/local/bin/software
sudo rm -rf /usr/local/lib/software
```

---
layout: default
---

# Paquets Snap et Flatpak 📱

### Paquets universels

**Snap (Canonical) :**
```bash
# Installation
sudo apt install snapd

# Utilisation
sudo snap install nom_paquet
sudo snap remove nom_paquet
snap list
snap info nom_paquet
snap refresh nom_paquet
```

**Flatpak (Red Hat) :**
```bash
# Installation
sudo apt install flatpak

# Ajouter le dépôt Flathub
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Utilisation
flatpak install flathub org.gnome.Gedit
flatpak uninstall org.gnome.Gedit
flatpak list
flatpak update
```

**Comparaison :**
```bash
# Snap : confinement strict, mise à jour automatique
# Flatpak : plus flexible, écosystème plus ouvert
# Les deux : paquets universels, isolation
```

---
layout: default
---

# Gestion des paquets Python 🐍

### pip et environnements virtuels

**pip (Python Package Installer) :**
```bash
# Installation de paquets
pip install nom_paquet
pip install nom_paquet==1.2.3
pip install -r requirements.txt

# Désinstallation
pip uninstall nom_paquet

# Liste des paquets
pip list
pip freeze > requirements.txt
```

**Environnements virtuels :**
```bash
# Créer un environnement virtuel
python3 -m venv mon_env
source mon_env/bin/activate

# Désactiver l'environnement
deactivate

# Avec virtualenvwrapper
mkvirtualenv mon_projet
workon mon_projet
deactivate
```

**Conda (Anaconda) :**
```bash
# Installation de paquets
conda install nom_paquet
conda install nom_paquet=1.2.3

# Créer un environnement
conda create -n mon_env python=3.9
conda activate mon_env
conda deactivate
```

---
layout: default
---

# Gestion des paquets Node.js 🟢

### npm et yarn

**npm (Node Package Manager) :**
```bash
# Installation globale
npm install -g nom_paquet

# Installation locale
npm install nom_paquet
npm install nom_paquet --save-dev

# Désinstallation
npm uninstall nom_paquet

# Liste des paquets
npm list
npm list -g
```

**yarn (alternative à npm) :**
```bash
# Installation
npm install -g yarn

# Utilisation
yarn add nom_paquet
yarn add nom_paquet --dev
yarn remove nom_paquet
yarn list
yarn install
```

**Gestion des versions :**
```bash
# Avec nvm (Node Version Manager)
nvm install 16.14.0
nvm use 16.14.0
nvm list

# Avec n (alternative)
n 16.14.0
n list
```

---
layout: default
---

# Paquets de conteneurs 🐳

### Gestion des images Docker

**Commandes Docker de base :**
```bash
# Télécharger une image
docker pull ubuntu:20.04
docker pull nginx:alpine

# Lister les images
docker images
docker image ls

# Supprimer une image
docker rmi nom_image
docker image rm nom_image

# Nettoyer les images inutilisées
docker image prune
docker system prune
```

**Construction d'images :**
```bash
# Construire depuis un Dockerfile
docker build -t mon_image .
docker build -t mon_image:v1.0 .

# Tag d'une image
docker tag mon_image mon_image:v1.0

# Pousser vers un registre
docker push mon_image:v1.0
```

---
layout: default
---

# Paquets de sécurité 🔒

### Paquets de sécurité et audit

**Outils de sécurité :**
```bash
# Audit de sécurité
sudo apt install lynis
sudo lynis audit system

# Scanner de vulnérabilités
sudo apt install rkhunter
sudo rkhunter --update
sudo rkhunter --check

# Outils de chiffrement
sudo apt install openssl
sudo apt install gpg
```

**Mises à jour de sécurité :**
```bash
# Mises à jour de sécurité uniquement
sudo apt update
sudo apt upgrade --only-upgrade

# Vérifier les vulnérabilités
sudo apt list --upgradable | grep security
```

**Configuration automatique :**
```bash
# unattended-upgrades
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
```

---
layout: default
---

# Gestion des paquets système ⚙️

### Paquets système critiques

**Noyau Linux :**
```bash
# Voir les noyaux installés
dpkg -l | grep linux-image

# Installer un nouveau noyau
sudo apt install linux-image-generic

# Supprimer les anciens noyaux
sudo apt autoremove --purge
```

**Firmware et pilotes :**
```bash
# Firmware propriétaire
sudo apt install firmware-linux
sudo apt install firmware-linux-nonfree

# Pilotes graphiques
sudo apt install nvidia-driver-470
sudo apt install mesa-utils
```

**Services système :**
```bash
# Services réseau
sudo apt install network-manager
sudo apt install ufw

# Services de base
sudo apt install systemd
sudo apt install dbus
```

---
layout: default
---

# Monitoring des paquets 📊

### Surveillance de l'état des paquets

**Commandes de diagnostic :**
```bash
# Vérifier l'intégrité des paquets
dpkg -V

# Voir les paquets cassés
dpkg --audit

# Vérifier les dépendances
apt check

# Voir l'espace utilisé
dpkg-query -W --showformat='${Installed-Size;10}\t${Package}\n' | sort -k1,1n
```

**Outils de monitoring :**
```bash
# Apticron (notifications de mises à jour)
sudo apt install apticron
sudo nano /etc/apticron/apticron.conf

# Debian-goodies
sudo apt install debian-goodies
checkrestart
```

**Logs de paquets :**
```bash
# Voir les logs d'installation
cat /var/log/dpkg.log

# Voir les erreurs récentes
grep -i error /var/log/dpkg.log
```

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique

**Exercice 1 : Gestion APT basique**
```bash
# Mettre à jour le système
sudo apt update
sudo apt upgrade

# Installer un paquet
sudo apt install htop

# Rechercher un paquet
apt search "text editor"

# Voir les informations
apt show vim
```

**Exercice 2 : Gestion des dépendances**
```bash
# Voir les dépendances d'un paquet
apt show nginx | grep -E "(Depends|Recommends)"

# Voir les paquets qui dépendent d'un paquet
apt rdepends python3

# Nettoyer les dépendances inutiles
sudo apt autoremove
```

**Exercice 3 : Environnement Python**
```bash
# Créer un environnement virtuel
python3 -m venv mon_projet
source mon_projet/bin/activate

# Installer des paquets
pip install requests
pip install flask

# Créer requirements.txt
pip freeze > requirements.txt
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour la gestion des paquets

**Sécurité**
- Mettre à jour régulièrement le système
- Utiliser des dépôts officiels
- Vérifier les signatures des paquets
- Surveiller les vulnérabilités
- Utiliser des environnements isolés

**Performance**
- Nettoyer régulièrement le cache
- Supprimer les paquets inutiles
- Utiliser des miroirs locaux
- Optimiser les dépôts
- Surveiller l'espace disque

**Maintenance**
- Documenter les installations
- Faire des sauvegardes de configuration
- Tester les mises à jour
- Maintenir une liste des paquets installés
- Utiliser des outils de monitoring

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Shell scripting** de base
2. **Virtualisation** et conteneurs
3. **Docker** introduction
4. **Ansible** introduction
5. **Projet final** intégrateur

**Préparation :**
- Familiarisez-vous avec apt
- Pratiquez l'installation de paquets
- Testez les environnements virtuels
- Explorez les différents gestionnaires

---
src: './pages/11-shell-scripting.md'
--- 