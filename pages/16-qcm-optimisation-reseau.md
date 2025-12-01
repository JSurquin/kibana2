---

# QCM - Optimisation Réseau Linux (BONUS) ✅

**Module BONUS : Validation des connaissances**

---

# Question 1 🎯

**Que signifie "optimiser le réseau" sous Linux ?**

A) Rendre Internet plus rapide  
B) Rendre la machine plus efficace pour traiter le trafic réseau  
C) Augmenter la vitesse de la connexion Internet  
D) Installer plus de cartes réseau

<v-click>

**Réponse : B**

Optimiser le réseau ne change pas la vitesse d'Internet, mais améliore la façon dont la machine traite le trafic.

</v-click>

---

# Question 2 ⚙️

**Quel est l'outil principal pour modifier les paramètres TCP/IP ?**

A) ifconfig  
B) netstat  
C) sysctl  
D) ping

<v-click>

**Réponse : C**

`sysctl` permet de modifier les paramètres du noyau Linux, notamment la pile TCP/IP.

</v-click>

---

# Question 3 🌐

**Quel algorithme de congestion TCP est recommandé en 2025 ?**

A) Reno  
B) Tahoe  
C) BBR  
D) Vegas

<v-click>

**Réponse : C**

BBR (Bottleneck Bandwidth and RTT) est l'algorithme moderne développé par Google, optimisé pour les réseaux actuels.

</v-click>

---

# Question 4 📡

**Quel outil permet d'optimiser les paramètres de la carte réseau ?**

A) sysctl  
B) ethtool  
C) iptables  
D) netcat

<v-click>

**Réponse : B**

`ethtool` est l'outil dédié à la configuration et l'optimisation des cartes réseau (NIC).

</v-click>

---

# Question 5 🐢

**Qu'est-ce que le bufferbloat ?**

A) Un virus réseau  
B) Une latence excessive causée par des buffers trop grands  
C) Un protocole de sécurité  
D) Un type de pare-feu

<v-click>

**Réponse : B**

Le bufferbloat est une latence excessive causée par des buffers réseau surdimensionnés.

</v-click>

---

# Question 6 ⚖️

**Quel algorithme de file d'attente est moderne et recommandé ?**

A) pfifo  
B) fq_codel  
C) bfifo  
D) red

<v-click>

**Réponse : B**

`fq_codel` (Fair Queue Controlled Delay) est un algorithme moderne qui combat le bufferbloat.

</v-click>

---

# Question 7 🔐

**Quel est le système de pare-feu moderne recommandé en 2025 ?**

A) ipchains  
B) iptables (classique)  
C) nftables  
D) firewalld seulement

<v-click>

**Réponse : C**

`nftables` est le successeur moderne d'iptables, avec de meilleures performances et une syntaxe améliorée.

</v-click>

---

# Question 8 🐋

**Quel mode réseau Docker offre les meilleures performances ?**

A) bridge (par défaut)  
B) none  
C) host  
D) overlay

<v-click>

**Réponse : C**

Le mode `host` élimine le NAT et offre les meilleures performances (mais moins d'isolation).

</v-click>

---

# Question 9 📊

**Quel outil permet de tester le débit réseau entre deux machines ?**

A) ping  
B) traceroute  
C) iperf3  
D) wget

<v-click>

**Réponse : C**

`iperf3` est l'outil standard pour tester les performances réseau (débit, latence).

</v-click>

---

# Question 10 🔍

**Quelle commande remplace netstat dans les systèmes modernes ?**

A) ip  
B) ss  
C) ifconfig  
D) route

<v-click>

**Réponse : B**

`ss` (socket statistics) remplace netstat et est plus rapide et moderne.

</v-click>

---

# Question 11 🚀

**Que signifie TSO dans le contexte de l'offloading réseau ?**

A) TCP Socket Optimization  
B) TCP Segmentation Offload  
C) Total System Offload  
D) Transport Security Offload

<v-click>

**Réponse : B**

TSO (TCP Segmentation Offload) décharge le CPU en faisant segmenter les paquets par la carte réseau.

