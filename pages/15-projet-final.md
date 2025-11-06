---
layout: intro
routeAlias: 'projet-final'
---

# Projet Final - Infrastructure Complète 🏗️

### Mise en pratique de tous les concepts appris

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Réalisons notre projet final <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Objectifs du Projet 🎯

### Infrastructure complète et automatisée

**Objectifs principaux :**
- Déployer une application web complète
- Automatiser l'infrastructure avec Ansible
- Conteneuriser l'application avec Docker
- Sécuriser l'environnement
- Monitorer les services

**Technologies utilisées :**
- **Linux** : Ubuntu Server 20.04
- **Docker** : Conteneurisation
- **Ansible** : Automatisation
- **Nginx** : Serveur web
- **MySQL** : Base de données
- **Monitoring** : Prometheus + Grafana

**Architecture cible :**
```
[Internet] → [Load Balancer] → [Web Servers] → [Database]
                    ↓
            [Monitoring Stack]
```

---
layout: default
---

# Architecture du Projet 🏗️

### Vue d'ensemble de l'infrastructure

**Composants de l'infrastructure :**
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Load Balancer │    │   Web Server 1  │    │   Web Server 2  │
│   (Nginx)       │───▶│   (Docker)      │    │   (Docker)      │
│   Port 80       │    │   Port 3000     │    │   Port 3000     │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 ▼
                    ┌─────────────────┐
                    │   Database      │
                    │   (MySQL)       │
                    │   Port 3306     │
                    └─────────────────┘
                                 │
                                 ▼
                    ┌─────────────────┐
                    │   Monitoring    │
                    │   (Prometheus)  │
                    │   Port 9090     │
                    └─────────────────┘
```

---
layout: default
---

# Préparation de l'Environnement 🔧

### Configuration initiale

**Machines virtuelles requises :**
```bash
# 1. Serveur Load Balancer
# - Ubuntu Server 20.04
# - 2 vCPU, 2GB RAM, 20GB disque
# - IP: 192.168.1.10

# 2. Serveur Web 1
# - Ubuntu Server 20.04
# - 2 vCPU, 2GB RAM, 20GB disque
# - IP: 192.168.1.11

# 3. Serveur Web 2
# - Ubuntu Server 20.04
# - 2 vCPU, 2GB RAM, 20GB disque
# - IP: 192.168.1.12

# 4. Serveur Database
# - Ubuntu Server 20.04
# - 2 vCPU, 4GB RAM, 50GB disque
# - IP: 192.168.1.13

# 5. Serveur Monitoring
# - Ubuntu Server 20.04
# - 2 vCPU, 2GB RAM, 20GB disque
# - IP: 192.168.1.14
```

**Configuration réseau :**
```bash
# Tous les serveurs sur le même réseau
# Gateway: 192.168.1.1
# DNS: 8.8.8.8, 8.8.4.4
```

---
layout: default
---

# Application Web 📱

### Développement de l'application

**Structure de l'application :**
```bash
mon_app/
├── app/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── Dockerfile
├── docker-compose.yml
└── requirements.txt
```

**Application simple (index.html) :**
```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mon Application</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Bienvenue sur mon application</h1>
        <p>Serveur: <span id="server-info"></span></p>
        <p>Date: <span id="current-time"></span></p>
        <button onclick="refreshData()">Actualiser</button>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

---
layout: default
---

# Dockerfile de l'Application 🐳

### Conteneurisation de l'application

**Dockerfile optimisé :**
```dockerfile
# Étape de build
FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Étape de production
FROM nginx:alpine
COPY --from=builder /app/node_modules ./node_modules
COPY app/ /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf

# Configuration nginx
RUN echo "server { \
    listen 80; \
    location / { \
        root /usr/share/nginx/html; \
        index index.html; \
    } \
    location /api { \
        proxy_pass http://backend:3000; \
    } \
}" > /etc/nginx/conf.d/default.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**docker-compose.yml :**
```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "3000:80"
    environment:
      - NODE_ENV=production
    restart: unless-stopped
```

---
layout: default
---

# Ansible Inventory 📋

### Configuration des serveurs

**inventory.yml :**
```yaml
all:
  children:
    loadbalancers:
      hosts:
        lb1:
          ansible_host: 192.168.1.10
          ansible_user: ubuntu
    webservers:
      hosts:
        web1:
          ansible_host: 192.168.1.11
          ansible_user: ubuntu
        web2:
          ansible_host: 192.168.1.12
          ansible_user: ubuntu
    databases:
      hosts:
        db1:
          ansible_host: 192.168.1.13
          ansible_user: ubuntu
    monitoring:
      hosts:
        monitor1:
          ansible_host: 192.168.1.14
          ansible_user: ubuntu
  vars:
    ansible_ssh_private_key_file: ~/.ssh/id_rsa
    ansible_python_interpreter: /usr/bin/python3
