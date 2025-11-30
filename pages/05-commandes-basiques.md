---
layout: intro
routeAlias: 'commandes-basiques'
---

<a name="commandes-basiques" id="commandes-basiques"></a>

# Commandes de base 🔧

### Les commandes essentielles d'Unix/Linux

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Maîtrisons les commandes de base <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Commandes de navigation 🧭

### Se déplacer dans le système

**pwd (Print Working Directory)**
```bash
pwd                        # Afficher le répertoire courant
pwd -P                     # Chemin physique (sans liens)
```

**cd (Change Directory)**
```bash
cd                         # Aller au répertoire home
cd /chemin/absolu          # Chemin absolu
cd chemin/relatif          # Chemin relatif
cd ..                      # Remonter d'un niveau
cd -                       # Retourner au répertoire précédent
cd ~                       # Aller au répertoire home
```

**ls (List)**
```bash
ls                         # Liste simple
ls -la                     # Liste détaillée avec fichiers cachés
ls -lh                     # Tailles humaines
ls -R                      # Récursif
ls -t                      # Trié par date
ls -S                      # Trié par taille
```

---
layout: default
---

# Commandes de manipulation 📁

### Créer, copier, déplacer, supprimer

**mkdir (Make Directory)**
```bash
mkdir nom_repertoire       # Créer un répertoire
mkdir -p chemin/complet    # Créer les parents si nécessaire
mkdir dir1 dir2 dir3       # Créer plusieurs répertoires
```

**cp (Copy)**
```bash
cp source destination      # Copier un fichier
cp -r source destination  # Copier un répertoire
cp -v source destination  # Verbose (afficher les opérations)
cp -i source destination  # Interactive (confirmation)
cp -p source destination  # Préserver les attributs
```

**mv (Move)**
```bash
mv ancien nouveau          # Renommer
mv fichier destination/    # Déplacer
mv -i fichier destination/ # Interactive
mv -v fichier destination/ # Verbose
```

---
layout: default
---

# Commandes de suppression 🗑️

### Supprimer fichiers et répertoires

**rm (Remove)**
```bash
rm fichier                 # Supprimer un fichier
rm -f fichier              # Forcer (sans confirmation)
rm -i fichier              # Interactive
rm -v fichier              # Verbose
rm fichier1 fichier2       # Plusieurs fichiers
```

**rmdir (Remove Directory)**
```bash
rmdir repertoire           # Supprimer un répertoire vide
rmdir -p chemin/complet   # Supprimer les parents vides
```

**rm avec options avancées**
```bash
rm -r repertoire          # Récursif (supprimer répertoire et contenu)
rm -rf repertoire         # Forcer récursif (dangereux !)
rm --preserve-root        # Protéger la racine
```

---
layout: default
---

# Commandes de visualisation 📄

### Afficher le contenu des fichiers

**cat (Concatenate)**
```bash
cat fichier                # Afficher le contenu complet
cat fichier1 fichier2      # Concaténer plusieurs fichiers
cat > nouveau_fichier      # Créer un fichier (Ctrl+D pour terminer)
cat >> fichier             # Ajouter à la fin
```

**less et more**
```bash
less fichier               # Navigation interactive
more fichier               # Navigation page par page
# Navigation dans less : espace (page suivante), b (page précédente), q (quitter)
```

**head et tail**
```bash
head fichier               # 10 premières lignes
head -n 20 fichier         # 20 premières lignes
tail fichier               # 10 dernières lignes
tail -n 20 fichier         # 20 dernières lignes
tail -f fichier            # Suivre en temps réel
```

---
layout: default
---

# Éditeurs de texte 📝

### Modifier des fichiers en ligne de commande

**nano (débutant-friendly)**
```bash
nano fichier.txt              # Ouvrir/créer un fichier
# Ctrl+O : Sauvegarder
# Ctrl+X : Quitter
# Ctrl+K : Couper une ligne
# Ctrl+U : Coller
```

**vim/vi (avancé)**
```bash
vim fichier.txt               # Ouvrir avec vim
# i : Mode insertion (pour écrire)
# Esc : Mode commande
# :w : Sauvegarder
# :q : Quitter
# :wq : Sauvegarder et quitter
# :q! : Quitter sans sauvegarder
```

**Édition rapide avec echo**
```bash
echo "contenu" > fichier.txt  # Écraser
echo "ajout" >> fichier.txt   # Ajouter
```

---
layout: default
---

# Commandes de recherche 🔍

### Trouver des informations

**grep (Global Regular Expression Print)**
```bash
grep "motif" fichier       # Rechercher un motif
grep -i "motif" fichier    # Insensible à la casse
grep -v "motif" fichier    # Inverser (lignes sans motif)
grep -r "motif" repertoire # Récursif
grep -n "motif" fichier    # Afficher les numéros de ligne
grep -l "motif" *          # Afficher seulement les noms de fichiers
```

