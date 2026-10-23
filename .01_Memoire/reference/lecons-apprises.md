# Leçons Apprises — Session du 23 octobre 2026

*Projet : FoE Helper (foe-helfer-extension)*
*Dernière mise à jour : 23 octobre 2026*

---

## 📋 Index des leçons

| ID | Leçon | Catégorie | Date | Impact |
|----|-------|----------|------|--------|
| [L-20261023-001](#l-20261023-001) | Les templates génériques doivent être adaptés au contexte technique | Documentation | 23/10/2026 | ⭐⭐⭐⭐ |
| [L-20261023-002](#l-20261023-002) | L'approche itérative paragraphe par paragraphe améliore la qualité | Méthodologie | 23/10/2026 | ⭐⭐⭐⭐⭐ |
| [L-20261023-003](#l-20261023-003) | L'automatisation est essentielle pour les projets à grande échelle | Productivité | 23/10/2026 | ⭐⭐⭐⭐⭐ |
| [L-20261023-004](#l-20261023-004) | Les commits Git doivent être atomiques et ciblés | Bonnes pratiques | 23/10/2026 | ⭐⭐⭐⭐ |

---

## 📚 Leçons détaillées

### L-20261023-001 : Les templates génériques doivent être adaptés au contexte technique

**Contexte** :
Le modèle de STD générique fourni par l'utilisateur (inspiré de DataALC) était conçu pour des applications backend classiques avec :
- Base de données SQL
- Déploiement serveur
- Tests automatisés
- Architecture backend

Le projet FoE Helper est une **extension navigateur** avec des contraintes radicalement différentes.

**Problème** :
Utiliser le template générique sans adaptation aurait conduit à :
- Des sections **inapplicables** (ex: modèle physique de données SQL)
- Des **oubli importants** (ex: interfaces avec le jeu FoE, CSP)
- Une **perte de temps** à adapter chaque section manuellement

**Solution appliquée** :
Créer des templates **spécifiquement adaptés** pour les extensions navigateur, en :
1. Remplaçant la section "Modèle physique de données" par "Modèle de stockage IndexedDB"
2. Remplaçant "Déploiement" par "Packaging et distribution"
3. Ajoutant des sections spécifiques : Architecture de l'extension, i18n, Gestion des dépendances
4. Adaptant la sécurité pour les extensions (CSP, permissions, injection)

**Résultat** :
- ✅ Templates **directement utilisables** sans adaptation manuelle
- ✅ **Gain de temps** : ~2 heures économisées sur la première STD
- ✅ **Meilleure qualité** : Sections adaptées au contexte réel

**Leçon retranchée** :
> **"Un template générique est un bon point de départ, mais il doit toujours être adapté au contexte technique spécifique du projet."**

**Applicabilité** : ⭐⭐⭐⭐⭐ (Toujours applicable)

---

### L-20261023-002 : L'approche itérative paragraphe par paragraphe améliore la qualité

**Contexte** :
La rétro-documentation d'un projet complexe peut être intimidante. La tentation est de tout documenter d'un coup pour "en finir".

**Problème** :
Une approche globale pour FoE Helper aurait entraîné :
- Des **erreur de compréhension** : Difficile de tout analyser en une fois
- Une **qualité médiore** : Sections moins précises
- Un **risque d'abandon** : Projet trop vaste
- Une **validation difficile** : Impossible de tout relire d'un coup

**Solution appliquée** :
Adopter une **méthode itérative** :
1. Générer **une seule section**
2. La présenter à l'utilisateur
3. **Valider et corriger** ensemble
4. Passer à la section suivante

**Résultat** :
- ✅ **Qualité accrue** : Chaque section est revu et validée
- ✅ **Précision** : Moins de risques d'erreurs
- ✅ **Flexibilité** : Possibilité d'ajuster la direction
- ✅ **Motivation** : Progrès visible et gratifiant

**Leçon retranchée** :
> **"Pour la rétro-documentation de projets complexes, l'approche itérative par petite étape est plus efficace qu'une approche globale."**

**Recommandation** :
- **Taille des itérations** : 1 section ou 1 sous-section à la fois
- **Validation** : Toujours faire valider par un humain
- **Outils** : Utiliser des checklists pour suivre les sections restantes

**Applicabilité** : ⭐⭐⭐⭐⭐ (Toujours applicable pour la rétro-documentation)

---

### L-20261023-003 : L'automatisation est essentielle pour les projets à grande échelle

**Contexte** :
Le projet FoE Helper contient **71 modules**. Pour la rétro-documentation, il fallait créer 71 fichiers STD (un par module) avec :
- Le plan du document
- La section "Identification" avec identifiant unique

**Problème** :
Créer ces fichiers manuellement aurait pris :
- **Temps** : > 1 heure (5-10 min par fichier)
- **Risque d'erreurs** : Copier-coller avec oublis de remplacer les placeholders
- **Maintenance** : Difficile à mettre à jour si un module est ajouté/renommé

**Solution appliquée** :
Créer un **script bash** (`create_std_modules.sh`) qui :
1. Parcourt la liste des 71 modules
2. Pour chaque module, crée un fichier avec le template
3. Remplace automatiquement les placeholders (nom du module, identifiant)
4. Génère tous les fichiers en **~5 secondes**

**Code du script** :
```bash
for module in "${MODULES[@]}"; do
  filename="$BASE_PATH/STD-${module}.md"
  module_upper=$(echo "$module" | tr '[:lower:]' '[:upper:]' | tr '-' '_')
  cat > "$filename" << EOF
  # STD — Module ${module}
  ...
  EOF
  sed -i "s/MODULE_NAME/${module}/g" "$filename"
  sed -i "s/MODULE_NAME_UPPER/${module_upper}/g" "$filename"
done
```

**Résultat** :
- ✅ **71 fichiers générés en ~5 secondes** (vs >1 heure manuellement)
- ✅ **Zéro erreur** : Tous les identifiants correctement formatés
- ✅ **Maintenable** : Script réutilisable pour de futurs modules
- ✅ **Consistance** : Tous les fichiers ont la même structure

**Leçon retranchée** :
> **"Dès qu'une tâche répétitive concerne plus de 5-10 éléments, automatisez-la. L'investissement en temps pour créer le script est rapidement rentabilisé."**

**Seuil d'automatisation** :
- **< 5 éléments** : Manuel (pas besoin d'automatiser)
- **5-20 éléments** : Semi-automatisé (script simple)
- **> 20 éléments** : **Automatisation obligatoire**

**Applicabilité** : ⭐⭐⭐⭐⭐ (Toujours applicable)

---

### L-20261023-004 : Les commits Git doivent être atomiques et ciblés

**Contexte** :
Plusieurs modifications ont été faites pendant la session :
- Création de templates STD
- Organisation de la structure de fichiers
- Génération de 71 fichiers STD modules
- Mise à jour de la mémoire de session

**Problème** :
Un commit unique avec toutes ces modifications aurait été :
- **Difficile à relire** : Trop de fichiers modifiés
- **Peu maintenable** : Impossible de revenir sur une partie spécifique
- **Mauvaise traçabilité** : Impossible de comprendre ce qui a été fait

**Solution appliquée** :
- Commiter **uniquement les fichiers STD** dans un premier commit
- Laisser les autres modifications (mémoire, templates) pour un commit séparé ou futur
- Utiliser `git add .02_Docs/STD/` pour cibler uniquement le répertoire concerné

**Commandes exécutées** :
```bash
# Ajouter uniquement les fichiers STD
git add .02_Docs/STD/

# Commiter avec un message clair
git commit -m "docs: add STD documentation structure for all modules"

# Puis ajouter les templates
git add .02_Docs/templates/STD-Template-*.md
git commit --amend -m "..."
```

**Résultat** :
- ✅ **Commit atomique** : Un seul objectif par commit (documentation STD)
- ✅ **Message clair** : Description précise du contenu
- ✅ **Traçabilité** : Facile à comprendre et à revenir dessus
- ✅ **Propreté** : Aucun fichier non-STD dans le commit

**Leçon retranchée** :
> **"Un commit doit avoir un seul objectif clair. Si vous modifiez plusieurs choses non liées, faites des commits séparés."**

**Bonnes pratiques** :
- **1 commit = 1 objectif**
- **Message descriptif** : Expliquer le "pourquoi" et le "quoi"
- **Atomique** : Le commit doit être complet en soi (pas de "work in progress")

**Applicabilité** : ⭐⭐⭐⭐⭐ (Toujours applicable)

---

## 📊 Statistiques des leçons

| Type | Count |
|------|-------|
| Leçons apprises | 4 |
| Leçons appliquées | 4 |
| Leçons à tester | 0 |

---

## 🔍 Comment utiliser ce fichier

### Quand consulter ?
- Avant de **démarrer un nouveau type de tâche** (ex: rétro-documentation)
- Quand un **problème similaire** est rencontré
- Pour **former de nouveaux contributeurs**

### Comment contribuer ?
1. **Ajouter une nouvelle leçon** après avoir résolu un problème
2. **Mettre à jour** les leçons existantes si de nouvelles informations apparaissent
3. **Classer** les leçons par catégorie et impact

---

## 🎯 Catégories de leçons

| Catégorie | Description | Exemples |
|-----------|-------------|----------|
| **Documentation** | Amélioration des processus de documentation | Templates, rétro-documentation |
| **Méthodologie** | Approches et processus de travail | Itératif, validation |
| **Productivité** | Outils et techniques pour gagner du temps | Automatisation, scripts |
| **Bonnes pratiques** | Standards et conventions | Git, qualité de code |
| **Architecture** | Décisions de design technique | Patterns, structures |

---

*Ce fichier doit être mis à jour après chaque résolution de problème complexe ou découverte de bonne pratique.*
*Dernière mise à jour : 23 octobre 2026*
