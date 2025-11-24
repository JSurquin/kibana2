---
layout: new-section
routeAlias: 'paquets-maintenance'
---

<a name="paquets-maintenance" id="paquets-maintenance"></a>

# 📦 Module 3
## Gestion des paquets et maintenance

### Installation, mise à jour et maintenance des logiciels

---

# Qu'est-ce qu'un paquet ? 📦

**Paquet** : archive contenant un logiciel et ses métadonnées

**Analogie : un meuble IKEA** 🪑

- **Paquet** = carton avec pièces et instructions
- **Gestionnaire de paquets** = outils pour assembler
- **Dépôt** = magasin IKEA (catalogue de paquets)
- **Dépendances** = autres meubles nécessaires (étagères pour certains objets)

**Contenu d'un paquet :**
- Fichiers binaires
- Fichiers de configuration
- Documentation
- Scripts d'installation/désinstallation
- Liste des dépendances

---

# Systèmes de paquets Linux 📚

**Deux grandes familles :**

**Debian/Ubuntu (.deb)**
- Gestionnaire : `dpkg` (bas niveau)
- Frontend : `apt`, `apt-get`, `aptitude`
- Distributions : Debian, Ubuntu, Mint, Pop!_OS

**Red Hat/Fedora (.rpm)**
- Gestionnaire : `rpm` (bas niveau)
- Frontend : `dnf` (ancien : `yum`)
- Distributions : RHEL, CentOS, AlmaLinux, Rocky, Fedora

---

# Pourquoi des gestionnaires de paquets ? 🤔

**Sans gestionnaire :**
- Télécharger manuellement
- Résoudre les dépendances à la main
- Pas de mises à jour automatiques
- Risque de conflits

**Avec gestionnaire :**
- Installation en une commande
- Dépendances résolues automatiquement
- Mises à jour centralisées
- Désinstallation propre

---

# APT : Advanced Package Tool 🔧

**apt** : gestionnaire pour Debian/Ubuntu

**Commandes essentielles :**

```bash
# Mettre à jour la liste des paquets
sudo apt update

# Mettre à jour les paquets installés
sudo apt upgrade

# Rechercher un paquet
apt search nginx

# Voir les infos d'un paquet
apt show nginx
```

---

# Installer des paquets avec apt 📥

```bash
# Installer un paquet
sudo apt install nginx

# Installer plusieurs paquets
sudo apt install nginx mysql-server php

# Installer sans confirmation
sudo apt install -y nginx

# Installer une version spécifique
sudo apt install nginx=1.18.0-0ubuntu1

# Réinstaller un paquet
sudo apt install --reinstall nginx
```

---

# Désinstaller des paquets avec apt 🗑️

```bash
# Désinstaller un paquet (garde la config)
sudo apt remove nginx

# Désinstaller avec la config
sudo apt purge nginx

# Supprimer les dépendances inutilisées
sudo apt autoremove

# Purge + autoremove
sudo apt purge nginx && sudo apt autoremove
```

---

# Mettre à jour le système avec apt 🔄

```bash
# Mettre à jour la liste des paquets
sudo apt update

# Mettre à jour les paquets (sans supprimer)
sudo apt upgrade

# Mise à jour complète (peut supprimer des paquets)
sudo apt full-upgrade

# Distribution upgrade (changement de version)
sudo apt dist-upgrade

# Combo classique
sudo apt update && sudo apt upgrade -y
```

---

# Différence update vs upgrade 🔀

**apt update** : met à jour la **liste** des paquets disponibles
- Comme consulter le catalogue du magasin
- Aucune installation
- À faire avant upgrade

**apt upgrade** : met à jour les paquets **installés**
- Comme acheter les nouvelles versions
- Installe vraiment les mises à jour
- Nécessite `update` avant

**Analogie : Netflix** 📺

