---
layout: exercices
---

# Exercices - Supervision & Analyse Système 🎯

**Module 4 : Mise en pratique**

---

# Exercice 1 : Surveillance en temps réel 📊

**Objectif :** Utiliser top et htop

**Consignes :**

1. Lancez `top` et identifiez :
   - Le load average actuel
   - Les 3 processus qui consomment le plus de CPU
   - Les 3 processus qui consomment le plus de RAM

2. Installez et lancez `htop`
   - Triez par utilisation mémoire
   - Affichez l'arbre des processus (F5)
   - Recherchez le processus "sshd" (F3)

**Durée :** 10 minutes

---

# Exercice 1 : Solution 💡

```bash
# 1. Top
top
# Observer la ligne "load average: ..."
# Appuyer sur 'P' pour trier par CPU
# Appuyer sur 'M' pour trier par RAM
# Noter les 3 premiers processus

# 2. Htop
sudo apt install htop -y    # si pas installé
htop
# F6 puis sélectionner PERCENT_MEM
# F5 pour vue arbre
# F3 puis taper "sshd"
# F10 pour quitter
```

---

# Exercice 2 : Analyse I/O disque 💽

**Objectif :** Identifier l'activité disque

**Consignes :**

1. Installez `iotop`
2. Lancez-le et observez pendant 30 secondes
3. Créez un fichier de 100 Mo pour générer de l'I/O :
   ```bash
   dd if=/dev/zero of=/tmp/test.img bs=1M count=100
   ```
4. Observez iotop pendant l'opération
5. Supprimez le fichier test

**Question :** Quel processus a généré le plus d'écriture disque ?

**Durée :** 10 minutes

---

# Exercice 2 : Solution 💡

```bash
# 1. Installation
sudo apt install iotop -y

# 2. Lancement
sudo iotop
# Observer pendant 30s

# 3. Terminal 2 : générer I/O
dd if=/dev/zero of=/tmp/test.img bs=1M count=100

# 4. Observer iotop : le processus 'dd' apparaît
# avec DISK WRITE élevé

# 5. Nettoyage
rm /tmp/test.img
```

**Réponse :** Le processus `dd` génère le plus d'écriture

---

# Exercice 3 : Analyse logs avec journalctl 📜

**Objectif :** Filtrer et analyser les logs

**Consignes :**

1. Affichez les logs depuis le dernier boot
2. Affichez seulement les erreurs des dernières 24 heures
3. Suivez en temps réel les logs du service SSH
4. Affichez les 20 dernières lignes des logs système
5. Vérifiez la taille totale des logs journald

**Durée :** 15 minutes

---

# Exercice 3 : Solution 💡

```bash
# 1. Logs depuis dernier boot
journalctl -b

# 2. Erreurs des 24 dernières heures
journalctl -p err --since "24 hours ago"

# 3. Suivre SSH en temps réel
journalctl -u ssh -f
# ou journalctl -u sshd -f (selon distrib)
# Ctrl+C pour arrêter

# 4. 20 dernières lignes
journalctl -n 20

# 5. Taille des logs
journalctl --disk-usage
```

---

# Exercice 4 : Surveillance avec vmstat et iostat 📈

**Objectif :** Comprendre les statistiques système

**Consignes :**

1. Installez sysstat : `sudo apt install sysstat -y`
2. Lancez `vmstat 2 5` (5 itérations de 2 secondes)
3. Notez les valeurs moyennes de :
   - `r` (processus en attente)
   - `si` et `so` (swap)
   - `wa` (attente I/O)
4. Lancez `iostat -x 2 3`
5. Notez le `%util` de votre disque principal

**Durée :** 10 minutes

---

# Exercice 4 : Solution 💡

```bash
# 1. Installation
sudo apt install sysstat -y

# 2. vmstat
vmstat 2 5

# Exemple de sortie :
# r  b  swpd   free   si  so  wa
# 1  0     0  524288   0   0   2

# Interprétation :
# r=1 : OK (< nb CPUs)
# si=so=0 : pas de swap (bon)
# wa=2 : peu d'attente I/O (bon)

# 4. iostat
iostat -x 2 3

# Observer %util :
# < 50% = OK
# 50-80% = chargé
# > 80% = goulot d'étranglement
```

---

# Exercice 5 : Script de monitoring ⚙️

**Objectif :** Créer un script d'alerte simple

**Consignes :**

Créez un script `/usr/local/bin/check_disk.sh` qui :

1. Vérifie l'espace disque de `/`
2. Affiche une alerte si > 80%
3. Rend le script exécutable
4. Testez-le manuellement

**Bonus :** Ajoutez-le dans crontab pour exécution toutes les heures

**Durée :** 20 minutes

---

# Exercice 5 : Solution 💡

```bash
# 1. Créer le script
sudo nano /usr/local/bin/check_disk.sh
```