</v-click>

---

# Question 12 📈

**Quelle valeur de net.core.somaxconn est recommandée pour un serveur web haute charge ?**

A) 128 (défaut)  
B) 512  
C) 4096 ou plus  
D) 50

<v-click>

**Réponse : C**

Pour un serveur web haute charge, somaxconn doit être augmenté à 4096 ou plus.

</v-click>

---

# Question 13 🔌

**Quel MTU est utilisé pour les Jumbo Frames ?**

A) 1500 (standard)  
B) 1450  
C) 9000  
D) 65535

<v-click>

**Réponse : C**

Les Jumbo Frames utilisent un MTU de 9000 octets (au lieu de 1500), utile pour le stockage réseau.

</v-click>

---

# Question 14 🚦

**Que permet la commande "tc" sous Linux ?**

A) Gérer les tâches cron  
B) Contrôler le trafic et les files d'attente réseau  
C) Tester la connectivité  
D) Créer des conteneurs

<v-click>

**Réponse : B**

`tc` (Traffic Control) gère les files d'attente réseau et le contrôle du trafic.

</v-click>

---

# Question 15 📏

**Quelle est la première étape avant d'optimiser un système ?**

A) Appliquer toutes les optimisations disponibles  
B) Mesurer les performances actuelles  
C) Redémarrer le serveur  
D) Installer de nouveaux outils

<v-click>

**Réponse : B**

Toujours mesurer avant d'optimiser ! On ne peut améliorer que ce qu'on mesure.

</v-click>

---

# Question 16 ⏱️

**Quel paramètre sysctl permet de réutiliser les sockets TIME_WAIT ?**

A) net.ipv4.tcp_reuse  
B) net.ipv4.tcp_tw_reuse  
C) net.ipv4.tcp_recycle  
D) net.ipv4.tcp_socket_reuse

<v-click>

**Réponse : B**

`net.ipv4.tcp_tw_reuse` permet de réutiliser les sockets en état TIME_WAIT.

</v-click>

---

# Question 17 🦭

**Quel backend réseau utilise Podman par défaut en 2025 ?**

A) CNI  
B) slirp4netns  
C) netavark  
D) bridge

<v-click>

**Réponse : C**

Podman utilise désormais `netavark` par défaut, plus performant que slirp4netns.

</v-click>

---

# Question 18 💾

**Pour un serveur de stockage NFS, quel paramètre est important ?**

A) Latence minimale uniquement  
B) TCP window large pour débit maximal  
C) Compression des données  
D) Chiffrement systématique

<v-click>

**Réponse : B**

Pour NFS, un TCP window large maximise le débit des gros transferts de fichiers.

</v-click>

---

# Question 19 📡

**Quel outil permet de visualiser la bande passante en temps réel ?**

A) ping  
B) iftop  
C) top  
D) htop

<v-click>

**Réponse : B**

`iftop` affiche la bande passante en temps réel par connexion.

</v-click>

---

# Question 20 🎯

**Quelle est la bonne approche pour l'optimisation réseau ?**

A) Appliquer tous les réglages trouvés sur Internet  
B) Mesurer, optimiser, re-mesurer  
C) Optimiser sans mesurer, ça marche toujours  
D) Ne jamais toucher aux paramètres par défaut

<v-click>

**Réponse : B**

La démarche scientifique : mesurer avant, optimiser, mesurer après pour valider l'amélioration.

</v-click>

---

# Score 🎯

**Barème :**
- 18-20/20 : Expert ! ⭐⭐⭐⭐
- 15-17/20 : Excellent ! ⭐⭐⭐
- 12-14/20 : Très bien ! ⭐⭐
- 10-11/20 : Bien ⭐
- < 10 : Réviser le module

---
layout: default
---

# Fin du QCM Optimisation Réseau ✅

Félicitations ! Vous maîtrisez maintenant l'optimisation réseau sous Linux !

Passez aux exercices pratiques pour expérimenter avec des serveurs réels.
