# 00_INDEX.md — Documentation FoE Helper

*Projet : FoE Helper (foe-helfer-extension)*
*Version : 4.3.0.0*
*Dernière mise à jour : 19 octobre 2026*

---

## 📚 Bienvenue dans la documentation FoE Helper

Ce répertoire contient la **documentation technique et fonctionnelle** du projet **FoE Helper**, une extension navigateur open source pour le jeu **Forge of Empires**.

---

## 🗺️ Navigation

### 📌 **Pour commencer**
| Document | Description | Public |
|----------|-------------|--------|
| [ONBOARDING.md](./ONBOARDING.md) | Guide d'installation et prise en main | ✅ Nouveaux contributeurs |
| [ARCHITECTURE.md](./ARCHITECTURE.md) | Architecture technique détaillée | ✅ Développeurs |

### 🏗️ **Architecture et Technique**
| Document | Description | Public |
|----------|-------------|--------|
| [ARCHITECTURE.md](./ARCHITECTURE.md) | Architecture globale du projet | ✅ Développeurs |

### 📋 **Développement**
| Document | Description | Statut |
|----------|-------------|--------|
| DEVELOPMENT.md | Guide de développement | ⏳ À créer |
| MODULES.md | Documentation des modules | ⏳ À créer |
| API.md | Référence API interne | ⏳ À créer |

### 🚀 **Exploitation**
| Document | Description | Statut |
|----------|-------------|--------|
| EXPLOITATION.md | Déploiement et maintenance | ⏳ À créer |
| TESTING.md | Stratégie et procédures de test | ⏳ À créer |

### 📊 **Chantiers**
| Document | Description | Statut |
|----------|-------------|--------|
| DECISIONS_A_TRANCHER.md | Questions ouvertes et arbitrages | ⏳ À créer |
| JOURNAL.md | Historique des interventions | ⏳ À créer |
| ROADMAP.md | Feuille de route | ⏳ À créer |

---

## 🎯 Qui doit lire quoi ?

### Pour les **utilisateurs**
→ Commencez par [ONBOARDING.md](./ONBOARDING.md)