```bash
#!/bin/bash
# Script de surveillance disque

THRESHOLD=80
USAGE=$(df -h / | tail -1 | awk '{print $5}' | sed 's/%//')

if [ $USAGE -gt $THRESHOLD ]; then
    echo "ALERTE: Disque / à ${USAGE}%"
    # echo "ALERTE: Disque à ${USAGE}%" | mail -s "Alerte Disque" admin@example.com
else
    echo "OK: Disque / à ${USAGE}%"
fi
```

---

# Exercice 5 : Solution (suite) 💡

```bash
# 2. Rendre exécutable
sudo chmod +x /usr/local/bin/check_disk.sh

# 3. Tester
/usr/local/bin/check_disk.sh

# Bonus: Ajouter dans crontab
sudo crontab -e
# Ajouter la ligne :
0 * * * * /usr/local/bin/check_disk.sh >> /var/log/disk_check.log
```

---

# Exercice 6 : Analyse avec sar 📊

**Objectif :** Activer et utiliser sar

**Consignes :**

1. Vérifiez que sysstat est installé
2. Activez la collecte :
   - Éditez `/etc/default/sysstat`
   - Mettez `ENABLED="true"`
3. Redémarrez sysstat
4. Affichez les stats CPU actuelles : `sar`
5. Affichez les stats mémoire : `sar -r`

**Note :** sar n'aura des données historiques qu'après quelques heures

**Durée :** 10 minutes

---

# Exercice 6 : Solution 💡

```bash
# 1. Installation (si besoin)
sudo apt install sysstat -y

# 2. Activer collecte
sudo nano /etc/default/sysstat
# Changer ENABLED="false" en ENABLED="true"

# 3. Redémarrer
sudo systemctl enable sysstat
sudo systemctl start sysstat

# 4. Stats CPU
sar
# Si "Cannot open /var/log/sysstat/..." = normal,
# attendre 10 minutes que la collecte commence

# 5. Stats mémoire
sar -r

# 6. Stats réseau
sar -n DEV
```

---

# Exercice 7 : Diagnostic complet 🔍

**Objectif :** Diagnostiquer un système "lent" (simulation)

**Consignes :**

Imaginez qu'un utilisateur dit "le serveur est lent".  
Effectuez ce diagnostic dans l'ordre :

1. Vérifiez le load average (top ou uptime)
2. Vérifiez l'utilisation CPU (top)
3. Vérifiez la RAM et le swap (free -h)
4. Vérifiez l'espace disque (df -h)
5. Vérifiez l'activité I/O (sudo iotop pendant 10s)
6. Vérifiez les erreurs récentes (journalctl -p err --since "10 min ago")

Documentez vos observations.

**Durée :** 15 minutes

---

# Exercice 7 : Solution 💡

```bash
# 1. Load average
uptime
top
# load < nb_cpus = OK

# 2. CPU
top
# Processus > 80% CPU ?

# 3. RAM et swap
free -h
# Swap used > 0 = problème mémoire

# 4. Disque
df -h
# / > 85% = problème espace

# 5. I/O
sudo iotop -o
# Processus avec DISK READ/WRITE élevé ?

# 6. Erreurs
journalctl -p err --since "10 min ago"
# Erreurs système ?
```

---

# Exercice 7 : Grille de diagnostic 📋

**Créez votre checklist personnelle :**

```
DIAGNOSTIC SYSTÈME LENT
========================
Date: __________  Heure: __________

□ Load average: _____ (< nb_cpus ?)
□ CPU max utilisé par: __________ (___%)
□ RAM utilisée: _____% (< 80% ?)
□ Swap utilisé: _____MB (= 0 ?)
□ Disque / utilisé: _____% (< 85% ?)
□ I/O disque: Processus __________ (élevé ?)
□ Erreurs logs: Oui / Non
  └─> Si oui: ____________________

Conclusion: ______________________
```

---

# Projet fil rouge : Monitoring automatisé 🤖

**Objectif final :** Script de monitoring complet

**Fonctionnalités :**
- Vérifier CPU, RAM, disque
- Logger dans /var/log/monitoring.log
- Envoyer alerte si seuil dépassé
- Exécuter toutes les 5 minutes

**Étapes :**
1. Créer le script
2. Tester
3. Ajouter dans crontab
4. Surveiller les logs pendant 1 heure

**Voir exercice 5 comme base**

---

# Récapitulatif des exercices ✅

**Compétences acquises :**

✅ Utiliser top/htop pour surveillance temps réel

✅ Analyser l'I/O avec iotop

✅ Filtrer et analyser les logs avec journalctl

✅ Interpréter vmstat et iostat

✅ Créer des scripts de monitoring

✅ Activer et utiliser sar

✅ Diagnostiquer un système lent méthodiquement

---
layout: default
---

# Bravo ! 🎉

Vous maîtrisez maintenant les bases de la supervision système.

**Prochaine étape :** Module 5 - Supervision avancée avec Zabbix

