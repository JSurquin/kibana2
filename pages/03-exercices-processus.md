---

# 🎯 Exercices pratiques - Module 6

**Gestion des processus et des services**

---

# Exercice 1 : Gestion de processus 🏃

**Objectif :** Manipuler les processus

**Consignes :**

1. Lancez la commande `sleep 300` en arrière-plan
2. Listez les jobs en cours
3. Trouvez le PID du processus `sleep`
4. Changez sa priorité (nice +10)
5. Tuez le processus proprement
6. Vérifiez qu'il est bien terminé

---

# Solution Exercice 1 💡

```bash
# 1. Lancer en arrière-plan
sleep 300 &

# 2. Lister les jobs
jobs
# [1]+  Running   sleep 300 &

# 3. Trouver le PID
pgrep sleep
# ou
ps aux | grep sleep
# ou avec jobs
jobs -l

# 4. Changer la priorité
PID=$(pgrep sleep)
sudo renice 10 -p $PID

# Vérifier
ps -o pid,ni,comm -p $PID

# 5. Tuer proprement (SIGTERM)
kill $PID

# 6. Vérifier
ps -p $PID
# → No such process
jobs
# → Terminated
```

---

# Exercice 2 : Monitoring des processus 📊

**Objectif :** Surveiller l'activité système

**Consignes :**

1. Affichez les 10 processus qui consomment le plus de CPU
2. Affichez les 10 processus qui consomment le plus de mémoire
3. Trouvez tous les processus appartenant à votre utilisateur
4. Comptez le nombre total de processus
5. Utilisez `top` pour surveiller en temps réel (puis quittez avec `q`)

---

# Solution Exercice 2 💡

```bash
# 1. Top 10 CPU
ps aux --sort=-%cpu | head -11

# 2. Top 10 Mémoire
ps aux --sort=-%mem | head -11

# 3. Processus de l'utilisateur
ps -u $USER
# ou
ps aux | grep ^$USER

# 4. Nombre total
ps aux | wc -l
# ou
ps -e | wc -l

# 5. Monitoring temps réel
top
# Dans top :
# - M : trier par mémoire
# - P : trier par CPU
# - u : filtrer par utilisateur
# - k : tuer un processus
# - q : quitter
```

---

# Exercice 3 : Services systemd 🎛️

**Objectif :** Gérer les services avec systemd

**Consignes :**

1. Listez tous les services actifs
2. Vérifiez l'état du service SSH
3. Redémarrez le service SSH (si installé)
4. Activez le démarrage automatique au boot
5. Listez les services qui ont échoué
6. Affichez les 50 dernières lignes de logs du service SSH

---

# Solution Exercice 3 💡

```bash
# 1. Services actifs
systemctl list-units --type=service --state=active

# 2. État de SSH
systemctl status sshd
# ou
systemctl status ssh

# 3. Redémarrer
sudo systemctl restart sshd

# 4. Activer au boot
sudo systemctl enable sshd
# Vérifier
systemctl is-enabled sshd

# 5. Services échoués
systemctl --failed

# 6. Logs SSH
journalctl -u sshd -n 50
# ou en temps réel
journalctl -u sshd -f
```

---

# Exercice 4 : Créer un service personnalisé 🛠️

**Objectif :** Créer et gérer son propre service

**Consignes :**

Créez un service qui :
1. Lance un script Python simple toutes les 10 secondes
2. Se relance automatiquement en cas de crash
3. Démarre automatiquement au boot
4. Enregistre des logs

**Script Python à créer : `/opt/hello_service.py`**

---

# Solution Exercice 4 - Script 💡

```bash
# 1. Créer le script
sudo mkdir -p /opt
sudo tee /opt/hello_service.py << 'EOF'
#!/usr/bin/env python3
import time
import datetime

while True:
    now = datetime.datetime.now()
    print(f"[{now}] Service en cours d'exécution...")
    time.sleep(10)
EOF

# Rendre exécutable
sudo chmod +x /opt/hello_service.py
```

---

# Solution Exercice 4 - Service 💡

```bash
# 2. Créer le service systemd
sudo tee /etc/systemd/system/hello.service << 'EOF'
[Unit]
Description=Hello Service
After=network.target

[Service]
Type=simple
User=nobody
ExecStart=/usr/bin/python3 /opt/hello_service.py
Restart=always
RestartSec=5s
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF

# 3. Recharger systemd
sudo systemctl daemon-reload

# 4. Démarrer le service
sudo systemctl start hello.service

# 5. Activer au boot
sudo systemctl enable hello.service

# 6. Vérifier
systemctl status hello.service
journalctl -u hello.service -f
```

