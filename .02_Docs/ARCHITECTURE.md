# ARCHITECTURE.md — Architecture Technique FoE Helper

*Projet : FoE Helper (foe-helfer-extension)*
*Version : 4.3.0.0*
*Dernière mise à jour : 19 octobre 2026*

---

## 📘 Sommaire

1. [Vue d'ensemble](#-vue-densemble)
2. [Architecture Globale](#-architecture-globale)
3. [Composants Principaux](#-composants-principaux)
4. [Flux de Données](#-flux-de-données)
5. [Structure des Modules](#-structure-des-modules)
6. [Technologies Utilisées](#-technologies-utilisées)
7. [Diagrammes](#-diagrammes)

---

## 🌐 Vue d'ensemble

**FoE Helper** est une **extension navigateur** (Manifest V3) pour le jeu **Forge of Empires**.

### Objectif
Améliorer l'expérience de jeu en ajoutant des fonctionnalités utiles **sans modifier les données du jeu** :
- Calculateurs (ressources, combat, etc.)
- Alertes et notifications
- Informations statistiques
- Outils de visualisation avancés
- Assistance aux quêtes et événements

### Caractéristiques Techniques
| Aspect | Technologie/Approche |
|--------|---------------------|
| **Type** | Extension navigateur (Chrome/Firefox) |
| **Manifest** | V3 (compatible Firefox via polyfills) |
| **Langage** | JavaScript (ES5/ES6) Vanilla |
| **Build** | Aucun (sources utilisées directement) |
| **Modules** | 71 modules indépendants |
| **Internationalisation** | 9 langues (Chrome i18n API) |
| **Stockage** | chrome.storage + IndexedDB (Dexie) |

---

## 🏗️ Architecture Globale

```
┌─────────────────────────────────────────────────────────────────┐
│                     FORGE OF EMPIRES (Jeu)                          │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  https://*.forgeofempires.com/game                            ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    FOE HELPER (Extension)                           │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  MANIFEST.V3 (manifest.json)                                   ││
│  │  • Permissions : alarms, storage, tabs, notifications        ││
│  │  • Host Permissions : forgeofempires.com, foe-helper.com     ││
│  │  • Content Scripts : foeproxy.js, inject.js                    ││
│  │  • Background : background.js (Service Worker)                ││
│  │  • UI : popup.html                                             ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────┬─────────────────────────┬───────────────┘
                          │                         │                   │
                          ▼                         ▼                   ▼
                ┌─────────────────┐     ┌─────────────────┐   ┌───────────┐
                │ Service Worker  │     │ Content Scripts  │   │   Popup   │
                │ (background.js) │     │ (foeproxy.js)    │   │ (popup)   │
                │                 │     │                 │   │           │
                │ • Alarmes       │     │ • Interception  │   │ • Config  │
                │ • Stockage      │     │ • Injection     │   │ • Paramètres│
                │ • Notifications │     │ • world: MAIN   │   │           │
                └─────────────────┘     └────────┬────────┘   └───────────┘
                                              │
                                              ▼
                                ┌─────────────────────────────┐
                                │       inject.js               │
                                │  • Chargement dynamique       │
                                │  • Détection contexte         │
                                │  • Gestion dépendances        │
                                └─────────────────┬───────────┘
                                                  │
                    ┌─────────────────────────────────────────┼─────────────────────────────────────────┐
                    ▼                                             ▼                                             ▼
            ┌───────────────┐                     ┌───────────────┐                     ┌───────────────┐
            │   Module A    │                     │   Module B    │                     │   Module C    │
            │  (alerts)     │                     │  (market)     │                     │  (greatbuild) │
            │               │                     │               │                     │               │
            │ • Fonction 1 │                     │ • Fonction 1 │                     │ • Fonction 1 │
            │ • Fonction 2 │                     │ • Fonction 2 │                     │ • Fonction 2 │
            └───────────────┘                     └───────────────┘                     └───────────────┘
                    (71 modules au total dans js/web/)
```

---

## 🧩 Composants Principaux

### 1️⃣ Manifest (manifest.json)

**Fichier** : `/manifest.json`
**Rôle** : Configuration de l'extension

#### Configuration clé
```json
{
  "manifest_version": 3,
  "name": "__MSG_appName__",
  "description": "__MSG_appDesc__",
  "version": "4.3.0.0",
  "default_locale": "en",
  
  "permissions": [
    "alarms", "storage", "tabs", "notifications", "clipboardWrite"
  ],
  
  "host_permissions": [
    "https://*.forgeofempires.com/*",
    "https://foe-helper.com/*"
  ],
  
  "background": {
    "service_worker": "background.js"
  },
  
  "content_scripts": [
    {
      "matches": ["https://*.forgeofempires.com/*"],
      "js": ["js/foeproxy.js"],
      "run_at": "document_start",
      "world": "MAIN"
    },
    {
      "matches": ["https://*.forgeofempires.com/*"],
      "js": ["vendor/browser-polyfill.js", "js/web/_languages/js/_languages.js", "js/inject.js"],
      "run_at": "document_end"
    }
  ],
  
  "action": {
    "default_popup": "content/popup.html",
    "default_icon": "images/app16.png"
  }
}
```

### 2️⃣ Service Worker (background.js)

**Fichier** : `/background.js` (20 Ko)
**Rôle** : Tâches en arrière-plan et gestion d'état

#### Responsabilités
- ✅ **Gestion des alarmes** : Planification de tâches périodiques
- ✅ **Stockage** : Utilisation de `chrome.storage.local` et `chrome.storage.sync`
- ✅ **Notifications** : Affichage de notifications système
- ✅ **Base de données** : IndexedDB via Dexie.js
- ✅ **Communication** : Échange de messages entre onglets

#### Exemple de code
```javascript
// Base de données Dexie
let alertsDB = new Dexie("Alerts");
alertsDB.version(1).stores({
  alerts: "++id,[server+playerId],data.expires"
});

// Écoute des messages
chrome.runtime.onMessage.addListener((request, sender, sendResponse) => {
  // Traitement des messages
});
```

### 3️⃣ Content Scripts

#### foeproxy.js
- **Exécution** : `document_start`, `world: MAIN`
- **Rôle** : Interception précoce des requêtes et injection dans le monde MAIN
- **Fonctionnalités** :
  - Interception des requêtes HTTP
  - Bridge entre monde isolé et monde MAIN
  - Préparation du contexte pour inject.js

#### foeproxy-fallback.js
- **Exécution** : `document_start` (sans world: MAIN)
- **Rôle** : Fallback pour les navigateurs ne supportant pas world: MAIN

#### inject.js
- **Exécution** : `document_end`
- **Rôle** : Script d'injection principal
- **Fonctionnalités** :
  - Chargement des dépendances (`vendor/`)
  - Détection du contexte (page du jeu)
  - Chargement dynamique des modules pertinents
  - Gestion des états de chargement

### 4️⃣ Modules Fonctionnels (js/web/)

**Organisation** : 71 modules dans 218 répertoires

#### Catégories principales

| Catégorie | Exemples | Nombre | Description |
|-----------|----------|--------|-------------|
| **Système** | _helper, _i18n, _main, _menu | ~15 | Utilitaires et infrastructure |
| **Jeu** | battle-assist, citymap, greatbuildings | ~20 | Fonctionnalités de gameplay |
| **Économie** | auctions, market, productions | ~10 | Marché et ressources |
| **Quêtes** | quests, recurring-quests | ~8 | Suivi des quêtes |
| **Statistiques** | stats, guildfights | ~10 | Analyse et metrics |
| **Outils** | calculator, planner | ~15 | Calculateurs |
| **UI** | customTooltip, infoboard | ~10 | Améliorations interface |
| **Événements** | eventchests, eventhandler | ~8 | Événements spéciaux |

#### Modules notables
- **MoppelHelper** : Filtres et assistance pour Moppel
- **GreatBuildings** : Gestion des Grands Bâtiments
- **Market** : Amélioration du marché
- **CityMap** : Carte de ville améliorée
- **Calculator** : Divers calculateurs
- **GuildFights** : Combats de guilde

---

## 🔄 Flux de Données

### 1. Chargement de l'extension

```
1. Utilisateur navigue vers Forge of Empires
   ↓
2. Navigateur charge manifest.json
   ↓
3. Chrome/Firefox injecte foeproxy.js (document_start, world: MAIN)
   ↓
4. Chrome/Firefox injecte foeproxy-fallback.js (document_start)
   ↓
5. DOM chargé → browser-polyfill.js se charge
   ↓
6. _languages.js se charge (internationalisation)
   ↓
7. inject.js s'exécute (document_end)
   ↓
8. inject.js charge les dépendances depuis vendor/
   ↓
9. inject.js détecte le type de page
   ↓
10. inject.js charge les modules pertinents
   ↓
11. Modules s'initialisent et ajoutent leur UI/fonctionnalités
```

### 2. Communication entre composants

```
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│   Content Script     │     │   Service Worker     │     │   Page du Jeu        │
│   (foeproxy.js)      │     │   (background.js)    │     │                     │
└──────────┬──────────┘     └──────────┬──────────┘     └──────────┬──────────┘
           │                           │                           │
           │ chrome.runtime.sendMessage  │                           │
           ▼                           ▼                           ▼
    ┌─────────────────────────────────────────────────────────────┐
    │                    ÉCHANGE DE MESSAGES                         │
    │  • Requêtes de données (storage, config)                       │
    │  • Synchronisation d'état entre onglets                        │
    │  • Notifications                                              │
    └─────────────────────────────────────────────────────────────┘
```

### 3. Stockage des données

```
┌─────────────────────────────────────────────────────────────────┐
│                        STOCKAGE                                     │
├─────────────────────────────────────────────────────────────────┤
│  📦 chrome.storage.local       - Préférences utilisateur        │
│  ☁️ chrome.storage.sync        - Synchronisation (si activé)    │
│  🗄️  IndexedDB (Dexie)         - Données volumineuses          │
│     └── Alerts DB              - Alertes planifiées              │
│  💾 LocalStorage               - Cache temporaire              │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📦 Structure des Modules

### Organisation physique

```
js/
├── inject.js                  # Script d'injection principal
├── foeproxy.js                # Proxy pour content scripts
├── foeproxy-fallback.js       # Fallback du proxy
├── alerts.js                  # Logique des alertes (backend)
├── popup.js                   # Logique de la popup
├── internal.json              # Configuration interne
├── vendor.json                # Liste des dépendances
└── web/                       # Modules fonctionnels
    ├── _helper/                # Utilitaires partagés
    │   ├── helper.js
    │   └── ...
    ├── _i18n/                  # Internationalisation
    │   └── ...
    ├── _languages/             # Gestion des langues
    │   └── js/
    │       └── _languages.js
    ├── _main/                  # Modules principaux
    │   └── ...
    ├── _menu/                  # Composants de menu
    │   └── ...
    ├── alerts/                 # Module Alertes (frontend)
    │   ├── alerts.js
    │   └── ...
    ├── auctions/               # Module Enchères
    │   ├── auctions.js
    │   └── ...
    ├── battle-assist/          # Module Combat
    │   └── ...
    └── ... (71 modules)
```

### Structure type d'un module

```
js/web/[nom-module]/
├── [nom-module].js          # Logique principale (REQUIS)
├── [nom-module].html       # Template HTML (optionnel)
├── [nom-module].css        # Styles spécifiques (optionnel)
├── README.md               # Documentation du module (optionnel)
└── assets/                 # Ressources (images, sons, etc.)
    └── ...
```

### Détection et chargement

`inject.js` utilise une logique de détection pour charger les modules :

```javascript
// Exemple de détection dans inject.js
if (isCityPage()) {
  loadModule('citymap');
  loadModule('greatbuildings');
  loadModule('productions');
}

if (isMarketPage()) {
  loadModule('market');
  loadModule('auctions');
}

if (isBattlePage()) {
  loadModule('battle-assist');
}
```

### Ajout d'un nouveau module

**Étapes :**
1. Créer le répertoire : `mkdir js/web/[nom-module]`
2. Créer le fichier principal : `js/web/[nom-module]/[nom-module].js`
3. Ajouter la détection dans `inject.js`
4. S'assurer que les dépendances sont disponibles
5. Tester dans le navigateur

---

## 🛠️ Technologies Utilisées

### Langages et Standards
| Technologie | Version/Info | Utilisation |
|-------------|--------------|-------------|
| **JavaScript** | ES5/ES6 | Langage principal |
| **HTML5** | Standard | UI des modules |
| **CSS3** | Standard | Styles |
| **JSON** | Standard | Configuration, données |

### Bibliothèques tierces (vendor/)

| Bibliothèque | Version | Utilisation |
|--------------|---------|-------------|
| **browser-polyfill** | ? | Polyfill pour Firefox |
| **Dexie** | ? | Wrapper IndexedDB |
| **jQuery** | ? | Manipulation DOM |
| **Bootstrap** | 4.2.1 | Framework CSS |
| **Highcharts** | 8.0.4 | Graphiques |
| **Moment.js** | ? | Manipulation de dates |
| **lit-html** | ? | Templating HTML |
| **Sortable** | ? | Drag & Drop |
| **TableSorter** | ? | Tri de tableaux |

**Total : 20+ bibliothèques** dans `vendor/`

### API Chrome/Firefox
| API | Utilisation |
|-----|-------------|
| `chrome.alarms` | Planification de tâches |
| `chrome.storage` | Stockage local/sync |
| `chrome.tabs` | Accès aux onglets |
| `chrome.notifications` | Alertes système |
| `chrome.i18n` | Internationalisation |
| `chrome.runtime` | Communication, messages |

### Internationalisation
- **9 langues** : de, en, fr, nl, pl, ro, ru, sv
- **Système** : Chrome i18n API
- **Fichiers** : `_locales/[langue]/messages.json`
- **Utilisation** : `__MSG_nom_de_la_chaine__` dans manifest.json et code

---

## 📊 Diagrammes

### Diagramme de flux principal

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Navigateur │────▶│   Manifest  │────▶│ Content     │
└─────────────┘     └─────────────┘     │  Scripts     │
                                      └──────┬──────┘
                                             │
                      ┌──────────────────────┼──────────────────────┐
                      ▼                      ▼                      ▼
              ┌─────────────┐          ┌─────────────┐      ┌─────────────┐
              │ foeproxy.js │          │ inject.js   │      │ background.js│
              │ (MAIN world)│          │ (injection) │      │ (service    │
              └──────┬──────┘          └──────┬──────┘      │  worker)     │
                     │                      │             └──────┬──────┘
                     │                      │                    │
                     └──────────┬───────────┘                    │
                                │                                │
                                ▼                                ▼
                        ┌─────────────────────────────┐     ┌───────────┐
                        │       Modules Chargés         │     │   Storage  │
                        │  (71 modules dans js/web/)    │     │ (chrome.  │
                        └─────────────────────────────┘     │  storage)  │
                                                           └───────────┘
```

### Diagramme des répertoires

```
foe-helfer-extension/
├── manifest.json              # Configuration
├── background.js              # Service Worker
├── inject.js                  # Injection principale
├── foeproxy.js                # Proxy
├── content/                   # HTML
│   ├── popup.html
│   └── alerts.html
├── css/                       # Styles
├── images/                    # Icônes
├── _locales/                  # Traductions (9 langues)
├── js/                        # Code source
│   ├── web/                   # 71 modules
│   │   ├── _helper/
│   │   ├── _i18n/
│   │   ├── _main/
│   │   ├── _menu/
│   │   ├── alerts/
│   │   ├── market/
│   │   └── ...
│   ├── inject.js
│   └── foeproxy.js
├── vendor/                    # Dépendances (20+)
└── manifests/                 # Manifests spécifiques
    ├── chromium/
    └── firefox/
```

---

## 📝 Notes Importantes

### Points forts de l'architecture
✅ **Modularité** : Chaque fonctionnalité est isolée dans son module
✅ **Chargement dynamique** : Seuls les modules nécessaires sont chargés
✅ **Performance** : Pas de framework lourd, Vanilla JS
✅ **Internationalisation** : Système complet avec 9 langues
✅ **Compatibilité** : Fonctionne sur Chromium et Firefox

### Points à améliorer
⚠️ **Gestion des dépendances** : Migrer vers npm/yarn pour une meilleure maintenance
⚠️ **Tests automatisés** : Introduire Jest/Mocha pour tester les modules
⚠️ **Build system** : Ajouter Webpack pour bundling et optimisation
⚠️ **TypeScript** : Migrer progressivement pour plus de robustesse
⚠️ **Documentation** : Compléter la documentation des modules

### Décisions architecturales
| Décision | Justification |
|----------|---------------|
| **Manifest V3** | Compatibilité future, sécurité, moderne |
| **Pas de framework** | Performance, simplicité, légèreté |
| **Chargement dynamique** | Optimisation des ressources |
| **Dexie pour IndexedDB** | API plus simple que l'API native |
| **Pas de build** | Développement simplifié |

---

## 🔗 Références

- [AGENTS.md](../AGENTS.md) — Consignes pour les agents IA
- [.01_Memoire/reference/architecture.md](../.01_Memoire/reference/architecture.md) — Documentation technique détaillée
- [ONBOARDING.md](./ONBOARDING.md) — Guide de prise en main
- [Repository GitHub](https://github.com/mainIine/foe-helfer-extension)

---

*Ce document décrit l'architecture technique réelle du projet.*
*Dernière vérification : 19 octobre 2026*
