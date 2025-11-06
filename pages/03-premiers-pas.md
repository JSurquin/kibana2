---
layout: intro
routeAlias: 'premiers-pas'
---

# Premiers pas avec la ligne de commande 🚀

### Découverte du terminal et des commandes de base

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Explorons le terminal ensemble <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Qu'est-ce qu'un terminal ? 🖥️

### Interface en ligne de commande

**Définition :**
- Interface texte pour interagir avec le système
- Plus puissant que l'interface graphique
- Automatisation et scripting
- Contrôle total du système

**Avantages :**
- Rapidité d'exécution
- Automatisation
- Scripting avancé
- Moins de ressources
- Accès distant (SSH)

**Types de terminaux :**
- Terminal physique
- Terminal virtuel (Ctrl+Alt+F1)
- Émulateur de terminal (GNOME Terminal)
- Terminal distant (SSH)

---
layout: default
---

# Ouvrir un terminal 🚪

### Différentes méthodes

**Interface graphique :**
```bash
# Raccourci clavier
Ctrl + Alt + T

# Ou via le menu
Applications > Accessoires > Terminal
```

**Terminal virtuel :**
```bash
# Basculer vers un terminal virtuel
Ctrl + Alt + F1 (F1 à F6)

# Retourner à l'interface graphique
Ctrl + Alt + F7
```

**SSH (accès distant) :**
```bash
# Se connecter à distance
ssh utilisateur@adresse_ip

# Exemple
ssh john@192.168.1.100
```

---
layout: default
---

# Structure d'une commande 📝

### Syntaxe et composants

**Format général :**
```bash
commande [options] [arguments]
```

**Exemple :**
```bash
ls -la /home/user
```

**Composants :**
- `ls` : commande
- `-la` : options (flags)
- `/home/user` : argument (chemin)

**Options courantes :**
- `-a` : afficher les fichiers cachés
- `-l` : format long
- `-h` : taille humaine
- `-r` : récursif

---
layout: default
---

# Commandes de navigation 🧭

### Se déplacer dans le système

**Afficher le répertoire courant :**
```bash
pwd
# Sortie : /home/user
```

**Changer de répertoire :**
```bash
cd /chemin/vers/repertoire
cd ..                    # Remonter d'un niveau
cd ~                     # Aller au répertoire home
cd -                     # Retourner au répertoire précédent
```

**Lister le contenu :**
```bash
ls                       # Liste simple
ls -la                   # Liste détaillée
ls -lh                   # Tailles humaines
ls -R                    # Récursif
```

---
layout: default
---

# Commandes de manipulation 📁

### Créer, copier, déplacer, supprimer

**Créer des répertoires :**
```bash
mkdir nom_du_repertoire
mkdir -p chemin/complet    # Créer les parents
mkdir dir1 dir2 dir3       # Créer plusieurs
```

**Copier des fichiers :**
```bash
cp fichier_source destination
cp -r repertoire_source destination
cp -v fichier_source destination    # Verbose
```

**Déplacer/renommer :**
```bash
mv ancien_nom nouveau_nom
mv fichier destination/
mv -i fichier destination/    # Confirmation
```

---
layout: default
---

# Commandes de suppression 🗑️

### Supprimer fichiers et répertoires

**Supprimer des fichiers :**
```bash
rm fichier
rm -f fichier              # Forcer sans confirmation
rm -i fichier              # Confirmation interactive
rm fichier1 fichier2       # Plusieurs fichiers
```

**Supprimer des répertoires :**
```bash
rmdir repertoire_vide
rm -r repertoire           # Récursif
rm -rf repertoire          # Forcer récursif
```

**Attention :**
- `rm -rf /` est dangereux
- Toujours vérifier avant de supprimer
- Utiliser `-i` pour confirmation
- Faire des sauvegardes

---
layout: default
---

# Visualisation de fichiers 📄

### Afficher le contenu