---

# Exercice 5 : Analyse du démarrage 🚀

**Objectif :** Analyser les performances de démarrage

**Consignes :**

1. Affichez le temps total de démarrage
2. Listez les 10 services les plus lents à démarrer
3. Affichez la chaîne critique de démarrage
4. Générez un graphique du démarrage (SVG)
5. Désactivez un service inutile pour accélérer le boot

---

# Solution Exercice 5 💡

```bash
# 1. Temps total
systemd-analyze

# 2. Top 10 services lents
systemd-analyze blame | head -10

# 3. Chaîne critique
systemd-analyze critical-chain

# 4. Graphique
systemd-analyze plot > /tmp/boot.svg
# Ouvrir avec un navigateur
firefox /tmp/boot.svg &

# 5. Désactiver un service inutile
# Exemple : bluetooth si non utilisé
sudo systemctl disable bluetooth.service
# Voir la différence
systemd-analyze
```

---

# Exercice 6 : Gestion des logs 📜

**Objectif :** Maîtriser journalctl

**Consignes :**

1. Affichez les logs depuis le dernier boot
2. Affichez seulement les erreurs
3. Suivez les logs en temps réel
4. Affichez les logs d'hier
5. Exportez les logs dans un fichier
6. Nettoyez les logs de plus de 7 jours

---

# Solution Exercice 6 💡

```bash
# 1. Logs du dernier boot
journalctl -b

# 2. Seulement les erreurs
journalctl -p err
# ou toutes les priorités <= error
journalctl -p err -b

# 3. Temps réel
journalctl -f
# Ctrl+C pour arrêter

# 4. Logs d'hier
journalctl --since yesterday --until today

# 5. Exporter
journalctl > /tmp/all_logs.txt
# ou pour un service spécifique
journalctl -u nginx > /tmp/nginx_logs.txt

# 6. Nettoyer (> 7 jours)
sudo journalctl --vacuum-time=7d

# Voir l'espace utilisé
journalctl --disk-usage
```

---

# Exercice bonus : Script de monitoring 📊

**Objectif :** Créer un script de surveillance système

**Créez `monitor.sh` qui :**
- Affiche les 5 processus utilisant le plus de CPU
- Affiche les 5 processus utilisant le plus de mémoire
- Affiche la charge système (load average)
- Alerte si un service critique est arrêté
- Sauvegarde les infos dans un fichier log

---

# Solution Exercice bonus 💡

```bash
#!/bin/bash
# Script de monitoring système

LOG_FILE="/var/log/monitor.log"
CRITICAL_SERVICES=("sshd" "nginx" "mysql")

echo "===== Monitoring $(date) =====" | tee -a $LOG_FILE

# Top 5 CPU
echo -e "\n🔥 Top 5 CPU:" | tee -a $LOG_FILE
ps aux --sort=-%cpu | head -6 | tail -5 | tee -a $LOG_FILE

# Top 5 Mémoire  
echo -e "\n💾 Top 5 Mémoire:" | tee -a $LOG_FILE
ps aux --sort=-%mem | head -6 | tail -5 | tee -a $LOG_FILE

# Load average
echo -e "\n⚖️  Charge système:" | tee -a $LOG_FILE
uptime | tee -a $LOG_FILE

# Vérifier services critiques
echo -e "\n🔍 Services critiques:" | tee -a $LOG_FILE
for service in "${CRITICAL_SERVICES[@]}"; do
    if systemctl is-active --quiet $service; then
        echo "✅ $service: OK" | tee -a $LOG_FILE
    else
        echo "❌ $service: ARRÊTÉ !" | tee -a $LOG_FILE
        # Envoyer une alerte
        logger -t MONITOR "ALERTE: $service est arrêté"
    fi
done

echo "============================" | tee -a $LOG_FILE
```

**Automatiser avec cron :**

```bash
# Exécuter toutes les 5 minutes
*/5 * * * * /usr/local/bin/monitor.sh
```

---

# Points clés à retenir 📌

**Processus :**
- PID, PPID, états (R, S, D, Z, T)
- nice/renice pour la priorité
- kill avec signaux (TERM, KILL, HUP)

**systemd :**
- `start`, `stop`, `restart`, `reload`
- `enable`, `disable` pour le boot
- `journalctl` pour les logs

**Monitoring :**
- `ps`, `top`, `htop`
- `systemd-analyze` pour le boot
- `journalctl` pour l'historique

