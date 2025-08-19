# Guide d'Utilisation - Add BackOfTheCard to Drawing

**Version** : 1.0.0  
**Date** : 2025-01-19  
**Auteur** : Rolland MELET & Claude Code

---

## 🎯 Objectif

Ce guide explique comment utiliser le script `add-backofcard-to-drawing.md` pour enrichir vos dessins Excalidraw avec des templates BackOfTheCard générés automatiquement.

---

## 📋 Prérequis

### Logiciels Requis
- **Obsidian** avec plugin Excalidraw ≥ 2.0.0
- **Vault configuré** avec dossier Scripts

### Fichiers Requis
- Script : `Scripts/add-backofcard-to-drawing.md`
- Un fichier Excalidraw avec **exactement un objet**

---

## 🚀 Utilisation Pas à Pas

### Étape 1 : Préparation
1. **Ouvrir** un fichier `.excalidraw` existant
2. **Vérifier** qu'il contient exactement **un seul objet**
   - ✅ Un rectangle, cercle, ou forme
   - ❌ Pas de texte libre ou éléments multiples
3. **Basculer** en mode Excalidraw si nécessaire

### Étape 2 : Lancement du Script
1. **Ouvrir** la palette de commandes : `Cmd+P` (Mac) ou `Ctrl+P` (Windows)
2. **Taper** : "Excalidraw: Run script"
3. **Sélectionner** : `add-backofcard-to-drawing.md`

### Étape 3 : Configuration
#### Popup 1 : Nom du Template
- **Question** : "Entrez le nom du template à créer :"
- **Exemple** : `MonWorkflowProduction`
- **Note** : Ce nom deviendra le nom du fichier créé

#### Popup 2 : Type d'Objet
- **Question** : Sélection dans la liste
- **Options** : OBJET, WORKFLOW, PROCESSUS, ÉTAT, ACTION, COMPORTEMENT, VUE, RÈGLE, DÉCISION
- **Exemple** : Choisir `WORKFLOW`

### Étape 4 : Résultat
Le script génère automatiquement :
1. **Texte visuel** : `WORKFLOW: {{MonWorkflowProduction}}` (avec couleur)
2. **Fichier template** : `template-monworkflowproduction.md`
3. **Embed intégré** : Le template s'affiche dans le dessin

---

## 🎨 Types d'Objets et Couleurs

| Type | Couleur | Usage | Exemple |
|------|---------|--------|---------|
| **OBJET** | 🟢 Vert | Entités métier | Commande, Client, Produit |
| **WORKFLOW** | 🟠 Orange | Orchestration globale | Traitement Commande Complète |
| **PROCESSUS** | 🔵 Bleu | Cycle de vie objet | Processus Validation Commande |
| **ÉTAT** | 🟣 Violet | Situation d'un objet | En Attente, Validé, Rejeté |
| **ACTION** | 🔴 Rouge | Déclencheur transition | Valider, Envoyer, Calculer |
| **COMPORTEMENT** | 🟦 Cyan | Logique réutilisable | Interface Saisie, Validation |
| **VUE** | 🩷 Rose | Interface utilisateur | Dashboard, Formulaire |
| **RÈGLE** | 🤎 Marron | Contrainte métier | Règle de Gestion Prix |
| **DÉCISION** | 🟡 Jaune | Point de branchement | Choix Approbation |

---

## 📚 Structure du Template Généré

### Sections Automatiques
Chaque template contient **10 sections structurées** :

1. **📋 Métadonnées**
   - UID unique, type, dates, version
   
2. **📝 Description**
   - Exemple contextuel selon le type
   - Zone pour votre description personnalisée

3. **⚖️ Rules (Règles de Gestion)**
   - **Règles Métier** : Logique business
   - **Règles Techniques** : Contraintes système
   - **Règles de Transition** : Conditions de passage

4. **🏗️ Propriétés**
   - Propriétés de base communes
   - Propriétés spécifiques au type

5. **🔗 Relations**
   - Relations entrantes et sortantes
   - Liens hiérarchiques

6. **🔄 États et Transitions**
   - États possibles selon le type
   - Conditions de passage

