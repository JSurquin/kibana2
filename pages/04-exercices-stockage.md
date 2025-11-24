---

# 🎯 Exercices pratiques - Module 5

**Gestion du stockage et du système de fichiers**

---

# Exercice 1 : Analyse de l'espace disque 💾

**Objectif :** Identifier l'utilisation du disque

**Consignes :**

1. Affichez l'espace disque de toutes les partitions
2. Trouvez le répertoire qui occupe le plus d'espace dans `/var`
3. Listez tous les fichiers de plus de 100 Mo
4. Affichez l'utilisation des inodes
5. Trouvez les 10 plus gros fichiers du système

---

# Solution Exercice 1 💡

```bash
# 1. Espace disque
df -h

# 2. Plus gros répertoires dans /var
sudo du -h /var --max-depth=1 | sort -hr | head -10

# 3. Fichiers > 100 Mo
sudo find / -type f -size +100M 2>/dev/null
# Avec détails
sudo find / -type f -size +100M -exec ls -lh {} \; 2>/dev/null

# 4. Utilisation des inodes
df -i

# 5. Top 10 plus gros fichiers
sudo find / -type f -exec du -h {} + 2>/dev/null | sort -hr | head -10
```

---

# Exercice 2 : Manipulation de fichiers image 🖼️

**Objectif :** Créer et utiliser des fichiers image

**Consignes :**

1. Créez un fichier image de 100 Mo
2. Formatez-le en ext4
3. Montez-le dans `/mnt/test`
4. Créez des fichiers dedans
5. Démontez proprement
6. Remontez et vérifiez que les fichiers sont toujours là

---

# Solution Exercice 2 💡

```bash
# 1. Créer fichier image
dd if=/dev/zero of=~/disk.img bs=1M count=100

# 2. Formater en ext4
sudo mkfs.ext4 ~/disk.img

# 3. Monter
sudo mkdir -p /mnt/test
sudo mount -o loop ~/disk.img /mnt/test

# Vérifier
df -h /mnt/test
mount | grep test

# 4. Créer des fichiers
sudo touch /mnt/test/fichier{1..5}.txt
echo "Test" | sudo tee /mnt/test/fichier1.txt
ls -l /mnt/test/

# 5. Démonter
sudo umount /mnt/test

# 6. Remonter et vérifier
sudo mount -o loop ~/disk.img /mnt/test
ls -l /mnt/test/
# Les fichiers sont toujours là !

# Nettoyer
sudo umount /mnt/test
rm ~/disk.img
```

---

# Exercice 3 : Liens symboliques et durs 🔗

**Objectif :** Comprendre les liens

**Consignes :**

1. Créez un fichier `original.txt` avec du contenu
2. Créez un lien dur `hard_link.txt`
3. Créez un lien symbolique `soft_link.txt`
4. Comparez les inodes
5. Supprimez `original.txt` et voyez ce qui se passe
6. Créez un lien symbolique vers un répertoire

---

# Solution Exercice 3 💡

```bash
# 1. Fichier original
echo "Contenu original" > /tmp/original.txt

# 2. Lien dur
ln /tmp/original.txt /tmp/hard_link.txt

# 3. Lien symbolique
ln -s /tmp/original.txt /tmp/soft_link.txt

# 4. Comparer inodes
ls -li /tmp/*.txt
# original et hard_link ont le MÊME inode
# soft_link a un inode différent

# Vérifier le contenu
cat /tmp/hard_link.txt
cat /tmp/soft_link.txt

# 5. Supprimer l'original
rm /tmp/original.txt

# hard_link fonctionne toujours !
cat /tmp/hard_link.txt  # ✅ OK

# soft_link est cassé !
cat /tmp/soft_link.txt  # ❌ No such file

# 6. Lien vers répertoire
ln -s /var/log /tmp/logs_link
ls -l /tmp/logs_link

# Nettoyer
rm /tmp/hard_link.txt /tmp/soft_link.txt /tmp/logs_link
```

---

# Exercice 4 : ACL - Permissions étendues 🔐

**Objectif :** Utiliser les ACL pour des permissions fines

**Consignes :**

