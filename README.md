# SAE 5.01 - Mise en oeuvre d'une infrastructure Système et Réseau

Ce dépôt contient l'ensemble des ressources, configurations et scripts réalisés dans le cadre de la SAE 5.01 "Administration Système et Réseau". Le projet vise à déployer une infrastructure réseau redondante et sécurisée, couplée à une gestion centralisée des utilisateurs via Active Directory.

## 📋 Description du Projet

L'objectif principal est de simuler et mettre en production un réseau d'entreprise robuste (Intranet WSL2024). Le projet couvre les aspects suivants :
- **Architecture Réseau** : Configuration de commutateurs Cisco (Core & Access) avec redondance (HSRP).
- **Administration Système** : Déploiement d'un contrôleur de domaine Windows Server (Active Directory).
- **Automatisation** : Scripting PowerShell pour la création massive d'utilisateurs et de groupes.
- **Documentation** : Rapport technique et vidéo de démonstration.

## 📂 Contenu du Dépôt

L'arborescence du projet est organisée comme suit :

| Dossier / Fichier | Description |
|-------------------|-------------|
| 📁 `Confs/` | Fichiers de configuration (`running-config`) pour les équipements Cisco (CORESW1, CORESW2, ACCSW1, ACCSW2). |
| 📄 `Script_Utils_OK.txt` | Script PowerShell permettant l'automatisation de la création des utilisateurs et groupes Active Directory. |
| 📄 `Rapport_Groupe2.pdf` | Rapport détaillé du projet (choix techniques, architecture, tests). |
| 🎥 `Groupe2_DHCP_REM.mp4` | Vidéo de démonstration illustrant le fonctionnement du service DHCP et de la redondance. |
| 📄 `EN Sujet-SAE-501...` | Sujet original du projet (PDF). |

## 🏗️ Architecture Technique

### Réseau (Cisco IOS)
L'infrastructure utilise une architecture hiérarchique :
- **Cœur de Réseau (Core Layer)** : `CORESW1` et `CORESW2`. Ils assurent le routage inter-VLAN et la passerelle par défaut via le protocole **HSRP** (Hot Standby Router Protocol) pour la haute disponibilité.
- **Accès (Access Layer)** : `ACCSW1` et `ACCSW2`. Connectent les terminaux utilisateurs.
- **Liaisons** : Utilisation de **LACP** (EtherChannel) pour l'agrégation de liens entre les switches.

**Plan d'adressage et VLANs :**
- **VLAN 10** : Administratif / Management (HSRP VIP : `10.2.10.60`)
- **VLAN 20** : Utilisateurs / Data (HSRP VIP : `10.2.21.252`)
- **VLAN 99** : Serveurs / Services (HSRP VIP : `10.2.99.4`)

### Système (Windows Server / Active Directory)
- **Domaine** : `wsl2024.org`
- **Serveur AD** : Gestion des identités et des accès.
- **Automatisation** : Le script PowerShell génère 1000 utilisateurs (`wslusr001` à `wslusr1000`) et les répartit dans les groupes `FirstGroup` et `LastGroup`.

## 🚀 Guide de Démarrage

### 1. Déploiement Réseau
1. Charger les configurations situées dans le dossier `Confs/` sur les équipements respectifs via la console ou TFTP.
2. Vérifier les voisinages CDP et le statut HSRP :
   ```bash
   show standby brief
   show etherchannel summary
   ```

### 2. Déploiement Active Directory
1. Sur le contrôleur de domaine, ouvrir PowerShell en tant qu'administrateur.
2. Adapter la variable `$ouPath` dans le fichier `Script_Utils_OK.txt` pour correspondre à votre structure OU.
3. Exécuter le script pour peupler l'annuaire :
   ```powershell
   .\Script_Utils_OK.txt
   ```

## 👥 Auteurs
**Groupe 2**
- Projet réalisé dans le cadre de la formation BUT R&T (Réseaux et Télécommunications).
