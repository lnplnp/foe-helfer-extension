# Plan type — Spécification Technique Détaillée (STD) par Module
# FoE Helper — [Nom du Module]

*Modèle adapté pour documenter chaque module de l'extension FoE Helper*
*À utiliser pour la rétro-documentation assistée par IA*

---

## 📋 Utilisation de ce template

**Pour chaque module dans `js/web/[nom-du-module]/`** :
1. Copier ce fichier dans `.02_Docs/STD/_modules/` sous le nom `STD-[NOM-DU-MODULE].md`
2. Remplir chaque section en analysant le code source du module
3. Ajouter des extraits de code pertinents
4. Lier à la STD globale pour l'architecture générale

**Exemple de naming** :
- `STD-MoppelHelper.md`
- `STD-ProductionsRating.md`
- `STD-CityView.md`

---

---

## 1. Identification du document

- **Titre** : STD – Module [NomDuModule]
- **Identifiant** : STD-FOE-MODULE-[NOM]-[VERSION] (ex: STD-FOE-MODULE-MoppelHelper-V1.0)
- **Version** : 1.0
- **Statut** : Brouillon / En relecture / Validé / Obsolète
- **Date de création** : AAAA-MM-JJ
- **Dernière mise à jour** : AAAA-MM-JJ

### Historique des versions

| Version | Date | Auteur | Modification | Lien PR/Commit |
|---------|------|--------|--------------|----------------|
| 1.0 | AAAA-MM-JJ | [Nom] | Création initiale | [lien] |

### Responsables

- **Auteur** : [Nom ou "Généré par IA + relecture humaine"]
- **Relecteur** : [À désigner]
- **Valideur** : [À désigner]

### Liens

- **Code source** : `js/web/[nom-du-module]/`
- **STD Globale** : [`../STD-FoE-Helper-Globale.md`](STD-FoE-Helper-Globale.md)
- **SFD** : [Lien vers spécification fonctionnelle si existe]
- **Dépôt** : [Lien vers le répertoire du module]

---

## 2. Contexte et objectifs du module

### 2.1 Contexte général

- **Positionnement** : [Ce module fait partie de quelle fonctionnalité globale ?]
- **Problématique** : [Quel problème ou besoin il adresse ?]
- **Public cible** : [Quels utilisateurs en bénéficient ?]

### 2.2 Objectifs spécifiques