- **apt update** = Rafraîchir le catalogue Netflix
  - Voir les nouveaux films disponibles
  - Ne télécharge rien
  - Rapide (quelques secondes)

- **apt upgrade** = Télécharger les épisodes
  - Installe vraiment les nouvelles versions
  - Peut prendre du temps
  - Utilise de la bande passante

**Exemple pratique :**

```bash
# Scénario : votre serveur web
sudo apt update          # Consulte si nouvelle version de nginx
# → "nginx 1.20 disponible (vous avez 1.18)"

sudo apt upgrade -y      # Installe nginx 1.20
# → Télécharge, installe, redémarre si nécessaire
```

---

# Rechercher des paquets 🔍

```bash
# Recherche simple
apt search nginx

# Recherche avec description
apt search --names-only nginx

# Lister tous les paquets disponibles
apt list

# Lister les paquets installés
apt list --installed

# Lister les paquets à mettre à jour
apt list --upgradable

# Informations détaillées
apt show nginx
```

---

# Dépendances 🔗

**Dépendance** : paquet nécessaire au fonctionnement

```bash
# Voir les dépendances d'un paquet
apt depends nginx

# Voir les dépendances inverses (qui dépend de ce paquet)
apt rdepends nginx

# Simuler une installation (voir ce qui sera installé)
apt install --simulate nginx

# Installer les dépendances d'un paquet sans l'installer
sudo apt build-dep nginx
```

---

# Dépôts APT 📚

**Dépôts** : sources des paquets

**Fichier principal : `/etc/apt/sources.list`**

```bash
cat /etc/apt/sources.list
```

```
deb http://fr.archive.ubuntu.com/ubuntu/ jammy main restricted
deb http://fr.archive.ubuntu.com/ubuntu/ jammy-updates main restricted
deb http://security.ubuntu.com/ubuntu/ jammy-security main restricted
```

---

# Format des sources APT 📝

```
deb http://archive.ubuntu.com/ubuntu/ jammy main
│   │                                  │     │
│   └── URL                            │     └── Composants
│                                      └── Version
└── Type (deb = binaire, deb-src = source)
```

**Composants Ubuntu/Debian :**
- **main** : logiciels libres supportés
- **restricted** : logiciels propriétaires supportés
- **universe** : logiciels libres communautaires
- **multiverse** : logiciels non-libres

---

# Ajouter un dépôt PPA (Ubuntu) ➕

**PPA** : Personal Package Archive

```bash
# Ajouter un PPA
sudo add-apt-repository ppa:ondrej/php
sudo apt update

# Supprimer un PPA
sudo add-apt-repository --remove ppa:ondrej/php

# Ou manuellement
sudo nano /etc/apt/sources.list.d/ondrej-ubuntu-php-jammy.list
```

**⚠️ Attention :** les PPA ne sont pas officiels, vérifier la source !

---

# Ajouter un dépôt externe 🌐

**Exemple : Docker**

```bash
# Installer les prérequis
sudo apt install ca-certificates curl gnupg

# Ajouter la clé GPG
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Ajouter le dépôt
echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Mettre à jour et installer
sudo apt update
sudo apt install docker-ce
```

---

# Nettoyer le système APT 🧹

```bash
# Supprimer les paquets téléchargés
sudo apt clean

# Supprimer les anciens paquets téléchargés
sudo apt autoclean

# Supprimer les dépendances inutilisées
sudo apt autoremove

# Combo nettoyage complet
sudo apt autoremove --purge && sudo apt clean
```

---

# dpkg : gestionnaire de bas niveau 🔩

**dpkg** : gestion directe de paquets .deb

```bash
# Installer un paquet .deb
sudo dpkg -i package.deb

# Si dépendances manquantes
sudo apt install -f

# Lister les paquets installés
dpkg -l

# Voir les fichiers d'un paquet
dpkg -L nginx

# Trouver à quel paquet appartient un fichier
dpkg -S /usr/sbin/nginx
```

