---
layout: intro
routeAlias: 'systeme-fichiers'
---

# Système de fichiers 📁

### Organisation et structure des fichiers Unix/Linux

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Explorons la structure des fichiers <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Philosophie "Tout est un fichier" 🎯

### Concept fondamental d'Unix

**Principe :**
- Tout dans le système est représenté comme un fichier
- Interface unifiée pour manipuler tout
- Fichiers, répertoires, périphériques, processus

**Types de "fichiers" :**
- **Fichiers ordinaires** : données, programmes
- **Répertoires** : conteneurs d'autres fichiers
- **Liens** : références vers d'autres fichiers
- **Périphériques** : matériel (disques, ports)
- **Sockets** : communication réseau
- **Pipes** : communication entre processus

---
layout: default
---

# Structure hiérarchique 🌳

### Organisation en arborescence

**Racine du système :**
```
/
├── bin/          # Commandes essentielles
├── boot/         # Fichiers de démarrage
├── dev/          # Périphériques
├── etc/          # Configuration
├── home/         # Répertoires utilisateurs
├── lib/          # Bibliothèques
├── media/        # Points de montage
├── mnt/          # Montage temporaire
├── opt/          # Applications optionnelles
├── proc/         # Informations processus
├── root/         # Répertoire root
├── sbin/         # Commandes système
├── tmp/          # Fichiers temporaires
├── usr/          # Applications utilisateur
└── var/          # Données variables
```

---
layout: default
---

# Répertoires système importants 🏗️

### Fonction de chaque répertoire

**`/bin` et `/sbin`**
- Commandes essentielles du système
- Accessibles même en mode single-user
- `/sbin` : commandes d'administration

**`/usr`**
- Applications et données utilisateur
- `/usr/bin` : commandes utilisateur
- `/usr/local` : applications locales
- `/usr/share` : données partagées

**`/etc`**
- Fichiers de configuration
- Scripts de démarrage
- Paramètres système

---
layout: default
---

# Répertoires système importants 🏗️

### Fonction de chaque répertoire

**`/home`**
- Répertoires personnels des utilisateurs
- `/home/username` pour chaque utilisateur
- Fichiers de configuration utilisateur

**`/var`**
- Données qui changent fréquemment
- `/var/log` : fichiers de logs
- `/var/spool` : files d'attente
- `/var/cache` : cache

**`/tmp` et `/var/tmp`**
- Fichiers temporaires
- Effacés au redémarrage
- Permissions spéciales

---
layout: default
---

# Répertoires spéciaux 🔍

### Répertoires virtuels

**`/proc`**
- Informations sur les processus
- `/proc/cpuinfo` : informations CPU
- `/proc/meminfo` : informations mémoire
- `/proc/net` : informations réseau

**`/dev`**
- Périphériques du système
- `/dev/sda` : premier disque dur
- `/dev/tty` : terminaux
- `/dev/null` : trou noir
- `/dev/zero` : source de zéros

**`/sys`**
- Informations sur le noyau
- Configuration matérielle
- Paramètres système

---
layout: default
---

# Chemins absolus et relatifs 🛤️

### Navigation dans l'arborescence

**Chemin absolu :**
```bash
# Commence toujours par /
/home/user/documents/file.txt
/etc/passwd
/usr/bin/python3
```

**Chemin relatif :**
```bash
# Relatif au répertoire courant
./fichier.txt              # Répertoire courant
../parent/file.txt         # Répertoire parent
../../grandparent/file.txt # Deux niveaux au-dessus
```

**Symboles spéciaux :**
```bash
.                         # Répertoire courant
..                        # Répertoire parent
~                         # Répertoire home
-                         # Répertoire précédent
```

---
layout: default
---

# Commandes de navigation avancée 🧭

### Outils pour explorer le système

**Afficher l'arborescence :**
```bash
tree                       # Vue arborescente
tree -L 2                  # Limiter la profondeur
tree -d                    # Seulement les répertoires
tree -I "node_modules"     # Ignorer des patterns
```

**Trouver des fichiers :**
```bash
find / -name "fichier.txt" # Recherche globale
find . -type f -name "*.py" # Fichiers Python
find . -mtime -7           # Modifiés dans les 7 jours
find . -size +100M         # Plus de 100MB
```

