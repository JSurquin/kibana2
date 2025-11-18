---

# QCM Module 7 : Automatisation et scripts ✅

**10 questions sur le scripting et les tâches planifiées**

---

# Question 1

Quelle est la première ligne d'un script bash ?

A) `#bash`
B) `#!/bin/bash`
C) `<bash>`
D) `//bin/bash`

---

# Réponse 1

**Bonne réponse : B) `#!/bin/bash`**

Le shebang indique l'interpréteur à utiliser.

---

# Question 2

Comment rendre un script exécutable ?

A) `chmod +x script.sh`
B) `execute script.sh`
C) `make executable script.sh`
D) `run script.sh`

---

# Réponse 2

**Bonne réponse : A) `chmod +x script.sh`**

Puis l'exécuter avec `./script.sh`.

---

# Question 3

Comment définir une variable en bash ?

A) `var = value`
B) `var=value`
C) `set var value`
D) `$var=value`

---

# Réponse 3

**Bonne réponse : B) `var=value`**

⚠️ Pas d'espace autour du `=` !

---

# Question 4

Comment accéder à la valeur d'une variable ?

A) `var`
B) `$var`
C) `&var`
D) `@var`

---

# Réponse 4

**Bonne réponse : B) `$var`**

Ou `${var}` (forme recommandée).

---

# Question 5

Quelle commande planifie des tâches récurrentes ?

A) `at`
B) `cron`
C) `timer`
D) `schedule`

---

# Réponse 5

**Bonne réponse : B) `cron`**

`crontab -e` pour éditer les tâches cron.

---

# Question 6

Que signifie `0 2 * * *` dans un crontab ?

A) Toutes les 2 minutes
B) À 2h du matin tous les jours
C) Tous les 2 jours
D) Le 2 de chaque mois

---

# Réponse 6

**Bonne réponse : B) À 2h du matin tous les jours**

Format : minute heure jour mois jour_semaine.

---

# Question 7

Comment voir les logs systemd en temps réel ?

A) `tail -f /var/log/syslog`
B) `journalctl -f`
C) `logwatch`
D) `logs -f`

---

# Réponse 7

**Bonne réponse : B) `journalctl -f`**

`-f` = follow (comme `tail -f`).

---

# Question 8

Quelle structure crée une boucle en bash ?

A) `loop ... end`
B) `for ... done`
C) `while ... endwhile`
D) `repeat ... until`

---

# Réponse 8

**Bonne réponse : B) `for ... done`**

Exemple : `for i in {1..5}; do echo $i; done`.

---

# Question 9

Comment rediriger stdout ET stderr vers un fichier ?

A) `commande > fichier`
B) `commande 2> fichier`
C) `commande &> fichier`
D) `commande >> fichier`

---

# Réponse 9

**Bonne réponse : C) `commande &> fichier`**

Ou `commande > fichier 2>&1`.

---

# Question 10

Quelle commande planifie une tâche ponctuelle ?

A) `cron`
B) `at`
C) `once`
D) `schedule`

---

# Réponse 10

**Bonne réponse : B) `at`**

`at now + 1 hour` pour dans 1 heure.

---

# Score du Module 7 📊

Comptez vos bonnes réponses :

- **9-10** : Excellent ! ⭐⭐⭐
- **7-8** : Bien ! ⭐⭐
- **5-6** : À revoir ⭐
- **< 5** : Relire le module 7

---

