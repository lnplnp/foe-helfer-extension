# INDEX.md — Index des compétences pour FoE Helper

*Projet : FoE Helper (foe-helfer-extension)*
*Dernière mise à jour : 19 octobre 2026*

---

## 🎯 À propos de ce répertoire

Ce répertoire contient l'**index des compétences spécialisées** disponibles pour les agents IA travaillant sur le projet **FoE Helper**.

Les compétences sont des **capacités réutilisables** qui permettent d'effectuer des tâches spécifiques de manière efficace et cohérente.

---

## 📚 Comment utiliser les compétences

### Protocole de chargement

1. **Ne pas charger toutes les compétences systématiquement**
2. **Charger une compétence quand elle est pertinente** pour la tâche
3. **Vérifier les prérequis** avant d'utiliser une compétence
4. **Suivre les instructions** de chaque compétence

### Structure type d'une compétence

```
.agent/
└── Skills/
    ├── INDEX.md               # ⭐ CE FICHIER — Index central
    ├── [nom-competence]/
    │   ├── README.md          # Description de la compétence
    │   ├── procedure.md       # Procédure détaillée
    │   ├── examples/          # Exemples d'utilisation
    │   └── templates/         # Modèles réutilisables
    └── [autre-competence]/
```

---

## 🔍 Compétences disponibles

### 📁 **Analyse et Compréhension**

| Compétence | Description | Fichier | Statut |
|------------|-------------|---------|--------|
| **Analyse Architecture** | Analyser l'architecture d'une extension browser | À créer | ⏳ |
| **Analyse Module** | Comprendre un module spécifique | À créer | ⏳ |
| **Cartographie Code** | Cartographier les dépendances entre fichiers | À créer | ⏳ |

### 🛠️ **Développement Extension Browser**

| Compétence | Description | Fichier | Statut |
|------------|-------------|---------|--------|
| **Manifest V3** | Créer/modifier un manifest.json MV3 | À créer | ⏳ |
| **Service Worker** | Développer un service worker | À créer | ⏳ |
| **Content Scripts** | Créer des content scripts efficaces | À créer | ⏳ |
| **Message Passing** | Communication entre composants | À créer | ⏳ |
| **Storage API** | Utiliser chrome.storage | À créer | ⏳ |

### 🌐 **JavaScript et Dépendances**

| Compétence | Description | Fichier | Statut |
|------------|-------------|---------|--------|
| **Vanilla JS** | Développer en JavaScript pur (ES5/ES6) | À créer | ⏳ |
| **jQuery** | Utiliser jQuery pour la manipulation DOM | À créer | ⏳ |
| **Bootstrap** | Intégrer Bootstrap 4.2.1 | À créer | ⏳ |
| **Dexie.js** | Utiliser Dexie pour IndexedDB | À créer | ⏳ |
| **Highcharts** | Créer des graphiques avec Highcharts 8.0.4 | À créer | ⏳ |

### 📦 **Gestion de Projet**

| Compétence | Description | Fichier | Statut |
|------------|-------------|---------|--------|
| **Gestion Modules** | Ajouter/supprimer un module | À créer | ⏳ |
| **Internationalisation** | Ajouter une langue | À créer | ⏳ |
| **Vérification Cohérence** | Vérifier la cohérence du projet | À créer | ⏳ |
| **Documentation** | Documenter une fonctionnalité | À créer | ⏳ |

### 🔍 **Débogage et Tests**

| Compétence | Description | Fichier | Statut |
|------------|-------------|---------|--------|
| **Débogage Extension** | Déboguer une extension browser | À créer | ⏳ |
| **Tests Manuels** | Procédures de test manuel | À créer | ⏳ |
| **Validation Code** | Valider du code JavaScript | À créer | ⏳ |

---

## 🎨 Compétences par scène

### 🎭 **Scène : "Je dois comprendre le projet"

**Objectif** : Comprendre rapidement l'architecture et l'état du projet

**Compétences à charger** :
1. **Analyse Architecture**
2. **Cartographie Code**
3. **Analyse Module** (pour les modules spécifiques)