**Localiser des fichiers :**
```bash
locate fichier.txt         # Base de données
updatedb                   # Mettre à jour la base
```

---
layout: default
---

# Types de fichiers 📄

### Classification des fichiers

**Fichiers ordinaires (-)**
```bash
# Fichiers de données, programmes
-rw-r--r-- 1 user group 1234 Jan 1 12:00 fichier.txt
```

**Répertoires (d)**
```bash
# Conteneurs d'autres fichiers
drwxr-xr-x 2 user group 4096 Jan 1 12:00 repertoire/
```

**Liens symboliques (l)**
```bash
# Références vers d'autres fichiers
lrwxrwxrwx 1 user group 15 Jan 1 12:00 lien -> cible
```

**Périphériques (b/c)**
```bash
# b = bloc, c = caractère
brw-rw---- 1 root disk 8, 0 Jan 1 12:00 /dev/sda
```

---
layout: default
---

# Permissions des fichiers 🔐

### Système de permissions Unix

**Structure des permissions :**
```bash
-rw-r--r-- 1 user group 1234 Jan 1 12:00 fichier
# Type | Owner | Group | Others
```

**Types de permissions :**
- **r** (read) : lecture
- **w** (write) : écriture
- **x** (execute) : exécution

**Pour les répertoires :**
- **r** : lister le contenu
- **w** : créer/supprimer des fichiers
- **x** : accéder au répertoire

**Représentation numérique :**
- r = 4, w = 2, x = 1
- 755 = rwxr-xr-x
- 644 = rw-r--r--

---
layout: default
---

# Commandes de gestion des permissions 🔧

### Modifier les permissions

**chmod (changer mode) :**
```bash
chmod 755 fichier          # Permissions numériques
chmod +x script.sh         # Ajouter exécution
chmod -w fichier.txt       # Retirer écriture
chmod u+x,g-w,o=r fichier  # Permissions symboliques
```

**chown (changer owner) :**
```bash
chown user fichier         # Changer propriétaire
chown user:group fichier   # Changer propriétaire et groupe
chown -R user repertoire/  # Récursif
```

**chgrp (changer group) :**
```bash
chgrp groupe fichier       # Changer groupe
chgrp -R groupe repertoire/ # Récursif
```

---
layout: default
---

# Liens symboliques et durs 🔗

### Créer des références

**Liens symboliques (soft links) :**
```bash
ln -s cible lien           # Créer un lien symbolique
ln -s /chemin/absolu lien  # Lien vers chemin absolu
ln -s ../parent/file lien  # Lien vers chemin relatif
```

**Liens durs (hard links) :**
```bash
ln cible lien              # Créer un lien dur
# Même inode, même fichier
```

**Différences :**
- **Lien symbolique** : pointeur vers un nom
- **Lien dur** : même inode, même fichier
- Liens symboliques peuvent pointer vers des cibles inexistantes
- Liens durs ne peuvent pointer que vers des fichiers existants

---
layout: default
---

# Montage de systèmes de fichiers 🖥️

### Attacher des périphériques

**Commandes de montage :**
```bash
mount /dev/sda1 /mnt      # Monter un périphérique
umount /mnt               # Démontage
mount -a                  # Monter tous les systèmes
```

**Fichier /etc/fstab :**
```bash
# Configuration automatique
/dev/sda1  /  ext4  defaults  0  1
/dev/sdb1  /home  ext4  defaults  0  2
```

**Types de systèmes de fichiers :**
- **ext4** : Linux moderne
- **xfs** : Haute performance
- **btrfs** : Avancé avec snapshots
- **ntfs** : Windows
- **fat32** : Compatibilité

---
layout: default
---

# Espace disque et inodes 💾

### Gestion de l'espace

**Vérifier l'espace disque :**
```bash
df -h                      # Espace disque humain
df -i                      # Utilisation des inodes
du -h repertoire/          # Taille des répertoires
du -sh *                   # Taille de tous les éléments
```

**Inodes :**
- Métadonnées des fichiers
- Limite le nombre de fichiers
- `df -i` pour vérifier l'utilisation
- Peut être épuisé même avec de l'espace libre

