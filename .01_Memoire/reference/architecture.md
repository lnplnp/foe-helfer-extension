# Architecture Technique — FoE Helper

*Projet : FoE Helper (foe-helfer-extension)*
*Version : 4.3.0.0*
*Dernière mise à jour : 19 octobre 2026*
*Source : Inspection directe du code*

---

## 📘 Table des matières

1. [Vue d'ensemble](#-vue-densemble)
2. [Architecture Globale](#-architecture-globale)
3. [Composants Principaux](#-composants-principaux)
4. [Flux de données](#-flux-de-données)
5. [Gestion des modules](#-gestion-des-modules)
6. [Internationalisation](#-internationalisation)
7. [Stockage et persistance](#-stockage-et-persistance)
8. [Sécurité et permissions](#-sécurité-et-permissions)
9. [Dépendances tierces](#-dépendances-tierces)
10. [Points d'extension](#-points-dextension)

---

## 🌐 Vue d'ensemble

**FoE Helper** est une **extension navigateur** conçue pour le jeu **Forge of Empires** (https://forgeofempires.com).

L'extension fonctionne comme une **couche d'assistance** qui s'exécute dans le contexte du jeu, sans modifier les données du jeu lui-même. Elle offre des fonctionnalités supplémentaires comme des calculateurs, des alertes, des informations statistiques, et des outils de visualisation avancés.

### Caractéristiques clés
- **Non intrusif** : Ne modifie pas le jeu, seulement l'interface utilisateur
- **Modulaire** : 71 modules indépendants activés selon le contexte
- **Multi-navigateurs** : Compatible Chromium (Chrome, Edge, Brave, Opera) et Firefox
- **Multilingue** : 9 langues supportées via le système i18n de Chrome
- **Sans build** : Les sources sont utilisées directement (pas de compilation)

---

## 🏗️ Architecture Globale

```
┌─────────────────────────────────────────────────────────────────┐
│                        NAVIGATEUR WEB                               │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    Forge of Empires                             ││
│  │  (https://*.forgeofempires.com/game)                          ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    EXTENSION FOE HELPER                             │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  MANIFEST.V3 (manifest.json)                                   ││
│  │  - permissions: alarms, storage, tabs, notifications, ...     ││
│  │  - host_permissions: forgeofempires.com, foe-helper.com      ││
│  │  - content_scripts: foeproxy.js, inject.js                     ││
│  │  - background: background.js (service worker)                 ││
│  │  - action: popup.html                                         ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
          │                           │                           │
          ▼                           ▼                           ▼
┌─────────────────────┐   ┌─────────────────────┐   ┌─────────────────────┐
│   SERVICE WORKER     │   │   CONTENT SCRIPT     │   │      UI POPUP        │
│   (background.js)    │   │   (foeproxy.js)      │   │   (popup.html)      │
│                     │   │                     │   │                     │
│  • Gestion alarms   │   │  • Interception      │   │  • Configuration     │
│  • Stockage         │   │    requêtes HTTP     │   │  • Paramètres        │
│  • Notifications    │   │  • Injection code    │   │  • État extension    │
│  • Communication    │   │  • world: MAIN       │   │  • Accès modules     │
│    entre onglets    │   │                     │   │                     │
└─────────────────────┘   └────────────┬────────┘   └─────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────┐
│                    INJECT.JS + MODULES                              │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  CHARGEUR DE MODULES (inject.js)                                ││
│  │  - Détection du contexte (page du jeu)                         ││
│  │  - Chargement dynamique des modules pertinents               ││
│  │  - Gestion des dépendances (vendor/)                          ││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                   │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ...          │
│  │   alerts/   │ │  auctions/   │ │ battle-assist│                 │
│  │  (alertes)  │ │ (enchères)   │ │ (combat)     │                 │
│  └─────────────┘ └─────────────┘ └─────────────┘                 │
│                                                                   │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ...          │
│  │  calculator │ │ citymap/    │ │ greatbuildings│                │
│  │ (calculs)  │ │ (carte)     │ │ (GB)          │                 │
│  └─────────────┘ └─────────────┘ └─────────────┘                 │
│                                                                   │
│  (71 modules au total, organisés dans js/web/)                    │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🧩 Composants Principaux

### 1. Manifest (manifest.json)

**Fichier** : `/manifest.json` (1,7 Ko)
**Rôle** : Configuration de l'extension (Manifest V3)

#### Permissions
```json
{
  "permissions": [
    "alarms",        // Planification de tâches
    "storage",       // Stockage local (chrome.storage)
    "tabs",          // Accès aux onglets
    "notifications", // Notifications système
    "clipboardWrite" // Écriture dans le presse-papiers
  ]
}
```

#### Host Permissions
```json
{
  "host_permissions": [
    "https://*.forgeofempires.com/*",
    "https://foe-helper.com/*",
    "https://*.foe-helper.com/*",
    "https://*.foe-rechner.de/*"
  ]
}
```

#### Content Scripts
| Script | Exécution | Rôle |
|--------|-----------|------|
| `foeproxy.js` | `document_start`, `world: MAIN` | Proxy pour l'injection précoce |
| `foeproxy-fallback.js` | `document_start` | Fallback du proxy |
| `browser-polyfill.js` | `document_end` | Polyfill pour Firefox |
| `_languages.js` | `document_end` | Système d'internationalisation |
| `inject.js` | `document_end` | Script d'injection principal |

#### Ressources accessibles
```json
{
  "web_accessible_resources": [
    {
      "resources": ["js/*", "vendor/*", "css/*", "content/*"],
      "matches": ["https://*/*"]
    }
  ]
}
```

### 2. Service Worker (background.js)

**Fichier** : `/background.js` (20 Ko)
**Rôle** : Gestion des tâches en arrière-plan

#### Fonctionnalités principales
- **Gestion des alarmes** (`chrome.alarms`) : Planification de tâches périodiques
- **Stockage** (`chrome.storage`) : Persistance des données utilisateur
- **Notifications** (`chrome.notifications`) : Alertes système
- **Communication** : Échange de messages entre onglets
- **Base de données** : Utilisation de **Dexie.js** (wrapper IndexedDB)

#### Schéma de base de données (Dexie)
```javascript
alertsDB.version(1).stores({
  alerts: "++id,[server+playerId],data.expires"
});
```

#### Dépendances
- `vendor/browser-polyfill/browser-polyfill.min.js`
- `vendor/dexie/dexie.min.js`

### 3. Proxy et Injection (foeproxy.js, inject.js)

#### foeproxy.js
**Rôle** : Content script exécuté très tôt (`document_start`) dans le monde `MAIN`

- **Interception des requêtes** : Peut intercepter et modifier les requêtes HTTP
- **Injection précoce** : Permet d'injecter du code avant que le DOM ne soit chargé
- **Communication** : Bridge entre le monde isolé et le monde MAIN

#### inject.js
**Rôle** : Script d'injection principal exécuté à `document_end`

**Fonctionnalités clés :**
```javascript
let scripts = {
  main: ["once", "primed"],
  vendor: ["once", "primed"],
  internal: ["once", "primed"]
};
```
- **Chargement des dépendances** : Chargement parallèle des bibliothèques tierces
- **Détection du contexte** : Identification de la page du jeu
- **Chargement dynamique des modules** : Activation des modules pertinents
- **Gestion des états** : Suivi des scripts chargés (once, primed)

### 4. Modules Fonctionnels (js/web/)

**Organisation** : 71 modules dans 218 répertoires sous `js/web/`

#### Catégories de modules

| Catégorie | Répertoire | Nombre | Description |
|-----------|------------|--------|-------------|
| **Système** | `_helper/`, `_i18n/`, `_languages/`, `_main/`, `_menu/` | ~15 | Utilitaires, internationalisation, menu |
| **Jeu** | `battle-assist/`, `city-builder/`, `citymap/`, `greatbuildings/` | ~20 | Fonctionnalités liées au gameplay |
| **Économie** | `market/`, `auctions/`, `productions/`, `investment/` | ~10 | Gestion des ressources et marché |
| **Quêtes** | `quests/`, `recurring-quests/`, `reconstruction/` | ~8 | Suivi des quêtes |
| **Statistiques** | `stats/`, `guildfights/`, `guildmemberstat/`, `scoutingtimes/` | ~10 | Analyse et statistiques |
| **Outils** | `calculator/`, `planner/`, `negotiation/`, `mouseActions/` | ~15 | Calculateurs et outils utilitaires |
| **UI** | `customTooltip/`, `infoboard/`, `notice/`, `settings/` | ~10 | Améliorations de l'interface |
| **Événements** | `eventchests/`, `eventhandler/`, `kits/`, `hidden-rewards/` | ~8 | Gestion des événements spéciaux |

#### Modules notables

| Module | Fichier | Description |
|--------|--------|-------------|
| **MoppelHelper** | `js/web/moppelhelper/` | Filtres et assistance pour Moppel Helper |
| **GreatBuildings** | `js/web/greatbuildings/` | Gestion des Grands Bâtiments |
| **Market** | `js/web/market/` | Amélioration de l'interface marché |
| **CityMap** | `js/web/citymap/` | Carte de la ville améliorée |
| **Calculator** | `js/web/calculator/` | Divers calculateurs |
| **Stats** | `js/web/stats/` | Statistiques de jeu |
| **GuildFights** | `js/web/guildfights/` | Gestion des combats de guilde |

---

## 🔄 Flux de données

### 1. Chargement initial

```
1. Navigateur charge Forge of Empires
   ↓
2. manifest.json déclenche les content scripts
   ↓
3. foeproxy.js s'exécute (document_start, world: MAIN)
   ↓
4. foeproxy-fallback.js s'exécute (document_start)
   ↓
5. browser-polyfill.js charge (document_end)
   ↓
6. _languages.js charge (document_end)
   ↓
7. inject.js s'exécute (document_end)
   ↓
8. inject.js charge les dépendances (vendor/)
   ↓
9. inject.js détecte le contexte (page du jeu)
   ↓
10. inject.js charge les modules pertinents
   ↓
11. Modules s'initialisent et injectent leur UI/functionalités
```

### 2. Communication entre composants

```
┌─────────────────────┐       ┌─────────────────────┐       ┌─────────────────────┐
│   Content Script     │       │   Service Worker     │       │   Page Web (Jeu)     │
│   (foeproxy.js)      │       │   (background.js)    │       │                     │
└──────────┬──────────┘       └──────────┬──────────┘       └──────────┬──────────┘
           │                           │                           │
           │ chrome.runtime.sendMessage  │                           │
           ▼                           ▼                           ▼
┌─────────────────────────────────────────────────────────────────┐
│                        ÉCHANGE DE MESSAGES                         │
│  - Requêtes de données (stockage, configuration)                    │
│  - Notifications entre onglets                                   │
│  - Synchronisation d'état                                        │
└─────────────────────────────────────────────────────────────────┘
```

### 3. Stockage des données

```
┌─────────────────────────────────────────────────────────────────┐
│                        STOCKAGE                                     │
├─────────────────────────────────────────────────────────────────┤
│  1. chrome.storage.local       - Préférences utilisateur         │
│  2. chrome.storage.sync        - Synchronisation (si activé)     │
│  3. IndexedDB (via Dexie)      - Données volumineuses            │
│     └── Alerts DB              - Alertes planifiées               │
│  4. LocalStorage               - Cache temporaire                │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📦 Gestion des modules

### Système de chargement

Le projet utilise un **système de chargement dynamique** basé sur la détection du contexte.

#### Détection de la page
`inject.js` contient la logique pour identifier :
- Le type de page (ville, carte, combat, marché, etc.)
- Les éléments DOM spécifiques
- Les URL et paramètres

#### Chargement conditionnel
Les modules sont chargés **uniquement si nécessaire** :
- Sur la page de la ville → `citymap/`, `greatbuildings/`, etc.
- Sur la page du marché → `market/`, `auctions/`
- En combat → `battle-assist/`
- etc.

#### Dépendances entre modules
- **Pas de gestion explicite** des dépendances entre modules
- Les modules supposent que les bibliothèques tierces (`vendor/`) sont déjà chargées
- L'ordre de chargement est géré par `inject.js`

### Ajout d'un nouveau module

**Étapes :**
1. Créer un répertoire dans `js/web/[nom-module]/`
2. Créer le fichier principal : `js/web/[nom-module]/[nom-module].js`
3. Ajouter la logique de détection dans `inject.js`
4. S'assurer que les dépendances sont disponibles
5. Tester dans le navigateur

**Structure type d'un module :**
```
js/web/[nom-module]/
├── [nom-module].js          # Logique principale
├── [nom-module].html       # UI (optionnel)
├── [nom-module].css        # Styles (optionnel)
└── assets/                 # Ressources (optionnel)
```

---

## 🌍 Internationalisation

### Système
- **API Chrome i18n** : Utilisation de `__MSG_nom_de_la_chaine__`
- **Fichiers de traduction** : `_locales/[langue]/messages.json`
- **9 langues supportées** : de, en, fr, nl, pl, ro, ru, sv

### Structure des fichiers
```
_locales/
├── de/
│   └── messages.json
├── en/
│   └── messages.json
├── fr/
│   └── messages.json
├── nl/
│   └── messages.json
├── pl/
│   └── messages.json
├── ro/
│   └── messages.json
├── ru/
│   └── messages.json
└── sv/
    └── messages.json
```

### Exemple de messages.json
```json
{
  "appName": {
    "message": "FoE Helper",
    "description": "Nom de l'extension"
  },
  "appDesc": {
    "message": "An extension for the Browser Game Forge of Empires",
    "description": "Description de l'extension"
  }
}
```

### Utilisation dans le code
```javascript
// Dans manifest.json
"name": "__MSG_appName__",
"description": "__MSG_appDesc__",

// Dans le code JavaScript
let title = chrome.i18n.getMessage("appName");
```

---

## 💾 Stockage et persistance

### 1. chrome.storage

**Utilisation :**
```javascript
// Écrire
chrome.storage.local.set({ key: value });
chrome.storage.sync.set({ key: value });

// Lire
chrome.storage.local.get(['key'], function(result) {
  console.log(result.key);
});
```

**Différences :**
| Type | Synchronisation | Quota | Utilisation |
|------|-----------------|-------|-------------|
| `local` | Non | 10 Mo | Préférences utilisateur |
| `sync` | Oui (avec compte Google) | 100 Ko | Synchronisation entre appareils |

### 2. IndexedDB (via Dexie.js)

**Utilisation :**
```javascript
// Définition du schéma (dans background.js)
let alertsDB = new Dexie("Alerts");
alertsDB.version(1).stores({
  alerts: "++id,[server+playerId],data.expires"
});

// Ajouter un élément
alertsDB.alerts.add({
  server: "fr",
  playerId: "123",
  data: { ... },
  expires: Date.now() + 86400000
});

// Requête
alertsDB.alerts
  .where("[server+playerId]")
  .equals(["fr", "123"])
  .toArray();
```

**Avantages :**
- Stockage de données volumineuses (> 10 Mo)
- Indexation et requêtes performantes
- Transactions supportées

### 3. LocalStorage

**Utilisation limitée** : Principalement pour du cache temporaire

---

## 🔐 Sécurité et permissions

### Permissions déclarées

| Permission | Utilisation | Niveau de risque |
|------------|--------------|------------------|
| `alarms` | Planification de tâches | ⭐ Faible |
| `storage` | Stockage local | ⭐ Faible |
| `tabs` | Accès aux onglets | ⭐⭐ Moyen |
| `notifications` | Alertes système | ⭐ Faible |
| `clipboardWrite` | Copie dans le presse-papiers | ⭐⭐ Moyen |

### Host Permissions

| URL | Accès | Justification |
|-----|-------|---------------|
| `https://*.forgeofempires.com/*` | Lecture/Écriture | Cœur du jeu |
| `https://foe-helper.com/*` | Lecture | Site de documentation |
| `https://*.foe-helper.com/*` | Lecture | Sites liés |
| `https://*.foe-rechner.de/*` | Lecture | Calculateurs externes |

### Bonnes pratiques de sécurité

1. **Ne pas stocker de données sensibles** dans chrome.storage
2. **Valider toutes les entrées utilisateur**
3. **Échapper le HTML** avant injection dans le DOM
4. **Ne pas exécuter de code non fiable** (eval, new Function)
5. **Respecter les CORS** (Cross-Origin Resource Sharing)
6. **Chiffrer les données sensibles** si nécessaire

### Content Security Policy (CSP)

Le manifest V3 impose une CSP stricte. L'extension doit :
- Éviter les `eval()` et `new Function()`
- Utiliser des scripts inline avec des hashs
- Charger les ressources uniquement depuis les chemins autorisés

---

## 📥 Dépendances tierces

### Liste des dépendances (vendor/)

| Bibliothèque | Version | Utilisation | Taille |
|--------------|---------|-------------|-------|
| **browser-polyfill** | ? | Polyfill pour Firefox | ~ |
| **CountUp** | ? | Animations de comptage | ~ |
| **dexie** | ? | Wrapper IndexedDB | ~ |
| **downloadjs** | ? | Téléchargement de fichiers | ~ |
| **highchart-8.0.4** | 8.0.4 | Graphiques | ~ |
| **i18njs** | ? | Internationalisation | ~ |
| **jQuery** | ? | Manipulation DOM | ~ |
| **jsZip** | ? | Création de ZIP | ~ |
| **lit-html** | ? | Templating HTML | ~ |
| **moment** | ? | Manipulation de dates | ~ |
| **ScrollTo** | ? | Scroll animé | ~ |
| **Sortable** | ? | Drag & Drop | ~ |
| **bootstrap-4.2.1** | 4.2.1 | Framework CSS | ~ |
| **date-range** | ? | Sélecteur de dates | ~ |
| **jquery** | ? | (duplicate?) | ~ |
| **SimpleMarkdown** | ? | Parsing Markdown | ~ |
| **Tabslet** | ? | Onglets | ~ |
| **tableSorter** | ? | Tri de tableaux | ~ |
| **tooltip** | ? | Info-bulles | ~ |
| **sounds** | ? | Sons | ~ |

**Total : 20+ bibliothèques** dans le répertoire `vendor/`

### Gestion des dépendances

**Aucun gestionnaire de paquets** (npm, yarn) n'est utilisé.

**Processus manuel :**
1. Télécharger les fichiers depuis les sources officielles
2. Copier dans `vendor/[nom-de-la-bibliotheque]/`
3. Mettre à jour `vendor.json`
4. Tester la compatibilité

**Risques :**
- Versions obsolètes
- Vulnérabilités de sécurité non corrigées
- Conflits entre versions

---

## 🔌 Points d'extension

### Où ajouter de nouvelles fonctionnalités ?

| Type de fonctionnalité | Emplacement | Exemple |
|------------------------|-------------|---------|
| **Nouveau module** | `js/web/[nom]/` | `js/web/nouveau-module/` |
| **Fonction utilitaire** | `js/web/_helper/` | `js/web/_helper/math.js` |
| **Traduction** | `_locales/[langue]/messages.json` | `_locales/fr/messages.json` |
| **Style CSS** | `css/web/` | `css/web/nouveau-module.css` |
| **Image** | `css/images/` | `css/images/icons/nouveau.png` |
| **Dépendance** | `vendor/` | `vendor/nouvelle-lib/` |

### Intégration avec le jeu

Les modules peuvent :
- **Lire** le DOM du jeu
- **Injecter** du HTML/CSS dans le jeu
- **Écouter** les événements DOM
- **Modifier** les requêtes HTTP (via foeproxy.js)
- **Stocker** des données localement
- **Afficher** des notifications

**Ne pas :**
- ❌ Modifier les données du serveur
- ❌ Envoyer des requêtes non autorisées
- ❌ Violenter les CGU du jeu

---

## 📊 Statistiques

- **Nombre total de fichiers JavaScript** : 71+ modules + fichiers racines
- **Nombre de répertoires** : 218 sous `js/web/`
- **Taille du projet** : ~ (à estimer)
- **Nombre de langues** : 9
- **Nombre de dépendances tierces** : 20+
- **Version de l'extension** : 4.3.0.0

---

## 🔍 Diagrammes

### Diagramme de classes simplifié

```
┌─────────────────────────────────────────────────────────────────┐
│                        FoEHelper                                   │
├─────────────────────────────────────────────────────────────────┤
│ + ServiceWorker (background.js)                                   │
│ + ContentScript (foeproxy.js)                                     │
│ + Injector (inject.js)                                            │
│ + ModuleManager                                                   │
└─────────────────────────────────────────────────────────────────┘
                              │
          ┌───────────────────┼───────────────────┐
          ▼                   ▼                   ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│  AlertsModule    │ │  MarketModule    │ │ GreatBuildings   │
│                 │ │                 │ │                 │
│ + checkAlerts() │ │ + enhanceUI()   │ │ + calculateGB()  │
│ + showAlert()   │ │ + parseOffers() │ │ + showDetails() │
└─────────────────┘ └─────────────────┘ └─────────────────┘
```

---

## 📝 Notes et observations

### Observations architecturales

1. **Architecture modulaire** : Très bien structurée, chaque fonctionnalité est isolée
2. **Pas de framework** : Vanilla JS, pas de React/Angular/Vue → léger et performant
3. **Chargement dynamique** : Optimisé pour ne charger que les modules nécessaires
4. **Internationalisation complète** : 9 langues, système robuste
5. **Stockage flexible** : Utilisation appropriée de chrome.storage et IndexedDB

### Points à améliorer

1. **Gestion des dépendances** : Migrer vers npm/yarn pour une meilleure gestion
2. **Tests automatisés** : Introduire un framework de test (Jest, Mocha)
3. **Build system** : Ajouter Webpack/Rollup pour bundling et optimisation
4. **Documentation** : Compléter la documentation technique
5. **TypeScript** : Migrer progressivement vers TypeScript pour plus de robustesse

### Décisions architecturales clés

| Décision | Justification | Date |
|----------|---------------|------|
| **Pas de framework** | Performance, simplicité, compatibilité | Début du projet |
| **Chargement dynamique** | Optimisation des ressources | Début du projet |
| **Manifest V3** | Compatibilité future, sécurité | Migration récente |
| **Dexie pour IndexedDB** | API plus simple que l'API native | À déterminer |

---

## 🔗 Références

- **AGENTS.md** : Consignes permanentes pour les agents IA
- **manifest.json** : Configuration de l'extension
- **background.js** : Service Worker
- **inject.js** : Script d'injection
- **Repository GitHub** : https://github.com/mainIine/foe-helfer-extension

---

*Ce document doit être mis à jour lors de tout changement architectural significatif.*
*Dernière vérification : 19 octobre 2026*
