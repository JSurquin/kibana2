---

# QCM Module 8 : Automatisation et scripts ✅

**10 questions sur le scripting et les tâches planifiées**

---

# Question 1

Quelle est la première ligne d'un script bash ?

A) `#bash`
B) `#!/bin/bash`
C) `<bash>`
D) `//bin/bash`

---

# Question 2

Comment rendre un script exécutable ?

A) `chmod +x script.sh`
B) `execute script.sh`
C) `make executable script.sh`
D) `run script.sh`

---

# Question 3

Comment définir une variable en bash ?

A) `var = value`
B) `var=value`
C) `set var value`
D) `$var=value`

---

# Question 4

Comment accéder à la valeur d'une variable ?

A) `var`
B) `$var`
C) `&var`
D) `@var`

---

# Question 5

Quelle commande planifie des tâches récurrentes ?

A) `at`
B) `cron`
C) `timer`
D) `schedule`

---

# Question 6

Que signifie `0 2 * * *` dans un crontab ?

A) Toutes les 2 minutes
B) À 2h du matin tous les jours
C) Tous les 2 jours
D) Le 2 de chaque mois

---

# Question 7

Comment voir les logs systemd en temps réel ?

A) `tail -f /var/log/syslog`
B) `journalctl -f`
C) `logwatch`
D) `logs -f`

---

# Question 8

Quelle structure crée une boucle en bash ?

A) `loop ... end`
B) `for ... done`
C) `while ... endwhile`
D) `repeat ... until`

---

# Question 9

Comment rediriger stdout ET stderr vers un fichier ?

A) `commande > fichier`
B) `commande 2> fichier`
C) `commande &> fichier`
D) `commande >> fichier`

---

# Question 10

Quelle commande planifie une tâche ponctuelle ?

A) `cron`
B) `at`
C) `once`
D) `schedule`

---

# Réponses du Module 8 📝

**Réponse 1 :** B) `#!/bin/bash`
- Le shebang indique l'interpréteur à utiliser

**Réponse 2 :** A) `chmod +x script.sh`
- Puis l'exécuter avec `./script.sh`

**Réponse 3 :** B) `var=value`
- ⚠️ Pas d'espace autour du `=` !

**Réponse 4 :** B) `$var`
- Ou `${var}` (forme recommandée)

**Réponse 5 :** B) `cron`
- `crontab -e` pour éditer les tâches cron

---

# Réponses du Module 8 (suite) 📝

**Réponse 6 :** B) À 2h du matin tous les jours
- Format : minute heure jour mois jour_semaine

**Réponse 7 :** B) `journalctl -f`
- `-f` = follow (comme `tail -f`)

**Réponse 8 :** B) `for ... done`
- Exemple : `for i in {1..5}; do echo $i; done`

**Réponse 9 :** C) `commande &> fichier`
- Ou `commande > fichier 2>&1`

**Réponse 10 :** B) `at`
- `at now + 1 hour` pour dans 1 heure

---

# Score du Module 8 📊

Comptez vos bonnes réponses :

- **9-10** : Excellent ! ⭐⭐⭐
- **7-8** : Bien ! ⭐⭐
- **5-6** : À revoir ⭐
- **< 5** : Relire le module 7

---
