# Étape 0 — Prérequis & Vérification Système
objectif : verifier que la machine est prete avant d'installer les outils du Mini-SOC.

## Environnement utilise
|composant  | Detail |
|-----------|--------|
|OS         |Ubuntu 22.04 LTS |
|RAN        | 16GB   |
|CPU        | Verifier! |
|Disque     | Verifier! |
|Reseau     | Interface Locale(NAT) |

## Verification des ressources systeme
lancer les commandes suivantes dans le terminal pour confirmer que la machine dispose des ressources minimales requises.
### RAN disponible
```
free -h
df -h /
lsb_release -a
```
<img width="828" height="293" alt="Screenshot from 2026-04-25 13-34-09" src="https://github.com/user-attachments/assets/a034b549-3b2e-4948-b294-b3c8bbe1f4d8" />

## Mise a jour du systeme
```
sudo apt update && sudo apt upgrade -y
```
## Installation des outils de base
Ces outils sont necessaires pour la suite du projet.
```
sudo apt install -y \
  curl \
  wget \
  git \
  unzip \
  net-tools \
  gnupg \
  lsb-release \
  ca-certificates \
  software-properties-common
```
### verification
```
curl --version
git --version
wget --version
```

## Installation de Docker & Docker Compose
Docker est requis pour déployer TheHive et Cortex.
```

# installation Docker
sudo apt install -y docker.io docker-compose

# Active Docker au demarrage
sudo systemctl enable docker
sudo systemctl start docker

# Ajouter l'utilisateur  courant au groupe docker
sudo usermod -aG docker $USER

# Appliquer le changement de groupe (sans redemarrer)
newgrp docker
```
### Verification 
```
docker --version
docker-compose --version
docker ps
```
### Resultat 

<img width="1595" height="201" alt="Screenshot from 2026-04-25 13-56-57" src="https://github.com/user-attachments/assets/b4752dde-74f7-4247-afca-2c2568bcd01c" />

## installation des outils de simulation d'attaques
Ces outils servent a simuler des attaques dans un environnement isole et controle.
```
sudo apt install -y \
  nmap \
  hydra \
  netcat-openbsd
```
### Vérification
```
nmap --version
hydra -h | head -5
```
## Création de la structure du projet (dans ton dossier existant)

Le projet sera structuré comme suit dans le dossier `~/projets/mini-soc` :
Créer l'arborescence complète du projet dès maintenant.
```
mkdir -p ~/mini-soc-lab/{docs,configs,docker,screenshots,rapport,playbooks}
cd ~/mini-soc-lab
git init
```
<img width="856" height="229" alt="Screenshot from 2026-04-25 14-28-39" src="https://github.com/user-attachments/assets/d25422bf-f070-4e43-b60c-d17e40108d70" />


----------------------------------------------------------------------------------------------------------
# Etape 1 - Installation de Wazuh(SIEM Central)
Objectif: Installer Wazuh en mode "tout-en-un"(Manager + Indexer + Dashboard) sur Ubuntu pour centraliser la collecte des logs et la detection des menaces.

----------------------------------------------------------------------------------------------------------
## C'est quoi Wazuh?
Wazuh est une plateforme open-source de securite qui combine:

| Fonctionnalite | Description |
|----------------|-------------|
| SIEM           | Collecte et correcle les evenements de securite |
| XDR            | Detection et reponse sur les endpoints |
| IDS            | Detection d'intrusions basee sur les logs |
| FIM            | Surveillance de l'integrite des fichiers |
| Dashboard      | interface web pour visualiser les alertes |

## Architecture Wazuh
+-------------+
| Agent Wazuh |
|             |
| Ubuntu      |
| Sysmon      |
+-------------+
      |
      |
      |
+----------------------+
| Wazuh Manager        |
| (Collecte & analyse  |
|   des logs/alertes)  |
+----------------------+
              |
              |
              |
 +-----------------------+
 |  Wazuh Indexer        |
 |  (OpenSearch          |
 |   stockage des logs)  |
 +----------┬------------+
            |
            ▼
 +----------------------+
|   Wazuh Dashboard     |
|  (Interface Kibana‑   |
|  based, visualisation)|
+-----------------------+
















