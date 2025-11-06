---
layout: intro
routeAlias: 'docker-intro'
---

# Docker - Introduction Approfondie 🐳

### Maîtrise de la conteneurisation avec Docker

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Plongeons dans Docker <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Architecture Docker 🏗️

### Composants fondamentaux

**Docker Engine :**
- **Docker daemon** : Service en arrière-plan
- **Docker CLI** : Interface en ligne de commande
- **REST API** : Interface de programmation

**Composants système :**
- **Namespaces** : Isolation des processus
- **cgroups** : Contrôle des ressources
- **Union File System** : Système de fichiers en couches
- **Container Runtime** : Runtime de conteneurs

**Flux de travail :**
```bash
# 1. Client envoie une commande
docker run ubuntu

# 2. Daemon traite la commande
# 3. Création du conteneur
# 4. Exécution du conteneur
```

---
layout: default
---

# Dockerfile Avancé 📝

### Création d'images optimisées

**Structure de base :**
```dockerfile
# Image de base
FROM ubuntu:20.04

# Métadonnées
LABEL maintainer="votre@email.com"
LABEL version="1.0"
LABEL description="Application web"

# Variables d'environnement
ENV DEBIAN_FRONTEND=noninteractive
ENV APP_HOME=/app

# Installation des dépendances
RUN apt update && \
    apt install -y nginx python3 && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

# Répertoire de travail
WORKDIR $APP_HOME

# Copie des fichiers
COPY requirements.txt .
RUN pip3 install -r requirements.txt

COPY . .

# Exposition du port
EXPOSE 80

# Commande de démarrage
CMD ["nginx", "-g", "daemon off;"]
```

---
layout: default
---

# Multi-stage Builds 🏭

### Optimisation des images

**Build multi-étapes :**
```dockerfile
# Étape de build
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Étape de production
FROM node:16-alpine
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY . .

EXPOSE 3000
CMD ["node", "app.js"]
```

**Build avec cache :**
```dockerfile
# Optimisation du cache
FROM ubuntu:20.04

# Copier les fichiers de dépendances d'abord
COPY requirements.txt .
RUN pip install -r requirements.txt

# Puis copier le code source
COPY . .
```

**Avantages :**
- Images plus petites
- Sécurité améliorée
- Builds plus rapides
- Séparation build/runtime

---
layout: default
---

# Docker Compose Avancé 🎼

### Orchestration complexe

**Fichier docker-compose.yml avancé :**
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "80:80"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/myapp
    depends_on:
      - db
    networks:
      - frontend
      - backend
    volumes:
      - ./logs:/var/log/nginx
    restart: unless-stopped

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - backend
    restart: unless-stopped

  redis:
    image: redis:6-alpine
    networks:
      - backend
    restart: unless-stopped

networks:
  frontend:
  backend:
    internal: true

volumes:
  postgres_data:
```

---
layout: default
---

# Gestion des Volumes Avancée 💾

### Stratégies de persistance

**Types de volumes :**
```bash
# Volume nommé
docker volume create mon_volume
docker run -v mon_volume:/data ubuntu

# Bind mount
docker run -v /host/path:/container/path ubuntu

# Volume anonyme
docker run -v /container/path ubuntu

# Volume avec options
docker run -v mon_volume:/data:ro ubuntu  # Lecture seule
```

**Gestion des volumes :**
```bash
# Inspecter un volume
docker volume inspect mon_volume

# Sauvegarder un volume
docker run --rm -v mon_volume:/data -v $(pwd):/backup \
  ubuntu tar czf /backup/volume_backup.tar.gz -C /data .

# Restaurer un volume
docker run --rm -v mon_volume:/data -v $(pwd):/backup \
  ubuntu tar xzf /backup/volume_backup.tar.gz -C /data
```

---
layout: default
---

# Réseaux Docker Avancés 🌐

### Configuration réseau complexe

**Types de réseaux :**
```bash
# Réseau bridge (défaut)
docker network create --driver bridge mon_reseau

# Réseau host
docker run --network host nginx

# Réseau none
docker run --network none ubuntu

# Réseau overlay (Swarm)
docker network create --driver overlay mon_overlay
```

**Configuration réseau personnalisée :**
```bash
# Réseau avec sous-réseau
docker network create \
  --driver bridge \
  --subnet=172.20.0.0/16 \
  --ip-range=172.20.5.0/24 \
  --gateway=172.20.5.254 \
  mon_reseau_personnalise
```

**DNS et résolution :**
```bash
# Conteneur avec DNS personnalisé
docker run --dns 8.8.8.8 --dns 8.8.4.4 ubuntu

# Réseau avec DNS interne
docker network create --driver bridge \
  --opt com.docker.network.bridge.name=docker0 \
  mon_reseau_dns
```

---
layout: default
---

# Sécurité Docker Avancée 🔒

### Hardening des conteneurs

**Exécution non-privilégiée :**
```bash
# Utilisateur non-root
docker run --user 1000:1000 ubuntu

# Limitation des capacités
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx

# Mode read-only
docker run --read-only ubuntu
```

**Isolation réseau :**
```bash
# Réseau interne uniquement
docker run --network none ubuntu

# Limitation des ports
docker run -p 127.0.0.1:8080:80 nginx

