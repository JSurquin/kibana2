---

# 🎯 Exercices pratiques - Module 6

**Gestion des paquets et maintenance**

---

# Exercice 1 : Gestion de base avec apt 📦

**Objectif :** Maîtriser apt

**Consignes :**

1. Mettez à jour la liste des paquets
2. Recherchez le paquet `htop`
3. Affichez les informations détaillées de `htop`
4. Installez `htop` et `tree`
5. Listez les paquets installés contenant "net"
6. Mettez à jour tous les paquets

---

# Solution Exercice 1 💡

```bash
# 1. Mise à jour de la liste
sudo apt update

# 2. Rechercher htop
apt search htop

# 3. Informations
apt show htop

# 4. Installer
sudo apt install htop tree -y

# 5. Paquets installés avec "net"
apt list --installed | grep net

# 6. Mise à jour complète
sudo apt upgrade -y
# ou pour upgrade complet
sudo apt full-upgrade -y

# Vérifier
htop
tree /etc
```

---

# Exercice 2 : Nettoyage et maintenance 🧹

**Objectif :** Nettoyer le système

**Consignes :**

1. Affichez l'espace utilisé par le cache apt
2. Nettoyez le cache
3. Supprimez les paquets orphelins
4. Listez les paquets qui ne sont plus nécessaires
5. Supprimez-les
6. Vérifiez l'espace libéré

---

# Solution Exercice 2 💡

```bash
# 1. Espace du cache
du -sh /var/cache/apt/archives/

# 2. Nettoyer le cache
sudo apt clean
# Vérifier
du -sh /var/cache/apt/archives/

# 3 & 4 & 5. Paquets orphelins
# Voir la liste
apt autoremove --dry-run

# Supprimer
sudo apt autoremove -y

# Purger complètement
sudo apt autoclean

# 6. Vérifier l'espace disque
df -h /

# Combo complet de nettoyage
sudo apt clean && sudo apt autoremove -y && sudo apt autoclean
```

---

# Exercice 3 : Gestion des versions 🔢

**Objectif :** Installer des versions spécifiques

**Consignes :**

1. Listez les versions disponibles de `nginx`
2. Installez une version spécifique
3. Verrouillez le paquet pour éviter les mises à jour
4. Tentez de mettre à jour (devrait échouer)
5. Déverrouillez le paquet
6. Désinstallez sans purger la configuration

---

# Solution Exercice 3 💡

```bash
# 1. Versions disponibles
apt-cache policy nginx

# 2. Installer version spécifique (exemple)
sudo apt install nginx=1.18.0-0ubuntu1

# 3. Verrouiller (hold)
sudo apt-mark hold nginx

# Vérifier
apt-mark showhold

# 4. Tenter mise à jour
sudo apt upgrade
# nginx ne sera PAS mis à jour

# 5. Déverrouiller
sudo apt-mark unhold nginx

# 6. Désinstaller sans purge
sudo apt remove nginx
# La config reste dans /etc/nginx

# Pour purger complètement
sudo apt purge nginx
```

---

# Exercice 4 : Ajout de dépôts PPA 📚

**Objectif :** Ajouter des sources tierces

**Consignes :**

1. Ajoutez le PPA de `git-core` (dernière version de Git)
2. Mettez à jour et installez git depuis ce PPA
3. Listez tous les PPA actifs
4. Supprimez le PPA
5. Revenez à la version officielle

⚠️ **Note :** PPA uniquement sur Ubuntu/dérivés

---

# Solution Exercice 4 💡

