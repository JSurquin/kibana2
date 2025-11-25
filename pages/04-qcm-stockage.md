---

# QCM Module 5 : Stockage et fichiers ✅

**10 questions sur disques, partitions et systèmes de fichiers**

---

# Question 1

Quelle commande affiche l'espace disque disponible ?

A) `du`

B) `df`

C) `disk`

D) `free`

---

# Question 2

Où sont stockées les configurations de montage automatique ?

A) `/etc/mount`

B) `/etc/fstab`

C) `/etc/filesystems`

D) `/boot/mount.conf`

---

# Question 3

Quelle commande monte un système de fichiers ?

A) `mount`

B) `attach`

C) `connect`

D) `link`

---

# Question 4

Que signifie LVM ?

A) Linux Virtual Memory

B) Logical Volume Manager

C) Linux Volume Management

D) Local Virtual Machine

---

# Question 5

Comment créer un lien symbolique ?

A) `link`

B) `ln`

C) `ln -s`

D) `symlink`

---

# Question 6

Que fait `fsck` ?

A) Monte un système de fichiers

B) Vérifie l'intégrité d'un système de fichiers

C) Formate un disque

D) Affiche l'espace disque

---

# Question 7

Qu'est-ce qu'une ACL ?

A) Un type de partition

B) Un système de fichiers

C) Une permission étendue (Access Control List)

D) Un type de disque

---

# Question 8

Quelle commande affiche les ACL d'un fichier ?

A) `acl`

B) `getfacl`

C) `showacl`

D) `chmod`

---

# Question 9

Qu'est-ce que le swap ?

A) Une partition de sauvegarde

B) Une extension de la RAM sur disque

C) Un type de RAID

D) Un système de fichiers

---

# Question 10

Quelle commande liste les disques et partitions ?

A) `disks`

B) `partitions`

C) `lsblk`

D) `listdisk`

---

# Réponses du Module 5 📝

**Réponse 1 :** B) `df`
- `df` (Disk Free) affiche l'espace par partition. `du` affiche l'usage par répertoire

**Réponse 2 :** B) `/etc/fstab`
- fstab = File System TABle

**Réponse 3 :** A) `mount`
- `mount /dev/sdb1 /mnt/data` monte la partition

**Réponse 4 :** B) Logical Volume Manager
- LVM permet une gestion flexible du stockage

**Réponse 5 :** C) `ln -s`
- `ln -s cible lien` crée un lien symbolique

---

# Réponses du Module 5 (suite) 📝

**Réponse 6 :** B) Vérifie l'intégrité d'un système de fichiers
- ⚠️ Ne jamais fsck une partition montée !

**Réponse 7 :** C) Une permission étendue (Access Control List)
- Les ACL permettent des permissions plus fines que rwx

**Réponse 8 :** B) `getfacl`
- `setfacl` pour définir, `getfacl` pour lire

**Réponse 9 :** B) Une extension de la RAM sur disque
- Utilisé quand la RAM est pleine

**Réponse 10 :** C) `lsblk`
- lsblk = list block devices

---

# Score du Module 5 📊

Comptez vos bonnes réponses :

- **9-10** : Excellent ! ⭐⭐⭐
- **7-8** : Bien ! ⭐⭐
- **5-6** : À revoir ⭐
- **< 5** : Relire le module 4
