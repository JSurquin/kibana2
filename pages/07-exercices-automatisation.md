---

# 🎯 Exercices pratiques - Module 8

**Automatisation, scripts et tâches planifiées**

---

# Exercice 1 : Premier script Bash 📝

**Objectif :** Créer un script fonctionnel

**Consignes :**

Créez un script `sysinfo.sh` qui affiche :
1. Le nom d'utilisateur
2. Le hostname
3. La date et l'heure
4. Le répertoire actuel
5. L'espace disque disponible
6. Les 5 processus utilisant le plus de mémoire

---

# Solution Exercice 1 💡

```bash
#!/bin/bash
# Script d'informations système

echo "======================================"
echo "  Informations Système"
echo "======================================"
echo

echo "Utilisateur : $(whoami)"
echo "Hostname : $(hostname)"
echo "Date : $(date '+%Y-%m-%d %H:%M:%S')"
echo "Répertoire : $(pwd)"
echo

echo "======================================"
echo "  Espace Disque"
echo "======================================"
df -h / | tail -1
echo

echo "======================================"
echo "  Top 5 Processus (Mémoire)"
echo "======================================"
ps aux --sort=-%mem | head -6 | tail -5
echo

echo "======================================"
```

**Utilisation :**

```bash
chmod +x sysinfo.sh
./sysinfo.sh
```

---

# Exercice 2 : Script avec arguments 🎯

**Objectif :** Gérer les arguments

**Créez `backup.sh` qui :**
- Prend 2 arguments : source et destination
- Vérifie que les arguments sont fournis
- Vérifie que la source existe
- Crée une archive tar.gz avec timestamp
- Affiche un message de succès/échec

---

# Solution Exercice 2 💡

```bash
#!/bin/bash
# Script de sauvegarde

# Vérifier les arguments
if [ $# -ne 2 ]; then
    echo "Usage: $0 <source> <destination>"
    echo "Exemple: $0 /home/user/documents /backup"
    exit 1
fi

SOURCE="$1"
DEST="$2"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
ARCHIVE="backup_${TIMESTAMP}.tar.gz"

# Vérifier que la source existe
if [ ! -d "$SOURCE" ]; then
    echo "❌ Erreur: $SOURCE n'existe pas"
    exit 1
fi

# Créer le répertoire destination si nécessaire
mkdir -p "$DEST"

# Créer l'archive
echo "Sauvegarde de $SOURCE vers $DEST/$ARCHIVE..."
tar -czf "$DEST/$ARCHIVE" "$SOURCE" 2>/dev/null

if [ $? -eq 0 ]; then
    SIZE=$(du -h "$DEST/$ARCHIVE" | cut -f1)
    echo "✅ Sauvegarde réussie ! Taille: $SIZE"
    ls -lh "$DEST/$ARCHIVE"
else
    echo "❌ Erreur lors de la sauvegarde"
    exit 1
fi
```

---

# Exercice 3 : Boucles et conditions 🔄

**Objectif :** Utiliser les structures de contrôle

**Créez un script qui :**
1. Boucle sur tous les fichiers .log dans /var/log
2. Affiche le nom et la taille de chaque fichier
3. Archive les fichiers > 10 Mo
4. Supprime les archives > 30 jours

---

# Solution Exercice 3 💡

```bash
#!/bin/bash
# Gestion des logs

LOG_DIR="/var/log"
ARCHIVE_DIR="/backup/logs"
MAX_SIZE=10485760  # 10 Mo en octets

mkdir -p "$ARCHIVE_DIR"

echo "Analyse des fichiers logs..."

# Boucle sur les fichiers .log
for logfile in $LOG_DIR/*.log; do
    # Vérifier que le fichier existe
    [ -f "$logfile" ] || continue
    
    filename=$(basename "$logfile")
    size=$(stat -f%z "$logfile" 2>/dev/null || stat -c%s "$logfile")
    size_mb=$(echo "scale=2; $size / 1048576" | bc)
    
    echo "📄 $filename : ${size_mb}Mo"
    
    # Si > 10 Mo, archiver
    if [ $size -gt $MAX_SIZE ]; then
        echo "  ⚠️  Fichier volumineux, archivage..."
        timestamp=$(date +%Y%m%d)
        gzip -c "$logfile" > "$ARCHIVE_DIR/${filename}_${timestamp}.gz"
        
        if [ $? -eq 0 ]; then
            echo "  ✅ Archivé"
            # Vider le fichier (ne pas supprimer)
            > "$logfile"
        fi
    fi
done

# Supprimer archives > 30 jours
echo
echo "Nettoyage des anciennes archives..."
find "$ARCHIVE_DIR" -name "*.gz" -mtime +30 -delete
echo "✅ Terminé"
```

