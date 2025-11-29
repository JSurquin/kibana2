---
layout: question
---

# QCM - Optimisation & Performances (BONUS) ✅

**Module 6 : Validation des connaissances**

---

# Question 1 🎯

**Quand faut-il optimiser un système ?**

A) Dès l'installation  
B) Tous les mois systématiquement  
C) Seulement si un problème de performance est identifié  
D) Jamais, le système s'optimise tout seul

<v-click>

**Réponse : C**

Ne pas optimiser prématurément. Optimiser seulement si un problème réel est identifié et mesuré.

</v-click>

---

# Question 2 💾

**Que signifie vm.swappiness=10 ?**

A) Utiliser 10% de swap  
B) Swap agressif  
C) Swap minimal, préférer la RAM  
D) Désactiver complètement le swap

<v-click>

**Réponse : C**

swappiness=10 minimise l'utilisation du swap, en privilégiant la RAM. Recommandé pour serveurs avec RAM suffisante.

</v-click>

---

# Question 3 🔧

**Comment appliquer les changements dans /etc/sysctl.conf ?**

A) `sudo sysctl -a`  
B) `sudo sysctl -p`  
C) `sudo systemctl reload sysctl`  
D) Redémarrer le serveur

<v-click>

**Réponse : B**

`sudo sysctl -p` relit et applique /etc/sysctl.conf sans redémarrage.

</v-click>

---

# Question 4 🚀

**Quelle valeur recommandée pour worker_processes dans Nginx ?**

A) 1  
B) 2  
C) 4  
D) auto

<v-click>

**Réponse : D**

`worker_processes auto` adapte automatiquement au nombre de CPUs disponibles.

</v-click>

---

# Question 5 🗄️

**Quelle valeur recommandée pour innodb_buffer_pool_size dans MySQL ?**

A) 128 Mo  
B) 10-20% de la RAM  
C) 70-80% de la RAM disponible  
D) 100% de la RAM

<v-click>

**Réponse : C**

70-80% de la RAM disponible pour MySQL est recommandé (laisser de la place pour l'OS).

</v-click>

---

# Score 🎯

**Barème :**
- 5/5 : Excellent ! ⭐⭐⭐
- 4/5 : Très bien ! ⭐⭐
- 3/5 : Bien ⭐
- < 3 : Réviser le module

---
layout: default
---

# Fin du QCM BONUS ✅

Passez aux exercices pratiques pour expérimenter !