# Sécurité des réseaux
docker network create --internal reseau_interne
```

**Scan de sécurité :**
```bash
# Scanner avec Docker Scout
docker scout cves mon_image

# Scanner avec Trivy
trivy image mon_image

# Audit de sécurité
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  aquasec/trivy image mon_image
```

---
layout: default
---

# Monitoring et Observabilité 📊

### Surveillance avancée

**Statistiques en temps réel :**
```bash
# Statistiques détaillées
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}"

# Statistiques d'un conteneur spécifique
docker stats mon_conteneur

# Monitoring avec cAdvisor
docker run -d \
  --name=cadvisor \
  --publish=8080:8080 \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  gcr.io/cadvisor/cadvisor
```

**Logs structurés :**
```bash
# Logs avec timestamps
docker logs -t mon_conteneur

# Logs depuis un timestamp
docker logs --since="2025-01-01T00:00:00" mon_conteneur

# Logs avec format JSON
docker logs --format json mon_conteneur
```

---
layout: default
---

# Performance et Optimisation ⚡

### Optimisation des conteneurs

**Optimisation des images :**
```dockerfile
# Image multi-stage optimisée
FROM golang:1.19 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/main .
CMD ["./main"]
```

**Optimisation des ressources :**
```bash
# Limitation de mémoire
docker run -m 512m ubuntu

# Limitation de CPU
docker run --cpus=2 ubuntu

# Limitation de stockage
docker run --storage-opt size=10G ubuntu
```

**Monitoring des performances :**
```bash
# Utilisation des ressources
docker system df

# Analyse des couches
docker history mon_image

# Optimisation des images
docker build --no-cache .
```

---
layout: default
---

# CI/CD avec Docker 🔄

### Intégration continue

**Pipeline CI/CD :**
```yaml
# .github/workflows/docker.yml
name: Docker CI/CD

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    
    - name: Build Docker image
      run: docker build -t mon_app .
    
    - name: Run tests
      run: docker run mon_app npm test
    
    - name: Push to registry
      run: |
        docker tag mon_app registry.com/mon_app:${{ github.sha }}
        docker push registry.com/mon_app:${{ github.sha }}
```

**Tests automatisés :**
```bash
# Tests unitaires dans le conteneur
docker run --rm mon_app npm test

# Tests d'intégration
docker-compose -f docker-compose.test.yml up --abort-on-container-exit

# Tests de sécurité
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock \
  aquasec/trivy image mon_app
```

---
layout: default
---

# Registres Docker 📦

### Gestion des images

**Docker Hub :**
```bash
# Connexion à Docker Hub
docker login

# Pousser une image
docker tag mon_app username/mon_app
docker push username/mon_app

# Tirer une image
docker pull username/mon_app
```

**Registre privé :**
```bash
# Déployer un registre privé
docker run -d \
  --name registry \
  -p 5000:5000 \
  -v registry_data:/var/lib/registry \
  registry:2

# Pousser vers le registre privé
docker tag mon_app localhost:5000/mon_app
docker push localhost:5000/mon_app
```

**Harbor (registre d'entreprise) :**
```bash
# Installation Harbor
curl -L https://github.com/goharbor/harbor/releases/download/v2.5.0/harbor-offline-installer-v2.5.0.tgz | tar xz
cd harbor
./prepare
./install.sh
```

---
layout: default
---

# Docker Swarm 🐝

### Orchestration native Docker

**Initialisation du swarm :**
```bash
# Initialiser le swarm
docker swarm init

# Ajouter des nœuds workers
docker swarm join --token SWMTKN-1-... manager_ip:2377

# Voir les nœuds
docker node ls
```

**Services Swarm :**
```bash
# Créer un service
docker service create --name web --replicas 3 nginx

# Mettre à jour un service
docker service update --image nginx:alpine web

# Voir les services
docker service ls
docker service ps web
```

**Stack avec docker-compose :**
```yaml
# docker-compose.yml pour Swarm
version: '3.8'
services:
  web:
    image: nginx
    deploy:
      replicas: 3
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
```

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique avancée

**Exercice 1 : Application multi-conteneurs**
```bash
# Créer une application web + base de données
mkdir mon_app && cd mon_app

# docker-compose.yml
cat > docker-compose.yml << EOF
version: '3.8'
services:
  web:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/myapp
  
  db:
    image: postgres:13
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
EOF

# Démarrer l'application
docker-compose up -d
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour Docker en production

**Sécurité :**
- Utiliser des images de base minimales
- Scanner régulièrement les vulnérabilités
- Limiter les privilèges des conteneurs
- Utiliser des secrets pour les données sensibles

**Performance :**
- Optimiser la taille des images
- Utiliser le cache Docker efficacement
- Surveiller l'utilisation des ressources
- Implémenter des health checks

**Maintenance :**
- Taguer les images avec des versions
- Nettoyer régulièrement les ressources
- Documenter les Dockerfiles
- Automatiser les builds et déploiements

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Ansible** introduction
2. **Projet final** intégrateur
3. **QCM** de validation
4. **Débriefing** et perspectives
5. **Certification** et ressources

**Préparation :**
- Pratiquez les Dockerfiles avancés
- Testez Docker Compose
- Explorez les registres privés
- Familiarisez-vous avec Swarm

---
src: './pages/14-ansible-intro.md'
--- 