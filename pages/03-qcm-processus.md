---

# QCM Module 6 : Processus et services ✅

**10 questions sur la gestion des processus et systemd**

---

# Question 1

Quel est le PID du premier processus système ?

A) 0

B) 1

C) 10

D) 100

---

# Question 2

Quelle commande affiche les processus en temps réel ?

A) `ps`

B) `top`

C) `pgrep`

D) `htop`

---

# Question 3

Quel signal envoie `kill` par défaut ?

A) SIGKILL (9)

B) SIGTERM (15)

C) SIGHUP (1)

D) SIGINT (2)

---

# Question 4

Comment démarrer un service avec systemd ?

A) `systemctl start service`

B) `service start`

C) `start service`

D) `systemd start service`

---

# Question 5

Quelle est la différence entre `start` et `enable` ?

A) Aucune

B) `start` démarre maintenant, `enable` au boot

C) `enable` démarre maintenant, `start` au boot

D) Les deux font la même chose

---

# Question 6

Où voir les logs d'un service systemd ?

A) `/var/log/service.log`

B) `journalctl -u service`

C) `systemctl logs service`

D) `dmesg`

---

# Question 7

Comment changer la priorité d'un processus ?

A) `priority`

B) `nice` ou `renice`

C) `top`

D) `ps`

---

# Question 8

Que fait `Ctrl+Z` ?

A) Tue le processus

B) Suspend le processus

C) Quitte le programme

D) Redémarre le processus

---

# Question 9

Comment lister les services qui ont échoué ?

A) `systemctl failed`

B) `systemctl --failed`

C) `systemctl list-failed`

D) `journalctl --failed`

---

# Question 10

Qu'est-ce qu'un processus zombie ?

A) Un processus qui consomme beaucoup de CPU

B) Un processus terminé mais non récupéré par le parent

C) Un processus malveillant

D) Un processus sans PID

---

# Réponses du Module 6 📝

**Réponse 1 :** B) 1
- Le PID 1 est `systemd` (ou `init` sur les anciens systèmes)

**Réponse 2 :** B) `top` (ou D `htop`)
- Les deux affichent les processus en temps réel, `htop` est plus moderne

**Réponse 3 :** B) SIGTERM (15)
- SIGTERM permet au processus de se terminer proprement

**Réponse 4 :** A) `systemctl start service`
- systemd utilise la commande `systemctl`

**Réponse 5 :** B) `start` démarre maintenant, `enable` au boot
- `enable` configure le démarrage automatique

---

# Réponses du Module 6 (suite) 📝

**Réponse 6 :** B) `journalctl -u service`
- journalctl est l'outil de logs de systemd

**Réponse 7 :** B) `nice` ou `renice`
- `nice` au lancement, `renice` pour un processus existant

**Réponse 8 :** B) Suspend le processus
- Le processus passe en arrière-plan suspendu. `fg` pour reprendre

**Réponse 9 :** B) `systemctl --failed`
- Affiche tous les services en état "failed"

**Réponse 10 :** B) Un processus terminé mais non récupéré par le parent
- Il reste dans la table des processus jusqu'à ce que le parent récupère son code de sortie

---

# Score du Module 6 📊

Comptez vos bonnes réponses :

- **9-10** : Excellent ! ⭐⭐⭐
- **7-8** : Bien ! ⭐⭐
- **5-6** : À revoir ⭐
- **< 5** : Relire le module 3