**find**
```bash
find . -name "fichier.txt" # Rechercher par nom
find . -type f              # Seulement les fichiers
find . -type d              # Seulement les répertoires
find . -size +100M          # Plus de 100MB
find . -mtime -7            # Modifiés dans les 7 jours
find . -user utilisateur    # Appartenant à un utilisateur
```

---
layout: default
---

# Commandes de traitement de texte 📝

### Manipuler le texte

**echo**
```bash
echo "texte"               # Afficher du texte
echo -n "texte"            # Sans retour à la ligne
echo $VARIABLE             # Afficher une variable
echo -e "texte\nligne"     # Interpréter les caractères spéciaux
```

**sed (Stream Editor)**
```bash
sed 's/ancien/nouveau/g' fichier  # Remplacer
sed '1,5d' fichier                # Supprimer les lignes 1-5
sed 's/^/prefixe/' fichier        # Ajouter un préfixe
sed 's/$/suffixe/' fichier        # Ajouter un suffixe
```

**awk**
```bash
awk '{print $1}' fichier          # Afficher la première colonne
awk '{print $1, $3}' fichier      # Afficher les colonnes 1 et 3
awk '/motif/' fichier             # Lignes contenant un motif
awk '{sum += $1} END {print sum}' fichier  # Somme de la première colonne
```

---
layout: default
---

# Commandes de tri et filtrage 📊

### Organiser les données

**sort**
```bash
sort fichier               # Trier alphabétiquement
sort -n fichier            # Trier numériquement
sort -r fichier            # Trier en ordre inverse
sort -k2 fichier           # Trier par la 2ème colonne
sort -u fichier            # Supprimer les doublons
```

**uniq**
```bash
uniq fichier               # Supprimer les lignes consécutives identiques
uniq -c fichier            # Compter les occurrences
uniq -d fichier            # Afficher seulement les doublons
sort fichier | uniq        # Combinaison classique
```

**wc (Word Count)**
```bash
wc fichier                 # Lignes, mots, caractères
wc -l fichier              # Seulement les lignes
wc -w fichier              # Seulement les mots
wc -c fichier              # Seulement les caractères
```

---
layout: default
---

# Commandes de compression 📦

### Compresser et décompresser

**gzip/gunzip**
```bash
gzip fichier               # Compresser (supprime l'original)
gzip -k fichier            # Garder l'original
gunzip fichier.gz          # Décompresser
gzip -d fichier.gz         # Alternative pour décompresser
```

**tar (Tape Archive)**
```bash
tar -czf archive.tar.gz repertoire/  # Créer une archive compressée
tar -xzf archive.tar.gz              # Extraire
tar -cjf archive.tar.bz2 repertoire/ # Compresser avec bzip2
tar -xjf archive.tar.bz2             # Extraire bzip2
tar -tf archive.tar.gz               # Lister le contenu
```

**zip/unzip**
```bash
zip archive.zip fichier    # Créer une archive zip
zip -r archive.zip repertoire/  # Récursif
unzip archive.zip          # Extraire
unzip -l archive.zip       # Lister le contenu
```

---
layout: default
---

# Commandes de réseau 🌐

### Configuration et test réseau

**ping**
```bash
ping google.com            # Test de connectivité
ping -c 4 google.com       # 4 pings seulement
ping -i 2 google.com       # Intervalle de 2 secondes
ping -s 1000 google.com    # Taille de paquet de 1000 bytes
```

**netstat**
```bash
netstat -tuln              # Ports en écoute
netstat -i                  # Interfaces réseau
netstat -r                  # Table de routage
netstat -s                  # Statistiques
```

**ss (Socket Statistics)**
```bash
ss -tuln                   # Ports en écoute (moderne)
ss -i                       # Informations détaillées
ss -s                       # Statistiques
```

---
layout: default
---

# Commandes système ⚙️

### Informations sur le système

**uname**
```bash
uname                      # Nom du système
uname -a                   # Toutes les informations
uname -r                   # Version du noyau
uname -m                   # Architecture
```

**who et w**
```bash
who                        # Utilisateurs connectés
who am i                   # Mon utilisateur
w                          # Utilisateurs et leurs processus
```

**date et time**
```bash
date                       # Date et heure actuelles
date +%Y-%m-%d             # Format personnalisé
time commande              # Temps d'exécution d'une commande
```

---
layout: default
---

# Commandes de processus ⚡

### Gérer les processus

**ps (Process Status)**
```bash
ps                         # Mes processus
ps aux                      # Tous les processus
ps -ef                      # Format étendu
ps -p PID                   # Processus spécifique
ps -u utilisateur           # Processus d'un utilisateur
```