1. Créez un fichier `projet.txt`
2. Donnez à `alice` la permission de lecture (en plus des permissions Unix)
3. Donnez à `bob` la permission d'écriture
4. Affichez les ACL
5. Supprimez les ACL de `bob`
6. Supprimez toutes les ACL

---

# Solution Exercice 4 💡

```bash
# 1. Créer le fichier
touch /tmp/projet.txt
echo "Projet secret" > /tmp/projet.txt

# 2. ACL pour alice (lecture)
setfacl -m u:alice:r /tmp/projet.txt

# 3. ACL pour bob (écriture)
setfacl -m u:bob:rw /tmp/projet.txt

# 4. Afficher les ACL
getfacl /tmp/projet.txt

# ls avec ACL
ls -l /tmp/projet.txt
# Un '+' apparaît : -rw-rw-r--+

# 5. Supprimer ACL de bob
setfacl -x u:bob /tmp/projet.txt
getfacl /tmp/projet.txt

# 6. Supprimer toutes les ACL
setfacl -b /tmp/projet.txt
getfacl /tmp/projet.txt
```

---

# Exercice 5 : Montage automatique avec fstab 📋

**Objectif :** Configurer des montages automatiques

**Consignes :**

1. Créez un fichier image et formatez-le
2. Créez un point de montage `/data`
3. Ajoutez une entrée dans `/etc/fstab` pour montage automatique
4. Testez avec `mount -a`
5. Redémarrez et vérifiez que c'est monté automatiquement

⚠️ **Attention :** Erreur dans fstab = système qui ne boot pas !

---

# Solution Exercice 5 💡

```bash
# 1. Créer et formater
dd if=/dev/zero of=/opt/data.img bs=1M count=200
sudo mkfs.ext4 /opt/data.img

# 2. Point de montage
sudo mkdir /data

# 3. Sauvegarder fstab avant modification
sudo cp /etc/fstab /etc/fstab.backup

# Ajouter entrée
echo "/opt/data.img /data ext4 loop,defaults 0 0" | \
    sudo tee -a /etc/fstab

# 4. Tester
sudo mount -a
df -h /data

# Si erreur, restaurer :
# sudo cp /etc/fstab.backup /etc/fstab

# 5. Redémarrer
sudo reboot

# Après reboot, vérifier
df -h /data
mount | grep data
```

---

# Exercice bonus : Script de nettoyage disque 🧹

**Objectif :** Automatiser le nettoyage

**Créez un script qui :**
- Supprime les fichiers temp de plus de 7 jours
- Vide les corbeilles
- Nettoie les caches apt
- Nettoie les vieux logs
- Affiche un rapport

---

# Solution Exercice bonus 💡

```bash
#!/bin/bash
# Script de nettoyage disque

echo "===== Nettoyage disque $(date) ====="

# Espace avant
BEFORE=$(df -h / | awk 'NR==2 {print $4}')
echo "Espace libre avant : $BEFORE"

# 1. Fichiers temp > 7 jours
echo -e "\n🗑️  Nettoyage /tmp..."
sudo find /tmp -type f -atime +7 -delete 2>/dev/null
FILES_DELETED=$(sudo find /tmp -type f -atime +7 2>/dev/null | wc -l)
echo "Fichiers supprimés : $FILES_DELETED"

# 2. Caches apt
echo -e "\n📦 Nettoyage apt..."
sudo apt clean
sudo apt autoclean
sudo apt autoremove -y

# 3. Logs anciens
echo -e "\n📜 Nettoyage logs..."
sudo journalctl --vacuum-time=7d
sudo find /var/log -name "*.log.*" -mtime +30 -delete

# 4. Thumbnails
echo -e "\n🖼️  Nettoyage thumbnails..."
rm -rf ~/.cache/thumbnails/*

# Espace après
AFTER=$(df -h / | awk 'NR==2 {print $4}')
echo -e "\n✅ Espace libre après : $AFTER"
echo "============================="
```

---

# Points clés à retenir 📌

**Gestion disque :**
- `df` pour partitions, `du` pour répertoires
- `lsblk` pour voir l'arborescence
- `mount`/`umount` pour monter/démonter

**Liens :**
- Lien dur : même inode, même contenu
- Lien symbolique : pointeur, peut casser

**ACL :**
- Permissions plus fines que rwx
- `setfacl` pour définir, `getfacl` pour lire

---

