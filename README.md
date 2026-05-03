🧩 Étape 0 — Prérequis & Vérification Système
🎯 Objectif

Vérifier que la machine est prête avant d’installer les outils du Mini-SOC.

🖥️ Environnement utilisé

| Composant | Détail |
|-----------|--------|
| OS	| Ubuntu 22.04 LTS |
| RAM	| 16 GB |
| CPU |	À vérifier |
| Disque |	À vérifier |
| Réseau |	Interface locale (NAT) |

🔍 Vérification des ressources système

Exécute les commandes suivantes :
```

free -h
df -h /
lsb_release -a

```

Vérifie :

RAM disponible
Espace disque
Version du système

🔄 Mise à jour du système
```

sudo apt update && sudo apt upgrade -y

```
🧰 Installation des outils de base

```

sudo apt install -y \
  curl wget git unzip net-tools \
  gnupg lsb-release ca-certificates \
  software-properties-common

```

✔ Vérification
```

curl --version
git --version
wget --version

```
<img width="748" height="231" alt="Screenshot from 2026-05-04 01-36-08" src="https://github.com/user-attachments/assets/c960d23d-5809-4505-81ec-c730c0a784a3" />


🐳 Installation de Docker & Docker Compose

Docker est requis pour déployer TheHive et Cortex.

```

# Installation
sudo apt install -y docker.io docker-compose

# Activation
sudo systemctl enable docker
sudo systemctl start docker

# Permissions utilisateur
sudo usermod -aG docker $USER
newgrp docker

```

✔ Vérification

```

docker --version
docker-compose --version
docker ps

```

<img width="1591" height="147" alt="Screenshot from 2026-05-04 01-38-49" src="https://github.com/user-attachments/assets/cb01fa81-bfee-42b4-a657-d47f250657b0" />


⚔️ Outils de simulation d’attaques

```

sudo apt install -y \
  nmap hydra netcat-openbsd

```

✔ Vérification

```

nmap --version
hydra -h | head -5

```
<img width="907" height="349" alt="Screenshot from 2026-05-04 01-41-20" src="https://github.com/user-attachments/assets/280fb762-cad5-42e4-aaab-d37ed9414856" />

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


🛡️ Étape 1 — Installation de Wazuh (SIEM Central)
🎯 Objectif

Installer Wazuh en mode tout-en-un pour centraliser :

>> Logs
>> Alertes
>> Détection de menaces

Qu’est-ce que Wazuh ?

Plateforme open-source combinant :

| Fonction | Description |
|----------|-------------|
| SIEM | Collecte & corrélation des événements |
| XDR	| Détection & réponse endpoint |
| IDS	| Détection d’intrusion |
| FIM	| Surveillance des fichiers |
| Dashboard |	Interface web |

🏗️ Architecture Wazuh

```

Agent → Manager → Indexer → Dashboard

```

Agent : collecte des logs
Manager : analyse
Indexer (OpenSearch) : stockage
Dashboard : visualisation

⚙️ Configuration requise

✔ CPU 64 bits (Intel / AMD / ARM)
✔ OS supportés :

Ubuntu 16.04 → 24.04
CentOS / RHEL / Amazon Linux

🔗 Installation de l’agent Wazuh
Ajouter le dépôt

```

sudo apt-get install gnupg apt-transport-https

curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | \
gpg --no-default-keyring \
--keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import

chmod 644 /usr/share/keyrings/wazuh.gpg

sudo apt update
sudo apt install wazuh-agent -y

```

🚀 Déploiement de l’agent

```
WAZUH_MANAGER="localhost.com" apt-get install wazuh-agent

```

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Étape 2 — Installation de Suricata (IDS/IPS)
🎯 Objectif

Installer Suricata sur Kali Linux et l’intégrer avec Wazuh.

Qu’est-ce que Suricata ?


| Fonction |	Description |
|----------|--------------| 
| IDS |	Détection d’attaques |
| IPS	| Blocage du trafic |
| NSM	| Surveillance réseau |
| EVE |	Logs JSON compatibles |


🔄 Flux dans le Mini-SOC


```
Attaque → Suricata → Logs (eve.json)
        → Wazuh Agent → Manager → Dashboard
```

⚙️ Prérequis

✔ Kali Linux
✔ Agent Wazuh installé
✔ Interface réseau : eth0

🔎 Vérification

```
suricata --version
```


<img width="907" height="98" alt="Screenshot from 2026-05-04 01-56-24" src="https://github.com/user-attachments/assets/87a4934f-2943-487c-abfd-4d9f3cd0ac87" />

📥 Mise à jour des règles

```
sudo suricata-update
```

⚙️ Configuration
Interface réseau

ip a | grep "state UP"

Modifier :

```

sudo nano /etc/suricata/suricata.yaml

af-packet:
  - interface: eth0

Activation des logs JSON

# Démarrage

sudo systemctl enable suricata
sudo systemctl start suricata


```

🔗 Intégration avec Wazuh

```

sudo nano /var/ossec/etc/ossec.conf

```
Ajouter:

```
<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>
```

🔄 Redémarrage

```
sudo systemctl restart wazuh-agent
```

🧪 Test















