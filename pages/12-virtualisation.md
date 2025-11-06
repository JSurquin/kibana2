---
layout: intro
routeAlias: 'virtualisation'
---

# Virtualisation et Conteneurs 🖥️

### Concepts de virtualisation et conteneurisation

<div class="pt-12">
  <span @click="next" class="px-2 p-3 rounded cursor-pointer hover:bg-white hover:bg-opacity-10 neon-border">
    Explorons la virtualisation <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# Concepts de base 🔧

### Qu'est-ce que la virtualisation ?

**Définition :**
- Technique permettant d'exécuter plusieurs systèmes d'exploitation
- Sur une même machine physique
- Isolation complète entre les environnements
- Optimisation de l'utilisation des ressources

**Types de virtualisation :**
- **Virtualisation de type 1** : Hyperviseur natif (bare metal)
- **Virtualisation de type 2** : Hyperviseur hébergé
- **Paravirtualisation** : Optimisations spéciales
- **Conteneurisation** : Isolation légère

**Avantages :**
- Optimisation des ressources
- Isolation et sécurité
- Flexibilité de déploiement
- Facilité de sauvegarde et migration

---
layout: default
---

# Virtualisation vs Conteneurisation 🔄

### Comparaison des approches

**Virtualisation traditionnelle :**
```bash
# Machine physique
├── Hyperviseur (VMware, KVM, Hyper-V)
    ├── VM 1 (OS complet + applications)
    ├── VM 2 (OS complet + applications)
    └── VM 3 (OS complet + applications)
```

**Conteneurisation :**
```bash
# Machine physique
├── OS hôte (Linux)
    ├── Conteneur 1 (applications + runtime)
    ├── Conteneur 2 (applications + runtime)
    └── Conteneur 3 (applications + runtime)
```

**Différences clés :**
- **Ressources** : VMs plus gourmandes, conteneurs légers
- **Démarrage** : VMs lentes, conteneurs rapides
- **Isolation** : VMs complète, conteneurs partielle
- **Portabilité** : Conteneurs plus portables

---
layout: default
---

# KVM (Kernel-based Virtual Machine) 🐧

### Virtualisation native Linux

**Installation KVM :**
```bash
# Vérifier la virtualisation matérielle
egrep -c '(vmx|svm)' /proc/cpuinfo

# Installation sur Ubuntu
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils

# Ajouter l'utilisateur au groupe libvirt
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

**Gestion avec virsh :**
```bash
# Lister les VMs
virsh list --all

# Démarrer une VM
virsh start nom_vm

# Arrêter une VM
virsh shutdown nom_vm

# Voir les informations
virsh dominfo nom_vm
```

**Gestion avec virt-manager :**
```bash
# Interface graphique
sudo apt install virt-manager
virt-manager
```

---
layout: default
---

# QEMU (Quick EMUlator) 🚀

### Émulateur et virtualiseur

**Installation QEMU :**
```bash
# Installation complète
sudo apt install qemu-system qemu-utils

# Créer un disque virtuel
qemu-img create -f qcow2 vm.qcow2 10G

# Démarrer une VM
qemu-system-x86_64 -hda vm.qcow2 -m 2048 -enable-kvm
```

**Options de démarrage :**
```bash
# VM avec interface graphique
qemu-system-x86_64 -hda vm.qcow2 -m 2048 -enable-kvm

# VM en mode console
qemu-system-x86_64 -hda vm.qcow2 -m 2048 -enable-kvm -nographic

# VM avec réseau
qemu-system-x86_64 -hda vm.qcow2 -m 2048 -enable-kvm -net nic -net user
```

**Gestion des images :**
```bash
# Convertir une image
qemu-img convert -f raw -O qcow2 image.raw image.qcow2

# Redimensionner une image
qemu-img resize vm.qcow2 +5G

# Voir les informations
qemu-img info vm.qcow2
```

---
layout: default
---

# LXC (Linux Containers) 📦

### Conteneurs Linux natifs

**Installation LXC :**
```bash
# Installation
sudo apt install lxc lxc-templates

# Vérifier l'installation
lxc-checkconfig

# Lister les conteneurs
lxc-ls
```

**Création de conteneurs :**
```bash
# Créer un conteneur Ubuntu
sudo lxc-create -n mon_conteneur -t ubuntu

# Démarrer le conteneur
sudo lxc-start -n mon_conteneur

# Se connecter au conteneur
sudo lxc-attach -n mon_conteneur

# Arrêter le conteneur
sudo lxc-stop -n mon_conteneur
```

**Gestion avancée :**
```bash
# Voir les informations
sudo lxc-info -n mon_conteneur

# Cloner un conteneur
sudo lxc-clone mon_conteneur mon_conteneur_clone

# Supprimer un conteneur
sudo lxc-destroy -n mon_conteneur
```

---
layout: default
---

# Docker Introduction 🐳

### Conteneurisation moderne

**Installation Docker :**
```bash
# Installation officielle
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Ajouter l'utilisateur au groupe docker
sudo usermod -aG docker $USER

# Vérifier l'installation
docker --version
docker run hello-world
```

**Commandes de base :**
```bash
# Lister les images
docker images

# Télécharger une image
docker pull ubuntu:20.04

# Exécuter un conteneur
docker run -it ubuntu:20.04 bash

# Lister les conteneurs
docker ps -a

