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
```
<img width="828" height="293" alt="Screenshot from 2026-04-25 13-34-09" src="https://github.com/user-attachments/assets/a034b549-3b2e-4948-b294-b3c8bbe1f4d8" />

## Mise a jour du systeme
```
sudo apt update && sudo apt upgrade -y
```
### Installation des outils de base
Ces outils sont necessaires pour la suite du projet.