### Pour les **nouveaux contributeurs**
1. Lisez [ONBOARDING.md](./ONBOARDING.md)
2. Consultez [ARCHITECTURE.md](./ARCHITECTURE.md)
3. Explorez la documentation existante sur [docs.foe-helper.com](https://docs.foe-helper.com/)

### Pour les **développeurs confirmés**
1. [ARCHITECTURE.md](./ARCHITECTURE.md) — Architecture technique
2. [DEVELOPMENT.md](./DEVELOPMENT.md) — Guide de développement (à créer)
3. [AGENTS.md](../AGENTS.md) — Consignes pour les agents IA
4. Documentation des modules spécifiques

### Pour les **mainteneurs**
1. Toute la documentation ci-dessus
2. [EXPLOITATION.md](./EXPLOITATION.md) — Déploiement (à créer)
3. [.01_Memoire/](../.01_Memoire/MANIFEST.md) — Système de mémoire interne

---

## 📂 Organisation des fichiers

```
.02_Docs/
├── 00_INDEX.md               # ⭐ CE FICHIER — Point d'entrée
├── ONBOARDING.md             # Guide d'installation et prise en main
├── ARCHITECTURE.md           # Architecture technique détaillée
├── DEVELOPMENT.md            # Guide de développement (à créer)
├── EXPLOITATION.md           # Déploiement et maintenance (à créer)
├── TESTING.md                # Stratégie de test (à créer)
├── MODULES.md                # Documentation des modules (à créer)
├── API.md                    # Référence API (à créer)
├── DECISIONS_A_TRANCHER.md    # Questions ouvertes (à créer)
├── JOURNAL.md                # Historique (à créer)
├── ROADMAP.md                # Feuille de route (à créer)
└── architecture/             # Documentation par composant (optionnel)
    ├── service-worker.md
    ├── content-scripts.md
    └── modules/
```

---

## 🔍 Comment trouver une information ?

| Besoin | Où chercher |
|--------|-------------|
| **Installer l'extension** | [ONBOARDING.md](./ONBOARDING.md) |
| **Comprendre l'architecture** | [ARCHITECTURE.md](./ARCHITECTURE.md) |
| **Développer un module** | [DEVELOPMENT.md](./DEVELOPMENT.md) (à créer) |
| **Déployer une nouvelle version** | [EXPLOITATION.md](./EXPLOITATION.md) (à créer) |
| **Tester l'extension** | [TESTING.md](./TESTING.md) (à créer) |
| **Comprendre un module spécifique** | Documentation du module dans `js/web/[module]/` |
| **Consignes pour les agents IA** | [../AGENTS.md](../AGENTS.md) |
| **Mémoire du projet** | [../.01_Memoire/MANIFEST.md](../.01_Memoire/MANIFEST.md) |

---

## 📊 État de la documentation

### ✅ Disponible
| Fichier | Version | Dernière MAJ | Qualité |
|---------|---------|---------------|---------|
| ONBOARDING.md | 1.0 | 19 oct 2026 | ⭐⭐⭐⭐ |
| ARCHITECTURE.md | 1.0 | 19 oct 2026 | ⭐⭐⭐⭐ |

### ⏳ À créer (priorité)
| Fichier | Priorité | Estimé | Dépendances |
|---------|----------|---------|--------------|
| DEVELOPMENT.md | ⭐⭐⭐⭐ | 1h | ARCHITECTURE.md |
| EXPLOITATION.md | ⭐⭐⭐ | 30min | ONBOARDING.md |
| TESTING.md | ⭐⭐⭐ | 30min | - |
| MODULES.md | ⭐⭐ | 1h | ARCHITECTURE.md |

### ❌ Non prévu (pour l'instant)
- Documentation utilisateur complète (existe déjà sur docs.foe-helper.com)
- Documentation API publique (l'extension n'a pas d'API publique)

---

## 🎨 Conventions de documentation

### Règles générales
1. **Langue** : Français principalement, anglais pour les termes techniques
2. **Format** : Markdown (CommonMark)
3. **Structure** : Hiérarchique, avec table des matières
4. **Exemples** : Toujours inclure des exemples de code
5. **Mises à jour** : Garder la documentation synchronisée avec le code

### Style
- **Titres** : Utiliser les niveaux de titres (#, ##, ###) de manière cohérente
- **Listes** : Préférer les listes aux paragraphes longs
- **Code** : Utiliser les blocs de code avec syntax highlighting
- **Tableaux** : Pour les informations structurées
- **Diagrammes** : Utiliser ASCII art ou Mermaid (si supporté)

### Bonnes pratiques
- ✅ **Documenter l'existant** : Ne pas documenter une architecture idéale qui n'existe pas
- ✅ **Être précis** : Éviter les généralités, donner des détails concrets
- ✅ **Lier à la source** : Référencer les fichiers sources quand utile
- ✅ **Signaler les limites** : Marquer les informations incomplètes ou incertaines
- ❌ **Ne pas copier** : Éviter de dupliquer le code dans la documentation
- ❌ **Ne pas mentir** : Ne pas prétendre qu'une fonctionnalité existe si elle ne fonctionne pas

---

## 🔗 Liens utiles

### Documentation interne
- [AGENTS.md](../AGENTS.md) — Consignes pour les agents IA
- [.01_Memoire/](../.01_Memoire/) — Système de mémoire du projet

### Documentation externe
- [Site officiel](https://docs.foe-helper.com/) — Documentation utilisateur
- [GitHub Repository](https://github.com/mainIine/foe-helfer-extension) — Code source
- [GitHub Issues](https://github.com/mainIine/foe-helfer-extension/issues) — Rapport de bugs

### Communauté
- [Discord](https://discord.gg/z97KZq4) — Support et discussions

---

## 📞 Contribuer à la documentation

### Comment aider ?
1. **Corriger** les erreurs ou informations obsolètes
2. **Ajouter** la documentation des modules manquants
3. **Améliorer** les exemples et explications
4. **Traduire** en anglais (si pertinent)

### Processus
1. Fork le repository
2. Modifie les fichiers de documentation
3. Vérifie que les liens fonctionnent
4. Soumets une Pull Request
5. Attends la revue et le merge

### Règles
- Respecter les conventions existantes
- Garder les fichiers concis et utiles
- Mettre à jour la table des matières si nécessaire
- Ne pas ajouter de documentation pour du code qui n'existe pas

---

## 📝 Historique des modifications

| Date | Modification | Auteur |
|------|--------------|--------|
| 19 oct 2026 | Création initiale | Agent IA (intégration mémoire) |

---

*Ce fichier est le point d'entrée de la documentation.*
*Mettez à jour ce fichier quand de nouveaux documents sont ajoutés.*