**Procédure** :
1. Lire `AGENTS.md`
2. Lire `.01_Memoire/reference/architecture.md`
3. Utiliser la compétence **Analyse Architecture**
4. Cartographier les modules clés avec **Cartographie Code**

---

### 🎭 **Scène : "Je dois ajouter une nouvelle fonctionnalité"

**Objectif** : Implémenter une nouvelle fonctionnalité dans l'extension

**Compétences à charger** :
1. **Gestion Modules**
2. **Manifest V3** (si modification du manifest nécessaire)
3. **Vanilla JS** ou **jQuery** (selon le module)
4. **Message Passing** (si communication nécessaire)
5. **Storage API** (si stockage nécessaire)

**Procédure** :
1. Comprendre l'existant avec **Analyse Architecture**
2. Créer le module avec **Gestion Modules**
3. Développer la logique avec **Vanilla JS**
4. Ajouter la détection dans `inject.js`
5. Tester avec **Tests Manuels**

---

### 🎭 **Scène : "Je dois corriger un bug"

**Objectif** : Identifier et corriger un bug dans le code

**Compétences à charger** :
1. **Analyse Module** (pour le module concerné)
2. **Débogage Extension**
3. **Validation Code**

**Procédure** :
1. Reproduire le bug
2. Utiliser **Débogage Extension** pour identifier la source
3. Analyser le module avec **Analyse Module**
4. Corriger avec **Vanilla JS** ou la compétence appropriée
5. Valider avec **Validation Code** et **Tests Manuels**

---

### 🎭 **Scène : "Je dois documenter une partie du code"

**Objectif** : Documenter une fonctionnalité ou un module

**Compétences à charger** :
1. **Analyse Module**
2. **Documentation**
3. **Cartographie Code**

**Procédure** :
1. Comprendre le module avec **Analyse Module**
2. Créer la documentation avec **Documentation**
3. Ajouter les schémas avec **Cartographie Code**
4. Mettre à jour l'index de documentation

---

## 📋 Liste complète des compétences à développer

### Priorité Haute (À créer en premier)

| # | Compétence | Description | Complexité | Temps estimé |
|---|------------|-------------|------------|---------------|
| 1 | **Analyse Architecture** | Analyser l'architecture d'une extension browser | ⭐⭐ | 2h |
| 2 | **Gestion Modules** | Ajouter/supprimer un module dans FoE Helper | ⭐⭐ | 1h |
| 3 | **Manifest V3** | Créer/modifier un manifest.json MV3 | ⭐ | 30min |
| 4 | **Débogage Extension** | Déboguer une extension browser | ⭐⭐ | 1h |
| 5 | **Vanilla JS** | Bonnes pratiques Vanilla JS pour ce projet | ⭐ | 1h |

### Priorité Moyenne

| # | Compétence | Description | Complexité | Temps estimé |
|---|------------|-------------|------------|---------------|
| 6 | **Service Worker** | Développer un service worker pour extensions | ⭐⭐⭐ | 2h |
| 7 | **Content Scripts** | Créer des content scripts efficaces | ⭐⭐⭐ | 2h |
| 8 | **Message Passing** | Communication entre composants d'extension | ⭐⭐⭐ | 1h |
| 9 | **Storage API** | Utiliser chrome.storage.local/sync | ⭐⭐ | 1h |
| 10 | **Internationalisation** | Ajouter une langue à l'extension | ⭐⭐ | 1h |
| 11 | **Cartographie Code** | Cartographier les dépendances | ⭐⭐⭐ | 2h |
| 12 | **Analyse Module** | Comprendre un module spécifique | ⭐⭐ | 1h |

### Priorité Basse

| # | Compétence | Description | Complexité | Temps estimé |
|---|------------|-------------|------------|---------------|
| 13 | **jQuery** | Utiliser jQuery dans le contexte de FoE Helper | ⭐ | 30min |
| 14 | **Bootstrap** | Intégrer Bootstrap 4.2.1 | ⭐ | 30min |
| 15 | **Dexie.js** | Utiliser Dexie pour IndexedDB | ⭐⭐ | 1h |
| 16 | **Highcharts** | Créer des graphiques avec Highcharts | ⭐⭐⭐ | 1h |
| 17 | **Documentation** | Rédiger une documentation technique | ⭐⭐ | 1h |
| 18 | **Validation Code** | Valider du code JavaScript | ⭐⭐ | 1h |
| 19 | **Tests Manuels** | Procédures de test manuel | ⭐⭐ | 1h |

