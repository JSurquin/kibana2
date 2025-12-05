---
layout: intro
routeAlias: 'processus-services'
---

# Processus et Services ⚡

### Gestion des processus et services système

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Gérons les processus ensemble <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Concepts de base 🔄

### Qu'est-ce qu'un processus ?

**Définition :**
- Programme en cours d'exécution
- Instance d'un programme en mémoire
- Possède un PID (Process ID) unique
- Consomme des ressources système

**Types de processus :**
- **Processus parents** : Créent d'autres processus
- **Processus enfants** : Créés par des processus parents
- **Processus orphelins** : Parents terminés
- **Processus zombies** : Terminés mais pas nettoyés

**États d'un processus :**
- **Running** : En cours d'exécution
- **Sleeping** : En attente
- **Stopped** : Arrêté
- **Zombie** : Terminé mais pas nettoyé

---
layout: default
---

# Commandes de base des processus 📊

### Visualiser et gérer les processus

**ps (Process Status)**
```bash
ps                           # Mes processus
ps aux                       # Tous les processus
ps -ef                       # Format étendu
ps -p PID                    # Processus spécifique
ps -u utilisateur            # Processus d'un utilisateur
ps -o pid,ppid,cmd           # Colonnes spécifiques
```

**top (interactif)**
```bash
top                          # Processus en temps réel
top -p PID                   # Processus spécifique
top -u utilisateur           # Processus d'un utilisateur
top -n 1                     # Une seule mise à jour
```

**htop (amélioré)**
```bash
htop                         # Interface améliorée
htop -p PID                  # Processus spécifique
htop -u utilisateur          # Processus d'un utilisateur
```

---
layout: default
---

# Gestion des processus ⚙️

### Contrôler les processus

**kill (terminer un processus)**
```bash
kill PID                     # Terminer proprement
kill -9 PID                  # Forcer la terminaison
kill -15 PID                 # Signal SIGTERM (défaut)
kill -1 PID                  # Signal SIGHUP (recharger)
kill -2 PID                  # Signal SIGINT (Ctrl+C)
```

**pkill et killall**
```bash
pkill nom_processus          # Tuer par nom
pkill -f "pattern"           # Tuer par pattern
killall nom_processus        # Tuer tous les processus par nom
pkill -u utilisateur         # Tuer tous les processus d'un utilisateur
```

**nice et renice**
```bash
nice -n 10 commande          # Exécuter avec priorité 10
renice 10 PID                # Changer la priorité d'un processus
renice -n 10 -p PID          # Alternative
```

---
layout: default
---

# Signaux système 📡

### Communication avec les processus

**Signaux courants :**
```bash
SIGHUP (1)   # Hangup - recharger la configuration
SIGINT (2)   # Interrupt - Ctrl+C
SIGQUIT (3)  # Quit - Ctrl+\
SIGKILL (9)  # Kill - terminaison forcée
SIGTERM (15) # Terminate - terminaison propre
SIGSTOP (19) # Stop - arrêter le processus
SIGCONT (18) # Continue - reprendre le processus
```

**Envoi de signaux :**
```bash
kill -SIGTERM PID            # Envoyer SIGTERM
kill -TERM PID               # Alternative
kill -1 PID                  # Envoyer SIGHUP
kill -9 PID                  # Envoyer SIGKILL
```

**Gestion des signaux dans les scripts :**
```bash
#!/bin/bash
trap 'echo "Signal reçu"; exit' SIGTERM SIGINT
while true; do
    echo "En cours..."
    sleep 1
done
```

---
layout: default
---

# Services systemd 🔧

### Gestion des services modernes

**Commandes systemctl :**
```bash
systemctl start service       # Démarrer un service
systemctl stop service        # Arrêter un service
systemctl restart service     # Redémarrer un service
systemctl reload service      # Recharger la configuration
systemctl status service      # Statut d'un service
systemctl enable service      # Activer au démarrage
systemctl disable service     # Désactiver au démarrage
```

**Informations sur les services :**
```bash
systemctl list-units --type=service  # Lister tous les services
systemctl list-units --failed         # Services échoués
systemctl show service                # Informations détaillées
systemctl cat service                 # Afficher le fichier de service
```

**Gestion des dépendances :**
```bash
systemctl list-dependencies service   # Dépendances d'un service
systemctl list-dependencies --reverse service  # Services dépendants
```

---
layout: default
---

# Fichiers de service systemd 📄

### Configuration des services