**Afficher tout le contenu :**
```bash
cat fichier
cat fichier1 fichier2      # Concaténer
cat > nouveau_fichier      # Créer un fichier
```

**Navigation dans les fichiers :**
```bash
less fichier               # Navigation interactive
more fichier               # Navigation page par page
head -n 10 fichier         # 10 premières lignes
tail -n 10 fichier         # 10 dernières lignes
```

**Recherche dans les fichiers :**
```bash
grep "motif" fichier
grep -i "motif" fichier   # Insensible à la casse
grep -r "motif" repertoire # Récursif
```

---
layout: default
---

# Redirection et pipes 🔄

### Combiner les commandes

**Redirection de sortie :**
```bash
commande > fichier         # Écraser
commande >> fichier        # Ajouter
commande 2> fichier        # Erreurs
commande &> fichier        # Tout
```

**Redirection d'entrée :**
```bash
commande < fichier
cat < fichier
```

**Pipes (tuyaux) :**
```bash
commande1 | commande2
ls -la | grep "fichier"
ps aux | grep "processus"
```

---
layout: default
---

# Variables d'environnement 🌍

### Configuration du système

**Variables importantes :**
```bash
echo $HOME                 # Répertoire home
echo $PATH                 # Chemin de recherche
echo $USER                 # Utilisateur actuel
echo $SHELL                # Shell utilisé
```

**Définir une variable :**
```bash
VARIABLE=valeur
export VARIABLE=valeur     # Exporter
export PATH=$PATH:/nouveau/chemin
```

**Variables système :**
```bash
$HOME                      # Répertoire utilisateur
$PWD                       # Répertoire courant
$USER                      # Nom utilisateur
$HOSTNAME                  # Nom de la machine
```

---
layout: default
---

# Historique des commandes 📚

### Gérer l'historique

**Afficher l'historique :**
```bash
history                    # Toutes les commandes
history 10                 # 10 dernières
history | grep "motif"     # Rechercher
```

**Navigation dans l'historique :**
```bash
Ctrl + R                  # Recherche inversée
↑ ↓                       # Flèches pour naviguer
!n                        # Exécuter la commande n
!!                        # Dernière commande
```

**Configuration :**
```bash
export HISTSIZE=1000      # Taille de l'historique
export HISTFILESIZE=2000  # Taille du fichier
export HISTCONTROL=ignoredups  # Ignorer les doublons
```

---
layout: default
---

# Complétion et alias 🎯

### Améliorer la productivité

**Complétion automatique :**
```bash
# Appuyer sur Tab pour compléter
cd /ho[TAB]               # Complète /home/
ls fichier[TAB]           # Complète le nom
```

**Alias utiles :**
```bash
alias ll='ls -la'
alias la='ls -A'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'
alias h='history'
alias grep='grep --color=auto'
```

**Créer des alias permanents :**
```bash
# Ajouter dans ~/.bashrc ou ~/.zshrc
echo 'alias ll="ls -la"' >> ~/.bashrc
source ~/.bashrc
```

---
layout: default
---

# Permissions et sudo 🔐

### Droits d'accès

**Voir les permissions :**
```bash
ls -la
# -rw-r--r-- 1 user group 1234 Jan 1 12:00 fichier
```

**Changer les permissions :**
```bash
chmod 755 fichier
chmod +x script.sh
chmod -w fichier
```

**Changer le propriétaire :**
```bash
chown utilisateur fichier
chown utilisateur:groupe fichier
chgrp groupe fichier
```

**Utiliser sudo :**
```bash
sudo commande              # Exécuter en tant que root
sudo -i                    # Shell root
sudo su -                  # Changer d'utilisateur
```

---
layout: default
---

# Recherche de fichiers 🔍

### Trouver des fichiers

**Commande find :**
```bash
find /chemin -name "nom"
find . -name "*.txt"
find /home -user utilisateur
find . -type f -mtime -7  # Modifiés dans les 7 jours
```

