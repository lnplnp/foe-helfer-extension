# ONBOARDING.md — Guide d'installation et prise en main

*Projet : FoE Helper (foe-helfer-extension)*
*Version : 4.3.0.0*
*Dernière mise à jour : 19 octobre 2026*

---

## 👋 Bienvenue !

Merci de votre intérêt pour **FoE Helper** ! Ce guide vous accompagne dans l'installation, la configuration et la prise en main de l'extension, que vous soyez un **utilisateur**, un **nouveau contributeur** ou un **développeur**.

---

## 🎯 Qui est ce guide pour ?

| Profil | Sections Pertinentes |
|--------|----------------------|
| **Utilisateur** | Installation, Configuration, Utilisation de base |
| **Nouveau Contributeur** | Tout le guide, puis [DEVELOPMENT.md](./DEVELOPMENT.md) |
| **Développeur Confirmé** | Installation Dev, Workflow, Dépannage |
| **Mainteneur** | Tout, plus [EXPLOITATION.md](./EXPLOITATION.md) |

---

## 📥 Installation

### Pour les **utilisateurs finaux**

#### Méthode 1 : Depuis les stores officiels (recommandé)

| Navigateur | Lien | Statut |
|------------|------|--------|
| **Chrome** | [Chrome Web Store](https://chrome.google.com/webstore/) | À vérifier |
| **Firefox** | [Firefox Add-ons](https://addons.mozilla.org/) | À vérifier |
| **Edge** | [Microsoft Edge Add-ons](https://microsoftedge.microsoft.com/addons/) | À vérifier |
| **Opera** | [Opera Add-ons](https://addons.opera.com/) | À vérifier |

**Étapes :**
1. Cliquez sur le lien correspondant à votre navigateur
2. Cliquez sur "Ajouter à [Navigateur]"
3. Confirmez l'installation
4. L'extension est prête à l'emploi !

#### Méthode 2 : Installation manuelle (pour les tests)

**Pour Chromium (Chrome, Edge, Brave, Opera) :**

1. **Télécharger l'extension** :
   - Soit depuis les [releases GitHub](https://github.com/mainIine/foe-helfer-extension/releases)
   - Soit en clonant le dépôt :
     ```bash
     git clone https://github.com/mainIine/foe-helfer-extension.git
     ```

2. **Activer le mode développeur** :
   - Ouvrez une nouvelle onglet
   - Allez sur `chrome://extensions/` (Chrome) ou `edge://extensions/` (Edge)
   - Activez le **Mode développeur** (interrupteur en haut à droite)

3. **Charger l'extension** :
   - Cliquez sur **"Charger l'extension non empaquetée"**
   - Sélectionnez le dossier `foe-helfer-extension` (celui contenant `manifest.json`)
   - Confirmez

4. **Vérifier l'installation** :
   - L'icône de l'extension apparaît dans la barre d'outils
   - Cliquez sur l'icône pour ouvrir la popup

**Pour Firefox :**

1. **Télécharger l'extension** (comme ci-dessus)

2. **Installation temporaire** :
   - Allez sur `about:debugging`
   - Dans le menu de gauche, cliquez sur **"Modules complémentaires temporaires"**
   - Cliquez sur **"Charger un module complémentaire temporaire"**
   - Sélectionnez le fichier `manifest.json` dans le dossier de l'extension

3. **Installation permanente** :
   - Créez un fichier `.zip` du dossier
   - Allez sur `about:addons`
   - Glissez-déposez le fichier `.zip`

---

## 🛠️ Installation pour le développement

### Prérequis

| Outil | Version | Obligatoire | Lien |
|-------|---------|-------------|------|
| **Git** | 2.x+ | ✅ Oui | [git-scm.com](https://git-scm.com/) |
| **Navigateur** | Chrome, Edge, Firefox | ✅ Oui | - |
| **Éditeur** | VS Code (recommandé) | ❌ Non | [code.visualstudio.com](https://code.visualstudio.com/) |
| **Node.js** | - | ❌ Non | (Pas utilisé dans ce projet) |

### Étapes

1. **Cloner le dépôt** :
   ```bash
   cd G:/Mon Drive/Forge Of Empires
   git clone https://github.com/mainIine/foe-helfer-extension.git
   cd foe-helfer-extension
   ```

2. **Ouvrir dans VS Code** (optionnel mais recommandé) :
   ```bash
   code foe-helfer-extension
   ```
   *Un fichier `.code-workspace` est disponible à la racine*

3. **Charger dans le navigateur** :
   - Suivez les instructions de [Installation manuelle](#méthode-2-installation-manuelle-pour-les-tests) ci-dessus

4. **Vérifier le chargement** :
   - Ouvrez Forge of Empires
   - Ouvrez la console du navigateur (F12)
   - Vérifiez qu'aucun message d'erreur n'apparaît
   - Testez quelques fonctionnalités (ex: calculateurs)

---

## ⚙️ Configuration

### Configuration de base

1. **Ouvrir la popup** :
   - Cliquez sur l'icône de l'extension dans la barre d'outils

2. **Langue** :
   - L'extension détecte automatiquement la langue du navigateur
   - Pour forcer une langue :
     - Allez dans `chrome://extensions/`
     - Trouvez FoE Helper
     - Cliquez sur **"Détails"**
     - Dans **"Langue"**, sélectionnez votre langue préférée

3. **Paramètres** :
   - La plupart des modules ont leurs propres paramètres
   - Accessibles via l'icône de l'extension ou directement dans le jeu

### Configuration avancée

#### Via l'URL

Certains modules peuvent être configurés via des paramètres d'URL :
```
https://forgeofempires.com/game/?foehelper=[module]=[value]
```

#### Fichiers de configuration

| Fichier | Description | Modifiable |
|---------|-------------|------------|
| `manifest.json` | Configuration de l'extension | ⚠️ Avec prudence |
| `js/inject.js` | Logique de chargement | ❌ Non recommandé |
| `background.js` | Service Worker | ❌ Non recommandé |

---

## 🚀 Première utilisation

### Pour les utilisateurs

1. **Installer l'extension** (voir ci-dessus)
2. **Ouvrir Forge of Empires** dans votre navigateur
3. **Explorer les fonctionnalités** :
   - Passez la souris sur les éléments du jeu pour voir les améliorations
   - Cliquez sur l'icône de l'extension pour accéder aux outils
   - Consultez la documentation en ligne : [docs.foe-helper.com](https://docs.foe-helper.com/)

### Fonctionnalités disponibles

| Catégorie | Exemples | Activation |
|----------|----------|------------|
| **Alertes** | Rappels, notifications | Automatique |
| **Calculateurs** | Ressources, combat, GB | Via icône extension |
| **Carte** | Améliorations de la carte | Automatique |
| **Marché** | Outils de marché | Automatique |
| **Quêtes** | Suivi des quêtes | Automatique |
| **Statistiques** | Stats de ville, guilde | Via icône extension |

### Pour les développeurs

1. **Cloner et installer** (voir [Installation pour le développement](#installation-pour-le-développement))
2. **Explorer le code** :
   ```bash
   # Structure du projet
   foe-helfer-extension/
   ├── manifest.json          # Point d'entrée
   ├── background.js          # Service Worker
   ├── inject.js              # Injection principale
   ├── js/web/                # 71 modules
   └── vendor/                # Dépendances
   ```

3. **Comprendre l'architecture** :
   - Lire [ARCHITECTURE.md](./ARCHITECTURE.md)
   - Examiner `manifest.json`
   - Examiner `inject.js` pour comprendre le chargement des modules

4. **Tester une modification** :
   - Modifiez un fichier
   - **Recharger l'extension** dans le navigateur :
     - Allez sur `chrome://extensions/`
     - Trouvez FoE Helper
     - Cliquez sur **🔄** (Recharger)
   - Testez dans le jeu

---

## 🎨 Personnalisation

### Activer/Désactiver des modules

1. **Via la popup** :
   - Cliquez sur l'icône de l'extension
   - Allez dans l'onglet **"Modules"** ou **"Paramètres"**
   - Activez/désactivez les modules de votre choix

2. **Via le code** (pour développeurs) :
   - Modifiez `inject.js` pour changer quels modules sont chargés
   - Ou modifiez le fichier du module pour ajouter une condition

### Modifier les paramètres

La plupart des modules stockent leurs paramètres dans `chrome.storage` :

```javascript
// Pour lire un paramètre
chrome.storage.local.get(['parametreName'], function(result) {
  console.log(result.parametreName);
});

// Pour écrire un paramètre
chrome.storage.local.set({ parametreName: value });
```

### Ajouter une traduction

1. **Identifier la chaîne à traduire** :
   - Cherchez `__MSG_nomDeLaChaine__` dans le code

2. **Ajouter la traduction** :
   - Ouvrez `_locales/[langue]/messages.json`
   - Ajoutez l'entrée :
     ```json
     {
       "nomDeLaChaine": {
         "message": "Traduction",
         "description": "Description de la chaîne"
       }
     }
     ```

3. **Tester** :
   - Rechargez l'extension
   - Changez la langue du navigateur
   - Vérifiez que la traduction apparaît

---

## 🐛 Dépannage

### Problèmes courants

| Problème | Cause possible | Solution |
|----------|-----------------|----------|
| **Extension ne se charge pas** | Manifest invalide | Vérifiez `manifest.json` |
| **Modules non chargés** | Erreur dans inject.js | Ouvrez la console (F12) |
| **Fonctionnalités manquantes** | Module désactivé | Vérifiez les paramètres |
| **Traductions manquantes** | Fichier messages.json incomplet | Complétez les traductions |
| **Erreurs de permissions** | Manifest V3 strict | Vérifiez les permissions |

### Vérification de base

1. **Ouvrir la console du navigateur** :
   - F12 ou Ctrl+Shift+I (Windows/Linux)
   - Cmd+Opt+I (Mac)
   - Onglet **"Console"**

2. **Vérifier les erreurs** :
   - Filtrez par "foe" ou "error"
   - Les erreurs en rouge sont critiques
   - Les warnings en jaune peuvent être ignorés

3. **Vérifier le chargement des modules** :
   - Dans la console, tapez :
     ```javascript
     // Vérifier si FoE Helper est chargé
     typeof FoEHelper !== 'undefined'
     ```

4. **Vérifier le stockage** :
   - Dans la console :
     ```javascript
     chrome.storage.local.get(null, console.log)
     ```

### Problèmes spécifiques

#### "Extension non valide" dans Chrome
- **Cause** : Manifest V3 strict
- **Solution** : Vérifiez que `manifest.json` est valide
  - Utilisez [Manifest Validator](https://extensionworkshop.com/documentation/develop/manifest-validator/)

#### Fonctionnalités ne s'affichent pas
- **Cause 1** : Module non chargé
  - Vérifiez dans `inject.js` que le module est bien détecté
- **Cause 2** : CSS bloqué
  - Vérifiez `manifest.json` → `web_accessible_resources`
- **Cause 3** : JavaScript bloqué par CSP
  - Vérifiez que le code respecte la Content Security Policy

#### Problèmes de stockage
- **Cause** : Quota dépassé
- **Solution** :
  ```javascript
  // Vérifier l'espace utilisé
  chrome.storage.local.getBytesInUse(function(bytes) {
    console.log('Espace utilisé :', bytes, 'bytes');
  });
  ```

---

## 📚 Ressources supplémentaires

### Documentation
- [Site officiel](https://docs.foe-helper.com/) — Documentation utilisateur complète
- [ARCHITECTURE.md](./ARCHITECTURE.md) — Architecture technique détaillée
- [DEVELOPMENT.md](./DEVELOPMENT.md) — Guide de développement (à créer)

### Communauté
- **Discord** : [https://discord.gg/z97KZq4](https://discord.gg/z97KZq4) — Support et discussions
- **GitHub Issues** : [https://github.com/mainIine/foe-helfer-extension/issues](https://github.com/mainIine/foe-helfer-extension/issues) — Rapports de bugs
- **GitHub Discussions** : Pour les questions générales

### Développement
- **Repository** : [https://github.com/mainIine/foe-helfer-extension](https://github.com/mainIine/foe-helfer-extension)
- **Contributing** : Voir [DEVELOPMENT.md](./DEVELOPMENT.md) (à créer)
- **Changelog** : [changelog-en.md](../changelog-en.md) / [changelog-de.md](../changelog-de.md)

---

## 🎓 Prochaines étapes

### Pour les utilisateurs
1. ✅ Installer l'extension
2. ✅ Configurer selon vos besoins
3. ✅ Explorer les fonctionnalités
4. ⏳ Consulter [docs.foe-helper.com](https://docs.foe-helper.com/) pour plus de détails

### Pour les nouveaux contributeurs
1. ✅ Installer et tester l'extension
2. ✅ Comprendre l'architecture ([ARCHITECTURE.md](./ARCHITECTURE.md))
3. ⏳ Lire [DEVELOPMENT.md](./DEVELOPMENT.md) (à créer)
4. ⏳ Choisir une fonctionnalité à améliorer ou un bug à corriger
5. ⏳ Fork le repository et soumettre une Pull Request

### Pour les développeurs
1. ✅ Cloner et configurer le projet
2. ✅ Comprendre le flux de chargement des modules
3. ⏳ Identifier un module à améliorer
4. ⏳ Implémenter et tester
5. ⏳ Documenter les modifications

---

## 📝 Historique des modifications

| Date | Modification | Auteur |
|------|--------------|--------|
| 19 oct 2026 | Création initiale | Agent IA (intégration mémoire) |

---

*Ce guide est conçu pour vous aider à démarrer rapidement avec FoE Helper.*
*Pour toute question non couverte ici, consultez la documentation en ligne ou rejoignez la communauté Discord.*
