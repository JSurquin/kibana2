---

# 🎯 Exercices pratiques - Module 9

**Sécurité système**

---

# Exercice 1 : Durcissement SSH 🔒

**Objectif :** Sécuriser SSH

**Consignes :**

1. Désactivez l'authentification root
2. Changez le port SSH (2222)
3. Désactivez l'authentification par mot de passe
4. Limitez les utilisateurs autorisés
5. Activez fail2ban pour SSH
6. Testez la configuration

---

# Solution Exercice 1 💡

```bash
# 1-4. Configuration SSH
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup

sudo tee -a /etc/ssh/sshd_config << 'EOF'

# Configuration sécurisée
Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers alice bob
MaxAuthTries 3
ClientAliveInterval 300
ClientAliveCountMax 2
EOF

# Tester la config
sudo sshd -t

# Redémarrer
sudo systemctl restart sshd

# 5. Installer fail2ban
sudo apt install fail2ban -y

# Configurer
sudo tee /etc/fail2ban/jail.local << 'EOF'
[sshd]
enabled = true
port = 2222
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600
findtime = 600
EOF

sudo systemctl restart fail2ban

# 6. Tester
# Depuis une autre machine
ssh -p 2222 alice@serveur

# Vérifier fail2ban
sudo fail2ban-client status sshd
```

---

# Exercice 2 : Audit des permissions 🔍

**Objectif :** Trouver les fichiers à risque

**Consignes :**

1. Trouvez tous les fichiers world-writable
2. Trouvez tous les fichiers SUID
3. Trouvez tous les fichiers SGID
4. Trouvez les fichiers sans propriétaire
5. Générez un rapport
6. Corrigez les problèmes critiques

---

# Solution Exercice 2 💡

```bash
#!/bin/bash
# Audit de sécurité des permissions

REPORT="/tmp/security_audit_$(date +%Y%m%d).txt"

echo "===== AUDIT SÉCURITÉ =====" > $REPORT
echo "Date: $(date)" >> $REPORT
echo >> $REPORT

# 1. Fichiers world-writable
echo "🔓 FICHIERS WORLD-WRITABLE:" >> $REPORT
find / -type f -perm -002 ! -path "/proc/*" ! -path "/sys/*" \
    2>/dev/null | tee -a $REPORT
echo >> $REPORT

# 2. Fichiers SUID
echo "⚠️  FICHIERS SUID:" >> $REPORT
find / -type f -perm -4000 2>/dev/null | tee -a $REPORT
echo >> $REPORT

# 3. Fichiers SGID
echo "⚠️  FICHIERS SGID:" >> $REPORT
find / -type f -perm -2000 2>/dev/null | tee -a $REPORT
echo >> $REPORT

# 4. Fichiers sans propriétaire
echo "❓ FICHIERS ORPHELINS:" >> $REPORT
find / -nouser -o -nogroup 2>/dev/null | tee -a $REPORT
echo >> $REPORT

# Vérifier /etc/passwd
echo "👤 COMPTES SYSTÈME:" >> $REPORT
awk -F: '$3 == 0 {print $1}' /etc/passwd | tee -a $REPORT
echo >> $REPORT

cat $REPORT

# 6. Corrections
echo "Corrections recommandées..."

# Retirer world-writable (exemple)
# find / -type f -perm -002 -exec chmod o-w {} \; 2>/dev/null
```

---

# Exercice 3 : Configuration de fail2ban 🛡️

**Objectif :** Protéger contre le brute-force

**Consignes :**

1. Configurez fail2ban pour SSH, HTTP et FTP
2. Définissez des règles strictes
3. Testez en générant des échecs
4. Vérifiez les IPs bannies
5. Débannissez une IP
6. Consultez les logs

---

# Solution Exercice 3 💡

```bash
# 1 & 2. Configuration
sudo tee /etc/fail2ban/jail.local << 'EOF'
[DEFAULT]
bantime = 3600
findtime = 600
maxretry = 3
destemail = admin@example.com
action = %(action_mwl)s

[sshd]
enabled = true
port = ssh
logpath = /var/log/auth.log
maxretry = 3

[nginx-http-auth]
enabled = true
port = http,https
logpath = /var/log/nginx/error.log

[nginx-noscript]
enabled = true
port = http,https
logpath = /var/log/nginx/access.log
maxretry = 6

[nginx-badbots]
enabled = true
port = http,https
logpath = /var/log/nginx/access.log
maxretry = 2
EOF

sudo systemctl restart fail2ban

# 4. Voir les IPs bannies
sudo fail2ban-client status
sudo fail2ban-client status sshd

# 5. Débannir une IP
sudo fail2ban-client set sshd unbanip 192.168.1.100

# 6. Logs
sudo tail -f /var/log/fail2ban.log

# Tester (depuis une autre machine)
# Essayez de vous connecter plusieurs fois avec un mauvais mot de passe
# ssh user@serveur  (3 fois avec mauvais mdp)
```

---

# Exercice 4 : Chiffrement avec LUKS 🔐

**Objectif :** Chiffrer un volume

**Consignes :**

1. Créez une image disque de 100 Mo
2. Chiffrez-la avec LUKS
3. Ouvrez le volume chiffré
4. Formatez et montez
5. Créez des fichiers sensibles
6. Fermez et testez la sécurité

---

# Solution Exercice 4 💡

