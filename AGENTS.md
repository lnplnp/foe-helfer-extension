# AGENTS.md — Consignes permanentes pour les agents IA

*Dernière mise à jour : 19 octobre 2026*
*Projet : FoE Helper (foe-helfer-extension)*

---

## 📌 Rôle et objectifs du projet

**FoE Helper** est une **extension navigateur open source** (Licence AGPLv3) conçue pour le jeu **Forge of Empires** (InnoGames).

### Objectifs principaux
- ✅ **Améliorer l'expérience de jeu** : Ajouter des fonctionnalités utiles et des informations supplémentaires
- ✅ **Respecter les règles du jeu** : Aucune triche, uniquement des outils d'assistance légale
- ✅ **Multi-navigateurs** : Compatible avec tous les navigateurs Chromium (Chrome, Edge, Brave, Opera) et Firefox
- ✅ **Internationalisation** : Support de 9 langues (allemand, anglais, français, néerlandais, polonais, roumain, russe, suédois)

### Public cible
- Joueurs de Forge of Empires souhaitant optimiser leur expérience
- Développeurs contributeurs open source
- Mainteneurs du projet

---

## 🏗️ Architecture et conventions techniques

### Structure du projet
```
foe-helfer-extension/
├── manifest.json              # Manifest V3 (point d'entrée)
├── background.js              # Service Worker (gestion alarms, storage)
├── inject.js                  # Script d'injection principal
├── foeproxy.js                # Proxy pour content scripts (world: MAIN)
├── foeproxy-fallback.js       # Fallback du proxy
├── js/
│   └── web/                   # 71 modules organisés en 218 répertoires
│       ├── _helper/           # Utilitaires partagés
│       ├── _i18n/            # Système d'internationalisation
│       ├── _languages/        # Gestion des langues
│       ├── _main/             # Modules principaux
│       ├── _menu/             # Composants UI
│       └── [50+ modules]      # Fonctionnalités par domaine
├── content/                   # HTML (popup, alerts)
├── css/                       # Styles CSS
├── images/                    # Ressources graphiques
├── _locales/                  # Traductions (9 langues)
├── vendor/                    # Dépendances tierces (20+ bibliothèques)
└── manifests/                 # Manifests spécifiques (Chromium, Firefox)
```

### Technologies utilisées
| Catégorie | Technologie | Version/Info |
|-----------|-------------|--------------|
| **Langage** | JavaScript (ES5/ES6) | Vanilla JS, pas de TypeScript |
| **Manifest** | Chrome Extension MV3 | Compatible Firefox via polyfills |
| **Build** | Aucun | Pas de bundler (Webpack/Rollup) |
| **Gestionnaire** | Aucun | Pas de npm/yarn, dépendances manuelles |
| **Stockage** | IndexedDB | Via Dexie.js |
| **UI** | jQuery + Bootstrap 4.2.1 | Dans vendor/ |
| **Graphiques** | Highcharts 8.0.4 | Dans vendor/ |
| **Internationalisation** | Chrome i18n API | Fichiers JSON dans _locales/ |
| **Polyfills** | browser-polyfill | Pour compatibilité Firefox |

### Conventions de code
- ⚠️ **Copyright** : Chaque fichier commence par un bloc de commentaire standardisé avec licence AGPLv3
- ⚠️ **Nommage** : Pas de convention stricte identifiée, mais les modules sont groupés par fonctionnalité
- ⚠️ **Internationalisation** : Utilisation de `__MSG_` pour les chaînes traduites
- ⚠️ **Structure** : Chaque module est généralement autonome dans son répertoire
- ⚠️ **Dépendances** : Les bibliothèques tierces sont dans `vendor/` et référencées dans `vendor.json`

### Points d'entrée
1. **manifest.json** - Configuration de l'extension (permissions, content scripts)
2. **background.js** - Service Worker (20 Ko, gestion des alarmes, stockage)
3. **inject.js** - Script d'injection principal (chargement dynamique des modules)
4. **foeproxy.js** - Content script exécuté à `document_start` (world: MAIN)
5. **content/popup.html** - Interface utilisateur de l'extension

---

## 🛠️ Commandes et environnement de développement

### Prérequis
- **Navigateur** : Chromium (Chrome, Edge, Brave, Opera) ou Firefox
- **Éditeur** : VS Code recommandé (fichier `.code-workspace` disponible)
- **Dépendances** : Aucune installation npm/yarn nécessaire
- **Dépôt** : Git (branche principale : `master`)

### Workflow de développement

