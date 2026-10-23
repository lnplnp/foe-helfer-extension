# MANIFEST.md — Index du système de mémoire

*Projet : FoE Helper (foe-helfer-extension)*
*Dernière mise à jour : 19 octobre 2026*

---

## 📋 Description

Ce fichier est **l'index central** du système de mémoire du projet FoE Helper.
Il recense tous les éléments de mémoire disponibles et définit les **règles de chargement** pour les agents IA.

---

## 🗺️ Structure du système de mémoire

```
.01_Memoire/
├── MANIFEST.md                # ⭐ CE FICHIER — Index central
├── court-terme/               # 🟡 Mémoire à court terme (chargée systématiquement)
│   ├── tache-courante.md      # État des travaux en cours
│   └── contexte-conversation.md # Contexte de la session (si applicable)
├── journalier/                # 📅 Journaux par date
│   └── AAAA-MM-JJ.md          # Événements du jour (créé au besoin)
├── reference/                # 🟢 Mémoire à long terme (stable)
│   ├── architecture.md        # Architecture technique
│   ├── decisions.md           # Décisions techniques importantes
│   └── lecons-apprises.md      # Retours d'expérience
└── long-terme/               # 🔵 Mémoire permanente
    ├── journal-decisions.md    # Historique des décisions
    └── lecons-apprises.md      # Leçons apprises (version long terme)
```

---

## 📚 Contenu détaillé

### 1️⃣ `court-terme/tache-courante.md`
**Priorité** : ⭐⭐⭐⭐⭐ (Toujours lire en premier)
**Description** : État actuel des travaux, objectif de la session, avancement, blocages
**Fréquence de mise à jour** : À chaque session
**Chargement** : **OBLIGATOIRE** au début de chaque session

### 2️⃣ `court-terme/contexte-conversation.md`
**Priorité** : ⭐⭐⭐⭐
**Description** : Informations spécifiques à la continuité du travail en cours
**Fréquence de mise à jour** : Pendant les sessions longues ou multi-étapes
**Chargement** : Si la tâche est en cours

### 3️⃣ `reference/architecture.md`
**Priorité** : ⭐⭐⭐⭐
**Description** : Documentation technique de l'architecture du projet
**Fréquence de mise à jour** : Lors de changements architecturaux majeurs
**Chargement** : Quand la compréhension de l'architecture est nécessaire
**Statut** : ✅ **Créé et à jour** (19 oct 2026)

### 4️⃣ `reference/decisions.md`
**Priorité** : ⭐⭐⭐
**Description** : Décisions techniques importantes et leurs justifications
**Fréquence de mise à jour** : Après chaque décision architecturale
**Chargement** : Quand une décision technique doit être compréhensible
**Statut** : ⏳ À créer

### 5️⃣ `reference/lecons-apprises.md`
**Priorité** : ⭐⭐⭐
**Description** : Difficultés rencontrées, solutions vérifiées, précautions
**Fréquence de mise à jour** : Après résolution de problèmes complexes
**Chargement** : Quand un problème similaire est rencontré
**Statut** : ⏳ À créer

### 6️⃣ `journalier/AAAA-MM-JJ.md`
**Priorité** : ⭐⭐
**Description** : Journal des interventions significatives par date
**Fréquence de mise à jour** : Après chaque session d'intervention majeure
**Chargement** : Pour comprendre l'historique récent
**Statut** : ⏳ À créer au besoin

### 7️⃣ `long-terme/journal-decisions.md`
**Priorité** : ⭐⭐
**Description** : Historique complet des décisions (archive)
**Fréquence de mise à jour** : Périodiquement (consolidation)
**Chargement** : Pour recherche historique
**Statut** : ⏳ À créer

---

## 🎯 Règles de chargement

### 🔴 **Toujours charger (OBLIGATOIRE)**
1. `AGENTS.md` (à la racine du projet)
2. `.01_Memoire/MANIFEST.md` (ce fichier)
3. `.01_Memoire/court-terme/tache-courante.md`

### 🟡 **Charger si pertinent**
- `.01_Memoire/reference/architecture.md` → Quand l'architecture doit être comprise
- `.01_Memoire/reference/decisions.md` → Quand une décision technique doit être rappelée
- `.01_Memoire/reference/lecons-apprises.md` → Quand un problème similaire est rencontré
- `.01_Memoire/journalier/AAAA-MM-JJ.md` → Quand l'historique récent est nécessaire

### 🟢 **Ne pas charger systématiquement**
- Les fichiers du `long-terme/` (archive, consultation ponctuelle)
- Les anciens journaux (`journalier/` anciens)
- Les fichiers obsolètes (marqués comme tels)

---

## 📊 État des éléments de mémoire

| Fichier | Statut | Dernière MAJ | Priorité | Prochaine action |
|---------|--------|---------------|----------|------------------|
| `tache-courante.md` | ⏳ À créer | - | ⭐⭐⭐⭐⭐ | Créer avec état initial |
| `architecture.md` | ✅ Créé | 19 oct 2026 | ⭐⭐⭐⭐ | - |
| `decisions.md` | ❌ Absent | - | ⭐⭐⭐ | Créer |
| `lecons-apprises.md` | ❌ Absent | - | ⭐⭐⭐ | Créer |
| `journal-decisions.md` | ❌ Absent | - | ⭐⭐ | Créer |

---

## 🔍 Recherche dans la mémoire

### Comment trouver une information ?

1. **Pour la tâche en cours** → `court-terme/tache-courante.md`
2. **Pour l'architecture** → `reference/architecture.md`
3. **Pour une décision technique** → `reference/decisions.md`
4. **Pour un problème récurrent** → `reference/lecons-apprises.md`
5. **Pour l'historique** → `journalier/AAAA-MM-JJ.md` ou `long-terme/`

### Si l'information n'est pas trouvée :
- Vérifier dans le code source
- Vérifier dans la documentation externe (docs.foe-helper.com)
- Demander clarification à l'utilisateur
- **Ne pas inventer** une information

---

## ✏️ Maintenance du système de mémoire

### Règles de mise à jour
1. **Un fichier = une responsabilité** : Éviter les doublons
2. **Distinguier faits/hypothèses** : Toujours marquer les incertitudes
3. **Lier à la source** : Quand utile, référencer le fichier source
4. **Gardé à jour** : Mettre à jour quand l'état réel change
5. **Concise** : Aller à l'essentiel, éviter les redondances

### Quand mettre à jour ?
- Après chaque **décision technique** → `decisions.md`
- Après chaque **session d'intervention** → `tache-courante.md` + journal
- Après résolution d'un **problème complexe** → `lecons-apprises.md`
- Après un **changement architectural** → `architecture.md`

### Quand ne PAS mettre à jour ?
- Pour des informations **déjà dans le code** (commentaires, README)
- Pour des **détails d'implémentation** évidents
- Pour des **changements mineurs** sans impact

---

## 📞 Références

- **AGENTS.md** : Consignes permanentes pour les agents IA
- **.02_Docs/** : Documentation technique et fonctionnelle
- **Projet GitHub** : https://github.com/mainIine/foe-helfer-extension

---

*Ce fichier doit être mis à jour quand de nouveaux éléments de mémoire sont ajoutés ou modifiés.*
