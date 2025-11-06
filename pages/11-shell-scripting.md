---
layout: intro
routeAlias: 'shell-scripting'
---

# Shell Scripting 📝

### Automatisation avec les scripts shell

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Automatisons avec les scripts <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Concepts de base 🔧

### Qu'est-ce qu'un script shell ?

**Définition :**
- Fichier texte contenant des commandes
- Exécuté par un interpréteur shell
- Automatise des tâches répétitives
- Peut être exécuté comme une commande

**Avantages :**
- Automatisation des tâches
- Réutilisabilité
- Documentation des procédures
- Gain de temps

**Types de scripts :**
- **Scripts de maintenance** : Nettoyage, sauvegarde
- **Scripts d'installation** : Configuration automatique
- **Scripts de monitoring** : Surveillance système
- **Scripts de déploiement** : Mise en production

---
layout: default
---

# Structure d'un script 📋

### Éléments essentiels

**Shebang (première ligne) :**
```bash
#!/bin/bash
# ou
#!/bin/sh
# ou
#!/usr/bin/env bash
```

**Commentaires :**
```bash
# Ceci est un commentaire sur une ligne

: '
Ceci est un commentaire
sur plusieurs lignes
'
```

**Structure de base :**
```bash
#!/bin/bash
# Nom du script : mon_script.sh
# Auteur : Votre nom
# Date : 2025-01-01
# Description : Ce que fait le script

# Variables
NOM="Mon Script"

# Fonctions
ma_fonction() {
    echo "Fonction appelée"
}

# Code principal
echo "Début du script"
ma_fonction
echo "Fin du script"
```

---
layout: default
---

# Variables et paramètres 📊

### Gestion des variables

**Déclaration de variables :**
```bash
# Variables simples
NOM="John"
AGE=25
PI=3.14159

# Variables d'environnement
export PATH=$PATH:/usr/local/bin
export EDITOR=vim

# Variables en lecture seule
readonly VERSION="1.0"
```

**Paramètres de ligne de commande :**
```bash
# $0 = nom du script
# $1, $2, $3... = arguments
# $# = nombre d'arguments
# $@ = tous les arguments
# $* = tous les arguments (une seule chaîne)

echo "Nom du script : $0"
echo "Premier argument : $1"
echo "Nombre d'arguments : $#"
echo "Tous les arguments : $@"
```

**Variables spéciales :**
```bash
$?          # Code de retour de la dernière commande
$$          # PID du script
$!          # PID du dernier processus en arrière-plan
$RANDOM     # Nombre aléatoire
```

---
layout: default
---

# Structures de contrôle 🔄

### Conditions et boucles

**Conditions if/else :**
```bash
if [ condition ]; then
    commandes
elif [ autre_condition ]; then
    commandes
else
    commandes
fi

# Exemple
if [ -f fichier.txt ]; then
    echo "Le fichier existe"
else
    echo "Le fichier n'existe pas"
fi
```

**Boucles for :**
```bash
# Boucle sur une liste
for i in 1 2 3 4 5; do
    echo "Nombre : $i"
done

# Boucle sur des fichiers
for fichier in *.txt; do
    echo "Traitement de $fichier"
done

# Boucle avec séquence
for i in {1..10}; do
    echo "Itération $i"
done
```

---
layout: default
---

# Structures de contrôle 🔄

### Boucles et conditions avancées

**Boucle while :**
```bash
# Boucle while
compteur=1
while [ $compteur -le 5 ]; do
    echo "Compteur : $compteur"
    compteur=$((compteur + 1))
done

# Boucle infinie avec break
while true; do
    echo "Appuyez sur Ctrl+C pour arrêter"
    sleep 1
done
```

**Boucle until :**
```bash
# Boucle until (jusqu'à ce que la condition soit vraie)
compteur=1
until [ $compteur -gt 5 ]; do
    echo "Compteur : $compteur"
    compteur=$((compteur + 1))
done
```

**Case statement :**
```bash
case $1 in
    "start")
        echo "Démarrage..."
        ;;
    "stop")
        echo "Arrêt..."
        ;;
    "restart")
        echo "Redémarrage..."
        ;;
    *)
        echo "Usage: $0 {start|stop|restart}"
        ;;
esac
```

---
layout: default
---

# Tests et comparaisons 🧪

### Opérateurs de test

**Tests sur les fichiers :**
```bash
[ -f fichier ]     # Fichier existe et est régulier
[ -d repertoire ]  # Répertoire existe
[ -r fichier ]     # Fichier lisible
[ -w fichier ]     # Fichier écrivable
[ -x fichier ]     # Fichier exécutable
[ -s fichier ]     # Fichier non vide
[ -e fichier ]     # Fichier existe
```

