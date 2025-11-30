---

# 🎯 Exercices pratiques - Module 1

**Mettre en pratique les bases Unix/Linux**

---

# Exercice 1 : Navigation dans le système 🗺️

**Objectif :** Maîtriser la navigation et les commandes de base

**Consignes :**

1. Affichez votre répertoire actuel
2. Listez tous les fichiers (y compris cachés) du répertoire `/etc`
3. Allez dans votre répertoire personnel
4. Créez l'arborescence suivante :

```
formation/
├── documents/
├── scripts/
└── backup/
```

5. Créez un fichier vide `test.txt` dans `documents/`
6. Affichez le chemin absolu de ce fichier

---

# Solution Exercice 1 💡

```bash
# 1. Répertoire actuel
pwd

# 2. Lister /etc avec fichiers cachés
ls -la /etc

# 3. Aller dans le répertoire personnel
cd ~
# ou
cd

# 4. Créer l'arborescence
mkdir -p formation/{documents,scripts,backup}

# 5. Créer le fichier
touch formation/documents/test.txt

# 6. Chemin absolu
realpath formation/documents/test.txt
# ou
readlink -f formation/documents/test.txt
```

---

# Exercice 2 : Manipulation de fichiers 📄

**Objectif :** Créer, copier, déplacer, supprimer des fichiers

**Consignes :**

1. Créez un fichier `infos.txt` contenant votre nom et la date
2. Copiez ce fichier dans le dossier `backup/`
3. Renommez la copie en `infos_backup.txt`
4. Créez 5 fichiers : `file1.txt` à `file5.txt`
5. Supprimez `file3.txt`
6. Déplacez tous les fichiers restants dans `documents/`

---

# Solution Exercice 2 💡

```bash
# 1. Créer le fichier avec contenu
echo "Nom: $(whoami)" > formation/infos.txt
echo "Date: $(date)" >> formation/infos.txt

# 2. Copier dans backup
cp formation/infos.txt formation/backup/

# 3. Renommer
mv formation/backup/infos.txt formation/backup/infos_backup.txt

# 4. Créer 5 fichiers
touch formation/file{1..5}.txt

# 5. Supprimer file3
rm formation/file3.txt

# 6. Déplacer les fichiers restants
mv formation/file*.txt formation/documents/
```

---

# Exercice 3 : Recherche et filtrage 🔍

**Objectif :** Utiliser grep, find et les pipes

**Consignes :**

1. Créez un fichier `users.txt` avec ce contenu :

```
alice:admin:alice@example.com
bob:user:bob@example.com
charlie:admin:charlie@example.com
david:user:david@example.com
```

2. Affichez uniquement les lignes contenant "admin"
3. Comptez le nombre total de lignes
4. Affichez uniquement les emails
5. Trouvez tous les fichiers `.txt` dans `formation/`

---

# Solution Exercice 3 💡

```bash
# 1. Créer le fichier
cat > formation/users.txt << EOF
alice:admin:alice@example.com
bob:user:bob@example.com
charlie:admin:charlie@example.com
david:user:david@example.com
EOF

# 2. Lignes avec "admin"
grep "admin" formation/users.txt

# 3. Compter les lignes
wc -l formation/users.txt
# ou
cat formation/users.txt | wc -l

# 4. Afficher seulement les emails (3ème colonne)
cut -d':' -f3 formation/users.txt
# ou
awk -F':' '{print $3}' formation/users.txt

# 5. Trouver tous les .txt
find formation/ -name "*.txt"
```

---

# Exercice 4 : Redirections et pipes 🔄

**Objectif :** Maîtriser les redirections et chaînage de commandes

**Consignes :**

1. Listez tous les fichiers de `/etc` et sauvegardez dans `etc_files.txt`
2. Comptez combien de fichiers contiennent "conf" dans leur nom
3. Affichez les 10 premiers fichiers uniquement
4. Créez un fichier `errors.log` qui capture les erreurs d'une commande
5. Créez une chaîne de commandes qui :
   - Liste les processus
   - Cherche ceux contenant "bash"
   - Compte combien il y en a

---

# Solution Exercice 4 💡

```bash
# 1. Sauvegarder la liste
ls /etc > formation/etc_files.txt

# 2. Compter les fichiers avec "conf"
ls /etc | grep "conf" | wc -l

# 3. 10 premiers fichiers
ls /etc | head -10

# 4. Capturer les erreurs
ls /root 2> formation/errors.log
# La commande échoue normalement, les erreurs vont dans le fichier

# 5. Chaîne de commandes
ps aux | grep bash | wc -l
```

---

# Exercice 5 : Manipulation de texte avancée 📝

**Objectif :** Utiliser sed, awk et autres outils

**Consignes :**

1. Créez un fichier `log.txt` avec des lignes de log simulées
2. Remplacez tous les "ERROR" par "ERREUR" avec sed
3. Affichez uniquement la 2ème colonne avec awk
4. Triez le fichier alphabétiquement
5. Supprimez les doublons

**Exemple de contenu :**

```
2025-11-24 INFO Démarrage
2025-11-24 ERROR Connexion échouée
2025-11-24 INFO Tentative 2
2025-11-24 ERROR Timeout
2025-11-24 INFO Succès
```

---

# Solution Exercice 5 💡

```bash
# 1. Créer le fichier
cat > formation/log.txt << EOF
2025-11-24 INFO Démarrage
2025-11-24 ERROR Connexion échouée
2025-11-24 INFO Tentative 2
2025-11-24 ERROR Timeout
2025-11-24 INFO Succès
EOF

# 2. Remplacer ERROR par ERREUR
sed 's/ERROR/ERREUR/g' formation/log.txt > formation/log_fr.txt

# 3. Afficher la 2ème colonne
awk '{print $2}' formation/log.txt

# 4. Trier
sort formation/log.txt

# 5. Supprimer les doublons
sort formation/log.txt | uniq
# ou directement
sort -u formation/log.txt
```

---

# Exercice bonus : Script d'information système 🖥️

**Objectif :** Créer un premier script utile

**Créez un script `sysinfo.sh` qui affiche :**
- Le nom d'utilisateur
- Le répertoire actuel
- La date et l'heure
- L'espace disque disponible
- Le nombre de processus en cours

---

# Solution Exercice bonus 💡

```bash
#!/bin/bash
# Script d'information système

echo "===== Informations Système ====="
echo
echo "Utilisateur : $(whoami)"
echo "Répertoire : $(pwd)"
echo "Date : $(date '+%Y-%m-%d %H:%M:%S')"
echo
echo "===== Espace Disque ====="
df -h /
echo
echo "===== Processus ====="
echo "Nombre de processus : $(ps aux | wc -l)"
echo
echo "===== Charge Système ====="
uptime
```

**Utilisation :**

```bash
chmod +x formation/scripts/sysinfo.sh
./formation/scripts/sysinfo.sh
```

---

# Conseils pour la pratique 💡

**Pour progresser :**

1. **Tapez les commandes vous-même**
   - Ne copiez-collez pas
   - La mémoire musculaire compte !

2. **Expérimentez**
   - Essayez des variantes
   - Voyez ce qui se passe en cas d'erreur

3. **Lisez les pages man**
   - `man ls`, `man grep`, etc.
   - Option `-h` ou `--help` aussi

4. **Utilisez tab pour l'autocomplétion**
   - Gagne du temps
   - Évite les erreurs

---

# Prochaine étape 🎯

**Module 2 : Commandes de base essentielles**

Maintenant que vous connaissez les concepts, nous allons voir :
- Les commandes essentielles de navigation
- La manipulation avancée de fichiers
- Les outils de recherche et filtrage

---

