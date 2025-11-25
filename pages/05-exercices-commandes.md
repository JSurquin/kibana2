---

# 🎯 Exercices pratiques - Module 2

**Maîtriser les commandes de base**

---

# Exercice 1 : Navigation 🗺️

**Objectif :** Se déplacer dans le système

**Consignes :**

1. Affichez votre répertoire actuel
2. Listez le contenu de `/usr/bin`
3. Allez dans `/tmp`
4. Revenez à votre répertoire personnel
5. Affichez le chemin complet de `/tmp`
6. Retournez au répertoire précédent avec `cd -`

---

# Solution Exercice 1 💡

```bash
# 1. Répertoire actuel
pwd

# 2. Contenu de /usr/bin
ls /usr/bin

# Avec détails
ls -l /usr/bin

# Seulement les 20 premiers
ls /usr/bin | head -20

# 3. Aller dans /tmp
cd /tmp

# 4. Retour au home
cd
# ou
cd ~

# 5. Chemin complet de /tmp
realpath /tmp
# ou
readlink -f /tmp

# 6. Retour au répertoire précédent
cd -
```

---

# Exercice 2 : Manipulation de fichiers 📁

**Objectif :** Créer, copier, déplacer, renommer

**Consignes :**

1. Créez un répertoire `test` dans votre home
2. Dans `test`, créez 5 fichiers : `file1.txt` à `file5.txt`
3. Copiez `file1.txt` en `file1_backup.txt`
4. Renommez `file2.txt` en `document.txt`
5. Déplacez `file3.txt` dans votre home
6. Créez un sous-répertoire `archive`
7. Déplacez tous les fichiers restants dans `archive`

---

# Solution Exercice 2 💡

```bash
# 1. Créer le répertoire
mkdir ~/test
cd ~/test

# 2. Créer 5 fichiers
touch file{1..5}.txt
# ou
touch file1.txt file2.txt file3.txt file4.txt file5.txt

# Vérifier
ls -l

# 3. Copier
cp file1.txt file1_backup.txt

# 4. Renommer
mv file2.txt document.txt

# 5. Déplacer
mv file3.txt ~/

# 6. Créer sous-répertoire
mkdir archive

# 7. Déplacer tous les fichiers
mv *.txt archive/

# Vérifier
ls -la
ls -la archive/
```

---

# Exercice 3 : Affichage et recherche 🔍

**Objectif :** Visualiser et chercher du contenu

**Consignes :**

1. Créez un fichier `nombres.txt` avec les nombres de 1 à 100
2. Affichez tout le contenu
3. Affichez seulement les 10 premières lignes
4. Affichez seulement les 10 dernières lignes
5. Cherchez le nombre "42"
6. Comptez le nombre de lignes du fichier

---

# Solution Exercice 3 💡

```bash
# 1. Créer le fichier avec nombres
seq 1 100 > nombres.txt

# Vérifier
ls -l nombres.txt

# 2. Afficher tout
cat nombres.txt

# 3. 10 premières lignes
head nombres.txt
# ou
head -n 10 nombres.txt

# 4. 10 dernières lignes
tail nombres.txt
# ou
tail -n 10 nombres.txt

# 5. Chercher 42
grep "42" nombres.txt
# Résultat: 42 (et aussi 142 si on va au-delà de 100)

# Pour match exact
grep "^42$" nombres.txt

# 6. Compter les lignes
wc -l nombres.txt
# ou
cat nombres.txt | wc -l
```

---

# Exercice 4 : Redirections et pipes 🔄

**Objectif :** Maîtriser les redirections

**Consignes :**

1. Listez `/etc` et sauvegardez dans `liste_etc.txt`
2. Ajoutez la date à la fin de ce fichier
3. Créez un fichier avec "Hello" puis ajoutez "World"
4. Comptez combien de fichiers `.conf` dans `/etc`
5. Trouvez les 5 plus gros fichiers de `/var/log`

---

# Solution Exercice 4 💡

```bash
# 1. Lister et sauvegarder
ls /etc > liste_etc.txt

# Vérifier
wc -l liste_etc.txt

# 2. Ajouter la date
date >> liste_etc.txt

# Vérifier
tail liste_etc.txt

# 3. Fichier avec contenu progressif
echo "Hello" > message.txt
echo "World" >> message.txt

# Vérifier
cat message.txt

# 4. Compter fichiers .conf
ls /etc/*.conf 2>/dev/null | wc -l
# ou
find /etc -name "*.conf" 2>/dev/null | wc -l

# 5. Plus gros fichiers de /var/log
sudo du -h /var/log/* 2>/dev/null | sort -hr | head -5
# ou
sudo find /var/log -type f -exec du -h {} + 2>/dev/null | \
    sort -hr | head -5
```

---

