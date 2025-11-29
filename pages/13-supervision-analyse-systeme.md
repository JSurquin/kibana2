---
layout: new-section
routeAlias: 'supervision-analyse'
---

<a name="supervision-analyse" id="supervision-analyse"></a>

# 📊 Module 4
## Supervision & Analyse Système

### Surveiller et analyser votre serveur Linux

---

# Pourquoi superviser un serveur ? 🤔

**Supervision** : surveiller l'état et les performances d'un système

**Analogie : le tableau de bord d'une voiture** 🚗
- **Vitesse** = charge CPU
- **Température moteur** = température système
- **Jauge essence** = espace disque
- **Voyants d'alerte** = logs d'erreurs

**Sans supervision :**
- Pannes inattendues
- Pas d'anticipation des problèmes
- Difficile à diagnostiquer
- Downtime prolongé

---

# Les 4 ressources clés à surveiller 🎯

**1. CPU** : charge processeur
- Trop élevé = système lent
- Objectif : < 70% en moyenne

**2. RAM** : mémoire vive
- Saturation = swap = lenteur
- Objectif : < 80%

**3. Disque** : espace de stockage
- Plein = plantages
- Objectif : < 85%

**4. Réseau** : bande passante
- Saturation = timeouts
- Objectif : surveiller les pics

---

# top : le basique incontournable 📊

```bash
top
```

**Première ligne : load average**
```
load average: 1.23, 0.87, 0.65
```
- 3 valeurs : 1 min, 5 min, 15 min
- Si > nombre de CPUs : surchargé

**Colonnes importantes :**
- `PID` : identifiant processus
- `USER` : propriétaire
- `%CPU` : utilisation CPU
- `%MEM` : utilisation mémoire
- `COMMAND` : nom du programme

---

# top : commandes utiles ⌨️

**Raccourcis interactifs :**

```
M    : trier par mémoire
P    : trier par CPU (défaut)
k    : tuer un processus (demande PID)
u    : filtrer par utilisateur
1    : afficher tous les CPU séparément
q    : quitter
```

**Exemple pratique :** trouver qui consomme le plus de RAM

```bash
top
# Appuyez sur M
# Les processus les plus gourmands en RAM sont en haut
```

---

# htop : top en mieux 🎨

```bash
# Installer
sudo apt install htop       # Debian/Ubuntu
sudo dnf install htop       # RHEL/Fedora

# Lancer
htop
```

**Avantages sur top :**
- Interface colorée et intuitive
- Barres graphiques CPU/RAM
- Navigation avec les flèches
- Arbre des processus (F5)
- Recherche (F3)
- Kill facile (F9)

---

# htop : navigation rapide 🎮

**Touches de fonction :**

```
F1  : Aide
F2  : Configuration
F3  : Rechercher un processus
F4  : Filtrer
F5  : Vue arbre
F6  : Trier par colonne
F9  : Kill processus
F10 : Quitter
```

**Touches directes :**
- `Space` : marquer un processus
- `u` : filtrer par utilisateur
- `t` : vue arbre
- `/` : recherche

---

# iotop : surveiller les I/O disque 💽

```bash
# Installer
sudo apt install iotop

# Lancer (root requis)
sudo iotop
```

**Colonnes clés :**
- `DISK READ` : lecture disque par seconde
- `DISK WRITE` : écriture disque par seconde
- `COMMAND` : processus responsable

**Cas d'usage :**
- Système lent : est-ce le disque ?
- Qui écrit énormément sur le disque ?
- Identifier un processus qui fait du I/O intensif

---

# iotop : options utiles 🔧

```bash
# Mode batch (pour script)
sudo iotop -b -n 3

# Seulement les processus actifs
sudo iotop -o

# Afficher les threads
sudo iotop -P

# Trier par écriture disque
# Appuyez sur 'a' dans iotop
```

**Exemple :** détecter une base de données qui écrit trop

---

# vmstat : statistiques mémoire 📈

```bash
# Vue instantanée
vmstat

# Mise à jour toutes les 2 secondes
vmstat 2

# 5 itérations toutes les 2 secondes
vmstat 2 5
```

**Colonnes importantes :**
- `r` : processus en attente CPU
- `b` : processus bloqués I/O
- `si` : swap in (Ko/s)
- `so` : swap out (Ko/s)
- `bi` : blocs lus disque
- `bo` : blocs écrits disque
- `us` : temps CPU user
- `sy` : temps CPU system
- `wa` : temps CPU attente I/O

