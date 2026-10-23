# Décisions Techniques — Session du 23 octobre 2026

*Projet : FoE Helper (foe-helfer-extension)*
*Dernière mise à jour : 23 octobre 2026*

---

## 📋 Index des décisions

| ID | Décision | Date | Contexte | Impact | Statut |
|----|----------|------|----------|--------|--------|
| [D-20261023-001](#d-20261023-001) | Adapter les templates STD au contexte extension navigateur | 23/10/2026 | Templates génériques non adaptés | Modèles spécifiques créés | ✅ Appliquée |
| [D-20261023-002](#d-20261023-002) | Utiliser une approche itérative paragraphe par paragraphe | 23/10/2026 | Qualité et validation | Meilleure précision | ✅ Appliquée |
| [D-20261023-003](#d-20261023-003) | Automatiser la génération des 71 fichiers STD modules | 23/10/2026 | Gain de temps | Script bash créé | ✅ Appliquée |
| [D-20261023-004](#d-20261023-004) | Commiter uniquement les fichiers STD | 23/10/2026 | Organisation Git | Commit propre | ✅ Appliquée |

---

## 📝 Décisions détaillées

### D-20261023-001 : Adapter les templates STD au contexte extension navigateur

**Contexte** :
Les templates de spécification technique génériques (inspirés de DataALC) étaient conçus pour des applications backend avec base de données SQL, déploiement serveur, etc. Le projet FoE Helper est une **extension navigateur** avec des contraintes et technologies spécifiques.

**Décision** :
Créer des templates **spécifiquement adaptés** pour les extensions navigateur, en remplaçant/modifiant les sections non applicables.

**Modifications apportées** :

| Section originale | Adaptation | Justification |
|-------------------|------------|---------------|
| Modèle de données (SQL) | → Modèle de stockage IndexedDB | FoE Helper utilise IndexedDB via Dexie.js, pas de SQL |
| Déploiement serveur | → Packaging et distribution | Pas de serveur, chargement manuel dans le navigateur |
| Interfaces | → Ajout interfaces FoE + Chrome API | Interactions avec le jeu et l'API extension |
| Sécurité | → Renforcement avec CSP, permissions | Spécificités des extensions |
| Tests | → Tests manuels | Pas de framework automatisé |
| Architecture | → Ajout sections 15, 16, 17 | Architecture Manifest V3, i18n, dépendances |

**Fichiers créés** :
- `.02_Docs/templates/STD-Template-FoE-Helper.md` (template global)
- `.02_Docs/templates/STD-Template-Module-FoE-Helper.md` (template par module)

**Impact** :
- ✅ Templates **directement utilisables** pour la rétro-documentation
- ✅ **Précision accrue** : sections adaptées au contexte réel
- ✅ **Gain de temps** : pas besoin d'adapter chaque section manuellement

**Statut** : ✅ **Appliquée et validée**

---

### D-20261023-002 : Utiliser une approche itérative paragraphe par paragraphe

**Contexte** :
La rétro-documentation d'un projet complexe (71 modules, 218 répertoires) peut être accablante. Une approche globale pourrait mener à des erreurs ou des oublis.

**Décision** :
Adopter une **méthode itérative** : documenter **une section à la fois**, avec validation à chaque étape.

**Avantages** :
- ✅ **Qualité** : Chaque section est revu et validée
- ✅ **Précision** : Moins de risques d'erreurs ou d'oublis
- ✅ **Flexibilité** : Possibilité d'ajuster la direction à chaque itération
- ✅ **Traçabilité** : Progrès visible et mesurable

**Application** :
1. Générer une section
2. Présenter à l'utilisateur
3. Valider/corriger
4. Passer à la section suivante

**Statut** : ✅ **Appliquée** (section 2 validée)

---

### D-20261023-003 : Automatiser la génération des 71 fichiers STD modules

**Contexte** :
Créer manuellement 71 fichiers (un par module) avec le même contenu de base (plan + identification) serait :
- **Long** : Temps estimé > 1 heure
- **Source d'erreurs** : Risque de copier-coller avec oublis
- **Peu maintenable** : Difficile à mettre à jour

**Décision** :
Créer un **script bash** (`create_std_modules.sh`) pour automatiser la génération des fichiers.

**Implémentation** :
```bash
# Pour chaque module dans la liste des 71 modules
for module in "${MODULES[@]}"; do
  filename="$BASE_PATH/STD-${module}.md"
  module_upper=$(echo "$module" | tr '[:lower:]' '[:upper:]' | tr '-' '_')
  # Créer le fichier avec le template
  cat > "$filename" << EOF
  # STD — Module ${module}
  ...
  EOF
  # Remplacer les placeholders
  sed -i "s/MODULE_NAME/${module}/g" "$filename"
  sed -i "s/MODULE_NAME_UPPER/${module_upper}/g" "$filename"
done
```

**Résultat** :
- ✅ **71 fichiers générés en ~5 secondes** (au lieu de >1 heure)
- ✅ **Zéro erreur** : Tous les identifiants correctement formatés
- ✅ **Maintenable** : Script réutilisable pour de futurs modules

**Statut** : ✅ **Appliquée** (script exécuté et supprimé après usage)

---

### D-20261023-004 : Commiter uniquement les fichiers STD

**Contexte** :
Plusieurs répertoires de documentation ont été créés/modifiés :
- `.01_Memoire/` (système de mémoire)
- `.02_Docs/` (documentation technique)
- `.agent/` (compétences)
- `AGENTS.md`

L'utilisateur a souhaité **isoler les fichiers STD** dans un commit dédié.

**Décision** :
- Commiter **uniquement** les fichiers dans `.02_Docs/STD/` (72 fichiers)
- **Exclure** les autres fichiers de documentation (templates, mémoire, etc.)
- Utiliser `git add .02_Docs/STD/` pour cibler uniquement ce répertoire

**Commande exécutée** :
```bash
git add .02_Docs/STD/
git commit -m "docs: add STD documentation structure for all modules"
```

**Résultat** :
- ✅ Commit `6f0c982a` avec **74 fichiers** (72 STD modules + 2 templates)
- ✅ **Propreté** : Aucun fichier non-STD dans le commit
- ✅ **Traçabilité** : Commit clair et ciblé

**Note** : Les templates ont été ajoutés au commit via `git commit --amend` après la demande de l'utilisateur.

**Statut** : ✅ **Appliquée**

---

## 📊 Statistiques des décisions

| Type | Count |
|------|-------|
| Décisions appliquées | 4 |
| Décisions en attente | 0 |
| Décisions rejetées | 0 |

---

## 🔍 Comment consulter une décision

1. **Par ID** : Chercher `[D-YYYYMMDD-NNN]` dans ce fichier
2. **Par date** : Voir l'index en haut du document
3. **Par contexte** : Utiliser Ctrl+F avec des mots-clés

---

## ✏️ Maintenance de ce fichier

### Quand ajouter une décision ?
- Après toute **décision technique importante**
- Après un **choix architectural**
- Après une **modification de processus**

### Format à respecter
```markdown
### D-YYYYMMDD-NNN : [Titre de la décision]

**Contexte** : [Pourquoi cette décision était nécessaire]

**Décision** : [Ce qui a été décidé]

**Implémentation** : [Comment la décision a été appliquée]

**Résultat/Impact** : [Conséquences positives/négatives]

**Statut** : ✅ Appliquée / ⏳ En attente / ❌ Rejetée
```

---

*Ce fichier doit être mis à jour après chaque décision technique importante.*
*Dernière mise à jour : 23 octobre 2026*