---

# dpkg : informations et maintenance 📊

```bash
# Informations sur un paquet installé
dpkg -s nginx

# Lister les fichiers d'un paquet .deb
dpkg -c package.deb

# Extraire un paquet .deb sans installer
dpkg-deb -x package.deb /tmp/extracted

# Reconfigurer un paquet
sudo dpkg-reconfigure package-name

# Réparer les paquets cassés
sudo dpkg --configure -a
```

---

# DNF : gestionnaire Red Hat/Fedora 🎩

**dnf** : remplace yum (depuis Fedora 22, RHEL 8)

```bash
# Mettre à jour la liste des paquets et installer les mises à jour
sudo dnf upgrade

# Rechercher un paquet
dnf search nginx

# Informations sur un paquet
dnf info nginx

# Installer un paquet
sudo dnf install nginx
```

---

# Installer avec DNF 📥

```bash
# Installer un paquet
sudo dnf install nginx

# Installer plusieurs paquets
sudo dnf install nginx mariadb-server php

# Installer sans confirmation
sudo dnf install -y nginx

# Installer un groupe de paquets
sudo dnf groupinstall "Development Tools"

# Réinstaller
sudo dnf reinstall nginx
```

---

# Désinstaller avec DNF 🗑️

```bash
# Désinstaller un paquet
sudo dnf remove nginx

# Supprimer avec les dépendances inutilisées
sudo dnf autoremove nginx

# Supprimer un groupe
sudo dnf groupremove "Development Tools"
```

---

# Mettre à jour avec DNF 🔄

```bash
# Vérifier les mises à jour disponibles
dnf check-update

# Mettre à jour tous les paquets
sudo dnf upgrade

# Mettre à jour un paquet spécifique
sudo dnf upgrade nginx

# Mettre à jour vers une nouvelle version de Fedora
sudo dnf system-upgrade download --releasever=39
sudo dnf system-upgrade reboot
```

---

# Rechercher avec DNF 🔍

```bash
# Recherche simple
dnf search nginx

# Lister tous les paquets
dnf list

# Lister les paquets installés
dnf list installed

# Lister les paquets disponibles
dnf list available

# Informations détaillées
dnf info nginx

# Voir l'historique des transactions
dnf history

# Annuler une transaction
sudo dnf history undo last
```

---

# Dépôts DNF 📚

**Fichiers de configuration : `/etc/yum.repos.d/`**

```bash
ls /etc/yum.repos.d/
```

**Exemple de fichier de dépôt :**

```ini
[epel]
name=Extra Packages for Enterprise Linux $releasever - $basearch
baseurl=https://download.fedoraproject.org/pub/epel/$releasever/Everything/$basearch/
enabled=1
gpgcheck=1
gpgkey=https://download.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-$releasever
```

---

# Gérer les dépôts DNF 🔧

```bash
# Lister les dépôts
dnf repolist

# Lister tous les dépôts (y compris désactivés)
dnf repolist --all

# Activer un dépôt
sudo dnf config-manager --enable epel

# Désactiver un dépôt
sudo dnf config-manager --disable epel

# Ajouter un dépôt
sudo dnf config-manager --add-repo https://example.com/repo/example.repo
```

---

# EPEL : paquets supplémentaires RHEL 🎁

**EPEL** : Extra Packages for Enterprise Linux

```bash
# Installer EPEL sur RHEL/AlmaLinux/Rocky
sudo dnf install epel-release

# Ou manuellement
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm

# Vérifier
dnf repolist | grep epel
```

**EPEL apporte des milliers de paquets additionnels !**

---

# Nettoyer le système DNF 🧹

```bash
# Nettoyer le cache
sudo dnf clean all

# Supprimer les paquets orphelins
sudo dnf autoremove

# Supprimer les anciens noyaux (garder 2 derniers)
sudo dnf remove $(dnf repoquery --installonly --latest-limit=-2 -q)
```