#### 1. Installation pour développement
```bash
# Cloner le dépôt
cd G:/Mon Drive/Forge Of Empires
# Ou utiliser le dépôt existant

# Charger l'extension dans le navigateur
# Chrome/Edge : chrome://extensions/ → Charger l'extension non empaquetée
# Firefox : about:debugging → Charger un module complémentaire temporaire
```

#### 2. Structure des modules
- Chaque fonctionnalité est dans `js/web/[nom-du-module]/`
- Les modules sont chargés dynamiquement via `inject.js`
- Pour ajouter un nouveau module :
  - Créer un répertoire dans `js/web/`
  - Ajouter le script principal (ex: `js/web/[module]/[module].js`)
  - Configurer le chargement dans `inject.js` ou via le système de détection

#### 3. Internationalisation
- Les chaînes de traduction sont dans `_locales/[langue]/messages.json`
- Utiliser `__MSG_nom_de_la_chaine__` dans le code
- 9 langues supportées : de, en, fr, nl, pl, ro, ru, sv
- Ajouter une nouvelle langue : créer le répertoire et le fichier messages.json

#### 4. Gestion des dépendances
- **Aucun gestionnaire de paquets** (npm/yarn)
- Les dépendances sont copiées manuellement dans `vendor/`
- `vendor.json` contient la liste des dépendances
- Pour ajouter une dépendance :
  - Télécharger les fichiers dans `vendor/[nom-de-la-bibliotheque]/`
  - Mettre à jour `vendor.json`
  - Mettre à jour `.gitignore` si nécessaire

#### 5. Build et déploiement
- **Aucun processus de build** : les sources sont utilisées directement
- Pour la production :
  - Vérifier que tous les fichiers sont à jour
  - Tester dans le navigateur cible
  - Créer un zip des fichiers (sauf .git, vendor/ si déjà inclus)
- **Versionnement** : Mettre à jour `manifest.json` (champ `version`)
- **Changelog** : Mettre à jour `changelog-en.md` et `changelog-de.md`

#### 6. Tests
- **Aucun framework de test** identifié
- Tests manuels recommandés :
  - Vérifier le chargement de l'extension
  - Tester chaque module dans le jeu
  - Vérifier l'internationalisation
  - Tester les fonctionnalités de stockage

---

## 🔒 Règles de sécurité et de préservation

### ❌ INTERDIT — Ne JAMAIS faire
- [ ] **Modifier le code applicatif** sans raison impérative et validation explicite
- [ ] **Supprimer ou renommer** des fichiers existants sans autorisation
- [ ] **Réécrire** un module pour des raisons esthétiques
- [ ] **Installer des dépendances** npm/yarn sans validation
- [ ] **Modifier manifest.json** sans tester la compatibilité
- [ ] **Changer la structure** des répertoires sans justification
- [ ] **Ignorer les conventions** existantes (copyright, structure, etc.)
- [ ] **Exécuter des scripts** inconnus ou non vérifiés
- [ ] **Pousser directement sur master** sans pull request ou validation
- [ ] **Forcer un push** (`--force`) sans raison critique

### ✅ AUTORISÉ — Sous conditions
- [x] **Créer** de nouveaux fichiers de documentation/mémoire
- [x] **Modifier** des fichiers de documentation existants (après analyse)
- [x] **Ajouter** de nouveaux modules (dans `js/web/`) avec validation
- [x] **Mettre à jour** des dépendances dans `vendor/` avec justification
- [x] **Corriger** des bugs avec preuves et tests
- [x] **Améliorer** la documentation technique

### ⚠️ OPÉRATIONS À RISQUE — Autorisation requise
| Opération | Risque | Autorisation requise |
|-----------|--------|----------------------|
| Modifier `manifest.json` | Peut casser l'extension | Validation explicite |
| Modifier `background.js` | Peut affecter le service worker | Validation explicite |
| Modifier `inject.js` | Peut casser le chargement des modules | Validation explicite |
| Modifier `foeproxy.js` | Peut casser l'injection | Validation explicite |
| Installer une dépendance npm | Peut introduire des conflits | Validation explicite |
| Exécuter un script de build | Effets inconnus | Validation explicite |
| Déployer en production | Impact utilisateurs | Validation explicite + tests |
| Modifier la structure des répertoires | Peut casser les chemins | Validation explicite |

---

## 📚 Emplacements de la documentation et de la mémoire

