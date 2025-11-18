---

# QCM Module 4 : Stockage et fichiers ✅

**10 questions sur disques, partitions et systèmes de fichiers**

---

# Question 1

Quelle commande affiche l'espace disque disponible ?

A) `du`
B) `df`
C) `disk`
D) `free`

---

# Réponse 1

**Bonne réponse : B) `df`**

`df` (Disk Free) affiche l'espace par partition. `du` affiche l'usage par répertoire.

---

# Question 2

Où sont stockées les configurations de montage automatique ?

A) `/etc/mount`
B) `/etc/fstab`
C) `/etc/filesystems`
D) `/boot/mount.conf`

---

# Réponse 2

**Bonne réponse : B) `/etc/fstab`**

fstab = File System TABle.

---

# Question 3

Quelle commande monte un système de fichiers ?

A) `mount`
B) `attach`
C) `connect`
D) `link`

---

# Réponse 3

**Bonne réponse : A) `mount`**

`mount /dev/sdb1 /mnt/data` monte la partition.

---

# Question 4

Que signifie LVM ?

A) Linux Virtual Memory
B) Logical Volume Manager
C) Linux Volume Management
D) Local Virtual Machine

---

# Réponse 4

**Bonne réponse : B) Logical Volume Manager**

LVM permet une gestion flexible du stockage.

---

# Question 5

Comment créer un lien symbolique ?

A) `link`
B) `ln`
C) `ln -s`
D) `symlink`

---

# Réponse 5

**Bonne réponse : C) `ln -s`**

`ln -s cible lien` crée un lien symbolique.

---

# Question 6

Que fait `fsck` ?

A) Monte un système de fichiers
B) Vérifie l'intégrité d'un système de fichiers
C) Formate un disque
D) Affiche l'espace disque

---

# Réponse 6

**Bonne réponse : B) Vérifie l'intégrité d'un système de fichiers**

⚠️ Ne jamais fsck une partition montée !

---

# Question 7

Qu'est-ce qu'une ACL ?

A) Un type de partition
B) Un système de fichiers
C) Une permission étendue (Access Control List)
D) Un type de disque

---

# Réponse 7

**Bonne réponse : C) Une permission étendue (Access Control List)**

Les ACL permettent des permissions plus fines que rwx.

---

# Question 8

Quelle commande affiche les ACL d'un fichier ?

A) `acl`
B) `getfacl`
C) `showacl`
D) `chmod`

---

# Réponse 8

**Bonne réponse : B) `getfacl`**

`setfacl` pour définir, `getfacl` pour lire.

---

# Question 9

Qu'est-ce que le swap ?

A) Une partition de sauvegarde
B) Une extension de la RAM sur disque
C) Un type de RAID
D) Un système de fichiers

---

# Réponse 9

**Bonne réponse : B) Une extension de la RAM sur disque**

Utilisé quand la RAM est pleine.

---

# Question 10

Quelle commande liste les disques et partitions ?

A) `disks`
B) `partitions`
C) `lsblk`
D) `listdisk`

---

# Réponse 10

**Bonne réponse : C) `lsblk`**

lsblk = list block devices.

---

# Score du Module 4 📊

Comptez vos bonnes réponses :

- **9-10** : Excellent ! ⭐⭐⭐
- **7-8** : Bien ! ⭐⭐
- **5-6** : À revoir ⭐
- **< 5** : Relire le module 4

---