---

# RPM : gestionnaire de bas niveau 🔩

**rpm** : gestion directe de paquets .rpm

```bash
# Installer un paquet .rpm
sudo rpm -ivh package.rpm

# Mettre à jour
sudo rpm -Uvh package.rpm

# Désinstaller
sudo rpm -e package-name

# Lister les paquets installés
rpm -qa

# Voir les fichiers d'un paquet
rpm -ql nginx
```

---

# RPM : informations 📊

```bash
# Informations sur un paquet installé
rpm -qi nginx

# Informations sur un fichier .rpm
rpm -qip package.rpm

# Trouver à quel paquet appartient un fichier
rpm -qf /usr/sbin/nginx

# Vérifier l'intégrité d'un paquet
rpm -V nginx

# Lister les scripts du paquet
rpm -q --scripts nginx
```

---

# Snap : paquets universels 📦

**Snap** : format de paquet universel (Canonical/Ubuntu)

```bash
# Installer snapd
sudo apt install snapd        # Ubuntu/Debian
sudo dnf install snapd        # Fedora

# Rechercher un snap
snap find vlc

# Installer un snap
sudo snap install vlc

# Lister les snaps installés
snap list

# Mettre à jour
sudo snap refresh vlc
sudo snap refresh   # Tous

# Désinstaller
sudo snap remove vlc
```

---

# Snap : avantages et inconvénients ⚖️

**Avantages ✅**
- Isolation (conteneurisé)
- Mises à jour automatiques
- Même paquet pour toutes les distributions
- Plusieurs versions en parallèle

**Inconvénients ❌**
- Plus volumineux (dépendances incluses)
- Démarrage plus lent
- Consomme plus de mémoire
- Accès limité au système (sandboxing)

---

# Flatpak : alternative à Snap 📦

**Flatpak** : format universel (projet libre)

```bash
# Installer Flatpak
sudo apt install flatpak             # Ubuntu/Debian
sudo dnf install flatpak             # Fedora

# Ajouter Flathub (dépôt principal)
flatpak remote-add --if-not-exists flathub \
  https://flathub.org/repo/flathub.flatpakrepo

# Rechercher une application
flatpak search firefox

# Installer
flatpak install flathub org.mozilla.firefox

# Lancer
flatpak run org.mozilla.firefox
```

---

# Flatpak : gestion 🔧

```bash
# Lister les applications installées
flatpak list

# Mettre à jour tout
flatpak update

# Désinstaller
flatpak uninstall org.mozilla.firefox

# Nettoyer les runtimes inutilisés
flatpak uninstall --unused

# Voir les infos
flatpak info org.mozilla.firefox
```

---

# AppImage : exécutables portables 🎒

**AppImage** : un seul fichier exécutable

```bash
# Télécharger une AppImage
wget https://example.com/app.AppImage

# Rendre exécutable
chmod +x app.AppImage

# Lancer
./app.AppImage

# C'est tout ! Aucune installation nécessaire
```

**Avantages :**
- Pas d'installation
- Portable (USB)
- Pas de privilèges admin

**Inconvénients :**
- Pas de mises à jour automatiques
- Pas d'intégration système

---

# Compiler depuis les sources 🔨

**Quand compiler ?**
- Paquet pas disponible
- Version très récente nécessaire
- Options de compilation spécifiques

```bash
# Installer les outils de compilation
sudo apt install build-essential    # Debian/Ubuntu
sudo dnf groupinstall "Development Tools"  # RHEL/Fedora

# Télécharger les sources
wget https://example.com/software-1.0.tar.gz
tar -xzf software-1.0.tar.gz
cd software-1.0
```

---

# Compiler depuis les sources (suite) 🔨

```bash
# Configuration
./configure --prefix=/usr/local

# Compilation
make

# Installation
sudo make install

# Désinstallation (si supporté)
sudo make uninstall
```

---