**Comparaisons de chaînes :**
```bash
[ "$a" = "$b" ]    # Égalité
[ "$a" != "$b" ]   # Différence
[ -z "$a" ]        # Chaîne vide
[ -n "$a" ]        # Chaîne non vide
[ "$a" < "$b" ]    # Ordre lexicographique
```

**Comparaisons numériques :**
```bash
[ $a -eq $b ]      # Égal
[ $a -ne $b ]      # Différent
[ $a -lt $b ]      # Inférieur
[ $a -le $b ]      # Inférieur ou égal
[ $a -gt $b ]      # Supérieur
[ $a -ge $b ]      # Supérieur ou égal
```

---
layout: default
---

# Fonctions 📦

### Création et utilisation de fonctions

**Définition de fonctions :**
```bash
# Fonction simple
ma_fonction() {
    echo "Fonction appelée"
}

# Fonction avec paramètres
salutation() {
    local nom=$1
    echo "Bonjour $nom"
}

# Fonction avec valeur de retour
addition() {
    local a=$1
    local b=$2
    local resultat=$((a + b))
    return $resultat
}
```

**Utilisation des fonctions :**
```bash
# Appeler une fonction
ma_fonction

# Appeler avec paramètres
salutation "John"

# Récupérer la valeur de retour
addition 5 3
resultat=$?
echo "Résultat : $resultat"
```

---
layout: default
---

# Gestion des erreurs ⚠️

### Traitement des erreurs

**Options de gestion d'erreur :**
```bash
#!/bin/bash
set -e          # Arrêter sur la première erreur
set -u          # Erreur si variable non définie
set -o pipefail # Erreur si une commande dans un pipe échoue
```

**Gestion des erreurs manuelle :**
```bash
# Vérifier le code de retour
commande
if [ $? -ne 0 ]; then
    echo "Erreur lors de l'exécution de la commande"
    exit 1
fi

# Ou plus court
commande || { echo "Erreur"; exit 1; }
```

**Trap pour nettoyage :**
```bash
# Fonction de nettoyage
cleanup() {
    echo "Nettoyage en cours..."
    rm -f fichier_temporaire
}

# Capturer les signaux
trap cleanup EXIT
trap 'echo "Interruption reçue"; exit 1' INT TERM
```

---
layout: default
---

# Entrées/Sorties 🔄

### Gestion des entrées et sorties

**Lecture d'entrée utilisateur :**
```bash
# Lecture simple
read nom
echo "Bonjour $nom"

# Lecture avec prompt
read -p "Entrez votre nom : " nom

# Lecture silencieuse (mot de passe)
read -s -p "Mot de passe : " password

# Lecture avec timeout
read -t 10 -p "Répondez vite : " reponse
```

**Redirection de sortie :**
```bash
# Redirection vers fichier
echo "Contenu" > fichier.txt
echo "Ajout" >> fichier.txt

# Redirection d'erreur
commande 2> erreurs.log

# Redirection de tout
commande &> tout.log

# Redirection d'entrée
commande < fichier.txt
```

---
layout: default
---

# Scripts pratiques 🛠️

### Exemples de scripts utiles

**Script de sauvegarde :**
```bash
#!/bin/bash
# Script de sauvegarde

# Variables
SOURCE="/home/user"
DESTINATION="/backup"
DATE=$(date +%Y%m%d_%H%M%S)
ARCHIVE="backup_$DATE.tar.gz"

# Vérifications
if [ ! -d "$SOURCE" ]; then
    echo "Erreur : Répertoire source inexistant"
    exit 1
fi

# Création de la sauvegarde
echo "Création de la sauvegarde..."
tar -czf "$DESTINATION/$ARCHIVE" -C "$SOURCE" .

if [ $? -eq 0 ]; then
    echo "Sauvegarde créée : $ARCHIVE"
else
    echo "Erreur lors de la sauvegarde"
    exit 1
fi
```

---
layout: default
---

# Scripts pratiques 🛠️

### Exemples de scripts utiles

**Script de monitoring système :**
```bash
#!/bin/bash
# Script de monitoring

# Variables
LOG_FILE="/var/log/system_monitor.log"
THRESHOLD=80

# Fonction de log
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> "$LOG_FILE"
}

# Vérification de l'espace disque
DISK_USAGE=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')
if [ "$DISK_USAGE" -gt "$THRESHOLD" ]; then
    log_message "ALERTE : Utilisation disque à ${DISK_USAGE}%"
fi

# Vérification de la mémoire
MEM_USAGE=$(free | awk 'NR==2{printf "%.0f", $3*100/$2}')
if [ "$MEM_USAGE" -gt "$THRESHOLD" ]; then
    log_message "ALERTE : Utilisation mémoire à ${MEM_USAGE}%"
fi
```