```

---
layout: default
---

# Playbook Principal 🎼

### Automatisation complète

**site.yml :**
```yaml
---
- name: Configure all servers
  hosts: all
  become: yes
  tasks:
    - name: Update package cache
      apt:
        update_cache: yes

    - name: Install common packages
      apt:
        name:
          - curl
          - wget
          - htop
          - vim
        state: present

- name: Configure load balancer
  hosts: loadbalancers
  become: yes
  roles:
    - nginx-lb

- name: Configure web servers
  hosts: webservers
  become: yes
  roles:
    - docker
    - web-app

- name: Configure database
  hosts: databases
  become: yes
  roles:
    - mysql

- name: Configure monitoring
  hosts: monitoring
  become: yes
  roles:
    - prometheus
    - grafana
```

---
layout: default
---

# Rôle Load Balancer 🔄

### Configuration Nginx Load Balancer

**roles/nginx-lb/tasks/main.yml :**
```yaml
---
- name: Install nginx
  apt:
    name: nginx
    state: present

- name: Create nginx configuration
  template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: restart nginx

- name: Start nginx service
  service:
    name: nginx
    state: started
    enabled: yes
```

**roles/nginx-lb/templates/nginx.conf.j2 :**
```nginx
events {
    worker_connections 1024;
}

http {
    upstream backend {
        server {{ hostvars['web1']['ansible_host'] }}:3000;
        server {{ hostvars['web2']['ansible_host'] }}:3000;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}
```

---
layout: default
---

# Rôle Web Application 🌐

### Déploiement de l'application

**roles/web-app/tasks/main.yml :**
```yaml
---
- name: Install Docker
  apt:
    name:
      - docker.io
      - docker-compose
    state: present

- name: Add user to docker group
  user:
    name: ubuntu
    groups: docker
    append: yes

- name: Create application directory
  file:
    path: /opt/mon_app
    state: directory
    mode: '0755'

- name: Copy application files
  copy:
    src: "{{ playbook_dir }}/mon_app/"
    dest: /opt/mon_app/
    mode: '0644'

- name: Build and start application
  docker_compose:
    project_src: /opt/mon_app
    state: present
```

---
layout: default
---

# Rôle Database 🗄️

### Configuration MySQL

**roles/mysql/tasks/main.yml :**
```yaml
---
- name: Install MySQL
  apt:
    name: mysql-server
    state: present

- name: Start MySQL service
  service:
    name: mysql
    state: started
    enabled: yes

- name: Create database
  mysql_db:
    name: mon_app
    state: present
    login_user: root
    login_password: "{{ mysql_root_password }}"

- name: Create database user
  mysql_user:
    name: app_user
    password: "{{ mysql_app_password }}"
    priv: 'mon_app.*:ALL'
    host: '%'
    state: present
    login_user: root
    login_password: "{{ mysql_root_password }}"
```

---
layout: default
---

# Rôle Monitoring 📊

### Configuration Prometheus et Grafana

**roles/prometheus/tasks/main.yml :**
```yaml
---
- name: Create prometheus user
  user:
    name: prometheus
    system: yes
    shell: /sbin/nologin

- name: Create prometheus directories
  file:
    path: "{{ item }}"
    state: directory
    owner: prometheus
    group: prometheus
  loop:
    - /etc/prometheus
    - /var/lib/prometheus

- name: Download Prometheus
  get_url:
    url: https://github.com/prometheus/prometheus/releases/download/v2.37.0/prometheus-2.37.0.linux-amd64.tar.gz
    dest: /tmp/prometheus.tar.gz

- name: Extract Prometheus
  unarchive:
    src: /tmp/prometheus.tar.gz
    dest: /tmp/
    remote_src: yes

- name: Copy Prometheus files
  copy:
    src: /tmp/prometheus-2.37.0.linux-amd64/{{ item }}
    dest: /usr/local/bin/{{ item }}
    mode: '0755'
    remote_src: yes
  loop:
    - prometheus
    - promtool
```

---
layout: default
---

# Configuration Prometheus 📈

### Monitoring des services

**roles/prometheus/templates/prometheus.yml.j2 :**
```yaml
global:
  scrape_interval: 15s

rule_files:
  - "first_rules.yml"

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'nginx'
    static_configs:
      - targets: ['{{ hostvars['lb1']['ansible_host'] }}:80']

  - job_name: 'web-app'
    static_configs:
      - targets: 
        - '{{ hostvars['web1']['ansible_host'] }}:3000'
        - '{{ hostvars['web2']['ansible_host'] }}:3000'

  - job_name: 'mysql'
    static_configs:
      - targets: ['{{ hostvars['db1']['ansible_host'] }}:3306']
```

---
layout: default
---

# Rôle Grafana 📊

### Interface de monitoring

**roles/grafana/tasks/main.yml :**
```yaml
---
- name: Add Grafana GPG key
  apt_key:
    url: https://packages.grafana.com/gpg.key
    state: present

- name: Add Grafana repository
  apt_repository:
    repo: "deb https://packages.grafana.com/oss/deb stable main"
    state: present

- name: Install Grafana
  apt:
    name: grafana
    state: present
    update_cache: yes

- name: Start Grafana service
  service:
    name: grafana-server
    state: started
    enabled: yes

- name: Create Grafana admin user
  uri:
    url: http://localhost:3000/api/admin/users
    method: POST
    body_format: json
    body: '{"name":"admin","email":"admin@example.com","login":"admin","password":"admin"}'
    status_code: 200
  register: grafana_user
  until: grafana_user.status == 200
  retries: 10
  delay: 5
```

---
layout: default
---

# Scripts de Déploiement 🚀

### Automatisation complète

**deploy.sh :**
```bash
#!/bin/bash
set -e

echo "🚀 Démarrage du déploiement..."

# Vérifications préalables
echo "📋 Vérifications préalables..."
if ! command -v ansible &> /dev/null; then
    echo "❌ Ansible n'est pas installé"
    exit 1
fi

# Test de connectivité
echo "🔍 Test de connectivité..."
ansible all -m ping

# Déploiement
echo "📦 Déploiement de l'infrastructure..."
ansible-playbook site.yml

# Vérification
echo "✅ Vérification du déploiement..."
ansible all -m shell -a "systemctl is-active --quiet nginx || systemctl is-active --quiet docker || systemctl is-active --quiet mysql || systemctl is-active --quiet grafana-server"

echo "🎉 Déploiement terminé avec succès!"
echo "📊 Accès aux services :"
echo "   - Application: http://192.168.1.10"
echo "   - Grafana: http://192.168.1.14:3000"
echo "   - Prometheus: http://192.168.1.14:9090"
```

---
layout: default
---

# Tests et Validation 🧪

### Vérification du déploiement

**test_deployment.sh :**
```bash
#!/bin/bash
echo "🧪 Tests de validation..."

# Test de connectivité
echo "1. Test de connectivité..."
for host in 192.168.1.{10..14}; do
    if ping -c 1 $host &> /dev/null; then
        echo "✅ $host accessible"
    else
        echo "❌ $host inaccessible"
    fi
done

# Test des services web
echo "2. Test des services web..."
curl -f http://192.168.1.10 > /dev/null && echo "✅ Load balancer OK" || echo "❌ Load balancer KO"
curl -f http://192.168.1.11:3000 > /dev/null && echo "✅ Web1 OK" || echo "❌ Web1 KO"
curl -f http://192.168.1.12:3000 > /dev/null && echo "✅ Web2 OK" || echo "❌ Web2 KO"

# Test de la base de données
echo "3. Test de la base de données..."
ansible db1 -m shell -a "mysql -u root -p'password' -e 'SHOW DATABASES;'" && echo "✅ Database OK" || echo "❌ Database KO"

# Test du monitoring
echo "4. Test du monitoring..."
curl -f http://192.168.1.14:9090 > /dev/null && echo "✅ Prometheus OK" || echo "❌ Prometheus KO"
curl -f http://192.168.1.14:3000 > /dev/null && echo "✅ Grafana OK" || echo "❌ Grafana KO"
```

---
layout: default
---

# Monitoring et Alertes 📊

### Surveillance de l'infrastructure

**Configuration des alertes :**
```yaml
# roles/prometheus/templates/alerts.yml.j2
groups:
  - name: infrastructure_alerts
    rules:
      - alert: HighCPUUsage
        expr: 100 - (avg by(instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "CPU usage high on {{ $labels.instance }}"
          description: "CPU usage is above 80% for 5 minutes"

      - alert: HighMemoryUsage
        expr: (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100 > 85
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Memory usage high on {{ $labels.instance }}"
          description: "Memory usage is above 85% for 5 minutes"

      - alert: ServiceDown
        expr: up == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Service down on {{ $labels.instance }}"
          description: "Service {{ $labels.job }} is down"
```

---
layout: default
---

# Sécurité de l'Infrastructure 🔒

### Hardening des serveurs

**roles/security/tasks/main.yml :**
```yaml
---
- name: Configure firewall
  ufw:
    state: enabled
    policy: deny
    rule: allow
    port: "{{ item }}"
    proto: tcp
  loop:
    - "22"    # SSH
    - "80"    # HTTP
    - "443"   # HTTPS
    - "3306"  # MySQL
    - "9090"  # Prometheus
    - "3000"  # Grafana

- name: Disable root login
  lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^PermitRootLogin'
    line: 'PermitRootLogin no'
  notify: restart ssh

- name: Configure password policy
  pam_pwquality:
    name: common-password
    local_users_only: yes
    minlen: 12
    minclass: 3

- name: Install fail2ban
  apt:
    name: fail2ban
    state: present

- name: Configure fail2ban
  template:
    src: jail.local.j2
    dest: /etc/fail2ban/jail.local
  notify: restart fail2ban
```

---
layout: default
---

# Sauvegarde et Récupération 💾

### Stratégie de sauvegarde

**roles/backup/tasks/main.yml :**
```yaml
---
- name: Create backup directory
  file:
    path: /opt/backups
    state: directory
    mode: '0755'

- name: Backup MySQL databases
  mysql_dump:
    login_user: root
    login_password: "{{ mysql_root_password }}"
    databases: mon_app
    dest: "/opt/backups/mysql_$(date +%Y%m%d_%H%M%S).sql"
  when: "'databases' in group_names"

- name: Backup application data
  archive:
    path: /opt/mon_app
    dest: "/opt/backups/app_$(date +%Y%m%d_%H%M%S).tar.gz"
    format: gz
  when: "'webservers' in group_names"

- name: Backup configuration files
  archive:
    path: /etc/nginx,/etc/mysql,/etc/prometheus
    dest: "/opt/backups/config_$(date +%Y%m%d_%H%M%S).tar.gz"
    format: gz

- name: Clean old backups
  file:
    path: "/opt/backups/{{ item }}"
    state: absent
  loop:
    - "mysql_*.sql"
    - "app_*.tar.gz"
    - "config_*.tar.gz"
  when: ansible_date_time.weekday == "Sunday"
```

---
layout: default
---

# Documentation et Maintenance 📚

### Documentation du projet

**README.md :**
```markdown
# Infrastructure Complète - Projet Final

## Architecture
- Load Balancer (Nginx) : 192.168.1.10
- Web Server 1 (Docker) : 192.168.1.11
- Web Server 2 (Docker) : 192.168.1.12
- Database (MySQL) : 192.168.1.13
- Monitoring (Prometheus/Grafana) : 192.168.1.14

## Déploiement
```bash
# Cloner le projet
git clone <repository>
cd infrastructure-project

# Configurer les clés SSH
ssh-copy-id ubuntu@192.168.1.10
ssh-copy-id ubuntu@192.168.1.11
ssh-copy-id ubuntu@192.168.1.12
ssh-copy-id ubuntu@192.168.1.13
ssh-copy-id ubuntu@192.168.1.14

# Déployer
./deploy.sh
```

## Monitoring
- Grafana : http://192.168.1.14:3000 (admin/admin)
- Prometheus : http://192.168.1.14:9090

## Maintenance
- Sauvegardes automatiques : Dimanche 2h00
- Logs : /var/log/ansible/
- Configuration : /etc/ansible/
```

---
layout: default
---

# Évaluation et Validation ✅

### Critères d'évaluation

**Fonctionnalités requises :**
- ✅ Infrastructure complète déployée
- ✅ Application web accessible
- ✅ Load balancing fonctionnel
- ✅ Base de données opérationnelle
- ✅ Monitoring en place
- ✅ Sécurité configurée
- ✅ Sauvegardes automatisées

**Tests de performance :**
```bash
# Test de charge
ab -n 1000 -c 10 http://192.168.1.10/

# Test de disponibilité
for i in {1..100}; do
    curl -f http://192.168.1.10/ || echo "Échec $i"
done

# Test de récupération
# Simuler une panne et vérifier la récupération
```

**Métriques de succès :**
- Temps de déploiement < 30 minutes
- Disponibilité > 99.9%
- Temps de réponse < 200ms
- Utilisation CPU < 80%
- Utilisation mémoire < 85%

---
layout: default
---

# Prochaines étapes 🎯

### Évolution du projet

**Améliorations possibles :**
- **Kubernetes** : Orchestration avancée
- **Terraform** : Infrastructure as Code
- **CI/CD** : Pipeline automatisé
- **Microservices** : Architecture distribuée
- **Cloud** : Migration vers AWS/Azure/GCP

**Formation continue :**
- **Certifications** : Linux Foundation, Red Hat
- **Communautés** : Meetups, conférences
- **Projets open source** : Contributions
- **Veille technologique** : Blogs, podcasts

**Ressources recommandées :**
- **Livres** : "The Phoenix Project", "Site Reliability Engineering"
- **Cours** : Coursera, edX, Linux Academy
- **Pratique** : Labs virtuels, projets personnels
- **Réseau** : LinkedIn, GitHub, Stack Overflow

---
src: './pages/16-qcm-validation.md'
--- 