# Sécurité : mises à jour automatiques 🔐

**Pourquoi automatiser ?**
- Correctifs de sécurité critiques
- Pas d'oubli
- Système toujours à jour

**Ubuntu : unattended-upgrades**

```bash
# Installer
sudo apt install unattended-upgrades

# Activer
sudo dpkg-reconfigure unattended-upgrades
```

---

# Configuration unattended-upgrades ⚙️

**Fichier : `/etc/apt/apt.conf.d/50unattended-upgrades`**

```bash
Unattended-Upgrade::Allowed-Origins {
    "${distro_id}:${distro_codename}-security";
    "${distro_id}:${distro_codename}-updates";
};

Unattended-Upgrade::Automatic-Reboot "false";
Unattended-Upgrade::Automatic-Reboot-Time "03:00";
Unattended-Upgrade::Mail "admin@example.com";
```

---

# DNF Automatic (RHEL/Fedora) 🤖

```bash
# Installer
sudo dnf install dnf-automatic

# Configuration
sudo nano /etc/dnf/automatic.conf
```

```ini
[commands]
upgrade_type = security
download_updates = yes
apply_updates = yes

[email]
email_to = admin@example.com
```

```bash
# Activer
sudo systemctl enable --now dnf-automatic.timer
```

---

# Vérifier les mises à jour de sécurité 🔒

**Ubuntu/Debian :**

```bash
# Lister les mises à jour de sécurité
sudo apt list --upgradable | grep security

# Installer seulement les mises à jour de sécurité
sudo unattended-upgrade -d
```

**RHEL/Fedora :**

```bash
# Lister les mises à jour de sécurité
sudo dnf updateinfo list security

# Installer seulement les correctifs de sécurité
sudo dnf upgrade --security
```

---

# Gestion des versions de noyau 🧠

```bash
# Voir la version actuelle
uname -r

# Lister les noyaux installés (Debian/Ubuntu)
dpkg -l | grep linux-image

# Supprimer un ancien noyau
sudo apt remove linux-image-5.15.0-25-generic

# Ou autoremove pour nettoyer automatiquement
sudo apt autoremove
```

---

# Gestion des versions de noyau (RHEL/Fedora) 🧠

```bash
# Lister les noyaux installés
rpm -q kernel

# Configuration : nombre de noyaux à garder
sudo nano /etc/dnf/dnf.conf
```

```ini
installonly_limit=3
```

```bash
# Supprimer les anciens noyaux
sudo dnf remove --oldinstallonly --setopt installonly_limit=2 kernel
```

---

# Dépannage : paquets cassés 🔧

**Debian/Ubuntu :**

```bash
# Réparer les paquets cassés
sudo apt install -f

# Reconfigurer tous les paquets non configurés
sudo dpkg --configure -a

# Forcer la suppression d'un paquet bloqué
sudo dpkg --remove --force-remove-reinstreq package-name
```

---

# Dépannage : paquets cassés (RHEL/Fedora) 🔧

```bash
# Vérifier les problèmes
sudo dnf check

# Réparer
sudo dnf distro-sync

# Réinitialiser un paquet
sudo dnf reinstall package-name

# En cas de problème avec la base de données RPM
sudo rpm --rebuilddb
```

---

# Verrouiller une version de paquet 🔒

**Debian/Ubuntu : apt-mark**

```bash
# Empêcher la mise à jour
sudo apt-mark hold nginx

# Voir les paquets verrouillés
apt-mark showhold

# Déverrouiller
sudo apt-mark unhold nginx
```

---

# Verrouiller une version de paquet (DNF) 🔒

**RHEL/Fedora : versionlock**

```bash
# Installer le plugin
sudo dnf install python3-dnf-plugin-versionlock

# Verrouiller un paquet
sudo dnf versionlock add nginx

# Lister les verrous
sudo dnf versionlock list

# Supprimer un verrou
sudo dnf versionlock delete nginx
```