### Hiérarchie documentaire
```
foe-helfer-extension/
├── AGENTS.md                  # ⭐ CE FICHIER — Consignes permanentes
├── .01_Memoire/              # 🧠 Système de mémoire
│   ├── MANIFEST.md            # Index des éléments de mémoire
│   ├── court-terme/           # Travaux en cours
│   │   └── tache-courante.md   # État actuel des tâches
│   ├── journalier/            # Journaux par date (AAAA-MM-JJ.md)
│   └── reference/             # Informations stables
│       └── architecture.md    # Architecture technique
│       └── decisions.md       # Décisions techniques importantes
│       └── lecons-apprises.md # Retours d'expérience
├── .02_Docs/                 # 📖 Documentation technique
│   ├── 00_INDEX.md            # Point d'entrée
│   ├── ONBOARDING.md          # Guide de prise en main
│   ├── ARCHITECTURE.md        # Architecture détaillée
│   ├── DEVELOPMENT.md         # Guide de développement
│   └── EXPLOITATION.md        # Déploiement et maintenance
└── .agent/                    # 🎯 Compétences spécialisées
    └── Skills/                # Index des compétences
        └── INDEX.md
```

### Protocole de chargement de la mémoire
1. **Toujours lire** `AGENTS.md` en premier
2. **Consulter** `.01_Memoire/MANIFEST.md` pour l'index
3. **Lire** `.01_Memoire/court-terme/tache-courante.md` pour le contexte
4. **Examiner** les fichiers pertinents pour la tâche
5. **Vérifier** que les informations sont à jour

---

## 🎯 Protocole de début et de fin de session

### ➕ **Début de session** (OBLIGATOIRE)

```
1. ✅ Lire AGENTS.md (ce fichier)
2. ✅ Consulter .01_Memoire/MANIFEST.md
3. ✅ Lire .01_Memoire/court-terme/tache-courante.md
4. ✅ Examiner le journal récent (.01_Memoire/journalier/)
5. ✅ Vérifier l'état Git :
   - git status
   - git branch
   - git log --oneline -5
6. ✅ Vérifier les modifications préexistantes
7. ✅ Comprendre la tâche demandée
8. ✅ Identifier les fichiers concernés
```

### ➖ **Fin de session** (OBLIGATOIRE après intervention)

```
1. ✅ Mettre à jour .01_Memoire/court-terme/tache-courante.md
2. ✅ Enregistrer les décisions dans .01_Memoire/reference/decisions.md
3. ✅ Documenter les enseignements dans .01_Memoire/reference/lecons-apprises.md
4. ✅ Créer un journal si intervention significative (.01_Memoire/journalier/AAAA-MM-JJ.md)
5. ✅ Lister les fichiers modifiés
6. ✅ Indiquer les tests exécutés et leurs résultats
7. ✅ Signaler les vérifications manquantes
8. ✅ Préparer la todo list pour la prochaine session
```

---

## ⚠️ Limitations connues et précautions

### Limitations de l'environnement
- **Pas de Node.js** : Impossible d'exécuter npm/yarn/package.json
- **Pas de tests automatisés** : Validation manuelle nécessaire
- **Dépendances manuelles** : Risque de versions obsolètes dans vendor/
- **Complexité** : 71 modules avec interdépendances implicites
- **Pas de build system** : Les modifications sont directes sur les sources

### Précautions spécifiques
- **Tester systématiquement** après toute modification
- **Vérifier la compatibilité** Chrome ET Firefox
- **Respecter les permissions** dans manifest.json
- **Ne pas modifier** les traductions sans validation native
- **Préserver les conventions** de copyright en tête de fichier
- **Documenter** toute modification importante

### Problèmes préexistants identifiés
- Fichier `.code-workspace` non suivi dans Git (à régulariser)
- Pas de documentation technique interne
- Risque de désynchronisation entre docs.foe-helper.com et le code

---

## 📞 Contacts et ressources externes

### Repository
- **GitHub** : https://github.com/mainIine/foe-helfer-extension
- **Issues** : https://github.com/mainIine/foe-helfer-extension/issues
- **Pull Requests** : Contributions bienvenues

### Documentation
- **Site officiel** : https://docs.foe-helper.com/
- **Wiki** : À vérifier si existe

### Communauté
- **Discord** : https://discord.gg/z97KZq4 (support et questions)
- **Forums** : À identifier

### Maintenance
- **Licence** : AGPLv3 (GNU Affero General Public License)
- **Copyright** : FoE-Helper team

---

## 📝 Historique des modifications de ce fichier

| Date | Modification | Auteur |
|------|--------------|--------|
| 19 oct 2026 | Création initiale | Agent IA (session d'intégration) |

---

*Ce fichier doit être lu avant toute intervention sur le projet.*
*Toute modification de ce fichier nécessite une validation explicite.*