**Nettoyage :**
```bash
find . -type f -size +100M # Fichiers volumineux
find . -name "*.tmp" -delete # Supprimer fichiers temporaires
```

---
layout: default
---

# Systèmes de fichiers journalisés 📝

### Intégrité des données

**Avantages du journaling :**
- Récupération après crash
- Cohérence des données
- Moins de vérification fsck

**Types de journaling :**
- **metadata** : journal des métadonnées
- **ordered** : données avant métadonnées
- **writeback** : performance maximale

**Commandes de vérification :**
```bash
fsck /dev/sda1            # Vérification du système de fichiers
e2fsck /dev/sda1          # Vérification ext2/ext3/ext4
xfs_repair /dev/sda1      # Vérification XFS
```

---
layout: default
---

# Quotas et limites 📊

### Contrôle de l'espace utilisateur

**Configuration des quotas :**
```bash
# Activer les quotas
mount -o usrquota,grpquota /dev/sda1 /home

# Initialiser les quotas
quotacheck -cugm /home
quotaon -av

# Éditer les quotas
edquota -u utilisateur
```

**Commandes de quota :**
```bash
quota -u utilisateur       # Voir les quotas utilisateur
repquota -a                # Rapport global
warnquota                  # Envoyer des avertissements
```

**Limites configurables :**
- Limite douce (soft limit)
- Limite dure (hard limit)
- Période de grâce

---
layout: default
---

# Sauvegarde et restauration 💾

### Stratégies de sauvegarde

**Sauvegarde complète :**
```bash
tar -czf backup_$(date +%Y%m%d).tar.gz /chemin/
dd if=/dev/sda of=backup.img bs=4M status=progress
```

**Sauvegarde incrémentale :**
```bash
rsync -av --delete source/ destination/
rsync -av --link-dest=/backup/previous source/ /backup/current/
```

**Sauvegarde différentielle :**
```bash
# Basée sur la dernière sauvegarde complète
tar -czf diff_$(date +%Y%m%d).tar.gz -N backup_complete_date /
```

**Restauration :**
```bash
tar -xzf backup.tar.gz -C /destination/
dd if=backup.img of=/dev/sda bs=4M status=progress
```

---
layout: default
---

# Monitoring du système de fichiers 📈

### Surveillance en temps réel

**Commandes de monitoring :**
```bash
# Utilisation disque
df -h
iostat -x 1               # Statistiques I/O

# Activité disque
iotop                     # Top des processus I/O
lsof                      # Fichiers ouverts

# Surveillance des logs
tail -f /var/log/syslog   # Suivre les logs système
journalctl -f             # Logs systemd
```

**Outils graphiques :**
- **Disk Usage Analyzer** (Baobab)
- **GParted** (partitionnement)
- **Smartmontools** (santé des disques)

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique

**Exercice 1 : Exploration**
```bash
# Explorer la structure système
ls -la /bin | head -10
find /usr/bin -name "*python*"
tree /etc -L 2
```

**Exercice 2 : Permissions**
```bash
# Créer et modifier des permissions
touch test.txt
chmod 755 test.txt
ls -la test.txt
chmod 644 test.txt
```

**Exercice 3 : Liens**
```bash
# Créer des liens
echo "contenu" > source.txt
ln -s source.txt lien_symbolique
ln source.txt lien_dur
ls -la
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour la gestion des fichiers

**Organisation :**
- Structure logique et cohérente
- Noms de fichiers descriptifs
- Éviter les espaces dans les noms
- Utiliser des extensions appropriées

**Sécurité :**
- Permissions minimales nécessaires
- Sauvegardes régulières
- Surveillance de l'espace disque
- Vérification de l'intégrité

**Performance :**
- Éviter les répertoires trop profonds
- Utiliser des systèmes de fichiers appropriés
- Défragmentation si nécessaire
- Monitoring des I/O

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Commandes avancées** et options
2. **Gestion des utilisateurs** et groupes
3. **Permissions et sécurité** avancées
4. **Processus et services** système
5. **Réseaux et connectivité**

**Préparation :**
- Familiarisez-vous avec la structure des répertoires
- Pratiquez les commandes de navigation
- Testez les permissions et liens

---
src: './pages/05-commandes-basiques.md'
--- 