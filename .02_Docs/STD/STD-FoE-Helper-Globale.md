# STD Globale — FoE Helper

*Spécification Technique Détaillée de l'architecture globale*
*Projet : FoE Helper (foe-helfer-extension)*

---

## 📋 Plan du document (Modèle : STD-Template-FoE-Helper.md)

1. [Identification du document](#1-identification-du-document)
2. [Contexte et objectifs](#2-contexte-et-objectifs)
3. [Découpage technique](#3-découpage-technique)
4. [Modèle de stockage client (IndexedDB)](#4-modèle-de-stockage-client-indexeddb)
5. [Description des traitements](#5-description-des-traitements)
6. [Interfaces techniques](#6-interfaces-techniques)
7. [Gestion des erreurs techniques](#7-gestion-des-erreurs-techniques)
8. [Performance et dimensionnement](#8-performance-et-dimensionnement)
9. [Sécurité applicative](#9-sécurité-applicative)
10. [Exigences non fonctionnelles et contraintes](#10-exigences-non-fonctionnelles-et-contraintes)
11. [Tests attendus](#11-tests-attendus)
12. [Packaging et distribution](#12-packaging-et-distribution)
13. [Critères de validation](#13-critères-de-validation)
14. [Traçabilité](#14-traçabilité)
15. [Architecture de l'extension](#15-architecture-de-lextension)
16. [Internationalisation (i18n)](#16-internationalisation-i18n)
17. [Gestion des dépendances](#17-gestion-des-dépendances)

---

## 1. Identification du document

- **Titre** : STD Globale — FoE Helper
- **Identifiant** : STD-FOE-GLOBAL-V1.0
- **Version** : 1.0
- **Statut** : Brouillon
- **Date de création** : 2026-10-23
- **Dernière mise à jour** : 2026-10-23

### Historique des versions

| Version | Date | Auteur | Modification |
|---------|------|--------|--------------|
| 1.0 | 2026-10-23 | Session IA + Utilisateur | Création initiale |

### Équipes

- **Auteur** : Généré par IA (session du 23/10/2026) avec relecture humaine
- **Relecteur** : À désigner
- **Valideur** : À désigner
- **Destinataires** : Développeurs FoE Helper, mainteneurs, contributeurs open source

### Liens

- **Code source** : `foe-helfer-extension/` (répertoire racine)
- **Manifest** : `manifest.json`
- **Background** : `background.js`
- **Injection** : `inject.js`, `foeproxy.js`
- **STD par module** : [`./_modules/`](./_modules/)
- **Architecture** : [`../../.01_Memoire/reference/architecture.md`](../../.01_Memoire/reference/architecture.md)

---

## 2. Contexte et objectifs

### 2.1 Contexte général

- **Projet** : FoE Helper – Extension navigateur open source (Licence AGPLv3) conçue pour le jeu **Forge of Empires** (InnoGames)
- **Type** : Extension Chrome **Manifest V3** avec compatibilité Firefox via polyfills
- **Version actuelle** : 4.3.0.0 (voir `manifest.json`)
- **Licence** : AGPLv3 – Obligation de redistribution du code source
- **Public cible** :
  - Joueurs de Forge of Empires souhaitant optimiser leur expérience de jeu
  - Développeurs contributeurs open source
  - Mainteneurs du projet
- **Objectif global** : **Améliorer l'expérience de jeu de manière légale** — fournir des outils d'assistance (calculs, alertes, visualisations) **sans modifier les données du jeu ni tricher**
- **Philosophie** : 
  - ❌ **Aucune triche** – Respect strict des règles du jeu
  - ✅ **Assistance légale** – Outils pour aider à la prise de décision
  - ✅ **Open source** – Transparence totale du code
  - ✅ **Multi-navigateurs** – Chrome, Edge, Brave, Opera, Firefox
  - ✅ **Multilingue** – 9 langues supportées (de, en, fr, nl, pl, ro, ru, sv)

### 2.2 Objectifs spécifiques du composant

**Cette STD documente l'architecture globale de FoE Helper** :

- **Fonctionnalité principale** : Fournir une plateforme modulaire d'extensions pour FoE, permettant à chaque utilisateur d'activer/désactiver les fonctionnalités selon ses besoins
- **Bénéfices pour l'utilisateur** :
  - **Optimisation** : Calculs automatiques (points de forge, coûts, rendements)
  - **Alertes** : Notifications pour les événements importants (GB, attaques, ressources)
  - **Visualisation** : Affichage amélioré des données (stats, bâtiments, marché)
  - **Automatisation** : Réduction des tâches répétitives (collecte de FP, gestion des quêtes)
- **Impact sur l'expérience de jeu** :
  - ✅ **Gain de temps** – Moins de calculs manuels
  - ✅ **Meilleure prise de décision** – Informations supplémentaires disponibles
  - ✅ **Réduction des erreurs** – Calculs précis et automatisés
  - ⚠️ **Aucun avantage injuste** – Respect des règles du jeu

### 2.3 Parties prenantes

| Rôle | Acteur | Responsabilités | Contact |
|------|--------|-----------------|---------|
| **Mainteneur principal** | FoE-Helper team | Validation des PR, gestion des releases, support | [GitHub](https://github.com/mainIine/foe-helfer-extension) |
| **Développeurs contributeurs** | Communauté open source | Implémentation de nouvelles fonctionnalités, corrections de bugs | [GitHub Issues](https://github.com/mainIine/foe-helfer-extension/issues) |
| **Utilisateurs finaux** | Joueurs de FoE | Feedback, signalement de bugs, suggestions | [Discord](https://discord.gg/z97KZq4) |
| **Communauté** | Discord, GitHub | Support technique, discussions, partage d'idées | [Discord](https://discord.gg/z97KZq4) |

### 2.4 Périmètre

- **Inclus dans cette STD** :
  - Architecture globale de l'extension (manifest, service worker, content scripts)
  - Mécanisme de chargement dynamique des modules
  - Gestion des dépendances tierces (vendor/)
  - Système de stockage (IndexedDB via Dexie.js)
  - Internationalisation (i18n)
  - Sécurité et permissions
  - Packaging et distribution

- **Exclus de cette STD** (documentés séparément) :
  - **Détails des 71 modules** → Voir les STD spécifiques dans `.02_Docs/STD/_modules/`
  - **Spécifications fonctionnelles** (SFD) → À créer pour chaque fonctionnalité
  - **Documentation utilisateur** → [docs.foe-helper.com](https://docs.foe-helper.com/)
  - **Décisions de design UI/UX** → Non couvert par la STD

- **Dépendances externes** :
  - **Navigateurs** : Chrome 90+, Edge 90+, Firefox 88+, Brave, Opera
  - **Jeu cible** : Forge of Empires (toutes versions, tous serveurs)
  - **Bibliothèques tierces** : Voir section 17 (Gestion des dépendances)

### 2.5 Définitions et acronymes

| Terme | Définition | Contexte |
|-------|------------|----------|
| **FoE** | Forge of Empires | Jeu de stratégie en temps réel développé par InnoGames |
| **MV3** | Manifest V3 | Standard moderne pour les extensions Chrome (remplace MV2) |
| **CSP** | Content Security Policy | Politique de sécurité pour limiter les sources de scripts |
| **i18n** | Internationalisation | Système de traduction pour supporter multiples langues |
| **DOM** | Document Object Model | Représentation du document HTML en mémoire |
| **GB** | Great Building | Bâtiment spécial dans FoE (ex: Tour de Guet, Phare) |
| **FP** | Forge Points | Points de forge, monnaie premium dans FoE |
| **UI** | User Interface | Interface utilisateur |
| **STD** | Spécification Technique Détaillée | Ce document |
| **SFD** | Spécification Fonctionnelle Détaillée | Document complémentaire (besoins utilisateur) |

### 2.6 Références

- **Documentation officielle** :
  - [Site officiel FoE Helper](https://docs.foe-helper.com/)
  - [Dépôt GitHub](https://github.com/mainIine/foe-helfer-extension)
  - [Issues GitHub](https://github.com/mainIine/foe-helfer-extension/issues)
- **Documentation technique** :
  - [Chrome Extension Docs (MV3)](https://developer.chrome.com/docs/extensions/mv3/)
  - [Firefox WebExtensions](https://extensionworkshop.com/)
  - [MDN Web Docs - IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
  - [Dexie.js Documentation](https://dexie.org/)
- **Ressources communautaires** :
  - [Serveur Discord](https://discord.gg/z97KZq4) – Support et discussions
  - [Wiki communautaire](https://github.com/mainIine/foe-helfer-extension/wiki) – Si existe

---

*Ce document est à compléter en utilisant le template STD-Template-FoE-Helper.md. Section 2 générée le 23/10/2026.*