---
layout: default
---

# Scripts pratiques 🛠️

### Exemples de scripts utiles

**Script d'installation automatique :**
```bash
#!/bin/bash
# Script d'installation

# Couleurs pour les messages
RED='\033[0;31m'
GREEN='\033[0;32m'
NC='\033[0m' # No Color

# Fonction d'affichage
print_status() {
    if [ $1 -eq 0 ]; then
        echo -e "${GREEN}✓ $2${NC}"
    else
        echo -e "${RED}✗ $2${NC}"
        exit 1
    fi
}

# Mise à jour du système
echo "Mise à jour du système..."
sudo apt update && sudo apt upgrade -y
print_status $? "Mise à jour terminée"

# Installation des paquets
echo "Installation des paquets..."
sudo apt install -y htop vim git curl wget
print_status $? "Installation des paquets"

# Configuration
echo "Configuration..."
mkdir -p ~/projects
print_status $? "Configuration terminée"

echo "Installation terminée avec succès !"
```

---
layout: default
---

# Debugging et débogage 🐛

### Outils de débogage

**Options de debug :**
```bash
#!/bin/bash
set -x          # Afficher chaque commande exécutée
set -v          # Afficher les commandes avant exécution
set -n          # Mode syntax check (ne pas exécuter)

# Ou activer pour une section
set -x
# Code à déboguer
set +x
```

**Variables de debug :**
```bash
# Afficher les variables
echo "DEBUG: NOM=$NOM"
echo "DEBUG: AGE=$AGE"

# Utiliser printf pour plus de contrôle
printf "DEBUG: NOM='%s', AGE='%d'\n" "$NOM" "$AGE"
```

**Logging avancé :**
```bash
# Fonction de log avec niveau
log() {
    local level=$1
    shift
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] [$level] $*" >> /var/log/script.log
}

log "INFO" "Début du script"
log "ERROR" "Une erreur s'est produite"
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour écrire de bons scripts

**Structure et organisation :**
```bash
#!/bin/bash
# =============================================================================
# Nom du script : mon_script.sh
# Auteur : Votre nom
# Date : 2025-01-01
# Version : 1.0
# Description : Description détaillée du script
# =============================================================================

# Configuration
set -euo pipefail

# Variables globales
readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly LOG_FILE="/var/log/script.log"

# Fonctions
main() {
    echo "Début du script"
    # Code principal ici
    echo "Fin du script"
}

# Point d'entrée
main "$@"
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour écrire de bons scripts

**Sécurité :**
- Toujours valider les entrées utilisateur
- Utiliser des chemins absolus quand nécessaire
- Éviter l'utilisation de `eval`
- Vérifier les permissions des fichiers

**Lisibilité :**
- Utiliser des noms de variables descriptifs
- Commenter le code complexe
- Structurer le code logiquement
- Utiliser des fonctions pour la réutilisabilité

**Robustesse :**
- Gérer les erreurs
- Nettoyer les fichiers temporaires
- Utiliser des timeouts pour les commandes longues
- Tester les scripts avant production

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique

**Exercice 1 : Script de base**
```bash
#!/bin/bash
# Créer un script qui :
# 1. Affiche un message de bienvenue
# 2. Demande le nom de l'utilisateur
# 3. Affiche "Bonjour [nom]"
# 4. Affiche la date actuelle

echo "Bienvenue !"
read -p "Entrez votre nom : " nom
echo "Bonjour $nom"
echo "Date actuelle : $(date)"
```

**Exercice 2 : Script avec conditions**
```bash
#!/bin/bash
# Créer un script qui :
# 1. Vérifie si un fichier existe
# 2. Si oui, affiche sa taille
# 3. Si non, crée le fichier

fichier="test.txt"
if [ -f "$fichier" ]; then
    taille=$(wc -c < "$fichier")
    echo "Le fichier $fichier existe, taille : $taille bytes"
else
    echo "Création du fichier $fichier"
    touch "$fichier"
fi
```

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Virtualisation** et conteneurs
2. **Docker** introduction
3. **Ansible** introduction
4. **Projet final** intégrateur
5. **QCM** de validation

**Préparation :**
- Pratiquez l'écriture de scripts simples
- Familiarisez-vous avec les structures de contrôle
- Testez la gestion d'erreurs
- Créez vos propres scripts utilitaires

---
src: './pages/12-virtualisation.md'
--- 