---

# Exercice 4 : Fonctions Bash 🎭

**Objectif :** Créer des fonctions réutilisables

**Créez un script avec ces fonctions :**
- `log_info()` : affiche un message info
- `log_error()` : affiche un message d'erreur
- `check_root()` : vérifie si on est root
- `check_command()` : vérifie si une commande existe

---

# Solution Exercice 4 💡

```bash
#!/bin/bash
# Script avec fonctions

# Fonction de log info
log_info() {
    echo "[$(date '+%H:%M:%S')] ℹ️  INFO: $1"
}

# Fonction de log erreur
log_error() {
    echo "[$(date '+%H:%M:%S')] ❌ ERROR: $1" >&2
}

# Vérifier si root
check_root() {
    if [ "$EUID" -ne 0 ]; then
        log_error "Ce script doit être exécuté en root"
        exit 1
    fi
}

# Vérifier si une commande existe
check_command() {
    if ! command -v $1 &> /dev/null; then
        log_error "Commande '$1' non trouvée"
        return 1
    fi
    log_info "Commande '$1' trouvée"
    return 0
}

# Exemple d'utilisation
log_info "Démarrage du script"

check_command "git"
check_command "docker"
check_command "kubectl"  # Va échouer si non installé

log_info "Script terminé"
```

---

# Exercice 5 : Tâches planifiées avec cron ⏰

**Objectif :** Automatiser avec cron

**Consignes :**

1. Créez un script qui nettoie `/tmp`
2. Planifiez-le tous les jours à 2h du matin
3. Créez une tâche qui sauvegarde `/etc` tous les lundis à 3h
4. Listez vos tâches cron
5. Redirigez les sorties vers un fichier log

---

# Solution Exercice 5 💡

```bash
# 1. Script de nettoyage
cat > /usr/local/bin/clean_tmp.sh << 'EOF'
#!/bin/bash
find /tmp -type f -mtime +7 -delete
find /tmp -type d -empty -delete
echo "[$(date)] Nettoyage /tmp effectué" >> /var/log/clean_tmp.log
EOF

chmod +x /usr/local/bin/clean_tmp.sh

# 2. Éditer crontab
crontab -e

# Ajouter:
# Nettoyage /tmp tous les jours à 2h
0 2 * * * /usr/local/bin/clean_tmp.sh

# 3. Sauvegarde /etc tous les lundis à 3h
0 3 * * 1 tar -czf /backup/etc_$(date +\%Y\%m\%d).tar.gz /etc

# 4. Lister les tâches
crontab -l

# 5. Avec redirection
0 2 * * * /usr/local/bin/clean_tmp.sh >> /var/log/cron_clean.log 2>&1
0 3 * * 1 tar -czf /backup/etc_$(date +\%Y\%m\%d).tar.gz /etc >> /var/log/backup.log 2>&1

# Exemples de syntaxe cron:
# */5 * * * *     # Toutes les 5 minutes
# 0 */2 * * *     # Toutes les 2 heures
# 0 9-17 * * 1-5  # 9h-17h, lundi-vendredi
# @reboot         # Au démarrage
# @daily          # Quotidien (minuit)
# @weekly         # Hebdomadaire
```

---

# Exercice 6 : Gestion des logs avec journalctl 📜

**Objectif :** Filtrer et analyser les logs

**Consignes :**

1. Créez un script qui analyse les logs systemd
2. Extrait les erreurs des 24 dernières heures
3. Compte les erreurs par service
4. Envoie un rapport
5. Exécutez-le quotidiennement

---

# Solution Exercice 6 💡

