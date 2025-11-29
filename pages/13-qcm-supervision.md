---

# QCM - Supervision & Analyse Système ✅

**Module 4 : Validation des connaissances**

---

# Question 1 📊

**Que signifie un load average de 4.0 sur une machine avec 2 CPUs ?**

A) Le système est sous-utilisé  
B) Le système est à 50% de charge  
C) Le système est surchargé (2x plus de travail que de CPUs)  
D) Le système fonctionne normalement

<v-click>

**Réponse : C**

Load average > nombre de CPUs = surchargé. Ici 4.0 pour 2 CPUs = 2x trop de travail.

</v-click>

---

# Question 2 🎨

**Quel est l'avantage principal de htop par rapport à top ?**

A) htop est plus rapide  
B) htop utilise moins de ressources  
C) htop a une interface colorée et plus intuitive  
D) htop fonctionne sur plus de systèmes

<v-click>

**Réponse : C**

htop offre une interface colorée avec des barres graphiques, navigation au clavier et actions plus faciles.

</v-click>

---

# Question 3 💽

**Quelle commande permet de voir quel processus utilise le plus d'I/O disque ?**

A) top  
B) htop  
C) iostat  
D) iotop

<v-click>

**Réponse : D**

iotop affiche l'utilisation I/O disque par processus en temps réel.

</v-click>

---

# Question 4 📜

**Comment voir les logs d'erreur du service nginx avec journalctl ?**

A) `journalctl nginx`  
B) `journalctl -u nginx -p err`  
C) `journalctl --error nginx`  
D) `journalctl -e nginx`

<v-click>

**Réponse : B**

`journalctl -u nginx -p err` filtre les logs du service nginx avec priorité erreur.

</v-click>

---

# Question 5 📈

**Quel outil collecte automatiquement les statistiques système toutes les 10 minutes ?**

A) top  
B) sar (sysstat)  
C) htop  
D) vmstat

<v-click>

**Réponse : B**

sar (System Activity Reporter) collecte automatiquement les stats système pour analyse historique.

</v-click>

---

# Question 6 🔄

**Quel fichier configure la rotation automatique des logs nginx ?**

A) /etc/nginx/logrotate.conf  
B) /etc/logrotate.d/nginx  
C) /var/log/nginx/rotate.conf  
D) /etc/log/rotate/nginx

<v-click>

**Réponse : B**

Les configurations logrotate spécifiques sont dans /etc/logrotate.d/

</v-click>

---

# Question 7 💾

**Quel seuil d'utilisation disque est généralement considéré comme critique ?**

A) > 50%  
B) > 70%  
C) > 85%  
D) > 95%

<v-click>

**Réponse : C**

Au-delà de 85% d'utilisation disque, il y a risque de plantage ou ralentissement.

</v-click>

---

# Question 8 🔍

**Dans vmstat, que signifie si/so > 0 ?**

A) Le CPU est surchargé  
B) Le système utilise le swap (problème mémoire)  
C) Le disque est lent  
D) Le réseau est saturé

<v-click>

**Réponse : B**

si (swap in) et so (swap out) > 0 indiquent que le système manque de RAM et utilise le swap.

</v-click>

---

# Question 9 🧹

**Comment nettoyer les logs journald pour garder seulement 7 jours ?**

A) `journalctl --clean 7d`  
B) `journalctl --vacuum-time=7d`  
C) `journalctl --delete-old 7`  
D) `journalctl -r 7d`

<v-click>

**Réponse : B**

`journalctl --vacuum-time=7d` supprime les logs plus vieux que 7 jours.

</v-click>

---

# Question 10 ⏰

**Quelle syntaxe cron pour exécuter un script toutes les 5 minutes ?**

A) `5 * * * * /script.sh`  
B) `*/5 * * * * /script.sh`  
C) `* 5 * * * /script.sh`  
D) `0/5 * * * * /script.sh`

<v-click>

**Réponse : B**

`*/5` signifie "toutes les 5 unités" pour le champ minutes.

</v-click>

---

# Score 🎯

**Barème :**
- 9-10 : Excellent ! ⭐⭐⭐
- 7-8 : Très bien ! ⭐⭐
- 5-6 : Bien, à consolider ⭐
- < 5 : Réviser le module

**Prochaine étape :** Exercices pratiques

---
layout: default
---

# Fin du QCM ✅

Passez maintenant aux exercices pratiques pour mettre en application ces connaissances !

