---
layout: question
---

# QCM - Supervision Avancée avec Zabbix ✅

**Module 5 : Validation des connaissances**

---

# Question 1 🏗️

**Quelle est l'architecture de Zabbix ?**

A) Un agent autonome sur chaque serveur  
B) Serveur centralisé + agents sur les machines surveillées  
C) Uniquement une interface web  
D) Un script cron qui collecte les données

<v-click>

**Réponse : B**

Zabbix utilise une architecture client/serveur : un serveur central collecte les données depuis des agents installés sur chaque machine.

</v-click>

---

# Question 2 🔌

**Sur quel port l'agent Zabbix écoute-t-il par défaut ?**

A) 10050  
B) 10051  
C) 3306  
D) 8080

<v-click>

**Réponse : A**

L'agent Zabbix écoute sur le port 10050. Le serveur Zabbix écoute sur 10051.

</v-click>

---

# Question 3 🗄️

**Quelle base de données peut être utilisée avec Zabbix ?**

A) Seulement MySQL  
B) Seulement PostgreSQL  
C) MySQL/MariaDB ou PostgreSQL  
D) SQLite uniquement

<v-click>

**Réponse : C**

Zabbix supporte MySQL/MariaDB et PostgreSQL. PostgreSQL est souvent recommandé pour les grandes installations.

</v-click>

---

# Question 4 🔑

**Quels sont les identifiants par défaut de Zabbix ?**

A) admin / admin  
B) Admin / zabbix  
C) root / password  
D) zabbix / zabbix

<v-click>

**Réponse : B**

User : `Admin` (avec majuscule) / Password : `zabbix`  
⚠️ À changer immédiatement après installation !

</v-click>

---

# Question 5 🚨

**Qu'est-ce qu'un "trigger" dans Zabbix ?**

A) Un bouton pour démarrer la surveillance  
B) Une règle qui déclenche une alerte selon des conditions  
C) Un type de graphique  
D) Un service système

<v-click>

**Réponse : B**

Un trigger est une condition logique (ex: CPU > 80%) qui, lorsqu'elle est vraie, génère une alerte.

</v-click>

---

# Question 6 📦

**Qu'est-ce qu'un template dans Zabbix ?**

A) Un modèle d'email  
B) Un thème graphique  
C) Un ensemble pré-configuré d'items, triggers et graphiques  
D) Un fichier de configuration

<v-click>

**Réponse : C**

Un template est un ensemble réutilisable de configuration (items, triggers, graphs) qu'on peut appliquer à plusieurs hosts.

</v-click>

---

# Question 7 ⚙️

**Dans le fichier zabbix_agentd.conf, à quoi sert le paramètre "Server" ?**

A) Le nom du serveur à surveiller  
B) L'IP du serveur Zabbix autorisé à se connecter  
C) Le type de serveur (web, db, etc.)  
D) Le port du serveur

<v-click>

**Réponse : B**

`Server=` indique l'adresse IP du serveur Zabbix autorisé à interroger cet agent (sécurité).

</v-click>

---

# Question 8 🟢

**Dans Zabbix, que signifie "ZBX" vert dans la colonne Availability ?**

A) L'agent est en erreur  
B) L'agent est accessible et répond  
C) L'agent est en maintenance  
D) L'agent est désactivé

<v-click>

**Réponse : B**

ZBX vert = l'agent Zabbix est accessible et répond correctement aux requêtes du serveur.

</v-click>

---

# Question 9 📊

**Où voir les dernières données collectées pour un host ?**

A) Configuration → Hosts  
B) Monitoring → Latest data  
C) Reports → Data  
D) Administration → Queue

<v-click>

**Réponse : B**

**Monitoring → Latest data** affiche les dernières valeurs collectées pour tous les items des hosts.

</v-click>

---

# Question 10 📧

**Pour recevoir des alertes par email, que faut-il configurer ?**

A) Seulement le serveur SMTP  
B) Seulement le trigger  
C) Le media type Email + l'assigner à l'utilisateur + créer une action  
D) Rien, c'est automatique

<v-click>

**Réponse : C**

Il faut : 1) Configurer le media type (Email avec SMTP), 2) L'assigner à l'utilisateur, 3) Créer une action pour envoyer l'alerte.

</v-click>

---

# Score 🎯

**Barème :**
- 9-10 : Excellent ! ⭐⭐⭐
- 7-8 : Très bien ! ⭐⭐
- 5-6 : Bien, à consolider ⭐
- < 5 : Réviser le module

**Prochaine étape :** Exercices pratiques Zabbix

---
layout: default
---

# Fin du QCM ✅

Passez maintenant aux exercices pratiques !