# Exercice 5 : Recherche avancée 🎯

**Objectif :** Utiliser find et grep efficacement

**Consignes :**

1. Trouvez tous les fichiers `.txt` dans votre home
2. Trouvez les fichiers modifiés dans les dernières 24h
3. Trouvez les fichiers de plus de 1 Mo
4. Créez un fichier `log.txt` avec différents niveaux (INFO, ERROR, WARN)
5. Extrayez seulement les lignes ERROR

---

# Solution Exercice 5 💡

```bash
# 1. Tous les .txt dans home
find ~ -name "*.txt" 2>/dev/null

# Avec détails
find ~ -name "*.txt" -ls 2>/dev/null

# 2. Modifiés dernières 24h
find ~ -mtime -1 2>/dev/null

# 3. Fichiers > 1 Mo
find ~ -size +1M 2>/dev/null

# Avec tailles
find ~ -size +1M -exec ls -lh {} \; 2>/dev/null

# 4. Créer fichier log
cat > log.txt << EOF
2025-11-24 10:00:00 INFO Application démarrée
2025-11-24 10:05:00 ERROR Connexion échouée
2025-11-24 10:10:00 WARN Mémoire basse
2025-11-24 10:15:00 INFO Traitement en cours
2025-11-24 10:20:00 ERROR Base de données inaccessible
2025-11-24 10:25:00 INFO Traitement terminé
EOF

# 5. Extraire les ERROR
grep ERROR log.txt

# Avec couleur
grep --color ERROR log.txt

# Compter les erreurs
grep -c ERROR log.txt

# Avec numéros de ligne
grep -n ERROR log.txt
```

---

# Exercice 6 : Combinaison de commandes 🔗

**Objectif :** Chaîner plusieurs commandes

**Consignes :**

1. Listez les processus, cherchez "bash", comptez-les
2. Affichez le contenu de `/etc/passwd`, triez-le, prenez les 10 premiers
3. Trouvez votre utilisation disque et triez par taille
4. Listez `/bin`, comptez combien commencent par "s"
5. Créez une ligne de commande qui affiche l'heure toutes les 5 secondes

---

# Solution Exercice 6 💡

```bash
# 1. Processus bash
ps aux | grep bash | wc -l

# 2. /etc/passwd trié
cat /etc/passwd | sort | head -10
# ou directement
sort /etc/passwd | head -10

# 3. Usage disque trié
du -h ~ | sort -hr | head -20

# 4. Commandes commençant par "s"
ls /bin | grep "^s" | wc -l

# 5. Affichage heure répété
while true; do clear; date; sleep 5; done
# Ctrl+C pour arrêter

# Alternative avec watch
watch -n 5 date
```

---

# Exercice bonus : Script de nettoyage 🧹

**Objectif :** Créer un script utile

**Créez `cleanup.sh` qui :**
- Trouve les fichiers temporaires (`.tmp`, `.bak`)
- Les liste
- Demande confirmation
- Les supprime
- Affiche un résumé

---

# Solution Exercice bonus 💡

```bash
#!/bin/bash
# Script de nettoyage

echo "===== Script de nettoyage ====="
echo

# Chercher fichiers temporaires
echo "Recherche de fichiers temporaires..."
TMP_FILES=$(find ~ -name "*.tmp" -o -name "*.bak" 2>/dev/null)

if [ -z "$TMP_FILES" ]; then
    echo "✅ Aucun fichier temporaire trouvé"
    exit 0
fi

# Lister les fichiers
echo "Fichiers trouvés :"
echo "$TMP_FILES"
echo

# Compter
COUNT=$(echo "$TMP_FILES" | wc -l)
echo "Total : $COUNT fichier(s)"

# Confirmation
read -p "Voulez-vous les supprimer ? (o/N) " -n 1 -r
echo

if [[ $REPLY =~ ^[Oo]$ ]]; then
    # Supprimer
    echo "$TMP_FILES" | while read file; do
        rm "$file"
        echo "  ✓ Supprimé: $file"
    done
    echo
    echo "✅ Nettoyage terminé : $COUNT fichier(s) supprimé(s)"
else
    echo "❌ Nettoyage annulé"
fi
```

**Utilisation :**

```bash
chmod +x cleanup.sh
./cleanup.sh
```

---

<small>

**Points clés à retenir 📌**

**Navigation :**
- `pwd`, `cd`, `ls`
- Chemins absolus vs relatifs
- `cd -` pour revenir

**Manipulation :**
- `mkdir`, `touch`
- `cp`, `mv`, `rm`
- Option `-r` pour récursif

**Visualisation :**
- `cat`, `less`, `more`
- `head`, `tail`
- `grep` pour chercher

**Redirections :**
- `>` écrase
- `>>` ajoute
- `|` pipe entre commandes

</small>