---

# Interpréter vmstat 🔍

```bash
vmstat 2
```

```
r  b   swpd   free   buff  cache   si   so    bi    bo
2  0      0  524288  89600 1048576   0    0    10    50
```

**Indicateurs de problème :**
- `r > nombre CPUs` : CPU surchargé
- `b > 0` : beaucoup d'attente I/O disque
- `si/so > 0` : système utilise le swap (mauvais !)
- `wa > 20` : le CPU attend le disque

**Bon état :**
- `r ≈ nombre CPUs`
- `si/so = 0` (pas de swap)
- `wa < 10`

---

# iostat : I/O par périphérique 💾

```bash
# Installer
sudo apt install sysstat

# Statistiques I/O
iostat

# Mise à jour toutes les 2 secondes
iostat -x 2
```

**Colonnes clés (-x pour détails) :**
- `r/s` : lectures par seconde
- `w/s` : écritures par seconde
- `rkB/s` : Ko lus par seconde
- `wkB/s` : Ko écrits par seconde
- `%util` : taux d'utilisation disque

**Attention :** `%util > 80%` = goulot d'étranglement disque

---

# journalctl : les logs système 📜

**Logs systemd** : journalctl remplace /var/log/*

```bash
# Tous les logs
journalctl

# Logs depuis le dernier boot
journalctl -b

# Logs d'un service
journalctl -u nginx

# Suivre en temps réel
journalctl -f

# Suivre un service
journalctl -u nginx -f
```

---

# journalctl : filtres utiles 🔎

```bash
# Depuis 1 heure
journalctl --since "1 hour ago"

# Depuis aujourd'hui
journalctl --since today

# Période précise
journalctl --since "2025-11-01" --until "2025-11-02"

# Seulement les erreurs
journalctl -p err

# Seulement les erreurs et critiques
journalctl -p crit

# Les 50 dernières lignes
journalctl -n 50
```

---

# journalctl : export et nettoyage 🧹

```bash
# Export vers fichier
journalctl -u nginx > nginx.log

# Taille du journal
journalctl --disk-usage

# Nettoyer : garder 7 jours
sudo journalctl --vacuum-time=7d

# Nettoyer : garder 500 Mo
sudo journalctl --vacuum-size=500M

# Vérifier l'intégrité
journalctl --verify
```

---

# sar : System Activity Reporter 📊

**sar** : collecte et rapporte l'activité système

```bash
# Installer sysstat
sudo apt install sysstat

# Activer la collecte (éditer /etc/default/sysstat)
sudo nano /etc/default/sysstat
# ENABLED="true"

sudo systemctl enable sysstat
sudo systemctl start sysstat
```

**Collecte automatique** : toutes les 10 minutes par défaut

---

# sar : consulter les stats 📈

```bash
# Stats CPU du jour
sar

# Stats mémoire
sar -r

# Stats swap
sar -S

# Stats I/O
sar -b

# Stats réseau
sar -n DEV

# Stats d'hier
sar -f /var/log/sysstat/sa$(date -d yesterday +%d)
```

**Exemple :** voir l'utilisation CPU à 14h

```bash
sar -s 14:00:00 -e 15:00:00
```

---

# Logs traditionnels : /var/log/ 📂

**Fichiers logs importants :**

```bash
# Logs système généraux
tail -f /var/log/syslog         # Debian/Ubuntu
tail -f /var/log/messages       # RHEL/Fedora

# Authentification
tail -f /var/log/auth.log       # Debian/Ubuntu
tail -f /var/log/secure         # RHEL/Fedora

# Noyau
dmesg
tail -f /var/log/kern.log

# Services spécifiques
tail -f /var/log/nginx/access.log
tail -f /var/log/nginx/error.log
tail -f /var/log/mysql/error.log
```

---

# logrotate : rotation automatique 🔄

**logrotate** : compresse et archive les vieux logs

**Configuration :** `/etc/logrotate.conf` et `/etc/logrotate.d/*`

**Exemple : /etc/logrotate.d/nginx**

```
/var/log/nginx/*.log {
    daily
    missingok
    rotate 14
    compress
    delaycompress
    notifempty
    create 0640 www-data adm
    sharedscripts
    postrotate
        systemctl reload nginx > /dev/null
    endscript
}
```

---

# Surveiller l'espace disque 💾

```bash
# Espace disque global
df -h

# Utilisation par répertoire
du -sh /var/*
du -sh /home/*

# Top 10 des plus gros fichiers dans /var
sudo find /var -type f -exec du -h {} + | sort -rh | head -10

# Surveiller en temps réel
watch -n 5 df -h
```

**Alerte :** disque > 85% = risque de plantage

---

# Exemple de script monitoring simple 🤖

**Script : /usr/local/bin/monitor.sh**

```bash
#!/bin/bash

# Seuils
CPU_THRESHOLD=80
MEM_THRESHOLD=80

# Récupérer les valeurs
CPU=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d'%' -f1)
MEM=$(free | grep Mem | awk '{print ($3/$2) * 100}')

# Alertes
if (( $(echo "$CPU > $CPU_THRESHOLD" | bc -l) )); then
    echo "ALERTE: CPU à ${CPU}%" | mail -s "Alerte CPU" admin@example.com
fi

if (( $(echo "$MEM > $MEM_THRESHOLD" | bc -l) )); then
    echo "ALERTE: RAM à ${MEM}%" | mail -s "Alerte RAM" admin@example.com
fi
```

---

# Automatiser le monitoring avec cron ⏰

```bash
# Rendre le script exécutable
sudo chmod +x /usr/local/bin/monitor.sh

# Ajouter dans crontab
sudo crontab -e
```

```
# Vérifier toutes les 5 minutes
*/5 * * * * /usr/local/bin/monitor.sh
```

**Alternative moderne :** systemd timer

---

# Checklist diagnostic serveur lent 🐌

**1. CPU**
```bash
top
# Appuyez sur '1' pour voir tous les CPU
# Processus qui utilisent > 50% CPU ?
```

**2. RAM**
```bash
free -h
# Swap utilisé ? Si oui, problème mémoire
```

**3. Disque**
```bash
df -h          # Espace disque
sudo iotop     # I/O en cours
```

---

# Checklist diagnostic (suite) 🔍

**4. Réseau**
```bash
# Connexions actives
ss -tuln | wc -l