```bash
# 1. Ajouter PPA
sudo add-apt-repository ppa:git-core/ppa -y

# 2. Mettre à jour et installer
sudo apt update
sudo apt install git -y

# Vérifier version
git --version

# 3. Lister les PPA
grep -r "ppa.launchpad.net" /etc/apt/sources.list.d/

# 4. Supprimer le PPA
sudo add-apt-repository --remove ppa:git-core/ppa -y

# 5. Revenir à la version officielle
sudo apt update
sudo apt install --reinstall git -y

# Alternative: installer depuis source
sudo apt install build-essential
wget https://github.com/git/git/archive/refs/tags/v2.43.0.tar.gz
tar -xzf v2.43.0.tar.gz
cd git-2.43.0
make prefix=/usr/local all
sudo make prefix=/usr/local install
```

---

# Exercice 5 : Paquets Snap 📦

**Objectif :** Utiliser Snap

**Consignes :**

1. Installez `snapd` si nécessaire
2. Listez les snaps disponibles populaires
3. Installez un snap (ex: `hello-world`)
4. Listez les snaps installés
5. Mettez à jour les snaps
6. Supprimez le snap

---

# Solution Exercice 5 💡

```bash
# 1. Installer snapd
sudo apt install snapd -y

# 2. Rechercher des snaps
snap find "editor"
snap find "browser"

# 3. Installer
sudo snap install hello-world

# Tester
hello-world

# 4. Lister installés
snap list

# 5. Mettre à jour
sudo snap refresh
# ou un snap spécifique
sudo snap refresh hello-world

# 6. Supprimer
sudo snap remove hello-world

# Voir les révisions
snap list --all

# Infos sur un snap
snap info firefox
```

---

# Exercice bonus : Script de mise à jour automatique 🤖

**Objectif :** Automatiser les mises à jour

**Créez un script qui :**
- Met à jour la liste des paquets
- Installe les mises à jour
- Nettoie automatiquement
- Envoie un rapport par email
- Log toutes les actions

---

# Solution Exercice bonus 💡

```bash
#!/bin/bash
# Script de mise à jour automatique

LOG_FILE="/var/log/auto_update.log"
ADMIN_EMAIL="admin@example.com"

log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a $LOG_FILE
}

log "===== Début mise à jour automatique ====="

# Mettre à jour la liste
log "Mise à jour de la liste des paquets..."
sudo apt update >> $LOG_FILE 2>&1

# Compter les mises à jour disponibles
UPDATES=$(apt list --upgradable 2>/dev/null | grep -c upgradable)
log "Mises à jour disponibles : $UPDATES"

if [ $UPDATES -gt 0 ]; then
    # Installer les mises à jour
    log "Installation des mises à jour..."
    sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y >> $LOG_FILE 2>&1
    
    if [ $? -eq 0 ]; then
        log "✅ Mises à jour installées avec succès"
    else
        log "❌ Erreur lors des mises à jour"
        echo "Erreur de mise à jour sur $(hostname)" | \
            mail -s "Erreur Mise à Jour" $ADMIN_EMAIL
    fi
else
    log "Système à jour"
fi

# Nettoyage
log "Nettoyage..."
sudo apt autoremove -y >> $LOG_FILE 2>&1
sudo apt autoclean >> $LOG_FILE 2>&1

# Vérifier si redémarrage nécessaire
if [ -f /var/run/reboot-required ]; then
    log "⚠️  Redémarrage requis"
    echo "Redémarrage nécessaire sur $(hostname)" | \
        mail -s "Redémarrage Requis" $ADMIN_EMAIL
fi

log "===== Fin mise à jour ====="
```

**Automatiser avec cron (tous les dimanches à 3h) :**

```bash
0 3 * * 0 /usr/local/bin/auto_update.sh
```

---

# Points clés à retenir 📌

**APT :**
- `update` : liste des paquets
- `upgrade` : installer mises à jour
- `install` : installer un paquet
- `remove`/`purge` : désinstaller

**Maintenance :**
- `autoremove` : supprimer orphelins
- `clean` : nettoyer cache
- `apt-mark hold` : verrouiller version

**Alternatives :**
- Snap : paquets universels
- Flatpak : alternative à Snap
- AppImage : applications portables

---

