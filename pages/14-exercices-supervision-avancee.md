---
layout: exercices
---

# Exercices - Supervision Avancée Zabbix 🎯

**Module 5 : Mise en pratique**

---

# Exercice 1 : Installation Zabbix Server 🖥️

**Objectif :** Installer Zabbix serveur complet

**Prérequis :** Machine Ubuntu 24.04 (VM ou cloud)

**Consignes :**

1. Installer le dépôt Zabbix 7.0
2. Installer serveur + frontend + agent + Apache
3. Installer MariaDB
4. Créer la base de données zabbix
5. Importer le schéma initial
6. Configurer zabbix_server.conf
7. Démarrer tous les services
8. Accéder à l'interface web

**Durée :** 30 minutes

---

# Exercice 1 : Solution 💡

```bash
# 1-2. Dépôt et installation
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
sudo dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
sudo apt update
sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent mariadb-server -y

# 3-5. Base de données
sudo mysql -e "CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;"
sudo mysql -e "CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'MonMotDePasse';"
sudo mysql -e "GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';"
sudo mysql -e "SET GLOBAL log_bin_trust_function_creators = 1;"
sudo zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

# 6. Configuration
sudo nano /etc/zabbix/zabbix_server.conf
# DBPassword=MonMotDePasse

# 7. Démarrage
sudo mysql -e "SET GLOBAL log_bin_trust_function_creators = 0;"
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2

# 8. Interface
firefox http://localhost/zabbix
```

---

# Exercice 2 : Configuration initiale 🔧

**Objectif :** Configurer l'interface web

**Consignes :**

1. Accédez à http://ip-serveur/zabbix
2. Suivez l'assistant d'installation
3. Connectez-vous avec Admin / zabbix
4. Changez le mot de passe Admin
5. Changez la langue en Français
6. Vérifiez que le host "Zabbix server" est actif (vert)

**Durée :** 10 minutes

---

# Exercice 2 : Solution 💡

```bash
# 1. Navigateur
http://<IP_SERVEUR>/zabbix

# 2. Assistant
# - Welcome → Next
# - Prerequisites → Next
# - Database: MySQL, localhost, zabbix, zabbix, MotDePasse → Next
# - Server details → Next
# - Finish

# 3. Connexion
User: Admin
Password: zabbix

# 4. Changer mot de passe
Administration → Users → Admin → Change password
Nouveau mot de passe (2x) → Update

# 5. Langue
Icône utilisateur (coin haut droite) → Language: French → Update

# 6. Vérifier
Configuration → Hôtes
# Ligne "Zabbix server" → colonne Disponibilité = ZBX vert ✅
```

---

# Exercice 3 : Ajouter un agent 📡

**Objectif :** Installer et configurer un agent sur un second serveur

**Prérequis :** Un second serveur (VM ou cloud)

**Consignes :**

1. Sur le serveur à surveiller, installer zabbix-agent
2. Configurer zabbix_agentd.conf :
   - Server = IP du serveur Zabbix
   - Hostname = serveur-test
3. Redémarrer l'agent
4. Ouvrir le port 10050 dans le firewall
5. Vérifier que l'agent écoute

**Durée :** 15 minutes

---

# Exercice 3 : Solution 💡

```bash
# Sur le serveur à surveiller

# 1. Installation
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
sudo dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
sudo apt update
sudo apt install zabbix-agent -y

# 2. Configuration
sudo nano /etc/zabbix/zabbix_agentd.conf

# Modifier:
Server=192.168.1.10        # IP serveur Zabbix
ServerActive=192.168.1.10
Hostname=serveur-test

# 3. Redémarrage
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent

# 4. Firewall
sudo ufw allow 10050/tcp

# 5. Vérification
sudo systemctl status zabbix-agent
sudo ss -tlnp | grep 10050
```

---

# Exercice 4 : Ajouter le host dans Zabbix 🖥️

**Objectif :** Déclarer le nouveau serveur dans Zabbix

**Consignes :**

1. Dans l'interface Zabbix : Configuration → Hôtes → Créer un hôte
2. Nom : `serveur-test`
3. Groupes : `Linux servers`
4. Interface : Agent, IP du serveur, port 10050
5. Templates : ajouter `Linux by Zabbix agent`
6. Ajouter l'hôte
7. Vérifier que ZBX devient vert
8. Consulter les données collectées

**Durée :** 10 minutes

---

# Exercice 4 : Solution 💡

```
1. Configuration → Hôtes → Créer un hôte

2-3. Onglet Hôte:
   - Nom de l'hôte: serveur-test
   - Groupes: Linux servers

4. Interfaces → Ajouter:
   - Type: Agent
   - Adresse IP: 192.168.1.20 (IP du serveur test)
   - Port: 10050

5. Templates → Sélectionner:
   - Chercher "Linux by Zabbix agent"
   - Cliquer sur le nom
   - Ajouter

6. Ajouter (bouton en bas)

7. Attendre 30-60 secondes
   Configuration → Hôtes
   Colonne Disponibilité → ZBX doit être vert ✅

8. Supervision → Dernières données
   Filtrer par hôte: serveur-test
   Voir CPU, RAM, disque, etc.
```

---

# Exercice 5 : Créer une alerte CPU 🚨

**Objectif :** Alerte si CPU > 80% pendant 5 minutes

**Consignes :**