```bash
#!/bin/bash
# Analyse des logs système

REPORT="/tmp/log_report_$(date +%Y%m%d).txt"

echo "===== Rapport d'analyse des logs =====" > $REPORT
echo "Date: $(date)" >> $REPORT
echo >> $REPORT

# 1 & 2. Erreurs des 24h
echo "📛 Erreurs des dernières 24h:" >> $REPORT
journalctl --since "24 hours ago" -p err --no-pager >> $REPORT
echo >> $REPORT

# 3. Compter par service
echo "📊 Erreurs par service:" >> $REPORT
journalctl --since "24 hours ago" -p err --no-pager | \
    grep -oP '(?<=\w{3} \d+ \d+:\d+:\d+ \w+ )\w+' | \
    sort | uniq -c | sort -rn | head -10 >> $REPORT
echo >> $REPORT

# Warnings aussi
echo "⚠️  Warnings des dernières 24h:" >> $REPORT
journalctl --since "24 hours ago" -p warning --no-pager | wc -l >> $REPORT
echo >> $REPORT

# Services failed
echo "❌ Services en échec:" >> $REPORT
systemctl --failed >> $REPORT 2>&1
echo >> $REPORT

echo "===== Fin du rapport =====" >> $REPORT

# Afficher le rapport
cat $REPORT

# Envoyer par email (optionnel)
# mail -s "Rapport Logs $(hostname)" admin@example.com < $REPORT
```

**Planifier quotidiennement à 7h :**

```bash
0 7 * * * /usr/local/bin/log_analysis.sh
```

---

# Exercice bonus : Dashboard système interactif 📊

**Objectif :** Script de monitoring avancé

**Créez un script qui affiche en continu :**
- Charge système
- Top processus
- Connexions réseau
- Espace disque
- Se rafraîchit automatiquement

---

# Solution Exercice bonus 💡

```bash
#!/bin/bash
# Dashboard système interactif

while true; do
    clear
    
    echo "╔════════════════════════════════════════════╗"
    echo "║      DASHBOARD SYSTÈME - $(date +%H:%M:%S)      ║"
    echo "╚════════════════════════════════════════════╝"
    echo
    
    # Charge système
    echo "📊 CHARGE SYSTÈME"
    uptime
    echo
    
    # CPU et Mémoire
    echo "💻 UTILISATION"
    printf "CPU: "
    top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d'%' -f1
    printf "MEM: "
    free | grep Mem | awk '{printf "%.1f%%\n", $3/$2 * 100.0}'
    echo
    
    # Top 5 processus
    echo "🔥 TOP 5 PROCESSUS (CPU)"
    ps aux --sort=-%cpu | head -6 | tail -5 | \
        awk '{printf "%-10s %5s%%  %s\n", $1, $3, $11}'
    echo
    
    # Espace disque
    echo "💾 ESPACE DISQUE"
    df -h / | tail -1 | \
        awk '{printf "Utilisé: %s / %s (%s)\n", $3, $2, $5}'
    echo
    
    # Connexions réseau
    echo "🌐 CONNEXIONS"
    echo "Actives: $(ss -tan | grep ESTAB | wc -l)"
    echo "En écoute: $(ss -tln | grep LISTEN | wc -l)"
    echo
    
    # Services
    echo "⚙️  SERVICES CRITIQUES"
    for svc in sshd nginx mysql; do
        if systemctl is-active --quiet $svc 2>/dev/null; then
            echo "✅ $svc"
        else
            echo "❌ $svc"
        fi
    done
    
    echo
    echo "Rafraîchissement dans 5s... (Ctrl+C pour quitter)"
    sleep 5
done
```

---

# Points clés à retenir 📌

**Scripts :**
- Shebang `#!/bin/bash`
- Arguments : `$1`, `$2`, `$@`, `$#`
- Conditions : `if`, `test`, `[ ]`
- Boucles : `for`, `while`, `until`

**Cron :**
- Format : minute heure jour mois jour_semaine
- `crontab -e` pour éditer
- Redirection importante pour les logs

**Logs :**
- `journalctl` pour systemd
- Filtres : `-p`, `-u`, `--since`
- `logger` pour écrire dans syslog

---