# Bande passante (si iftop installé)
sudo iftop
```

**5. Logs**
```bash
# Erreurs récentes
journalctl -p err --since "10 minutes ago"
dmesg | tail -20
```

---

# Outils complémentaires utiles 🛠️

**nmon** : monitoring complet interactif
```bash
sudo apt install nmon
nmon
# Appuyez sur 'c' pour CPU, 'm' pour mémoire, etc.
```

**glances** : monitoring moderne
```bash
sudo apt install glances
glances
```

**multitail** : suivre plusieurs logs
```bash
sudo apt install multitail
multitail /var/log/nginx/access.log /var/log/nginx/error.log
```

---

# Bonnes pratiques 📋

**1. Surveiller régulièrement**
- Consulter htop tous les jours
- Vérifier les logs d'erreurs

**2. Définir des seuils**
- CPU < 70%
- RAM < 80%
- Disque < 85%
- Load average < nb_cpus

**3. Automatiser**
- Scripts de monitoring
- Alertes email/SMS
- Collecte historique (sar)

---

# Bonnes pratiques (suite) ✅

**4. Garder l'historique**
- sar collecte automatique
- Logs rotatés mais conservés 14-30 jours
- Graphiques de tendance

**5. Documenter les incidents**
- Quand ?
- Symptômes ?
- Cause ?
- Solution ?

**6. Anticiper**
- Tendance d'utilisation disque
- Planifier l'upgrade avant saturation

---

# Récapitulatif du Module 4 ✅

**Ce que vous avez appris :**

✅ Outils de surveillance temps réel (top, htop, iotop)

✅ Statistiques système (vmstat, iostat)

✅ Logs système (journalctl, /var/log/*)

✅ Collecte historique (sar)

✅ Rotation logs (logrotate)

✅ Scripts monitoring custom

✅ Diagnostic de performance

✅ Bonnes pratiques supervision

---

# Prochaine étape 🎯

**Module 5 : Supervision avancée avec Zabbix**

- Installation Zabbix serveur et agent
- Configuration interface web
- Création hosts et alertes
- Notifications automatiques

---
layout: default
---

# Questions ? 🤔

N'hésitez pas à poser vos questions maintenant !

Le prochain module explorera la supervision avancée avec Zabbix.