---

## 📝 Modèles de compétences

### Structure d'une compétence

```
.agent/
└── Skills/
    └── [nom-competence]/
        ├── README.md          # Description complète
        ├── procedure.md       # Procédure étape par étape
        ├── prerequisites.md   # Prérequis et dépendances
        ├── examples/          # Exemples concrets
        │   ├── exemple-1.md
        │   └── exemple-2.md
        └── templates/         # Modèles réutilisables
            └── template.js
```

### Contenu type d'un README.md de compétence

```markdown
# [Nom de la Compétence]

*Dernière mise à jour : [date]*
*Version : [x.x]*

## 📌 Description

[Description détaillée de ce que fait la compétence]

## 🎯 Objectifs

- [x] Objectif 1
- [x] Objectif 2
- [ ] Objectif 3 (à venir)

## ✅ Prérequis

- [ ] Connaissance de base en JavaScript
- [ ] Compréhension de l'architecture FoE Helper
- [ ] Accès au code source

## 📋 Procédure

1. **Étape 1** : [Description]
2. **Étape 2** : [Description]
3. **Étape 3** : [Description]

## ⚠️ Avertissements

- ⚠️ [Avertissement 1]
- ⚠️ [Avertissement 2]

## 📚 Ressources

- [Lien vers documentation]
- [Lien vers exemples]

## 📝 Historique

| Date | Modification | Auteur |
|------|--------------|--------|
| [date] | Création initiale | [auteur] |
```

---

## 🔗 Liens utiles

### Documentation du projet
- [AGENTS.md](../../AGENTS.md) — Consignes permanentes
- [.01_Memoire/MANIFEST.md](../.01_Memoire/MANIFEST.md) — Système de mémoire
- [.02_Docs/00_INDEX.md](../.02_Docs/00_INDEX.md) — Documentation technique

### Références externes
- **MDN Web Docs** : [https://developer.mozilla.org/](https://developer.mozilla.org/) — Référence JavaScript/HTML/CSS
- **Chrome Extension Docs** : [https://developer.chrome.com/docs/extensions/](https://developer.chrome.com/docs/extensions/) — Documentation officielle
- **Firefox WebExtensions** : [https://extensionworkshop.com/](https://extensionworkshop.com/) — Documentation Firefox
- **GitHub** : [https://github.com/mainIine/foe-helfer-extension](https://github.com/mainIine/foe-helfer-extension) — Repository du projet

---

## 📊 Statistiques

- **Compétences créées** : 0/19
- **Compétences prioritaires** : 0/5
- **Temps estimé total** : ~20 heures
- **Priorité** : Moyenne (à développer au fur et à mesure des besoins)

---

## 🎯 Prochaines étapes

### Priorité 1 : Créer les compétences de base
1. [ ] **Analyse Architecture** — Pour comprendre rapidement le projet
2. [ ] **Gestion Modules** — Pour ajouter/modifier des modules
3. [ ] **Manifest V3** — Pour gérer le manifest
4. [ ] **Débogage Extension** — Pour résoudre les problèmes
5. [ ] **Vanilla JS** — Pour développer dans le style du projet

### Priorité 2 : Créer les compétences techniques
1. [ ] **Service Worker**
2. [ ] **Content Scripts**
3. [ ] **Message Passing**
4. [ ] **Storage API**

---

## 📝 Historique des modifications

| Date | Modification | Auteur |
|------|--------------|--------|
| 19 oct 2026 | Création initiale de l'index | Agent IA (intégration mémoire) |

---

*Ce fichier est l'index central des compétences pour le projet FoE Helper.*
*Les compétences doivent être créées au fur et à mesure des besoins, pas toutes à la fois.*
*Charger uniquement les compétences pertinentes pour la tâche en cours.*
