---

# 🎯 Exercices pratiques - Module 4

**Gestion utilisateurs, groupes et permissions**

---

# Exercice 1 : Création d'utilisateurs 👥

**Objectif :** Créer et gérer des utilisateurs

**Consignes :**

1. Créez trois utilisateurs : `dev1`, `dev2`, `admin1`
2. Définissez des mots de passe pour chacun
3. Vérifiez leur présence dans `/etc/passwd`
4. Affichez les informations de `dev1` avec `id`
5. Listez les dernières connexions avec `last`

---

# Solution Exercice 1 💡

```bash
# 1. Créer les utilisateurs
sudo useradd -m -s /bin/bash dev1
sudo useradd -m -s /bin/bash dev2
sudo useradd -m -s /bin/bash admin1

# 2. Définir les mots de passe
sudo passwd dev1
sudo passwd dev2
sudo passwd admin1

# Ou en une ligne (automatisé)
echo "dev1:password123" | sudo chpasswd
echo "dev2:password123" | sudo chpasswd
echo "admin1:admin123" | sudo chpasswd

# 3. Vérifier dans passwd
grep -E "dev1|dev2|admin1" /etc/passwd

# 4. Informations de dev1
id dev1

# 5. Dernières connexions
sudo last | head -20
```

---

# Exercice 2 : Gestion des groupes 👨‍👩‍👧‍👦

**Objectif :** Créer et gérer des groupes

**Consignes :**

1. Créez deux groupes : `developers` et `admins`
2. Ajoutez `dev1` et `dev2` au groupe `developers`
3. Ajoutez `admin1` au groupe `admins`
4. Vérifiez les appartenances avec `groups`
5. Affichez le contenu de `/etc/group`

---

# Solution Exercice 2 💡

```bash
# 1. Créer les groupes
sudo groupadd developers
sudo groupadd admins

# 2. Ajouter dev1 et dev2 au groupe developers
sudo usermod -aG developers dev1
sudo usermod -aG developers dev2

# 3. Ajouter admin1 au groupe admins
sudo usermod -aG admins admin1

# 4. Vérifier les appartenances
groups dev1
groups dev2
groups admin1

# Ou avec id
id dev1

# 5. Voir /etc/group
grep -E "developers|admins" /etc/group
```

---

# Exercice 3 : Permissions de base 🔐

**Objectif :** Maîtriser les permissions Unix

**Consignes :**

1. Créez un fichier `projet.txt` appartenant à `dev1`
2. Donnez les permissions `rw-r--r--` (644)
3. Créez un fichier `secret.txt` avec permissions `rw-------` (600)
4. Créez un script `hello.sh` et rendez-le exécutable
5. Créez un répertoire `shared/` avec permissions `rwxrwxr-x` (775)

---

# Solution Exercice 3 💡

```bash
# 1. Créer le fichier
sudo -u dev1 touch /home/dev1/projet.txt

# 2. Permissions 644
sudo chmod 644 /home/dev1/projet.txt
# Vérifier
ls -l /home/dev1/projet.txt

# 3. Fichier secret
sudo -u dev1 touch /home/dev1/secret.txt
sudo chmod 600 /home/dev1/secret.txt

# 4. Script exécutable
echo '#!/bin/bash' | sudo -u dev1 tee /home/dev1/hello.sh
echo 'echo "Hello World!"' | sudo -u dev1 tee -a /home/dev1/hello.sh
sudo chmod +x /home/dev1/hello.sh
# ou
sudo chmod 755 /home/dev1/hello.sh

# 5. Répertoire partagé
sudo mkdir /home/dev1/shared
sudo chmod 775 /home/dev1/shared
```

---

# Exercice 4 : Projet collaboratif 🤝

**Objectif :** Mettre en place un espace de travail partagé

**Scénario :** Vous devez créer un espace où `dev1` et `dev2` peuvent collaborer

**Consignes :**

1. Créez `/shared/project`
2. Changez le propriétaire en `root` et le groupe en `developers`
3. Permissions : rwxrwx--- (770)
4. Créez un fichier dedans en tant que `dev1`
5. Vérifiez que `dev2` peut le modifier
6. Vérifiez que `admin1` ne peut PAS y accéder

---

# Solution Exercice 4 💡

```bash
# 1. Créer le répertoire
sudo mkdir -p /shared/project

# 2. Changer propriétaire et groupe
sudo chown root:developers /shared/project

# 3. Permissions 770
sudo chmod 770 /shared/project

# Vérifier
ls -ld /shared/project

# 4. Créer un fichier en tant que dev1
sudo -u dev1 touch /shared/project/code.txt
sudo -u dev1 bash -c 'echo "Hello from dev1" > /shared/project/code.txt'

# 5. Modifier en tant que dev2
sudo -u dev2 bash -c 'echo "Modified by dev2" >> /shared/project/code.txt'
cat /shared/project/code.txt

# 6. Tester l'accès de admin1 (devrait échouer)
sudo -u admin1 ls /shared/project/
# → Permission denied
```