```bash
# 1. Créer image
dd if=/dev/zero of=/tmp/encrypted.img bs=1M count=100

# 2. Chiffrer avec LUKS
sudo cryptsetup luksFormat /tmp/encrypted.img
# Entrez un mot de passe fort

# 3. Ouvrir le volume
sudo cryptsetup luksOpen /tmp/encrypted.img encrypted_vol

# 4. Formater
sudo mkfs.ext4 /dev/mapper/encrypted_vol

# Monter
sudo mkdir /mnt/encrypted
sudo mount /dev/mapper/encrypted_vol /mnt/encrypted

# 5. Créer des fichiers
echo "Données confidentielles" | sudo tee /mnt/encrypted/secret.txt
sudo ls -la /mnt/encrypted/

# 6. Fermer
sudo umount /mnt/encrypted
sudo cryptsetup luksClose encrypted_vol

# Tester : impossible de lire sans le mot de passe !
# sudo mount /tmp/encrypted.img /mnt/test  # Échec

# Réouvrir
sudo cryptsetup luksOpen /tmp/encrypted.img encrypted_vol
# Entrez le mot de passe
sudo mount /dev/mapper/encrypted_vol /mnt/encrypted
cat /mnt/encrypted/secret.txt

# Nettoyage
sudo umount /mnt/encrypted
sudo cryptsetup luksClose encrypted_vol
rm /tmp/encrypted.img
```

---

# Exercice 5 : Audit avec auditd 📝

**Objectif :** Surveiller les actions système

**Consignes :**

1. Installez auditd
2. Surveillez les modifications de `/etc/passwd`
3. Surveillez les commandes sudo
4. Générez des événements
5. Consultez les rapports
6. Recherchez des événements spécifiques

---

# Solution Exercice 5 💡

```bash
# 1. Installer
sudo apt install auditd audispd-plugins -y

# 2. Règle pour /etc/passwd
sudo auditctl -w /etc/passwd -p wa -k passwd_changes

# 3. Règle pour sudo
sudo auditctl -w /var/log/sudo.log -p wa -k sudo_log

# Règles pour exécutions
sudo auditctl -a always,exit -F arch=b64 -S execve -k exec_commands

# Voir les règles
sudo auditctl -l

# Rendre permanent
sudo tee -a /etc/audit/rules.d/custom.rules << 'EOF'
-w /etc/passwd -p wa -k passwd_changes
-w /etc/shadow -p wa -k shadow_changes
-w /etc/sudoers -p wa -k sudoers_changes
-w /var/log/sudo.log -p wa -k sudo_log
EOF

# 4. Générer des événements
sudo useradd testuser
sudo userdel testuser
sudo cat /etc/shadow > /dev/null

# 5. Consulter les rapports
sudo ausearch -k passwd_changes
sudo ausearch -k sudo_log

# Par date
sudo ausearch -ts today

# 6. Recherche spécifique
sudo ausearch -ua alice
sudo ausearch -x /usr/bin/sudo

# Rapport détaillé
sudo aureport --summary
sudo aureport --auth
sudo aureport --file
```

---

# Exercice bonus : Script de hardening automatique 🛡️

**Objectif :** Script de sécurisation

**Créez un script qui :**
- Désactive les services inutiles
- Configure le pare-feu
- Sécurise SSH
- Met en place fail2ban
- Configure les mises à jour auto
- Génère un rapport

---

# Solution Exercice bonus 💡

```bash
#!/bin/bash
# Script de hardening système

if [ "$EUID" -ne 0 ]; then
    echo "Doit être exécuté en root"
    exit 1
fi

echo "===== HARDENING SYSTÈME ====="

# 1. Désactiver services inutiles
echo "Désactivation services inutiles..."
for svc in bluetooth cups avahi-daemon; do
    systemctl disable $svc 2>/dev/null
    systemctl stop $svc 2>/dev/null
done

# 2. Pare-feu
echo "Configuration pare-feu..."
ufw --force enable
ufw default deny incoming
ufw default allow outgoing
ufw allow 22/tcp
ufw allow 80/tcp
ufw allow 443/tcp

# 3. SSH sécurisé
echo "Sécurisation SSH..."
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config
systemctl restart sshd

# 4. Fail2ban
echo "Installation fail2ban..."
apt install fail2ban -y
systemctl enable fail2ban
systemctl start fail2ban

# 5. Mises à jour auto
echo "Configuration mises à jour automatiques..."
apt install unattended-upgrades -y
dpkg-reconfigure -plow unattended-upgrades

# 6. Permissions /tmp
echo "Sécurisation /tmp..."
chmod 1777 /tmp

# Rapport
echo "===== RAPPORT ====="
echo "✅ Services désactivés"
echo "✅ Pare-feu configuré"
echo "✅ SSH sécurisé"
echo "✅ Fail2ban actif"
echo "✅ Mises à jour auto activées"
echo "==================="
echo "Hardening terminé !"
```

---

# Points clés à retenir 📌

**SSH :**
- Désactiver root login
- Clés au lieu de mots de passe
- Changer le port par défaut
- fail2ban pour protection

**Audits :**
- Fichiers SUID/SGID à surveiller
- World-writable dangereux
- auditd pour traçabilité

**Chiffrement :**
- LUKS pour volumes
- GPG pour fichiers
- SSH pour communications

---