7. **📊 Métriques et KPIs**
   - Indicateurs de performance
   - Métriques de suivi

8. **🔍 Points de Contrôle**
   - Validations requises
   - Contrôles qualité

9. **📚 Documentation**
   - Références et liens
   - Exemples d'usage

10. **📝 Notes**
    - Todo list personnalisable
    - Historique des modifications

---

## ✨ Exemples Inspirants Intégrés

### Pour un WORKFLOW
```markdown
## 📝 Description
Ce workflow orchestre plusieurs processus pour atteindre un objectif business (ex: Traitement Commande Complète).

## 🏗️ Propriétés Spécifiques
- **processus_orchestres**: Liste des processus coordonnés
- **declencheurs**: Événements qui initient le workflow
- **points_synchronisation**: Moments de coordination
```

### Pour un ÉTAT
```markdown
## 🔄 États et Transitions
- **Conditions d'entrée**: Si [condition] alors [action]
- **Conditions de sortie**: Quand [événement] alors [transition]
- **Actions d'entrée**: Actions à exécuter à l'entrée
- **Actions de sortie**: Actions à exécuter à la sortie
```

---

## 🚨 Gestion des Erreurs

### Messages Courants
| Message | Cause | Solution |
|---------|-------|----------|
| "❌ Veuillez ouvrir un fichier Excalidraw" | Fichier non .excalidraw | Ouvrir un fichier .excalidraw |
| "❌ Aucun élément dans le dessin" | Dessin vide | Créer un objet dans le dessin |
| "⚠️ Le dessin doit contenir exactement UN objet" | Trop d'objets | Garder un seul objet |
| "❌ Nom du template requis" | Nom vide | Saisir un nom valide |
| "Fichier existant, écraser ?" | Template existe | Choisir oui/non |

### Conseils de Dépannage
1. **Console Développeur** : F12 pour voir les logs détaillés
2. **Version Plugin** : Vérifier Excalidraw ≥ 2.0.0
3. **Permissions** : Vérifier l'accès en écriture au dossier

---

## 🎯 Bonnes Pratiques

### Nommage
- **Clair et descriptif** : `GestionCommandeClient` plutôt que `GCC`
- **Pas d'espaces** : Remplacés automatiquement par des tirets
- **Éviter caractères spéciaux** : Utiliser lettres et chiffres

### Organisation
- **Un template par concept** : Ne pas mélanger workflow et processus
- **Dossiers thématiques** : Organiser par projet ou domaine
- **Versions** : Numéroter les itérations importantes

### Édition Post-Génération
1. **Personnaliser** les exemples selon votre contexte
2. **Compléter** les sections avec vos données
3. **Supprimer** les exemples non pertinents
4. **Ajouter** des propriétés spécifiques

---

## 🔄 Workflow Recommandé

### 1. Conception Visuelle
- Créer le dessin Excalidraw avec les éléments principaux
- Un objet = un futur template BackOfTheCard

### 2. Génération Templates
- Lancer le script sur chaque objet individuellement
- Personnaliser le nom selon le rôle

### 3. Enrichissement
- Éditer chaque template généré
- Compléter avec vos données métier
- Supprimer les exemples non utiles

### 4. Liaison
- Utiliser les sections "Relations" pour lier les templates
- Référencer les UIDs pour traçabilité

---

## 📈 Évolutions Futures

### Prochaines Fonctionnalités Prévues
- Script de liaison automatique entre templates
- Génération batch pour plusieurs objets
- Export markdown complet du canvas
- Templates spécialisés par domaine métier

### Personnalisation Avancée
- Couleurs configurables par projet
- Templates de base modifiables
- Propriétés personnalisées par type

---

## 🆘 Support

### Ressources
- **Documentation** : `ProcessMetaLanguage/Documentation/`
- **Exemples** : Vault de test `PML-Beta-Test-v2`
- **GitHub** : https://github.com/RollandMELET/ProcessMetaLanguage

### Contact
- **Auteur** : Rolland MELET
- **Email** : rm@360sc.io
- **Version** : ProcessMetaLanguage v2.3

---

**Fin du guide d'utilisation**