**top et htop**
```bash
top                        # Processus en temps réel
htop                       # Interface améliorée
# Navigation : q (quitter), k (tuer), r (renice), h (aide)
```

**kill et pkill**
```bash
kill PID                   # Terminer un processus
kill -9 PID                # Forcer la terminaison
killall nom_processus      # Tuer par nom
pkill nom_processus        # Alternative moderne
```

---
layout: default
---

# Commandes de monitoring 📊

### Surveiller le système

**df (Disk Free)**
```bash
df                         # Espace disque
df -h                       # Format humain
df -i                       # Utilisation des inodes
df -T                       # Type de système de fichiers
```

**du (Disk Usage)**
```bash
du                         # Utilisation du répertoire courant
du -h                       # Format humain
du -sh *                    # Taille de tous les éléments
du -d 1                     # Profondeur 1
```

**free**
```bash
free                        # Utilisation mémoire
free -h                     # Format humain
free -s 5                   # Actualisation toutes les 5 secondes
```

---
layout: default
---

# Commandes de redirection 🔄

### Contrôler les entrées/sorties

**Redirection de sortie**
```bash
commande > fichier          # Écraser
commande >> fichier         # Ajouter
commande 2> fichier         # Rediriger les erreurs
commande &> fichier         # Tout (sortie + erreurs)
```

**Redirection d'entrée**
```bash
commande < fichier          # Lire depuis un fichier
cat < fichier               # Afficher le contenu
```

**Pipes (tuyaux)**
```bash
commande1 | commande2       # Sortie de 1 vers entrée de 2
ls -la | grep "fichier"     # Filtrer la sortie
ps aux | grep "processus"   # Chercher un processus
cat fichier | sort | uniq   # Chaîner plusieurs commandes
```

---
layout: default
---

# Commandes de variables 🌍

### Gérer les variables d'environnement

**Définir des variables**
```bash
VARIABLE=valeur             # Variable locale
export VARIABLE=valeur      # Variable d'environnement
export PATH=$PATH:/nouveau/chemin  # Ajouter au PATH
```

**Variables importantes**
```bash
echo $HOME                  # Répertoire utilisateur
echo $USER                  # Nom utilisateur
echo $PWD                   # Répertoire courant
echo $SHELL                 # Shell utilisé
echo $PATH                  # Chemin de recherche
```

**Variables de position**
```bash
echo $1 $2 $3              # Arguments de ligne de commande
echo $#                     # Nombre d'arguments
echo $@                     # Tous les arguments
echo $?                     # Code de retour de la dernière commande
```

---
layout: default
---

# Commandes de scriptage 📝

### Automatisation de base

**Créer un script**
```bash
#!/bin/bash
# Mon premier script
echo "Hello World!"
echo "Date: $(date)"
echo "Utilisateur: $USER"
```

**Exécuter un script**
```bash
chmod +x script.sh         # Rendre exécutable
./script.sh                # Exécuter
bash script.sh             # Exécuter avec bash
source script.sh           # Exécuter dans le shell actuel
```

**Variables dans les scripts**
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

### Mise en pratique des commandes

**Exercice 1 : Navigation et manipulation**
```bash
# Créer une structure de test
mkdir -p test/{docs,src,backup}
cd test
echo "contenu1" > docs/fichier1.txt
echo "contenu2" > src/script.py
ls -la
tree
```

**Exercice 2 : Recherche et filtrage**
```bash
# Créer des fichiers de test
echo "ligne1" > test.txt
echo "ligne2" >> test.txt
echo "ligne3" >> test.txt
grep "ligne" test.txt
wc -l test.txt
sort test.txt
```

**Exercice 3 : Redirection et pipes**
```bash
# Utiliser les pipes
ls -la | grep "fichier"
ps aux | grep "bash"
cat test.txt | sort | uniq
```

---
layout: default
---

### Bonnes pratiques 💡

<div class="text-xs">

**Sécurité**
- Toujours vérifier avant de supprimer
- Utiliser `-i` pour les commandes destructives
- Faire des sauvegardes régulières
- Ne pas exécuter de commandes inconnues

**Productivité**
- Utiliser la complétion automatique (Tab)
- Créer des alias pour les commandes fréquentes
- Utiliser l'historique efficacement
- Combiner les commandes avec des pipes

**Organisation**
- Structurer les répertoires logiquement
- Utiliser des noms de fichiers descriptifs
- Documenter les scripts
- Maintenir un environnement propre

</div>

---
layout: default
---

# Prochaine étape 🎯

**Module 3 : Gestion des paquets et maintenance**

- Gestionnaires de paquets (apt, dnf, yum)
- Installation et mise à jour de logiciels
- Gestion des dépendances
- Nettoyage et maintenance du système
- Snapshots et rollbacks

---
src: './pages/06-gestion-utilisateurs.md'
--- 