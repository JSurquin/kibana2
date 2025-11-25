---
layout: new-section
routeAlias: 'securite-systeme'
---

<a name="securite-systeme" id="securite-systeme"></a>

# 🔐 Module 9
## Sécurité système

### Protéger et durcir votre système Linux

---

# La sécurité : un principe de couches 🧅

**Analogie : la sécurité d'une maison** 🏠

1. **Portail** (pare-feu)
2. **Porte blindée** (authentification forte)
3. **Alarme** (monitoring)
4. **Coffre-fort** (chiffrement)
5. **Caméras** (logs d'audit)

**Principe de défense en profondeur :**
- Une couche échoue → les autres protègent
- Jamais se reposer sur une seule protection

---

# Les principes de sécurité 🛡️

**1. Moindre privilège**
- Donner uniquement les droits nécessaires
- Ne jamais travailler en root

**2. Défense en profondeur**
- Plusieurs couches de sécurité

**3. Sécurité par la simplicité**
- Moins de services = moins de vulnérabilités

**4. Mise à jour régulière**
- Correctifs de sécurité critiques

**5. Audit et surveillance**
- Savoir ce qui se passe sur le système

**Analogie : la sécurité de votre banque** 🏦

**Moindre privilège** : L'employé au guichet ne peut pas ouvrir le coffre-fort

**Défense en profondeur** :
1. Vigiles à l'entrée (pare-feu)
2. Porte blindée (authentification)
3. Caméras (logs)
4. Alarme (fail2ban)
5. Coffre-fort (chiffrement)

**Sécurité par simplicité** : Moins de portes = moins de points d'entrée

**Mise à jour** : Changer les codes d'accès régulièrement

**Audit** : Vérifier les enregistrements vidéo

---

# Sécurité des comptes utilisateurs 👤

**Rappels des bonnes pratiques :**

✅ Ne jamais utiliser root directement

✅ Désactiver le login root en SSH

✅ Utiliser sudo avec parcimonie

✅ Politique de mots de passe forte

✅ Supprimer les comptes inutilisés

✅ Auditer régulièrement les comptes

---

# Politique de mot de passe : PAM 🔑

**PAM** : Pluggable Authentication Modules

**Configuration : `/etc/pam.d/common-password`**

```bash
# Installer
sudo apt install libpam-pwquality

# Configuration
sudo nano /etc/security/pwquality.conf
```

---

# Configuration pwquality 📝

```ini
# Longueur minimum
minlen = 12

# Au moins 1 minuscule
lcredit = -1

# Au moins 1 majuscule
ucredit = -1

# Au moins 1 chiffre
dcredit = -1

# Au moins 1 caractère spécial
ocredit = -1

# Nombre de caractères différents minimum
difok = 3

# Interdire les mots du dictionnaire
dictcheck = 1
```

---

# Expiration des mots de passe ⏳

```bash
# Configurer l'expiration (90 jours)
sudo chage -M 90 alice

# Prévenir 7 jours avant
sudo chage -W 7 alice

# Délai minimum entre changements (1 jour)
sudo chage -m 1 alice

# Voir la config
sudo chage -l alice

# Forcer le changement au prochain login
sudo chage -d 0 alice
```

---

# Configuration par défaut dans /etc/login.defs 📋

```bash
sudo nano /etc/login.defs
```

```
PASS_MAX_DAYS   90      # Expiration
PASS_MIN_DAYS   1       # Délai minimum
PASS_MIN_LEN    12      # Longueur minimum
PASS_WARN_AGE   7       # Avertissement
```

**⚠️ S'applique uniquement aux nouveaux utilisateurs**

---

# Verrouillage automatique après échecs 🔒

**Fichier : `/etc/pam.d/common-auth`**

```bash
auth required pam_tally2.so deny=5 unlock_time=900
```

**Paramètres :**
- `deny=5` : bloquer après 5 échecs
- `unlock_time=900` : débloquer après 15 minutes (900 secondes)

---

# Gérer les verrouillages 🗝️

```bash
# Voir les tentatives d'un utilisateur
sudo pam_tally2 -u alice

# Réinitialiser le compteur
sudo pam_tally2 -u alice --reset

# Voir tous les utilisateurs
sudo pam_tally2
```

---

# Sudo : configuration avancée 🎛️

**Rappel : toujours utiliser `visudo` !**

```bash
sudo visudo
```

**Log des commandes sudo :**

```
Defaults logfile="/var/log/sudo.log"
Defaults log_year, log_host, loglinelen=0
```

**Timeout du mot de passe (en minutes) :**

```
Defaults timestamp_timeout=5
```

**Toujours demander le mot de passe :**

```
Defaults timestamp_timeout=0
```

---

# Sudo : restreindre les commandes 🔧

```
# Alice peut seulement redémarrer nginx
alice ALL=(ALL) /usr/bin/systemctl restart nginx

# Bob peut gérer les paquets
bob ALL=(ALL) /usr/bin/apt, /usr/bin/dnf

# Groupe ops peut tout
%ops ALL=(ALL) ALL

# Interdire certaines commandes
charlie ALL=(ALL) ALL, !/bin/su, !/bin/bash
```

---

# Sudo : environnement sécurisé 🌍

**Nettoyer les variables d'environnement :**

```
Defaults env_reset
Defaults env_keep += "LANG LC_* HOME"
Defaults secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
```

**Empêcher l'utilisation de LD_PRELOAD :**

```
Defaults !env_keep += "LD_PRELOAD"
```

---

# Fail2ban : protection contre brute-force 🚫

**Fail2ban** : bannit les IP après plusieurs échecs

```bash
# Installer
sudo apt install fail2ban

# Démarrer
sudo systemctl enable --now fail2ban

# État
sudo systemctl status fail2ban
```

---

# Configuration Fail2ban 📝

**Fichier : `/etc/fail2ban/jail.local`**

```ini
[DEFAULT]
bantime = 3600         # Bannir 1 heure
findtime = 600         # Fenêtre de 10 minutes
maxretry = 5           # 5 échecs max
destemail = admin@example.com
sendername = Fail2ban
action = %(action_mwl)s   # Ban + email avec logs

[sshd]
enabled = true
port = ssh
logpath = /var/log/auth.log
maxretry = 3
```

---

# Gérer Fail2ban 🎮

```bash
# Voir l'état des jails
sudo fail2ban-client status

# État d'une jail spécifique
sudo fail2ban-client status sshd

# Bannir manuellement une IP
sudo fail2ban-client set sshd banip 203.0.113.51

# Débannir une IP
sudo fail2ban-client set sshd unbanip 203.0.113.51

# Recharger la config
sudo fail2ban-client reload
```

---

# Créer une jail personnalisée 🔧

**Protection du serveur web :**

```ini
[nginx-403]
enabled = true
port = http,https
filter = nginx-403
logpath = /var/log/nginx/error.log
maxretry = 10
findtime = 600
bantime = 7200
```

**Fichier de filtre : `/etc/fail2ban/filter.d/nginx-403.conf`**

```ini
[Definition]
failregex = ^<HOST>.*"(GET|POST|HEAD).*" 403
ignoreregex =
```

---

# SELinux : Security-Enhanced Linux 🛡️

**SELinux** : système de contrôle d'accès obligatoire (MAC)

**Analogie : badges d'accès dans une entreprise**
- **Permissions Unix** : "vous êtes autorisé"
- **SELinux** : "vous êtes autorisé ET vous avez le bon badge"

**Par défaut sur :** RHEL, CentOS, Fedora, AlmaLinux

**3 modes :**
- **Enforcing** : applique les règles
- **Permissive** : log mais n'applique pas
- **Disabled** : désactivé

---

# Vérifier l'état de SELinux 📊

```bash
# État actuel
getenforce

# Informations détaillées
sestatus

# Changer temporairement (jusqu'au reboot)
sudo setenforce 0    # Permissive
sudo setenforce 1    # Enforcing
```

---

# Configurer SELinux 📝

**Fichier : `/etc/selinux/config`**

```bash
SELINUX=enforcing
SELINUXTYPE=targeted
```

**Modes :**
- `enforcing` : actif
- `permissive` : logs uniquement
- `disabled` : désactivé

**⚠️ Changer de disabled à enforcing nécessite un relabel complet !**

```bash
# Forcer le relabel au prochain boot
sudo touch /.autorelabel
sudo reboot
```

---

# SELinux : contextes 🏷️

**Chaque fichier/processus a un contexte SELinux**

```bash
# Voir le contexte des fichiers
ls -Z /var/www/html

# Contexte d'un processus
ps auxZ | grep nginx

# Contexte de l'utilisateur
id -Z
```

**Format :** `user:role:type:level`

Exemple : `system_u:object_r:httpd_sys_content_t:s0`

---

# Changer les contextes SELinux 🔧

```bash
# Changer temporairement
sudo chcon -t httpd_sys_content_t /var/www/html/index.html

# Changer récursivement
sudo chcon -R -t httpd_sys_content_t /var/www/html

# Restaurer le contexte par défaut
sudo restorecon /var/www/html/index.html
sudo restorecon -R /var/www/html

# Définir le contexte par défaut (persistant)
sudo semanage fcontext -a -t httpd_sys_content_t "/srv/web(/.*)?"
sudo restorecon -R /srv/web
```

---

# Booléens SELinux 🔘

**Booléens** : options on/off pour des fonctionnalités

```bash
# Lister tous les booléens
getsebool -a

# Voir un booléen spécifique
getsebool httpd_can_network_connect

# Activer temporairement
sudo setsebool httpd_can_network_connect on

# Activer de façon permanente
sudo setsebool -P httpd_can_network_connect on
```

**Exemples courants :**
- `httpd_can_network_connect` : Apache peut se connecter au réseau
- `httpd_read_user_content` : Apache peut lire les fichiers utilisateurs
- `ftpd_full_access` : FTP a accès complet

---

# Déboguer SELinux 🐛

```bash
# Installer les outils
sudo dnf install setroubleshoot-server

# Voir les alertes
sudo sealert -a /var/log/audit/audit.log

# Analyser un problème spécifique
sudo sealert -l <ID-de-alerte>

# Les logs SELinux
sudo tail -f /var/log/audit/audit.log | grep denied
```

**⚠️ En cas de problème, passer temporairement en permissive pour tester**

---

# AppArmor : alternative à SELinux 🍎

**AppArmor** : plus simple que SELinux

**Par défaut sur :** Ubuntu, Debian, SUSE

```bash
# Vérifier si AppArmor est actif
sudo aa-status

# Profils actifs
sudo aa-enabled
```

---

# Modes de profils AppArmor 🎭

**Enforce** : applique les restrictions
**Complain** : log mais n'applique pas

```bash
# Mettre un profil en mode complain
sudo aa-complain /etc/apparmor.d/usr.sbin.nginx

# Remettre en mode enforce
sudo aa-enforce /etc/apparmor.d/usr.sbin.nginx

# Désactiver un profil
sudo ln -s /etc/apparmor.d/usr.sbin.nginx /etc/apparmor.d/disable/
sudo apparmor_parser -R /etc/apparmor.d/usr.sbin.nginx

# Réactiver
sudo rm /etc/apparmor.d/disable/usr.sbin.nginx
sudo apparmor_parser -r /etc/apparmor.d/usr.sbin.nginx
```

---

# Créer un profil AppArmor 🔧

```bash
# Générer un profil de base
sudo aa-genprof /usr/bin/mon-programme

# Améliorer un profil existant
sudo aa-logprof

# Recharger tous les profils
sudo systemctl reload apparmor
```

---

# Exemple de profil AppArmor 📝

**Fichier : `/etc/apparmor.d/usr.bin.mon-app`**

```
#include <tunables/global>

/usr/bin/mon-app {
  #include <abstractions/base>

  # Lecture de la config
  /etc/mon-app/** r,

  # Écriture des logs
  /var/log/mon-app/** rw,

  # Accès réseau
  network inet stream,

  # Exécuter des binaires
  /usr/bin/curl rix,
}
```

---

# Durcissement SSH 🔐

**Fichier : `/etc/ssh/sshd_config`**

```bash
# Désactiver root login
PermitRootLogin no

# Désactiver l'authentification par mot de passe
PasswordAuthentication no

# Seulement clés publiques
PubkeyAuthentication yes

# Désactiver les mots de passe vides
PermitEmptyPasswords no

# Changer le port (obscurité, pas sécurité)
Port 2222
```

---

# Durcissement SSH (suite) 🔐

```bash
# Limiter les utilisateurs autorisés
AllowUsers alice bob

# Ou par groupe
AllowGroups sshusers

# Timeout de connexion
ClientAliveInterval 300
ClientAliveCountMax 2

# Nombre de tentatives de connexion
MaxAuthTries 3

# Désactiver X11 forwarding (si non utilisé)
X11Forwarding no

# Désactiver le forwarding de port (si non utilisé)
AllowTcpForwarding no
```

---

# Durcissement SSH : authentification à deux facteurs 🔑🔑

**Google Authenticator :**

```bash
# Installer
sudo apt install libpam-google-authenticator

# Configurer pour l'utilisateur
google-authenticator

# Modifier /etc/pam.d/sshd
auth required pam_google_authenticator.so
```

**Fichier `/etc/ssh/sshd_config` :**

```
ChallengeResponseAuthentication yes
AuthenticationMethods publickey,keyboard-interactive
```

```bash
# Redémarrer SSH
sudo systemctl restart sshd
```

---

# Audit du système : auditd 📝

**auditd** : système d'audit du noyau

```bash
# Installer
sudo apt install auditd

# Démarrer
sudo systemctl enable --now auditd

# État
sudo auditctl -s
```

---

# Règles d'audit 🔍

```bash
# Auditer l'accès à un fichier
sudo auditctl -w /etc/passwd -p wa -k passwd_changes

# Auditer les modifications de permissions
sudo auditctl -a always,exit -F arch=b64 -S chmod,fchmod,fchmodat -k perm_mod

# Auditer les suppressions de fichiers
sudo auditctl -a always,exit -F arch=b64 -S unlink,unlinkat,rename,renameat -k delete

# Lister les règles
sudo auditctl -l
```

**Options :**
- `-w` : watch (surveiller un fichier)
- `-p` : permissions (r=read, w=write, x=execute, a=attribute)
- `-k` : key (identifiant pour recherche)

---

# Rechercher dans les logs d'audit 🔎

```bash
# Tous les événements
sudo ausearch -k passwd_changes

# Événements d'un utilisateur
sudo ausearch -ua alice

# Événements récents
sudo ausearch -ts today

# Événements entre deux dates
sudo ausearch -ts 10:00 -te 12:00

# Générer un rapport
sudo aureport

# Rapport des modifications de fichiers
sudo aureport -f
```

---

# Rendre les règles d'audit permanentes 💾

**Fichier : `/etc/audit/rules.d/audit.rules`**

```bash
# Surveiller /etc
-w /etc/ -p wa -k etc_changes

# Surveiller sudo
-w /var/log/sudo.log -p wa -k sudo_log

# Surveiller les connexions
-w /var/log/lastlog -p wa -k logins

# Recharger
sudo augenrules --load
```

---

# Détection d'intrusion : AIDE 🚨

**AIDE** : Advanced Intrusion Detection Environment

```bash
# Installer
sudo apt install aide

# Initialiser la base de données
sudo aideinit

# Copier la base de données
sudo mv /var/lib/aide/aide.db.new /var/lib/aide/aide.db

# Vérifier l'intégrité
sudo aide --check

# Mettre à jour la base après changements légitimes
sudo aide --update
```

---

# Configuration AIDE 📝

**Fichier : `/etc/aide/aide.conf`**

```bash
# Surveiller /bin
/bin    p+i+n+u+g+s+b+m+c+md5+sha256

# Surveiller /etc
/etc    p+i+n+u+g+s+b+m+c+md5+sha256

# Ignorer certains fichiers
!/var/log
!/tmp
```

---

# Chiffrement des disques 💿🔒

**LUKS** : Linux Unified Key Setup

```bash
# Chiffrer une partition
sudo cryptsetup luksFormat /dev/sdb1

# Ouvrir une partition chiffrée
sudo cryptsetup luksOpen /dev/sdb1 encrypted_disk

# Le périphérique apparaît dans /dev/mapper/
ls -l /dev/mapper/encrypted_disk

# Créer un système de fichiers
sudo mkfs.ext4 /dev/mapper/encrypted_disk

# Monter
sudo mount /dev/mapper/encrypted_disk /mnt/secure

# Démonter et fermer
sudo umount /mnt/secure
sudo cryptsetup luksClose encrypted_disk
```

---

# Montage automatique de partition chiffrée 🔐

**Fichier : `/etc/crypttab`**

```
encrypted_disk /dev/sdb1 none luks
```

**Fichier : `/etc/fstab`**

```
/dev/mapper/encrypted_disk /mnt/secure ext4 defaults 0 2
```

**Au boot, le système demandera la passphrase**

---

# Chiffrer le home directory 🏠🔒

**ecryptfs** : chiffrement du home

```bash
# Installer
sudo apt install ecryptfs-utils

# Migrer un utilisateur existant
sudo ecryptfs-migrate-home -u alice

# L'utilisateur doit se reconnecter pour chiffrer
```

**Lors de la première connexion après migration :**

```bash
ecryptfs-unwrap-passphrase
```

**⚠️ Noter la passphrase de récupération !**

---

# Limites de ressources : ulimit et cgroups 🚧

**Prévenir les attaques par déni de service**

```bash
# Limite de processus pour un utilisateur
echo "alice hard nproc 100" | sudo tee -a /etc/security/limits.conf

# Limite de fichiers ouverts
echo "alice hard nofile 1024" | sudo tee -a /etc/security/limits.conf

# Limite de mémoire
echo "alice hard as 1000000" | sudo tee -a /etc/security/limits.conf
```

---

# Désactiver les services inutiles 🔌

**Principe : moins de services = moins de surface d'attaque**

```bash
# Lister les services actifs
systemctl list-units --type=service --state=running

# Désactiver un service inutile
sudo systemctl disable --now cups    # Impression
sudo systemctl disable --now bluetooth
sudo systemctl disable --now avahi-daemon    # mDNS

# Vérifier les ports en écoute
sudo ss -tulnp
```

---

# Modules du noyau : désactiver les inutiles 🧠

```bash
# Lister les modules chargés
lsmod

# Désactiver des modules (USB, FireWire si non utilisés)
echo "install usb-storage /bin/true" | sudo tee -a /etc/modprobe.d/disable-usb-storage.conf

# Désactiver IPv6 si non utilisé
echo "net.ipv6.conf.all.disable_ipv6 = 1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

---

# Kernel hardening : sysctl 🛡️

**Fichier : `/etc/sysctl.conf`**

```bash
# Protection contre IP spoofing
net.ipv4.conf.all.rp_filter = 1

# Ignorer les pings (optionnel)
net.ipv4.icmp_echo_ignore_all = 1

# Pas de redirection ICMP
net.ipv4.conf.all.accept_redirects = 0

# Pas de source routing
net.ipv4.conf.all.accept_source_route = 0

# SYN cookies (protection SYN flood)
net.ipv4.tcp_syncookies = 1

# Logs des paquets suspects
net.ipv4.conf.all.log_martians = 1
```

---

# Kernel hardening (suite) 🛡️

```bash
# Randomisation de l'espace d'adressage (ASLR)
kernel.randomize_va_space = 2

# Empêcher l'accès aux dmesg
kernel.dmesg_restrict = 1

# Interdire les core dumps
kernel.core_uses_pid = 1
fs.suid_dumpable = 0

# Appliquer les changements
sudo sysctl -p
```

---

# Vérifier les ports ouverts 🔍

```bash
# Ports en écoute
sudo ss -tulnp

# Avec netstat
sudo netstat -tulnp

# Scanner les ports externes (depuis une autre machine)
nmap -sS target-ip

# Vérifier les connexions établies
sudo ss -tn
```

---

# Détection de rootkits : rkhunter 🕵️

```bash
# Installer
sudo apt install rkhunter

# Mettre à jour la base de données
sudo rkhunter --update

# Scanner le système
sudo rkhunter --check

# Scanner silencieusement
sudo rkhunter --check --sk --nocolors
```

---

# Détection de rootkits : chkrootkit 🕵️

```bash
# Installer
sudo apt install chkrootkit

# Scanner
sudo chkrootkit

# Vérifier seulement certains tests
sudo chkrootkit -l
sudo chkrootkit sniffer
```

---

# Antivirus : ClamAV 🦠

```bash
# Installer
sudo apt install clamav clamav-daemon

# Mettre à jour les signatures
sudo freshclam

# Scanner un répertoire
sudo clamscan -r /home

# Scanner avec suppression des infections
sudo clamscan -r --remove /home

# Scanner en arrière-plan
sudo systemctl enable --now clamav-daemon
```

---

# Sauvegarde et récupération 💾

**3-2-1 Rule :**
- **3** copies de vos données
- **2** supports différents
- **1** copie hors site

```bash
# Sauvegarde avec rsync
rsync -avz --delete /home /backup/

# Sauvegarde distante
rsync -avz --delete /home user@backup-server:/backup/

# Sauvegarde chiffrée avec tar
tar -czf - /home | openssl enc -aes-256-cbc -e > backup.tar.gz.enc
```

---

# Bonnes pratiques de sécurité 📋

1. **Principe du moindre privilège**
   - Uniquement les droits nécessaires

2. **Mises à jour régulières**
   - Automatiser les correctifs de sécurité

3. **Authentification forte**
   - Clés SSH, 2FA

4. **Surveillance et logs**
   - Monitoring actif, alertes

5. **Sauvegardes régulières**
   - Testées et hors site

---

# Bonnes pratiques de sécurité (suite) 📋

6. **Chiffrement**
   - Disques, communications (SSH, HTTPS)

7. **Pare-feu**
   - Bloquer tout sauf nécessaire

8. **Désactiver l'inutile**
   - Services, modules kernel

9. **Audit régulier**
   - Comptes, permissions, ports ouverts

10. **Documentation**
    - Procédures de sécurité, incidents

---

# Checklist de durcissement système ✅

**Utilisateurs et authentification :**
- [ ] Root login désactivé
- [ ] Sudo configuré correctement
- [ ] Politique de mot de passe forte
- [ ] Comptes inutilisés supprimés
- [ ] SSH par clés uniquement

**Réseau :**
- [ ] Pare-feu activé et configuré
- [ ] Services inutiles désactivés
- [ ] Fail2ban installé
- [ ] SSH durci

---

# Checklist de durcissement système (suite) ✅

**Système :**
- [ ] Mises à jour automatiques de sécurité
- [ ] SELinux/AppArmor activé
- [ ] Audit activé (auditd)
- [ ] Modules kernel inutiles désactivés
- [ ] sysctl durci

**Surveillance :**
- [ ] Logs centralisés
- [ ] Monitoring en place
- [ ] Alertes configurées
- [ ] Détection d'intrusion (AIDE, rkhunter)

---

# Ressources et certifications 📚

**Certifications :**
- **CompTIA Security+** : bases de sécurité
- **CEH** : Certified Ethical Hacker
- **OSCP** : Offensive Security Certified Professional
- **GIAC** : diverses spécialisations

**Ressources :**
- CIS Benchmarks (guides de durcissement)
- NIST Cybersecurity Framework
- OWASP (sécurité web)
- CVE/NVD (vulnérabilités)

---

# Récapitulatif du Module 9 ✅

**Ce que vous avez appris :**

✅ Sécurisation des comptes (PAM, sudo, fail2ban)

✅ SELinux et AppArmor (MAC)

✅ Durcissement SSH et 2FA

✅ Audit système (auditd, AIDE)

✅ Chiffrement (LUKS, ecryptfs)

✅ Pare-feu et durcissement réseau

✅ Détection d'intrusion (rkhunter, chkrootkit)

✅ Kernel hardening (sysctl)

✅ Bonnes pratiques et checklist

---

# Prochaine étape 🎯

**Module 9 : Services serveurs**

- Serveur web (Nginx, Apache)
- Serveur SSH
- Serveur DNS
- Serveur de base de données
- Monitoring et supervision
- Haute disponibilité

---
layout: default
---

# Questions ? 🤔

N'hésitez pas à poser vos questions maintenant !

Le prochain module explorera la mise en place et la gestion de services serveurs courants.