**Structure d'un fichier .service :**
```ini
[Unit]
Description=Mon Service
After=network.target
Requires=network.target

[Service]
Type=simple
User=service_user
Group=service_group
ExecStart=/chemin/vers/service
ExecReload=/bin/kill -HUP $MAINPID
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

**Types de services :**
```bash
Type=simple                  # Service simple
Type=forking                 # Service qui fork
Type=oneshot                 # Service qui se termine
Type=notify                  # Service avec notification
Type=dbus                    # Service D-Bus
```

**Emplacements des fichiers :**
```bash
/etc/systemd/system/         # Services système
/usr/lib/systemd/system/     # Services packages
~/.config/systemd/user/      # Services utilisateur
```

---
layout: default
---

# Monitoring des processus 📈

### Surveillance en temps réel

**Commandes de monitoring :**
```bash
# Utilisation CPU et mémoire
top -p PID                   # Processus spécifique
htop -p PID                  # Interface améliorée
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head  # Top mémoire

# Utilisation des ressources
iotop                        # Top I/O
iotop -p PID                 # I/O d'un processus
lsof -p PID                  # Fichiers ouverts par un processus
```

**Monitoring avancé :**
```bash
# Surveillance continue
watch -n 1 'ps aux | grep processus'
watch -n 1 'netstat -tuln | grep port'

# Logs de processus
journalctl -u service -f     # Suivre les logs d'un service
journalctl -p err            # Erreurs système
```

---
layout: default
---

# Gestion de la mémoire 💾

### Monitoring de la mémoire

**Commandes mémoire :**
```bash
free                         # Utilisation mémoire
free -h                      # Format humain
free -s 5                    # Actualisation toutes les 5 secondes
cat /proc/meminfo            # Informations détaillées
vmstat 1                     # Statistiques virtuelles
```

**Gestion du swap :**
```bash
swapon -s                    # Statut du swap
swapon /dev/sda2             # Activer un swap
swapoff /dev/sda2            # Désactiver un swap
mkswap /dev/sda2             # Formater pour swap
```

**Nettoyage mémoire :**
```bash
sync                         # Synchroniser les buffers
echo 3 > /proc/sys/vm/drop_caches  # Vider les caches
sysctl vm.drop_caches=3      # Alternative
```

---
layout: default
---

# Gestion du CPU 🔥

### Monitoring des performances CPU

**Commandes CPU :**
```bash
uptime                       # Charge système
cat /proc/loadavg            # Charge moyenne
cat /proc/cpuinfo            # Informations CPU
nproc                        # Nombre de processeurs
lscpu                        # Informations détaillées CPU
```

**Monitoring de la charge :**
```bash
# Charge système
cat /proc/loadavg
# Format : 1min 5min 15min processus_running processus_total dernier_pid

# Utilisation CPU par processus
ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head
top -p 1                     # Processus avec plus de CPU
```

**Limitation CPU :**
```bash
# Utiliser cpulimit
cpulimit -p PID -l 50        # Limiter à 50% CPU
cpulimit -e processus -l 30  # Limiter par nom
```

---
layout: default
---

# Gestion des jobs 📋

### Jobs en arrière-plan

**Commandes de jobs :**
```bash
command &                    # Exécuter en arrière-plan
jobs                         # Lister les jobs
fg %1                        # Ramener le job 1 au premier plan
bg %1                        # Remettre le job 1 en arrière-plan
kill %1                      # Tuer le job 1
```

**nohup et screen :**
```bash
nohup command &              # Continuer après déconnexion
screen -S session            # Créer une session screen
screen -r session            # Rejoindre une session
screen -ls                   # Lister les sessions
```

**tmux (terminal multiplexer) :**
```bash
tmux                         # Démarrer tmux
tmux new-session -s nom      # Nouvelle session nommée
tmux attach -t nom           # Rejoindre une session
tmux list-sessions           # Lister les sessions
```

---
layout: default
---

# Logs et debugging 🐛

### Surveillance des logs

**Logs système :**
```bash
journalctl                   # Logs systemd
journalctl -u service        # Logs d'un service
journalctl -f                # Suivre en temps réel
journalctl --since "1 hour ago"  # Logs de la dernière heure
journalctl -p err            # Seulement les erreurs
```

**Logs traditionnels :**
```bash
tail -f /var/log/syslog      # Suivre les logs système
tail -f /var/log/auth.log    # Suivre les logs d'authentification
grep "ERROR" /var/log/service.log  # Chercher les erreurs
```

**Debugging des processus :**
```bash
strace -p PID                # Tracer les appels système
ltrace -p PID                # Tracer les appels de bibliothèque
gdb -p PID                   # Débogueur GNU
```

---
layout: default
---

# Gestion des cgroups v2 📦

### Contrôle des groupes de processus (version moderne)

**Créer un cgroup manuellement (cgroups v2) :**

```bash
# Créer un cgroup
sudo mkdir /sys/fs/cgroup/mon_groupe

