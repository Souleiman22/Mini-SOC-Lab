# Mini-SOC-Lab
Mini SOC lab with SIEM, IDS/IPS, and incident response using Wazuh, ELK, Suricata, and TheHive.

Bonne idée 😄 tu veux une version “blink blink” (= claire, visuelle, prête à suivre). Je te restructure **PHASE 1** proprement comme un vrai guide SOC.

---

# 🚀 Mini-SOC Lab — PHASE 1 (Blink Version)

## 🎯 Objectif

Déployer :

* SIEM → **Wazuh**
* Visualisation → **ELK Stack**

👉 C’est le **cœur du SOC**

---

# ⚡ 📅 DAY 1 — Setup & Installation

## 🔵 STEP 1 — Update système

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🔵 STEP 2 — Installer dépendances

```bash
sudo apt install -y curl wget git apt-transport-https gnupg2 software-properties-common unzip
```

---

## 🔵 STEP 3 — Télécharger Wazuh

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
curl -sO https://packages.wazuh.com/4.7/config.yml
ls -lh wazuh-install.sh config.yml
```

👉 Vérifie que les fichiers sont bien là

---

## 🔵 STEP 4 — Configurer `config.yml`

```bash
nano config.yml
```

👉 Remplace TOUT par :

```yaml
nodes:
  indexer:
    - name: node-1
      ip: "127.0.0.1"
  server:
    - name: wazuh-1
      ip: "127.0.0.1"
  dashboard:
    - name: dashboard
      ip: "127.0.0.1"
```

💾 Sauvegarde :

* `CTRL + O` → Enter
* `CTRL + X`

---

## 🔵 STEP 5 — Générer certificats 🔐

```bash
sudo bash wazuh-install.sh --generate-config-files
```

👉 Ça crée les certificats SSL pour sécuriser les communications

---

## 🔵 STEP 6 — Installer Indexer (base de données)

```bash
sudo bash wazuh-install.sh --wazuh-indexer node-1 -i
```

👉 C’est l’équivalent de **Elasticsearch**

---

## 🔵 STEP 7 — Installer Server (SIEM)

```bash
sudo bash wazuh-install.sh --wazuh-server wazuh-1 -i
```

👉 C’est le cerveau (analyse logs + alertes)

---

## 🔵 STEP 8 — Installer Dashboard (interface web)

```bash
sudo bash wazuh-install.sh --wazuh-dashboard dashboard -i
```

👉 Interface web type **Kibana**

---

# ✅ À la fin de la PHASE 1

Tu dois avoir :

* ✔ Wazuh Server actif
* ✔ Indexer actif
* ✔ Dashboard accessible

---

## 🌐 Accès Web

Ouvre ton navigateur :

```
https://localhost
```

Login par défaut :

```
user: admin
password: (affiché à la fin de l’installation)
```

---

# ⚠️ Avant de passer PHASE 2

Vérifie :

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

👉 Tout doit être **active (running)**

---

# 💡 Petit conseil (important)

Si quelque chose bloque :

```bash
journalctl -xe
```

---

# 🔥 Ensuite (PHASE 2)

Tu vas ajouter :

* Suricata → détection réseau
* TheHive → gestion incidents

---

Si tu veux, je peux te faire la **PHASE 2 en mode blink blink aussi (encore plus stylé 😎)** ou t’aider si une étape bloque.
