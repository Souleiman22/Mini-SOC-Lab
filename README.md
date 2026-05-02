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

Wazuh est une plateforme de sécurité offrant une protection XDR et SIEM unifiée pour les terminaux et les charges de travail cloud. La solution se compose de l'agent Wazuh et de trois composants principaux : le serveur Wazuh, l'indexeur Wazuh et le tableau de bord Wazuh. Pour plus d'informations, consultez la documentation « Premiers pas ».

Wazuh est gratuit et open source. Ses composants sont distribués sous licence GNU GPL v2 et Apache v2.0 (ALv2).

Vous pouvez installer l'indexeur Wazuh, le serveur Wazuh et le tableau de bord Wazuh sur un seul hôte ou les répartir en cluster. Chaque composant central de Wazuh propose deux méthodes d'installation, chacune fournissant des instructions pour l'installation sur un seul hôte ou sur des hôtes distincts.

Pour une installation tout-en-un, consultez notre documentation de démarrage rapide. C'est la méthode la plus rapide pour mettre en service les composants centraux de Wazuh.

Pour plus de flexibilité et de personnalisation, installez les composants centraux de Wazuh en commençant par le déploiement de l'indexeur. Cette méthode prend en charge l'installation tout-en-un et l'installation des composants sur des hôtes distincts.

Suivez ce processus d'installation :
```
## Wazuh indexer
## Wazuh server
## Wazuh dashboard

### Configuration requise

Vérifiez les systèmes d'exploitation compatibles et la configuration matérielle recommandée pour l'installation de l'indexeur Wazuh. Assurez-vous que votre environnement système répond à toutes les exigences et que vous disposez des privilèges d'administrateur.

Systèmes d'exploitation recommandés

L'indexeur Wazuh nécessite un processeur Linux 64 bits Intel, AMD ou ARM (architecture x86_64/AMD64 ou AARCH64/ARM64) pour fonctionner. Wazuh prend en charge les versions suivantes :

Amazon Linux 2, Amazon Linux 2023

CentOS Stream 10

Red Hat Enterprise Linux 7, 8, 9, 10

Ubuntu 16.04, 18.04, 20.04, 22.04, 24.04
```
L'agent Wazuh est un logiciel de surveillance unique et léger. Ce composant multiplateforme peut être déployé sur ordinateurs portables, ordinateurs de bureau, serveurs, instances cloud, conteneurs ou machines virtuelles. Il offre une visibilité sur le terminal surveillé en collectant les enregistrements système et applicatifs critiques, les données d'inventaire et en détectant les anomalies potentielles.

Sélectionnez le système d'exploitation de votre terminal ci-dessous et suivez les étapes d'installation pour déployer l'agent Wazuh.

link : https://documentation.wazuh.com/current/installation-guide/index.html


<img width="1222" height="240" alt="Screenshot from 2026-04-27 07-42-53" src="https://github.com/user-attachments/assets/b49a1083-a351-4812-b527-b3bd4f77b5d4" />

## Deploying Wazuh agents on Linux endpoints
```
Ajoutez le dépôt Wazuh pour télécharger les paquets officiels.

Installez les paquets suivants s'ils sont manquants :
apt-get install gnupg apt-transport-https

Installez la clé GPG :
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg

Ajouter le dépôt :
    echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list
    sudo apt install wazuh-agent -y

Mettre à jour les informations du colis :
    apt-get update

```

## Déployer un agent Wazuh

Suivez ces étapes pour déployer l'agent Wazuh sur votre terminal Linux.

Sélectionnez votre gestionnaire de paquets et exécutez la commande ci-dessous. Remplacez la valeur WAZUH_MANAGER par l'adresse IP ou le nom d'hôte de votre gestionnaire Wazuh :
```
WAZUH_MANAGER="localhost.com" apt-get install wazuh-agent

```

Étape 2 — Installation de Suricata IDS/IPS sur Kali Linux

📌 Objectif : Installer et configurer Suricata comme IDS/IPS sur Kali Linux, puis l'intégrer avec Wazuh pour centraliser les alertes réseau.

C'est quoi Suricata ?
Suricata est un moteur open-source de détection d'intrusions réseau (IDS/IPS) qui analyse le trafic en temps réel.

| Fonctionnalité | Description | 
|----------------|-------------|
| IDS | Détecte les attaques réseau et génère des alertes |
| IPS | Bloque activement le trafic malveillant |
| NSM | Surveillance réseau complète |
| EVE | JSONFormat de logs compatible avec Wazuh/ELK |

Rôle dans notre Mini-SOC

Trafic réseau (Nmap, Hydra, attaques)
            ↓
      Suricata IDS/IPS          ← installé sur Kali
      (analyse le trafic)
            ↓
      eve.json (logs)
            ↓
      Wazuh Agent               ← lit les logs Suricata
            ↓
      Wazuh Manager             ← Ubuntu Host
      (Dashboard + Alertes)


Prérequis

Kali Linux avec Wazuh Agent installé et connecté ✅

Connexion internet ✅

Interface réseau : eth0 ✅

Vérifier la version installée
```
suricata --version
```

 2. Téléchargement des règles de détection
Les règles permettent à Suricata de reconnaître les attaques connues.

```
sudo suricata-update
```

Ces règles proviennent de Emerging Threats — une base de données de signatures d'attaques connues mise à jour régulièrement.

 3. Configuration de Suricata
Trouver l'interface réseau active

```
ip a | grep "state UP"
```

Modifier le fichier de configuration
```
sudo nano /etc/suricata/suricata.yaml

```

Cherche la section af-packet et vérifie que l'interface est eth0 :
```
af-packet:
  - interface: eth0
```

Cherche aussi eve-log et vérifie que le format JSON est activé :
```
outputs:
  - eve-log:
      enabled: yes
      filetype: regular
      filename: eve.json
```

Sauvegarde CTRL+X → Y → Entrée

```
sudo systemctl enable suricata
sudo systemctl start suricata
sudo systemctl status suricata
```

5. Intégration Suricata → Wazuh
On ajoute les logs Suricata dans la configuration de l'agent Wazuh pour qu'il les envoie au manager.
```

sudo nano /var/ossec/etc/ossec.conf
```

Ajoute avant </ossec_config> :

```
<localfile>
    <log_format>json</log_format>
    <location>/var/log/suricata/eve.json</location>
  </localfile>
```

Le fichier doit ressembler à ceci :

```
<ossec_config>
  <client>
    <server>
      <address>192.168.1.157</address>
      <port>1514</port>
      <protocol>tcp</protocol>
    </server>
  </client>

  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/syslog</location>
  </localfile>

  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/auth.log</location>
  </localfile>

  <!-- Suricata IDS logs -->
  <localfile>
    <log_format>json</log_format>
    <location>/var/log/suricata/eve.json</location>
  </localfile>

</ossec_config>

```

Sauvegarde CTRL+X → Y → Entrée
Redémarrer l'agent Wazuh

```
sudo systemctl restart wazuh-agent
sudo systemctl status wazuh-agent | grep Active
```
6. Test — Générer une alerte Suricata
On va simuler un scan réseau depuis Kali pour vérifier que Suricata détecte l'attaque.






















