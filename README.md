# Désactivation AppDefaultHashRotation & UCPD – Scripts PowerShell

## 📌 Description

Ce projet contient **deux scripts PowerShell** permettant de :

1. Désactiver les fonctionnalités Windows suivantes via **ViveTool** :
   - `AppDefaultHashRotation` (ID: 43229420)
   - `AppDefaultHashRotationUpdateHashes` (ID: 27623730)

2. Désactiver le service **UCPD** ainsi que sa tâche planifiée associée.

Ces scripts sont principalement utilisés pour empêcher Windows de réinitialiser ou faire tourner les associations d’applications par défaut.

---

# 🧩 Script 1 – Gestion automatique de ViveTool et désactivation des features

## 🔹 Fonctionnement

Ce script :

- Vérifie si **ViveTool** est présent dans :
  ```
  %TEMP%\ViveTool\
  ```
- Si absent :
  - Télécharge automatiquement la dernière version depuis GitHub
  - L’extrait dans le dossier temporaire
- Vérifie si les fonctionnalités sont activées au scope **User**
- Désactive les features si elles sont actives :
  - `43229420`
  - `27623730`
- Vérifie après exécution que la désactivation a bien fonctionné

## 🔧 Fonction utilitaire

Le script contient une fonction :

```powershell
Is-UserEnabled($featureID)
```

Elle vérifie si une feature est :
- Priority : `User`
- State : `Enabled (2)`

---

## ▶️ Exécution

Lancer PowerShell **en tant qu’administrateur**, puis :

```powershell
.\HashRotation.ps1
```

---

## 📦 Dépendances

- PowerShell 5.1+
- Accès Internet (si ViveTool doit être téléchargé)
- Droits administrateur

---

# 🧩 Script 2 – Désactivation du service UCPD

## 🔹 Fonctionnement

Ce script :

- Désactive le service :
  ```
  UCPD
  ```
- Désactive la tâche planifiée :
  ```
  \Microsoft\Windows\AppxDeploymentClient\UCPD velocity
  ```
- Affiche un message indiquant qu’un redémarrage est nécessaire

Le script est protégé par un bloc `try/catch` pour éviter les erreurs si le service n’existe pas.

---

## ▶️ Exécution

```powershell
.\UCPD.ps1
```

⚠️ **Un redémarrage est nécessaire après exécution.**

---

# 🔐 Important

- Ces modifications affectent le comportement interne de Windows.
- Elles peuvent être réinitialisées lors d’une mise à jour majeure de Windows.
- Toujours tester en environnement de validation avant déploiement en production.

---

# 📁 Structure recommandée

```
.
├── HashRotation.ps1
├── UCPD.ps1
└── README.md
```

---

# 🖥️ Compatibilité

- Windows 10
- Windows 11