- **Fonction principale** : [En une phrase, que fait ce module ?]
- **Fonctions secondaires** : [Liste des fonctionnalités additionnelles]
- **Bénéfices utilisateur** : [Impact sur l'expérience de jeu]

### 2.3 Périmètre fonctionnel

| Élément | Inclus | Exclus | Justification |
|---------|--------|--------|--------------|
| [Fonctionnalité A] | ✅ | | |
| [Fonctionnalité B] | | ❌ | [Raison] |

### 2.4 Dépendances externes

- **Modules FoE Helper** : [Liste des autres modules requis]
- **Bibliothèques tierces** : [Dans vendor/, ex: jQuery, Highcharts]
- **API navigateur** : [chrome.storage, chrome.alarms, etc.]
- **API du jeu FoE** : [Endpoints ou sélecteurs DOM utilisés]

---

## 3. Architecture du module

### 3.1 Structure des fichiers

```
js/web/[nom-du-module]/
├── [nom-du-module].js          # Point d'entrée principal
├── [nom-du-module].ui.js       # Gestion de l'interface (si applicable)
├── [nom-du-module].helper.js   # Fonctions utilitaires
├── [nom-du-module].css         # Styles spécifiques
├── config.json                 # Configuration (si applicable)
└── README.md                   # Documentation inline (si existe)
```

### 3.2 Points d'entrée

- **Fichier principal** : `[nom-du-module].js`
- **Fonction d'initialisation** : [Nom de la fonction, ex: `init()`, `start()`]
- **Déclencheur** : [Quand le module est chargé : détection DOM, événement, etc.]

### 3.3 Diagramme des composants

```
┌─────────────────────────────────────────┐
│              [NomDuModule]                │
├─────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────────┐ │
│  │   Core       │  │   UI             │ │
│  │  (logic)     │  │  (interface)      │ │
│  └──────┬───────┘  └──────────────────┘ │
│         │                              │
│  ┌──────▼───────┐  ┌──────────────────┐ │
│  │  Helpers     │  │   Data           │ │
│  │  (utils)     │  │  (stockage)       │ │
│  └──────────────┘  └──────────────────┘ │
└─────────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────────┐
│             Interactions                   │
│  ┌──────────────┐  ┌──────────────────┐ │
│  │   FoE DOM    │  │ Autres modules    │ │
│  └──────────────┘  └──────────────────┘ │
└─────────────────────────────────────────┘
```

### 3.4 Responsabilités des composants

| Composant | Fichier | Responsabilité | Dépendances |
|-----------|---------|----------------|--------------|
| Core | `[module].js` | [Description] | [Liste] |
| UI | `[module].ui.js` | [Description] | [Liste] |
| Helpers | `[module].helper.js` | [Description] | [Liste] |

---

## 4. Intégration avec FoE Helper

### 4.1 Mécanisme de chargement

- **Détection** : [Comment le module est détecté comme nécessaire ?]
  - Ex: `if (document.querySelector('.some-foe-class'))`
- **Chargement** : [Synchronement/Asynchrone, via inject.js ?]
- **Conditions** : [Quelles conditions doivent être remplies ?]

### 4.2 Communication avec d'autres modules

| Module | Type de communication | Méthode | Données échangées |
|--------|----------------------|---------|-------------------|
| [Module A] | Événement | `CustomEvent` | [Données] |
| [Module B] | Message | `chrome.runtime.sendMessage` | [Données] |
| [Module C] | Storage | `chrome.storage.local` | [Données] |

### 4.3 Dépendances vers d'autres modules

- **Modules requis** : [Liste des modules qui doivent être chargés avant]
- **Ordre de chargement** : [Séquence si importante]
- **Conflits connus** : [Problèmes d'incompatibilité]

---

## 5. Stockage des données

*Seulement si le module utilise IndexedDB ou chrome.storage*

### 5.1 Stores utilisés

| Store | Description | Utilisation dans ce module |
|-------|-------------|----------------------------|
| [store] | [Description globale] | [Lecture/Écriture, quelle donnée] |

### 5.2 Schéma des données

#### Store : [nom-du-store]

```javascript
// Structure typique
{
  // Champ 1
  champ1: {
    type: 'string',
    description: 'Description du champ',
    example: 'valeur exemple'
  },
  // Champ 2
  champ2: {
    type: 'number',
    default: 0,
    description: 'Valeur numérique'
  },
  // Champ 3
  champ3: {
    type: 'object',
    properties: {
      sousChamp1: 'string',
      sousChamp2: 'boolean'
    }
  }
}
```

### 5.3 Opérations de stockage

| Opération | Méthode | Fréquence | Données concernées |
|-----------|---------|-----------|-------------------|
| Lire config | `dexie.[store].get()` | Au chargement | Settings utilisateur |
| Sauvegarder | `dexie.[store].put()` | À chaque modification | State du module |

### 5.4 Données spécifiques au module

- **Clé de stockage** : [Préfixe ou clé utilisée, ex: `moppelHelper_settings`]
- **Structure** : [Format complet des données]
- **Durée de vie** : [Permanente/Temporaire, nettoyage ?]

---

## 6. Fonctionnalités et traitements

### 6.1 Liste des fonctionnalités

| ID | Nom | Description | Trigger |
|----|-----|-------------|---------|
| FCT-001 | [Nom] | [Description] | [Événement/DOM/Manuel] |
| FCT-002 | [Nom] | [Description] | [Événement/DOM/Manuel] |

### 6.2 Flux principaux

#### Flux : [Nom du flux]

```
1. [Étape 1] → [Action/Événement]
   - Code : [Extrait pertinent]
   - Résultat : [Effet]

2. [Étape 2] → [Action/Événement]
   - Condition : [Si applicable]
   - Code : [Extrait]

3. [Étape 3] → [Action/Événement]
   - Résultat final : [Effet visible/utilisateur]
```

### 6.3 Algorithmes complexes

#### Algorithme : [Nom]

**Description** : [À quoi sert cet algorithme ?]

**Pseudo-code** :
```javascript
function nomAlgorithme(param1, param2) {
  // Étape 1
  let result = initialValue;
  
  // Étape 2 : Traitement principal
  for (let item of param1) {
    result = processItem(item, param2);
  }
  
  // Étape 3
  return optimize(result);
}
```

**Complexité** : [O(n), O(n²), etc.]

**Optimisations** : [Techniques utilisées pour améliorer les performances]

### 6.4 Points d'extension

- **Hooks disponibles** : [Points où d'autres modules peuvent s'intégrer]
- **Événements émis** : [CustomEvents ou messages envoyés]
- **API publique** : [Fonctions exposées aux autres modules]

---

## 7. Interfaces avec le jeu FoE

### 7.1 Sélecteurs DOM utilisés

| Sélecteur | Type | Rôle | Exemple |
|-----------|------|------|---------|
| `.foe-class` | CSS | [Description] | `<div class="foe-class">` |
| `#foe-id` | ID | [Description] | `<span id="foe-id">` |
| `[data-foe]` | Attribute | [Description] | `<div data-foe="value">` |

### 7.2 Événements DOM écoutés

| Événement | Sélecteur | Action | Données extraites |
|-----------|-----------|--------|-------------------|
| `click` | `.button-class` | [Action] | [Données] |
| `mouseover` | `.tooltip-trigger` | [Action] | [Données] |
| `foe:customEvent` | `document` | [Action] | [Données] |

### 7.3 Manipulation du DOM

#### Éléments créés

| Élément | Sélecteur | Contenu | Style |
|---------|-----------|---------|-------|
| `div` | `.foe-helper-[module]` | [Description] | [Classes CSS] |
| `button` | `#[module]-button` | [Label] | [Style] |

#### Modifications du DOM existant

| Action | Cible | Modification | Impact |
|--------|-------|--------------|--------|
| Ajouter classe | `.existing` | `foe-helper-enhanced` | [Effet visuel] |
| Modifier texte | `#resource-value` | Afficher valeur calculée | [Effet] |

### 7.4 Extraction de données

#### Données extraites du DOM

| Donnée | Sélecteur | Format | Fréquence |
|--------|-----------|--------|-----------|
| Ressources | `.resource-value` | Number | Toutes les 5s |
| Bâtiments | `.building-item` | Object | Au chargement |

#### Exemple de code d'extraction

```javascript
function extraireRessources() {
  const ressources = {};
  
  // Exemple : extraction des ressources depuis le DOM de FoE
  document.querySelectorAll('.resource-item').forEach(item => {
    const type = item.dataset.type;
    const valeur = parseInt(item.textContent);
    ressources[type] = valeur;
  });
  
  return ressources;
}
```

---

## 8. Interfaces avec Chrome Extension API

### 8.1 Storage

- **Type utilisé** : `chrome.storage.local` / `chrome.storage.sync`
- **Clés utilisées** : [Liste des clés spécifiques au module]
- **Structure** : [Format des données stockées]

### 8.2 Message Passing

#### Messages envoyés

| Message | Destination | Données | Fréquence |
|---------|-------------|---------|-----------|
| `[module]:action` | background.js | [Payload] | [Quand] |

#### Messages reçus

| Message | Source | Action | Données attendues |
|---------|--------|--------|------------------|
| `[other]:notification` | [Module] | [Réaction] | [Payload] |

### 8.3 Alarms (si applicable)

- **Alarmes créées** : [Liste des alarms spécifiques au module]
- **Fréquence** : [Toutes les X minutes/heures]
- **Action** : [Ce que fait l'alarme]

---

## 9. Gestion des erreurs

### 9.1 Erreurs spécifiques au module

| Erreur | Cause | Détection | Traitement | Logging |
|--------|-------|-----------|------------|---------|
| [Nom] | [Condition] | [Code de détection] | [Action corrective] | [Niveau: ERROR/WARN] |

### 9.2 Erreurs courantes dans le contexte FoE

| Scénario | Erreur | Solution implémentée |
|----------|--------|----------------------|
| FoE non chargé | `document.querySelector` retourne null | Vérification pré-chargement + retry |
| Élément DOM manquant | Sélecteur introuvable | Gestion graceful degradation |
| Données FoE modifiées | Structure DOM changée | Fallback + notification utilisateur |

### 9.3 Validation des données

- **Entrées utilisateur** : [Si applicable, comment sont-elles validées ?]
- **Données extraites** : [Vérification des données depuis le DOM]
- **Données stockées** : [Validation avant sauvegarde]

---

## 10. Performance

### 10.1 Points de performance critiques

| Opération | Temps cible | Optimisation | Mesure actuelle |
|-----------|-------------|--------------|----------------|
| Chargement initial | < 200ms | Lazy loading | [Valeur] |
| Calcul [X] | < 50ms | Cache | [Valeur] |
| Rendu UI | < 100ms | Virtual DOM | [Valeur] |

### 10.2 Optimisations implémentées

- **Lazy loading** : [Chargement à la demande]
- **Caching** : [Données mises en cache]
- **Debouncing** : [Pour les événements fréquents]
- **Throttling** : [Pour les calculs intensifs]

### 10.3 Consommation mémoire

- **Estimation** : [X Ko/Mo par instance]
- **Fuite mémoire** : [Risques identifiés et préventions]
- **Nettoyage** : [Mécanismes de cleanup]

---

## 11. Sécurité

### 11.1 Permissions requises

- **Permissions spécifiques** : [Liste des permissions dans manifest.json pour ce module]
- **Justification** : [Pourquoi chaque permission est nécessaire]

### 11.2 Sécurité DOM

- **Prévention XSS** : [Méthodes utilisées]
  - ✅ `textContent` au lieu de `innerHTML`
  - ✅ Sanitization des inputs
  - ✅ Échappement des données
- **Sélecteurs sûrs** : [Éviter les sélecteurs trop larges]

### 11.3 Données sensibles

- **Collecte** : ❌ Aucune donnée personnelle
- **Stockage** : ✅ Local uniquement
- **Partage** : ❌ Aucun partage externe

---

## 12. Internationalisation (i18n)

*Seulement si le module a des chaînes à traduire*

### 12.1 Chaînes de traduction

| ID | Texte EN | Texte FR | Contexte |
|----|----------|----------|----------|
| `[module]_title` | "Title" | "Titre" | [Où s'affiche] |
| `[module]_button` | "Click" | "Cliquer" | [Bouton] |

### 12.2 Utilisation dans le code

```javascript
// Dans HTML
<button>__MSG_[module]_button__</button>

// Dans JS
const title = chrome.i18n.getMessage('[module]_title');
```

### 12.3 Fichiers de traduction

- **Fichiers concernés** : `_locales/[lang]/messages.json`
- **Clés à ajouter** : [Liste des clés spécifiques au module]

---

## 13. Tests

### 13.1 Cas de test

#### Cas nominaux

| ID | Description | Étapes | Résultat attendu |
|----|-------------|--------|------------------|
| TC-N-001 | [Nom] | 1. [Action] 2. [Action] | [Résultat visible] |

#### Cas limites

| ID | Description | Condition | Résultat attendu |
|----|-------------|-----------|------------------|
| TC-L-001 | [Nom] | [Condition extrême] | [Comportement] |

#### Cas d'erreur

| ID | Description | Erreur simulée | Comportement attendu |
|----|-------------|----------------|----------------------|
| TC-E-001 | [Nom] | [Comment provoquer] | [Message d'erreur/Logs] |

### 13.2 Procédure de test manuel

1. **Prérequis** :
   - Extension chargée
   - Jeu FoE ouvert
   - [Autres conditions]

2. **Étapes** :
   - [Étape 1]
   - [Étape 2]
   - [Étape 3]

3. **Vérifications** :
   - [Vérification 1] dans la console
   - [Vérification 2] visuelle
   - [Vérification 3] dans chrome.storage

### 13.3 Outils de débogage

- **Console** : Messages de log à vérifier
- **Storage** : Données à inspecter
- **Network** : Requêtes à surveiller (si applicable)

---

## 14. Configuration

### 14.1 Paramètres configurables

| Paramètre | Type | Valeur par défaut | Description |
|-----------|------|------------------|-------------|
| `enabled` | boolean | true | Active/désactive le module |
| `interval` | number | 5000 | Fréquence de rafraîchissement (ms) |

### 14.2 Fichier de configuration

- **Format** : JSON / Objet JavaScript
- **Emplacement** : [Dans chrome.storage / IndexedDB / fichier dédié]
- **Exemple** :
  ```json
  {
    "enabled": true,
    "interval": 5000,
    "options": {
      "showTooltips": true,
      "colorTheme": "dark"
    }
  }
  ```

### 14.3 UI de configuration

- **Emplacement** : [Où l'utilisateur peut configurer : popup, settings panel]
- **Champs** : [Liste des contrôles disponibles]
- **Validation** : [Comment les valeurs sont validées]

---

## 15. Dépendances du module

### 15.1 Dépendances internes (FoE Helper)

| Module | Version min. | Utilisation |
|--------|--------------|-------------|
| [Module A] | [X.Y.Z] | [À quoi il sert] |

### 15.2 Dépendances externes (vendor/)

| Bibliothèque | Version | Fichier | Utilisation |
|-------------|---------|---------|-------------|
| jQuery | 3.x | vendor/jquery/ | Manipulation DOM |
| Highcharts | 8.0.4 | vendor/highcharts/ | Graphiques |

### 15.3 Dépendances implicites

- **API navigateur** : [chrome.storage, chrome.alarms, etc.]
- **Fonctionnalités FoE** : [Quelles parties du jeu sont nécessaires]

---

## 16. Exigences non fonctionnelles

### 16.1 Contraintes techniques

- **Navigateurs** : Chrome, Edge, Brave, Opera, Firefox
- **Manifest** : V3 compatible (avec polyfills)
- **ES Version** : ES5/ES6

### 16.2 Contraintes de performance

- **Temps de réponse** : < 100ms pour les opérations critiques
- **Mémoire** : < 10 Mo par module
- **CPU** : Pas de blocage du thread principal

### 16.3 Contraintes d'accessibilité

- [Si applicable : compatibilité clavier, lecteurs d'écran, etc.]

---

## 17. Documentation et maintenance

### 17.1 Documentation inline

- **Commentaires dans le code** : [Niveau de commentaires existants]
- **JSDoc** : [Utilisation de JSDoc pour les fonctions]

### 17.2 Maintenance

- **Fréquence de mise à jour** : [Combien souvent ce module change]
- **Risques de casse** : [Qu'est-ce qui peut casser ce module]
  - Changement du DOM de FoE
  - Mise à jour de Chrome API
  - Modification d'autres modules

### 17.3 Points d'attention

- **⚠️ Fragilité** : [Parties du code sensibles aux changements de FoE]
- **⚠️ Performance** : [Opérations coûteuses à surveiller]
- **⚠️ Sécurité** : [Points de vigilance]

---

## 18. Annexes

### A. Extraits de code clés

#### [Nom de l'extrait]

```javascript
// Code important à documenter
function exempleFonction(param) {
  // Implémentation
  return result;
}
```

### B. Schémas

[Diagrammes Mermaid si utile]

```mermaid
graph LR
    A[Événement FoE] --> B[Détection par le module]
    B --> C[Traitement]
    C --> D[Affichage/Mise à jour]
```

### C. Glossaire spécifique

| Terme | Définition |
|-------|------------|
| [Terme] | [Définition dans le contexte du module] |

---

## 📝 Checklist de complétion

- [ ] 1. Identification : ✅ Complété
- [ ] 2. Contexte : ✅ Complété
- [ ] 3. Architecture : ✅ Complété
- [ ] 4. Intégration : ✅ Complété
- [ ] 5. Stockage : [✅/❌] Complété (si applicable)
- [ ] 6. Fonctionnalités : ✅ Complété
- [ ] 7. Interfaces FoE : ✅ Complété
- [ ] 8. Interfaces Chrome API : [✅/❌] Complété
- [ ] 9. Gestion des erreurs : ✅ Complété
- [ ] 10. Performance : [✅/❌] Complété
- [ ] 11. Sécurité : ✅ Complété
- [ ] 12. i18n : [✅/❌] Complété (si applicable)
- [ ] 13. Tests : ✅ Complété
- [ ] 14. Configuration : [✅/❌] Complété
- [ ] 15. Dépendances : ✅ Complété
- [ ] 16. Exigences non fonctionnelles : ✅ Complété
- [ ] 17. Documentation : ✅ Complété
- [ ] Relecture : ⏳ En attente
- [ ] Validation : ⏳ En attente

---

## 📌 Notes pour le rédacteur

### Conseils pour la rétro-documentation de ce module

1. **Commencez par le code** : Lisez `js/web/[nom-du-module]/[nom-du-module].js` en premier
2. **Identifiez les points d'entrée** : Où et comment le module est initialisé
3. **Suivez les flux** : Comment les données circulent dans le module
4. **Notez les dépendances** : Quels autres modules ou bibliothèques sont utilisés
5. **Documentez les choix** : Pourquoi cette implémentation plutôt qu'une autre ?
6. **Ajoutez des exemples** : Extraits de code > longues descriptions
7. **Soyez honête sur les limites** : Fragilités, performances, risques

### Ressources utiles

- **Code source** : `js/web/[nom-du-module]/`
- **STD Globale** : [`../STD-FoE-Helper-Globale.md`](STD-FoE-Helper-Globale.md)
- **Architecture** : [`../../.01_Memoire/reference/architecture.md`](../../.01_Memoire/reference/architecture.md)
- **GitHub** : [Lien vers le répertoire du module]

---

*Ce template est optimisé pour la rétro-documentation des modules FoE Helper.*
*Adaptez-le selon les spécificités du module que vous documentez.*