**Commande locate :**
```bash
locate nom_fichier
updatedb                   # Mettre à jour la base
```

**Commande which :**
```bash
which commande             # Chemin de la commande
whereis commande           # Plus d'informations
```

---
layout: default
---

# Processus et services ⚡

### Gérer les processus

**Voir les processus :**
```bash
ps aux                     # Tous les processus
ps -ef                     # Format étendu
top                        # Interactif
htop                       # Interface améliorée
```

**Gérer les processus :**
```bash
kill PID                   # Terminer un processus
kill -9 PID                # Forcer la terminaison
killall nom_processus      # Tuer par nom
pkill nom_processus        # Tuer par nom
```

**Services systemd :**
```bash
systemctl status service
systemctl start service
systemctl stop service
systemctl restart service
```

---
layout: default
---

# Réseau et connectivité 🌐

### Commandes réseau

**Configuration réseau :**
```bash
ip addr show               # Adresses IP
ip route show              # Routes
ifconfig                   # Interfaces (déprécié)
```

**Test de connectivité :**
```bash
ping google.com
ping -c 4 google.com      # 4 pings
traceroute google.com      # Tracer le chemin
```

**Ports et connexions :**
```bash
netstat -tuln              # Ports en écoute
ss -tuln                   # Alternative moderne
lsof -i                    # Fichiers ouverts
```

---
layout: default
---

# Compression et archives 📦

### Gérer les archives

**Créer des archives :**
```bash
tar -czf archive.tar.gz repertoire/
tar -cjf archive.tar.bz2 repertoire/
zip -r archive.zip repertoire/
```

**Extraire des archives :**
```bash
tar -xzf archive.tar.gz
tar -xjf archive.tar.bz2
unzip archive.zip
```

**Options tar :**
```bash
-c                        # Créer
-x                        # Extraire
-z                        # Gzip
-j                        # Bzip2
-f                        # Fichier
-v                        # Verbose
```

---
layout: default
---

# Scripting de base 📝

### Premiers scripts

**Créer un script :**
```bash
#!/bin/bash
# Mon premier script
echo "Hello World!"
echo "Date: $(date)"
echo "Utilisateur: $USER"
```

**Exécuter un script :**
```bash
chmod +x script.sh
./script.sh
bash script.sh
```

**Variables dans les scripts :**
```bash
#!/bin/bash
NOM="John"
echo "Bonjour $NOM"
echo "Arguments: $1 $2 $3"
echo "Nombre d'arguments: $#"
```

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique

**Exercice 1 : Navigation**
```bash
# Créer une structure de répertoires
mkdir -p projet/{src,docs,tests}
cd projet
ls -la
pwd
```

**Exercice 2 : Manipulation**
```bash
# Créer des fichiers
touch src/main.py
echo "print('Hello')" > src/main.py
cp src/main.py tests/
ls -R
```

**Exercice 3 : Recherche**
```bash
# Trouver tous les fichiers .py
find . -name "*.py"
# Compter les lignes
wc -l src/main.py
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour la ligne de commande

**Sécurité :**
- Toujours vérifier avant de supprimer
- Utiliser des chemins absolus quand nécessaire
- Faire des sauvegardes régulières
- Ne pas exécuter de commandes inconnues

**Productivité :**
- Utiliser la complétion automatique
- Créer des alias pour les commandes fréquentes
- Utiliser l'historique efficacement
- Combiner les commandes avec des pipes

**Organisation :**
- Structurer les répertoires logiquement
- Utiliser des noms de fichiers descriptifs
- Documenter les scripts
- Maintenir un environnement propre

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Système de fichiers** et organisation
2. **Commandes avancées** et options
3. **Gestion des utilisateurs** et groupes
4. **Permissions et sécurité** avancées
5. **Processus et services** système

**Préparation :**
- Pratiquez les commandes de base
- Familiarisez-vous avec le terminal
- Créez votre propre structure de fichiers

---
src: './pages/04-systeme-fichiers.md'
--- 