---

# Alternatives : plusieurs versions 🔀

**update-alternatives** : gérer plusieurs versions d'un outil

```bash
# Lister les alternatives pour un outil
update-alternatives --list editor

# Voir la configuration
update-alternatives --display editor

# Changer l'alternative
sudo update-alternatives --config editor

# Ajouter une alternative
sudo update-alternatives --install /usr/bin/editor editor \
  /usr/bin/vim 100
```

---

# Exemples d'alternatives courantes 📝

```bash
# Éditeur par défaut
sudo update-alternatives --config editor

# Java
sudo update-alternatives --config java

# Navigateur par défaut
sudo update-alternatives --config x-www-browser

# Python
sudo update-alternatives --config python
```

---

# Logs et historique 📜

**Logs APT :**

```bash
# Historique des installations
cat /var/log/apt/history.log

# Journal détaillé
cat /var/log/apt/term.log

# Historique complet (y compris archives)
zcat /var/log/apt/history.log.*.gz | less
```

---

# Logs et historique DNF 📜

```bash
# Historique DNF
dnf history

# Détails d'une transaction
dnf history info 5

# Annuler une transaction
sudo dnf history undo 5

# Refaire une transaction
sudo dnf history redo 5

# Logs
cat /var/log/dnf.log
```

---

# Bonnes pratiques 📋

1. **Toujours faire `update` avant `upgrade`**
   ```bash
   sudo apt update && sudo apt upgrade
   ```

2. **Lire les changements avant de confirmer**
   - Vérifier ce qui sera installé/supprimé

3. **Sauvegarder avant les mises à jour majeures**
   - Snapshot VM ou backup

4. **Activer les mises à jour de sécurité automatiques**
   - unattended-upgrades ou dnf-automatic

---

# Bonnes pratiques (suite) 📋

5. **Nettoyer régulièrement**
   ```bash
   sudo apt autoremove && sudo apt clean
   ```

6. **Ne pas mélanger les sources**
   - Éviter de mélanger Debian et Ubuntu
   - Éviter les PPA non vérifiés

7. **Surveiller les obsolescences**
   ```bash
   apt list --upgradable
   ```

8. **Documenter les modifications**
   - Noter les dépôts ajoutés
   - Noter les versions verrouillées

---

# Cas pratiques 💼

**Installer LAMP (Linux, Apache, MySQL, PHP) :**

```bash
# Debian/Ubuntu
sudo apt update
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql

# RHEL/Fedora
sudo dnf install httpd mariadb-server php php-mysqlnd
sudo systemctl enable --now httpd mariadb
```

---

# Cas pratiques (suite) 💼

**Installer Docker depuis le dépôt officiel :**

```bash
# Debian/Ubuntu
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# RHEL/Fedora
sudo dnf config-manager --add-repo \
  https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io
sudo systemctl enable --now docker
```

---

# Récapitulatif du Module 3 ✅

**Ce que vous avez appris :**

✅ Concepts de paquets et dépendances
✅ APT (Debian/Ubuntu) : install, remove, update, upgrade
✅ DNF/YUM (RHEL/Fedora) : install, remove, upgrade
✅ Gestion des dépôts
✅ Formats universels (Snap, Flatpak, AppImage)
✅ Mises à jour automatiques de sécurité
✅ Compilation depuis les sources
✅ Dépannage de paquets cassés
✅ Bonnes pratiques de maintenance

---

# Prochaine étape 🎯

**Module 7 : Automatisation, scripts et tâches planifiées**

- Scripts Bash de base
- Variables, boucles, conditions
- Tâches planifiées avec cron
- at pour tâches ponctuelles
- systemd timers
- Logs et journalctl

---
layout: default
---

# Questions ? 🤔

N'hésitez pas à poser vos questions maintenant !

Le prochain module explorera l'automatisation avec les scripts et les tâches planifiées.

