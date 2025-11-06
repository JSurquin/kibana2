---
layout: intro
routeAlias: 'ansible-intro'
---

# Ansible - Introduction 🔧

### Automatisation de l'infrastructure avec Ansible

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Automatisons avec Ansible <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Concepts de base 🔧

### Qu'est-ce qu'Ansible ?

**Définition :**
- Outil d'automatisation d'infrastructure
- Configuration management et déploiement
- Agentless (pas d'agent à installer)
- Utilise SSH pour la communication

**Avantages :**
- **Simplicité** : Syntaxe YAML claire
- **Agentless** : Pas d'installation sur les serveurs
- **Idempotent** : Exécution sécurisée multiple fois
- **Cross-platform** : Support Linux, Windows, macOS

**Composants principaux :**
- **Playbooks** : Fichiers YAML décrivant les tâches
- **Inventory** : Liste des serveurs à gérer
- **Modules** : Actions prédéfinies
- **Roles** : Réutilisabilité des configurations

---
layout: default
---

# Installation et Configuration ⚙️

### Mise en place d'Ansible

**Installation sur Ubuntu :**
```bash
# Ajouter le dépôt Ansible
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible

# Installation
sudo apt install ansible

# Vérifier l'installation
ansible --version
```

**Configuration SSH :**
```bash
# Générer une clé SSH
ssh-keygen -t rsa -b 4096 -C "ansible@example.com"

# Copier la clé sur les serveurs
ssh-copy-id user@server1
ssh-copy-id user@server2

# Tester la connexion
ssh user@server1
```

**Fichier de configuration :**
```bash
# /etc/ansible/ansible.cfg
[defaults]
inventory = /etc/ansible/hosts
remote_user = ubuntu
host_key_checking = False
```

---
layout: default
---

# Inventory (Inventaire) 📋

### Gestion des serveurs cibles

**Fichier inventory simple :**
```ini
# /etc/ansible/hosts
[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com
db2.example.com

[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

**Inventory avec variables :**
```ini
# /etc/ansible/hosts
[webservers]
web1.example.com ansible_host=192.168.1.10
web2.example.com ansible_host=192.168.1.11

[dbservers]
db1.example.com ansible_host=192.168.1.20

[webservers:vars]
http_port=80
max_clients=200

[dbservers:vars]
mysql_port=3306
```

**Inventory en YAML :**
```yaml
# inventory.yml
all:
  children:
    webservers:
      hosts:
        web1.example.com:
          ansible_host: 192.168.1.10
        web2.example.com:
          ansible_host: 192.168.1.11
      vars:
        http_port: 80
    dbservers:
      hosts:
        db1.example.com:
          ansible_host: 192.168.1.20
      vars:
        mysql_port: 3306
```

---
layout: default
---

# Commandes de base 🚀

### Premiers pas avec Ansible

**Test de connectivité :**
```bash
# Tester la connexion à tous les serveurs
ansible all -m ping

# Tester un groupe spécifique
ansible webservers -m ping

# Tester un serveur spécifique
ansible web1.example.com -m ping
```

**Exécution de commandes :**
```bash
# Exécuter une commande sur tous les serveurs
ansible all -m shell -a "uptime"

# Exécuter avec sudo
ansible all -m shell -a "apt update" --become

# Exécuter sur un groupe spécifique
ansible webservers -m shell -a "systemctl status nginx"
```

**Gestion des fichiers :**
```bash
# Copier un fichier
ansible all -m copy -a "src=/local/file dest=/remote/file"

# Créer un répertoire
ansible all -m file -a "path=/tmp/test state=directory"

# Modifier les permissions
ansible all -m file -a "path=/tmp/test mode=755"
```

---
layout: default
---

# Playbooks 📝

### Automatisation avec YAML

**Playbook simple :**
```yaml
# install_nginx.yml
---
- name: Install and configure nginx
  hosts: webservers
  become: yes
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start nginx service
      service:
        name: nginx
        state: started
        enabled: yes
```

**Exécution du playbook :**
```bash
# Exécuter le playbook
ansible-playbook install_nginx.yml

# Exécuter avec verbose
ansible-playbook install_nginx.yml -v

# Exécuter avec check mode
ansible-playbook install_nginx.yml --check

# Exécuter avec limite
ansible-playbook install_nginx.yml --limit web1.example.com
```

---
layout: default
---

# Variables et Templates 📊

### Gestion de la configuration

**Variables dans le playbook :**
```yaml
# configure_web.yml
---
- name: Configure web server
  hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
    server_name: example.com
  tasks:
    - name: Create nginx configuration
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/default
      notify: restart nginx

  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

**Template Jinja2 :**
```jinja2
# nginx.conf.j2
server {
    listen {{ http_port }};
    server_name {{ server_name }};
    
    location / {
        root /var/www/html;
        index index.html;
    }
    
    client_max_body_size {{ max_clients }}M;
}
```

---
layout: default
---

# Roles 🔄

### Réutilisabilité des configurations

**Structure d'un rôle :**
```bash
roles/
├── nginx/
│   ├── defaults/
│   │   └── main.yml
│   ├── handlers/
│   │   └── main.yml
│   ├── tasks/
│   │   └── main.yml
│   ├── templates/
│   │   └── nginx.conf.j2
│   └── vars/
│       └── main.yml
```

**Playbook utilisant un rôle :**
```yaml
# site.yml
---
- name: Configure web servers
  hosts: webservers
  become: yes
  roles:
    - nginx
    - php
    - mysql
```

**Création d'un rôle :**
```bash
# Créer la structure du rôle
ansible-galaxy init nginx

# Ou créer manuellement
mkdir -p roles/nginx/{tasks,handlers,templates,vars,defaults}
```

---
layout: default
---

# Modules Ansible 📦

### Actions prédéfinies

**Modules système :**
```yaml
# Gestion des paquets
- name: Install package
  apt:
    name: nginx
    state: present

# Gestion des services
- name: Start service
  service:
    name: nginx
    state: started
    enabled: yes

# Gestion des fichiers
- name: Create directory
  file:
    path: /var/www/html
    state: directory
    mode: '0755'
```

**Modules réseau :**
```yaml
# Configuration réseau
- name: Configure network interface
  network:
    name: eth0
    state: up
    ip: 192.168.1.100/24

# Configuration firewall
- name: Allow HTTP traffic
  ufw:
    rule: allow
    port: '80'
    proto: tcp
```

---
layout: default
---

# Conditionals et Loops 🔄

### Logique avancée

**Conditions :**
```yaml
- name: Install nginx on Ubuntu
  apt:
    name: nginx
    state: present
  when: ansible_os_family == "Debian"

- name: Install nginx on CentOS
  yum:
    name: nginx
    state: present
  when: ansible_os_family == "RedHat"

- name: Create backup if file exists
  copy:
    src: /etc/nginx/nginx.conf
    dest: /etc/nginx/nginx.conf.backup
  when: ansible_facts.files['/etc/nginx/nginx.conf'] is defined
```

**Boucles :**
```yaml
- name: Install multiple packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - nginx
    - php-fpm
    - mysql-server

- name: Create multiple directories
  file:
    path: "{{ item }}"
    state: directory
  loop:
    - /var/www/html
    - /var/log/nginx
    - /etc/nginx/sites-available
```

---
layout: default
---

# Vault - Sécurité 🔐

### Chiffrement des données sensibles

**Création d'un fichier vault :**
```bash
# Créer un fichier vault
ansible-vault create secret.yml

# Éditer un fichier vault existant
ansible-vault edit secret.yml

# Chiffrer un fichier existant
ansible-vault encrypt secret.yml
```

**Utilisation dans un playbook :**
```yaml
# main.yml
---
- name: Configure database
  hosts: dbservers
  vars_files:
    - secret.yml
  tasks:
    - name: Create database user
      mysql_user:
        name: "{{ db_user }}"
        password: "{{ db_password }}"
        state: present
```

**Exécution avec vault :**
```bash
# Exécuter avec mot de passe
ansible-playbook main.yml --ask-vault-pass

# Exécuter avec fichier de mot de passe
ansible-playbook main.yml --vault-password-file ~/.vault_pass

# Exécuter avec variable d'environnement
ANSIBLE_VAULT_PASSWORD_FILE=~/.vault_pass ansible-playbook main.yml
```

---
layout: default
---

# Ansible Galaxy 🌌

### Partage et réutilisation

**Recherche de rôles :**
```bash
# Rechercher des rôles
ansible-galaxy search nginx

# Voir les détails d'un rôle
ansible-galaxy info geerlingguy.nginx

# Installer un rôle
ansible-galaxy install geerlingguy.nginx
```

**Création et publication :**
```bash
# Initialiser un nouveau rôle
ansible-galaxy init mon_role

# Publier un rôle
ansible-galaxy login
ansible-galaxy publish --api-key=YOUR_API_KEY
```

**Requirements file :**
```yaml
# requirements.yml
---
- src: geerlingguy.nginx
  version: 2.0.0

- src: geerlingguy.php
  version: 1.0.0

- src: https://github.com/user/repo
  version: master
```

---
layout: default
---

# Monitoring et Debugging 🔍

### Surveillance et diagnostic

**Mode verbose :**
```bash
# Différents niveaux de verbosité
ansible-playbook playbook.yml -v
ansible-playbook playbook.yml -vv
ansible-playbook playbook.yml -vvv
ansible-playbook playbook.yml -vvvv
```

**Mode check :**
```bash
# Mode dry-run
ansible-playbook playbook.yml --check

# Mode diff
ansible-playbook playbook.yml --check --diff
```

**Logs et debugging :**
```bash
# Activer les logs
export ANSIBLE_LOG_PATH=/var/log/ansible.log

# Debug avec fact gathering
ansible all -m setup

# Debug avec fact gathering sur un hôte
ansible web1.example.com -m setup
```

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique

**Exercice 1 : Playbook de base**
```yaml
# install_web.yml
---
- name: Install web server
  hosts: webservers
  become: yes
  tasks:
    - name: Update package cache
      apt:
        update_cache: yes

    - name: Install nginx
      apt:
        name: nginx
        state: present

    - name: Start nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

**Exercice 2 : Configuration avec template**
```yaml
# configure_nginx.yml
---
- name: Configure nginx
  hosts: webservers
  become: yes
  vars:
    server_name: example.com
    root_path: /var/www/html
  tasks:
    - name: Create nginx config
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/default
      notify: restart nginx

  handlers:
    - name: restart nginx
      service:
        name: nginx
        state: restarted
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour Ansible

**Organisation :**
- Utiliser des rôles pour la réutilisabilité
- Structurer les playbooks logiquement
- Utiliser des variables pour la flexibilité
- Documenter les playbooks

**Sécurité :**
- Utiliser Ansible Vault pour les secrets
- Limiter les privilèges sudo
- Valider les playbooks avant production
- Utiliser des clés SSH sécurisées

**Performance :**
- Utiliser le mode parallèle
- Optimiser les fact gathering
- Utiliser des connexions persistantes
- Surveiller les temps d'exécution

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Projet final** intégrateur
2. **QCM** de validation
3. **Débriefing** et perspectives
4. **Certification** et ressources
5. **Ressources** et formation continue

**Préparation :**
- Pratiquez les playbooks de base
- Familiarisez-vous avec les rôles
- Testez Ansible Vault
- Explorez Ansible Galaxy

---
src: './pages/15-projet-final.md'
--- 