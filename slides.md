---
theme: ./
colorSchema: "auto"
layout: intro
highlighter: shiki
# https://sli.dev/custom/highlighters.html
title: Formation Unix/Linux - Initiation 2025
# download: true
#transition: slide-left
# remoteAssets: false
# export:
#   zoom: 1
#   format: pdf
#   timeout: 300000000
#   pdfOptions:
#     format: A4
download: "https://unix-linux.andromed.fr/slides.pdf"
themeConfig:
  logoHeader: "/avatar.png"
  eventLogo: "https://img2.storyblok.com/352x414/f/84560/2388x414/23d8eb4b8d/vue-amsterdam-with-name.png"
  eventUrl: "https://vuejs.amsterdam/"
#addons:
  #- "@slidev/addon-sharp-long-code"
---

# Unix/Linux - Initiation 2025

🐧 Une formation présentée par Ascent et Andromed.

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Appuyez sur espace pour la page suivante <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: presenter
eventLogo: 'https://img2.storyblok.com/352x0/f/84560/2388x414/23d8eb4b8d/vue-amsterdam-with-name.png'
eventUrl: 'https://vuejs.amsterdam/'
twitter: '@jimmylansrq'

twitterUrl: 'https://twitter.com/jimmylansrq'
presenterImage: 'https://legacy.andromed.fr/images/fondator.jpg'
---

# Jimmylan Surquin

Fondateur <a  href="https://www.andromed.fr/"><logos-storyblok-icon  mr-1/>Andromed</a>

- Lille, France 🇫🇷
- Création de contenu sur <a href="https://www.youtube.com/channel/jimmylansrq"> <logos-youtube-icon mr-1 /> jimmylansrq </a>
- Blog & Portfolio <a href="https://jimmylan.fr"> jimmylan.fr </a>

---
layout: text-image
media: 'https://i.pinimg.com/originals/f5/5e/80/f55e8059ea945abfd6804b887dd4a0af.gif'
caption: 'UNIX/LINUX 2025'
---

# DISCLAIMER 🐧

### Dans cette formation nous allons voir les concepts fondamentaux d'Unix/Linux en 2025.

---
layout: two-cols
routeAlias: 'sommaire'
---

<a name="SOMMAIRE-UNIX-LINUX" id="sommaire-unix-linux"></a>

# SOMMAIRE UNIX/LINUX - INITIATION 📜

<br>

<div class="flex flex-col gap-2">
<Link to="introduction-unix-linux">🧱 1. Introduction et historique Unix/Linux</Link>
<Link to="commandes-basiques">🔧 2. Commandes de base essentielles</Link>
<Link to="paquets-maintenance">📦 3. Gestion des paquets et maintenance</Link>
<Link to="gestion-systeme-utilisateurs">👥 4. Gestion des utilisateurs et permissions</Link>
<Link to="stockage-systeme-fichiers">💾 5. Stockage et système de fichiers</Link>
</div>

::right::

<div class="flex flex-col gap-2">
<Link to="processus-services">⚙️ 6. Gestion des processus et services</Link>
<Link to="reseau">📡 7. Configuration réseau</Link>
<Link to="automatisation-scripts">🤖 8. Automatisation et scripts</Link>
<Link to="securite-systeme">🔐 9. Sécurité système</Link>
<Link to="services-serveurs">☁️ 10. Services serveurs</Link>
<Link to="supervision-analyse">📊 11. Supervision & Analyse Système</Link>
<Link to="supervision-avancee">🚨 12. Supervision Avancée (Zabbix)</Link>
<Link to="optimisation-performances">⚡ 13. Optimisation & Performances (BONUS)</Link>
<Link to="optimisation-reseau-linux">🌐 14. Optimisation Réseau Linux (BONUS)</Link>
<Link to="qcm-final">✅ QCM de validation finale</Link>
</div>

**Note :** Chaque module contient :
- 📖 Cours théorique
- ✅ QCM de validation
- 🎯 Exercices pratiques

---
layout: two-cols
routeAlias: 'programme-initiation'
---

### PROGRAMME INITIATION UNIX/LINUX 📅

<small>

**Module 1 - Introduction**
- Historique Unix, GNU/Linux
- Philosophie Unix
- Architecture système
- Noyau, shell

**Module 2 - Commandes de base** 🆕
- Navigation (cd, ls, pwd)
- Manipulation fichiers (cp, mv, rm)
- Recherche (find, grep)
- Visualisation (cat, less, head, tail)
- Redirections et pipes

</small>

::right::

<small>