# Supprimer un conteneur
docker rm nom_conteneur
```

---
layout: default
---

# Docker Images et Conteneurs 📦

### Gestion des images

**Création d'images :**
```dockerfile
# Dockerfile simple
FROM ubuntu:20.04
RUN apt update && apt install -y nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Construction d'image :**
```bash
# Construire une image
docker build -t mon_image .

# Tag d'une image
docker tag mon_image mon_image:v1.0

# Pousser vers un registre
docker push mon_image:v1.0
```

**Gestion des conteneurs :**
```bash
# Démarrer un conteneur
docker start nom_conteneur

# Arrêter un conteneur
docker stop nom_conteneur

# Redémarrer un conteneur
docker restart nom_conteneur

# Voir les logs
docker logs nom_conteneur
```

---
layout: default
---

# Docker Volumes et Réseaux 💾

### Persistance et communication

**Volumes Docker :**
```bash
# Créer un volume
docker volume create mon_volume

# Monter un volume
docker run -v mon_volume:/data ubuntu:20.04

# Bind mount
docker run -v /host/path:/container/path ubuntu:20.04

# Voir les volumes
docker volume ls
```

**Réseaux Docker :**
```bash
# Créer un réseau
docker network create mon_reseau

# Exécuter un conteneur sur un réseau
docker run --network mon_reseau --name app1 nginx

# Connecter un conteneur existant
docker network connect mon_reseau conteneur_existant

# Voir les réseaux
docker network ls
```

---
layout: default
---

# Docker Compose 🎼

### Orchestration de conteneurs

**Fichier docker-compose.yml :**
```yaml
version: '3.8'
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
    depends_on:
      - db
  
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: myapp
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

**Commandes Compose :**
```bash
# Démarrer les services
docker-compose up -d

# Voir les logs
docker-compose logs

# Arrêter les services
docker-compose down

# Reconstruire les images
docker-compose build
```

---
layout: default
---

# Monitoring et Debugging 🔍

### Surveillance des conteneurs

**Commandes de monitoring :**
```bash
# Statistiques en temps réel
docker stats

# Informations détaillées
docker inspect nom_conteneur

# Processus dans un conteneur
docker top nom_conteneur

# Utilisation des ressources
docker system df
```

**Debugging :**
```bash
# Exécuter une commande dans un conteneur
docker exec -it nom_conteneur bash

# Copier des fichiers
docker cp fichier.txt nom_conteneur:/chemin/

# Voir les logs en temps réel
docker logs -f nom_conteneur
```

**Nettoyage :**
```bash
# Supprimer les conteneurs arrêtés
docker container prune

# Supprimer les images non utilisées
docker image prune

# Nettoyage complet
docker system prune -a
```

---
layout: default
---

# Sécurité des Conteneurs 🔒

### Bonnes pratiques de sécurité

**Exécution non-privilégiée :**
```bash
# Exécuter en tant qu'utilisateur non-root
docker run --user 1000:1000 ubuntu:20.04

# Limiter les capacités
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx
```

**Isolation réseau :**
```bash
# Réseau interne uniquement
docker run --network none ubuntu:20.04

# Réseau personnalisé
docker run --network mon_reseau_intern --name app nginx
```

**Scan de sécurité :**
```bash
# Scanner une image
docker scan nom_image

# Avec Trivy
trivy image nom_image

# Vérifier les vulnérabilités
docker scout cves nom_image
```

---
layout: default
---

# Exercices pratiques 🎯

### Mise en pratique

**Exercice 1 : VM avec KVM**
```bash
# Créer une VM Ubuntu
sudo virt-install \
  --name ubuntu-vm \
  --ram 2048 \
  --disk path=/var/lib/libvirt/images/ubuntu-vm.qcow2,size=10 \
  --vcpus 2 \
  --os-type linux \
  --os-variant ubuntu20.04 \
  --network bridge=virbr0 \
  --graphics vnc,listen=0.0.0.0 \
  --noautoconsole \
  --location http://archive.ubuntu.com/ubuntu/dists/focal/main/installer-amd64/
```

**Exercice 2 : Conteneur Docker**
```bash
# Créer un conteneur web simple
docker run -d \
  --name mon_web \
  -p 8080:80 \
  -v $(pwd)/html:/usr/share/nginx/html \
  nginx:alpine

# Vérifier l'accès
curl http://localhost:8080
```

---
layout: default
---

# Bonnes pratiques 💡

### Conseils pour la virtualisation

**Performance :**
- Allouer les ressources appropriées
- Utiliser les pilotes paravirtualisés
- Optimiser le stockage (SSD recommandé)
- Surveiller l'utilisation des ressources

**Sécurité :**
- Maintenir les VMs à jour
- Utiliser des réseaux isolés
- Limiter les accès privilégiés
- Scanner régulièrement les vulnérabilités

**Maintenance :**
- Faire des sauvegardes régulières
- Documenter les configurations
- Nettoyer les ressources inutilisées
- Surveiller les logs

---
layout: default
---

# Prochaines étapes 🎯

### Ce qui nous attend

1. **Docker** approfondissement
2. **Ansible** introduction
3. **Projet final** intégrateur
4. **QCM** de validation
5. **Débriefing** et perspectives

**Préparation :**
- Familiarisez-vous avec Docker
- Testez la création de conteneurs
- Pratiquez les commandes de base
- Explorez les concepts de virtualisation

---
src: './pages/13-docker-intro.md'
--- 