---

# Exercice 5 : Utilisation de sudo 🔑

**Objectif :** Configurer sudo de manière sécurisée

**Consignes :**

1. Donnez à `admin1` les droits sudo complets
2. Donnez à `dev1` le droit de redémarrer nginx uniquement
3. Créez un groupe `sysadmin` avec tous les droits sudo
4. Testez les commandes avec chaque utilisateur
5. Vérifiez les logs sudo dans `/var/log/auth.log`

---

# Solution Exercice 5 💡

```bash
# 1. Donner tous les droits à admin1
echo "admin1 ALL=(ALL:ALL) ALL" | sudo tee /etc/sudoers.d/admin1
sudo chmod 440 /etc/sudoers.d/admin1

# 2. Droits limités pour dev1
echo "dev1 ALL=(ALL) NOPASSWD: /bin/systemctl restart nginx" | \
    sudo tee /etc/sudoers.d/dev1
sudo chmod 440 /etc/sudoers.d/dev1

# 3. Groupe sysadmin
sudo groupadd sysadmin
echo "%sysadmin ALL=(ALL:ALL) ALL" | sudo tee /etc/sudoers.d/sysadmin
sudo chmod 440 /etc/sudoers.d/sysadmin

# 4. Tester
# En tant qu'admin1
sudo -u admin1 sudo apt update

# En tant que dev1 (devrait échouer)
sudo -u dev1 sudo apt update  # → Devrait échouer

# 5. Voir les logs
sudo tail -f /var/log/auth.log | grep sudo
```

---

# Exercice 6 : Gestion avancée des permissions 🎯

**Objectif :** Utiliser chown, chmod et umask

**Consignes :**

1. Créez `/project` appartenant à `dev1:developers`
2. Mettez umask à `0027` pour `dev1`
3. Créez un fichier en tant que `dev1` et vérifiez ses permissions
4. Changez récursivement le propriétaire de `/project` en `dev2`
5. Donnez les permissions `rwxr-x---` à `/project`

---

# Solution Exercice 6 💡

```bash
# 1. Créer /project
sudo mkdir /project
sudo chown dev1:developers /project

# 2. Modifier umask pour dev1
sudo -u dev1 bash -c 'echo "umask 0027" >> /home/dev1/.bashrc'

# 3. Créer un fichier avec le nouveau umask
sudo -u dev1 bash -c 'umask 0027; touch /project/test.txt'
ls -l /project/test.txt
# → -rw-r----- (permissions 640)

# 4. Changer propriétaire récursif
sudo chown -R dev2:developers /project

# 5. Permissions sur /project
sudo chmod 750 /project
ls -ld /project
```

---

# Exercice bonus : Script de gestion d'utilisateurs 🤖

**Objectif :** Automatiser la création d'utilisateurs

**Créez un script `create_users.sh` qui :**
- Lit un fichier CSV avec : nom,groupe,shell
- Crée les utilisateurs automatiquement
- Génère des mots de passe aléatoires
- Crée un rapport

**Exemple CSV :**

```
alice,developers,/bin/bash
bob,admins,/bin/bash
charlie,developers,/bin/zsh
```

---

#### Solution Exercice bonus 💡

```bash
#!/bin/bash
# Script de création d'utilisateurs

CSV_FILE="users.csv"
REPORT="users_report.txt"

echo "===== Rapport de création =====" > $REPORT
echo "Date: $(date)" >> $REPORT
echo >> $REPORT

while IFS=',' read -r username group shell; do
    # Ignorer l'en-tête
    [[ "$username" == "nom" ]] && continue
    
    echo "Création de $username..."
    
    # Créer l'utilisateur
    sudo useradd -m -s "$shell" "$username" 2>/dev/null
    
    # Créer le groupe si nécessaire
    sudo groupadd "$group" 2>/dev/null
    
    # Ajouter au groupe
    sudo usermod -aG "$group" "$username"
    
    # Générer mot de passe
    PASSWORD=$(openssl rand -base64 12)
    echo "$username:$PASSWORD" | sudo chpasswd
    
    # Rapport
    echo "Utilisateur: $username" >> $REPORT
    echo "Groupe: $group" >> $REPORT
    echo "Shell: $shell" >> $REPORT
    echo "Password: $PASSWORD" >> $REPORT
    echo "---" >> $REPORT
    
done < "$CSV_FILE"

echo "Terminé ! Voir $REPORT"
```

---

### Points clés à retenir 📌

<div class="text-xs">

**Sécurité :**
- Ne jamais travailler en root
- Utiliser sudo avec parcimonie
- Permissions minimales nécessaires

**Bonnes pratiques :**
- Groupes pour organiser
- umask pour sécuriser par défaut
- Documenter les configurations sudo

**Commandes essentielles :**
- `useradd`, `usermod`, `userdel`
- `groupadd`, `groupmod`, `gpasswd`
- `chmod`, `chown`, `chgrp`
- `sudo`, `visudo`

</div>