**Module 3 - Paquets et maintenance**
- apt/dnf : installation
- Mise à jour système
- Gestion dépendances

**Module 4 - Utilisateurs et permissions**
- Comptes et groupes
- Permissions (chmod, chown)
- sudo et sudoers
- Quotas disque

**Module 5 - Stockage**
- Disques et partitions
- Montage (mount, fstab)
- Liens symboliques
- ACL

</small>

---
layout: two-cols
---

### PROGRAMME INITIATION UNIX/LINUX 📅

<small>

**Module 6 - Processus et services**
- PID, signaux
- nice, kill
- systemd
- journalctl

**Module 7 - Réseau**
- Configuration IP
- SSH sécurisé
- Pare-feu (ufw)
- Diagnostic (ping, traceroute)

</small>

::right::

<small>

**Module 8 - Automatisation**
- Scripts Bash
- Cron et at
- Logs système

**Module 9 - Sécurité**
- Durcissement SSH
- fail2ban
- Audit (auditd)
- Chiffrement (LUKS)

**Module 10 - Services serveurs**
- Nginx
- MySQL
- SSL/TLS
- Monitoring

</small>

---
layout: two-cols
---

### PROGRAMME INITIATION UNIX/LINUX 📅

<small>

**Module 11 - Supervision & Analyse**
- Monitoring (top, htop, iotop)
- Analyse des logs
- Métriques système
- Tableaux de bord

**Module 12 - Supervision Avancée**
- Zabbix serveur et agent
- Alertes et notifications
- Templates personnalisés
- Dashboard avancés

</small>

::right::

<small>

**Module 13 - Optimisation (BONUS)** ⚡
- Tuning kernel (sysctl)
- Optimisation mémoire
- Optimisation serveurs
- Performance des services

**Module 14 - Optimisation Réseau (BONUS)** 🌐
- Pile TCP/IP et sysctl
- Carte réseau (NIC)
- Files d'attente & bufferbloat
- Pare-feu & conteneurs

**QCM Final** ✅
- Validation des connaissances
- Tous les modules
- Certification de la formation

</small>

---
src: './pages/01-introduction-unix-linux.md'
---

---
src: './pages/01-qcm-introduction.md'
---

---
src: './pages/05-commandes-basiques.md'
---

---
src: './pages/05-qcm-commandes.md'
---

---
src: './pages/05-exercices-commandes.md'
---

---
src: './pages/06-paquets-maintenance.md'
---

---
src: './pages/06-qcm-paquets.md'
---

---
src: './pages/06-exercices-paquets.md'
---

---
src: './pages/02-gestion-systeme-utilisateurs.md'
---

---
src: './pages/02-qcm-utilisateurs.md'
---

---
src: './pages/02-exercices-utilisateurs.md'
---

---
src: './pages/04-stockage-systeme-fichiers.md'
---

---
src: './pages/04-qcm-stockage.md'
---

---
src: './pages/04-exercices-stockage.md'
---

---
src: './pages/03-processus-services.md'
---

---
src: './pages/03-qcm-processus.md'
---

---
src: './pages/03-exercices-processus.md'
---

---
src: './pages/05-reseau.md'
---

---
src: './pages/05-qcm-reseau.md'
---

---
src: './pages/05-exercices-reseau.md'
---

---
src: './pages/07-automatisation-scripts.md'
---

---
src: './pages/07-qcm-automatisation.md'
---

---
src: './pages/07-exercices-automatisation.md'
---

---
src: './pages/08-securite-systeme.md'
---

---
src: './pages/08-qcm-securite.md'
---

---
src: './pages/08-exercices-securite.md'
---

---
src: './pages/09-services-serveurs.md'
---

---
src: './pages/09-qcm-serveurs.md'
---

---
src: './pages/09-exercices-serveurs.md'
---

---
src: './pages/13-supervision-analyse-systeme.md'
---

---
src: './pages/13-qcm-supervision.md'
---

---
src: './pages/13-exercices-supervision.md'
---

---
src: './pages/14-supervision-avancee-alertes.md'
---

---
src: './pages/14-qcm-supervision-avancee.md'
---

---
src: './pages/14-exercices-supervision-avancee.md'
---

---
src: './pages/15-optimisation-performances.md'
---

---
src: './pages/15-qcm-optimisation.md'
---

---
src: './pages/15-exercices-optimisation.md'
---

---
src: './pages/16-optimisation-reseau-linux.md'
---

---
src: './pages/16-qcm-optimisation-reseau.md'
---

---
src: './pages/16-exercices-optimisation-reseau.md'
---

---
src: './pages/10-qcm-final.md'
---