# Définir limite mémoire (500 Mo)
echo "500M" | sudo tee /sys/fs/cgroup/mon_groupe/memory.max

# Définir limite CPU (50% = 50000/100000 µs)
echo "50000 100000" | sudo tee /sys/fs/cgroup/mon_groupe/cpu.max

# Ajouter un processus (remplacer PID par le vrai PID)
echo PID | sudo tee /sys/fs/cgroup/mon_groupe/cgroup.procs
```

---

# Gestion des cgroups avec systemd 🔧

**Méthode recommandée : systemctl set-property**

```bash
# Limiter la mémoire d'un service (syntaxe moderne)
sudo systemctl set-property nginx.service MemoryMax=512M

# Limiter le CPU
sudo systemctl set-property nginx.service CPUQuota=50%

# Voir les limites actuelles
systemctl show nginx.service --property=MemoryMax,CPUQuota,TasksMax
```

**Lancer une commande avec limites (test rapide) :**

```bash
systemd-run --scope -p MemoryMax=256M -p CPUQuota=25% ./script.sh
```

**Outils de monitoring :**

```bash
systemd-cgtop                # Top des cgroups (temps réel)
systemd-cgls                 # Arborescence des cgroups
```

---
layout: default
---

# Automatisation des services 🤖

### Scripts de service

**Script de démarrage classique :**
```bash
#!/bin/bash
# /etc/init.d/mon_service

case "$1" in
    start)
        echo "Démarrage du service..."
        /chemin/vers/service &
        ;;
    stop)
        echo "Arrêt du service..."
        killall service
        ;;
    restart)
        $0 stop
        $0 start
        ;;
    *)
        echo "Usage: $0 {start|stop|restart}"
        exit 1
        ;;
esac
```

**Service systemd moderne :**
```ini
[Unit]
Description=Mon Service Automatisé
After=network.target

[Service]
Type=simple
ExecStart=/chemin/vers/service
ExecStop=/bin/kill -TERM $MAINPID
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

---
layout: default
---

# Monitoring avancé 🔍

### Outils de surveillance

**htop personnalisé :**
```bash
# Configuration ~/.config/htop/htoprc
# Colonnes personnalisées
# Couleurs par type de processus
# Alertes de mémoire/CPU
```

**Monitoring avec Prometheus :**
```bash
# Exporter les métriques système
node_exporter --collector.processes

# Métriques disponibles
# - process_cpu_seconds_total
# - process_memory_bytes
# - process_open_fds
```

**Grafana pour visualisation :**
```bash
# Dashboard système
# - Utilisation CPU par processus
# - Utilisation mémoire
# - Nombre de processus
# - Temps de réponse
```

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique

**Exercice 1 : Gestion des processus**
```bash
# Créer un processus en arrière-plan
sleep 100 &
jobs

# Changer la priorité
renice 10 $!

# Tuer le processus
kill %1
```

**Exercice 2 : Service systemd**
```bash
# Créer un service simple
sudo nano /etc/systemd/system/mon_service.service

# Activer et démarrer
sudo systemctl daemon-reload
sudo systemctl enable mon_service
sudo systemctl start mon_service
sudo systemctl status mon_service
```

**Exercice 3 : Monitoring**
```bash
# Surveiller un processus spécifique
watch -n 1 'ps aux | grep processus'

# Analyser l'utilisation mémoire
ps -eo pid,ppid,cmd,%mem --sort=-%mem | head -10
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour la gestion des processus

**Performance**
- Surveiller régulièrement les processus
- Identifier les processus gourmands
- Optimiser les services critiques
- Utiliser les cgroups pour limiter les ressources

**Sécurité**
- Limiter les privilèges des services
- Surveiller les processus suspects
- Utiliser des utilisateurs dédiés pour les services
- Logging des actions importantes

**Maintenance**
- Nettoyer les processus zombies
- Redémarrer les services problématiques
- Mettre à jour les configurations
- Documenter les services personnalisés

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Réseaux et connectivité**
2. **Paquets logiciels** et gestion
3. **Shell scripting** de base
4. **Virtualisation** et conteneurs
5. **Docker** introduction

**Préparation :**
- Familiarisez-vous avec systemctl
- Pratiquez la gestion des processus
- Testez les commandes de monitoring
- Créez vos premiers services

---
src: './pages/09-reseaux.md'
--- 