1. Créer un trigger sur serveur-test :
   - Nom : `CPU élevé sur {HOST.NAME}`
   - Gravité : Haute
   - Expression : CPU avg(5m) > 80
2. L'alerte apparaîtra dans Supervision → Problèmes

**Durée :** 10 minutes

---

# Exercice 5 : Solution 💡

```
1. Configuration → Hôtes → serveur-test
2. Déclencheurs → Créer un déclencheur

3. Remplir:
   Nom: CPU élevé sur {HOST.NAME}
   Gravité: Haute

4. Expression → Ajouter:
   - Item: CPU utilization
   - Function: avg
   - Last of (T): 5m
   - Result: > 80
   
5. Ajouter (l'expression)
6. Ajouter (le trigger)

Test:
# Sur serveur-test, stresser le CPU
sudo apt install stress -y
stress --cpu 4 --timeout 300s

# Dans Zabbix
Supervision → Problèmes
# L'alerte apparaît après 5 minutes
```

---

# Exercice 6 : Configuration email 📧

**Objectif :** Configurer l'envoi d'emails (simulation)

**Note :** Pour Gmail, créer un "mot de passe d'application"  
(Google Account → Security → 2-Step Verification → App passwords)

**Consignes :**

1. Administration → Types de média → Email
2. Configurer SMTP Gmail :
   - Serveur : smtp.gmail.com:587
   - STARTTLS
   - Identifiants
3. Tester l'envoi
4. Assigner l'email à l'utilisateur Admin
5. Créer une action "Alerte CPU"

**Durée :** 20 minutes

---

# Exercice 6 : Solution 💡

```
1. Administration → Types de média → Email

2. Configuration:
   Serveur SMTP: smtp.gmail.com
   Port: 587
   SMTP helo: gmail.com
   Courriel SMTP: votre-email@gmail.com
   Sécurité de connexion: STARTTLS
   Authentification: Nom d'utilisateur et mot de passe
   Nom d'utilisateur: votre-email@gmail.com
   Mot de passe: [mot de passe application]
   
3. Test → Envoyer à: votre-email@gmail.com → Test
   Vérifier réception

4. Administration → Utilisateurs → Admin → Média → Ajouter:
   Type: Email
   Envoyer à: votre-email@gmail.com
   Actif: 1-7, 00:00-24:00
   Gravité: toutes
   Ajouter → Mettre à jour

5. Configuration → Actions → Créer une action:
   Nom: Alerte CPU
   Conditions: Gravité >= Haute
   Operations: Envoyer à Admin via Email
   Ajouter
```

---

# Exercice 7 : Dashboard personnalisé 📊

**Objectif :** Créer un tableau de bord

**Consignes :**

1. Supervision → Tableaux de bord → Créer un tableau de bord
2. Nom : `Mon serveur test`
3. Ajouter widget Graph : CPU
4. Ajouter widget Graph : RAM
5. Ajouter widget Graph : Disque
6. Enregistrer

**Durée :** 15 minutes

---

# Exercice 7 : Solution 💡

```
1. Supervision → Tableaux de bord → Créer un tableau de bord

2. Nom: Mon serveur test

3. Ajouter un widget:
   Type: Graph
   Nom: CPU
   Hôte: serveur-test
   Item: CPU utilization
   Ajouter

4. Ajouter un widget:
   Type: Graph
   Nom: Mémoire
   Hôte: serveur-test
   Item: Memory utilization
   Ajouter

5. Ajouter un widget:
   Type: Graph
   Nom: Disque
   Hôte: serveur-test
   Item: Disk space usage /
   Ajouter

6. Appliquer
7. Réorganiser les widgets par drag & drop
8. Enregistrer

# Résultat: Vue d'ensemble graphique de votre serveur !
```

---

# Projet fil rouge : Monitoring complet 🎯

**Objectif :** Mettre en place un monitoring complet de production

**Scénario :** Vous gérez 3 serveurs :
- 1 serveur web (Nginx)
- 1 serveur base de données (MySQL)
- 1 serveur applicatif

**Tâches :**

1. Installer les agents sur les 3 serveurs
2. Les ajouter dans Zabbix avec templates appropriés
3. Créer des triggers pour :
   - CPU > 80%
   - RAM > 90%
   - Disque > 85%
4. Configurer les alertes email
5. Créer un dashboard global

**Durée :** 1-2 heures

---

# Checklist de validation ✅

**Votre installation Zabbix est opérationnelle si :**

□ Serveur Zabbix installé et actif  
□ Interface web accessible  
□ Au moins 1 agent installé et vert (ZBX)  
□ Données collectées visibles (Latest data)  
□ 1 trigger créé et fonctionnel  
□ Configuration email testée  
□ 1 dashboard personnalisé créé

**Si tous les points sont cochés : BRAVO ! 🎉**

---

# Récapitulatif des exercices ✅

**Compétences acquises :**

✅ Installation complète de Zabbix

✅ Configuration serveur et agents

✅ Ajout de hosts à surveiller

✅ Création de triggers d'alerte

✅ Configuration notifications email

✅ Création de dashboards

✅ Utilisation des templates

**Vous savez maintenant déployer un monitoring professionnel !**

---
layout: default
---

# Bravo ! 🎉

Vous maîtrisez maintenant Zabbix pour la supervision avancée.

**Prochaine étape (BONUS) :** Module 6 - Optimisation & Performances

