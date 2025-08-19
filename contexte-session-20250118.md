# Contexte de Session - ProcessMetaLanguage BackOfTheCard

**Date** : 2025-01-18  
**Version Projet** : 2.0.0  
**Auteur** : Rolland MELET & Claude Code  
**État** : Templates et Script v2.0.0 créés, en attente de tests

---

## 📍 Situation Actuelle

### Problème Initial Résolu
L'utilisateur avait un script v1.1.1 qui ne trouvait pas les templates car :
- Chemins absolus codés en dur
- Ne correspondaient pas à la structure du vault Obsidian
- Confusion entre types d'objets workflow et catégories métier

### Solution Implémentée
Création complète d'un nouveau système v2.0.0 avec :
- Architecture template-first
- Utilisation des Settings Obsidian pour les chemins
- Templates pour tous les types ProcessMetaLanguage

---

## 🎯 Objectif du Projet

Créer un système complet ProcessMetaLanguage avec le concept "BackOfTheCard" :
- **Face Avant** : Dessin visuel dans Excalidraw
- **Face Arrière** : Métadonnées structurées dans fichier .md embedé

### Hiérarchie ProcessMetaLanguage v2.3
```
WORKFLOW → PROCESSUS → ÉTAT → COMPORTEMENT → ACTION → VUE
```

---

## ✅ Travail Complété (18/01/2025)

### 1. Scripts Créés

| Script | Version | État | Description |
|--------|---------|------|-------------|
| create-element-from-template.md | 2.0.0 | ✅ Créé | Script principal avec architecture template-first |
| create-object-from-template.md | 1.1.1 | ⚠️ Obsolète | Ancien script avec chemins codés en dur |

### 2. Templates BackOfTheCard (6/6)

Tous créés dans : `/ProcessMetaLanguage/Templates/BackOfCard/`

| Template | Fichier | État | Type d'Élément |
|----------|---------|------|----------------|
| OBJET | template-objet-backofcard.md | ✅ Existant | Éléments manipulés |
| ÉTAT | template-etat-backofcard.md | ✅ Créé | Situations dans processus |
| COMPORTEMENT | template-comportement-backofcard.md | ✅ Créé | Patterns réutilisables |
| ACTION | template-action-backofcard.md | ✅ Créé | Déclencheurs transitions |
| VUE | template-vue-backofcard.md | ✅ Créé | Interfaces utilisateur |
| PROCESSUS | template-processus-backofcard.md | ✅ Créé | Cycle de vie objet |
| WORKFLOW | template-workflow-backofcard.md | ✅ Créé | Orchestration globale |

### 3. Documentation

| Document | État | Description |
|----------|------|-------------|
| flux-script-create-object.md | ✅ Créé | Diagramme Mermaid du flux v1.1.1 |
| script-v2-explication.md | ✅ Créé | Explication simple du v2.0.0 |
| plan.md | ✅ Existant | Plan projet complet |

---

## 🔧 Configuration Requise

### Dans Obsidian Settings

**À CONFIGURER PAR L'UTILISATEUR :**
1. Ouvrir Settings → Templates
2. Dans "Template folder location", mettre : `ProcessMetaLanguage/Templates`
3. Sauvegarder

### Structure Attendue
```
Vault Obsidian/
├── ProcessMetaLanguage/
│   ├── Templates/
│   │   └── BackOfCard/
│   │       ├── template-objet-backofcard.md
│   │       ├── template-etat-backofcard.md
│   │       └── ... (tous les templates)
│   └── Scripts/
│       └── create-element-from-template.md (v2.0.0)
```

---

## 🐛 Problèmes Identifiés

### Script v1.1.1 (Ancien)
- ❌ Cherche dans : `/Users/rollandmelet/Library/CloudStorage/.../Templates/`
- ❌ Chemin absolu non portable
- ❌ Ne fonctionne pas dans le vault actuel

### Script v2.0.0 (Nouveau)
- ✅ Cherche dans : `app.vault.config.templatesFolder`
- ✅ Utilise les Settings Obsidian
- ✅ Portable et flexible
- ⏳ **EN ATTENTE DE TEST**

---

## 📋 Prochaines Étapes

### 1. Configuration (Utilisateur)
- [ ] Configurer le dossier Templates dans Settings Obsidian
- [ ] Vérifier que tous les templates sont présents

### 2. Tests du Script v2.0.0
- [ ] Tester création d'un OBJET
- [ ] Tester création d'un ÉTAT
- [ ] Vérifier l'affichage des embeds
- [ ] Valider la sélection de couleur

### 3. Si Tests OK
- [ ] Remplacer définitivement v1.1.1 par v2.0.0
- [ ] Documenter les cas d'usage
- [ ] Créer des exemples

### 4. Phase Suivante (Optionnel)
- [ ] Script de transformation canvas→markdown
- [ ] Scripts de liaison entre éléments
- [ ] Dashboard de visualisation

---

## 💡 Points Clés à Retenir

### Différences v1.1.1 vs v2.0.0

| Aspect | v1.1.1 (Ancien) | v2.0.0 (Nouveau) |
|--------|-----------------|------------------|
| **Chemins Templates** | Absolus codés en dur | Settings Obsidian |
| **Types Supportés** | 3 types d'objets | 7 types d'éléments |
| **Questions** | Codées dans le script | Définies dans template |
| **Couleurs** | Automatiques par type | Choix utilisateur |
| **Architecture** | Script-first | Template-first |
| **Portabilité** | Non portable | Totalement portable |

### Avantages v2.0.0
1. **Flexible** : Nouveaux types = nouveaux templates
2. **Intelligent** : Questions dynamiques selon template
3. **Portable** : Fonctionne sur n'importe quel vault
4. **User-friendly** : Choix de couleurs, meilleurs prompts

---

## 🔍 Debug Info

### Logs Console v2.0.0
Le script génère des logs détaillés avec `[timestamp] ProcessMetaLanguage v2:`

### Messages d'Erreur Communs
- "Template non trouvé" → Vérifier Settings Obsidian
- "Aucun fichier Excalidraw actif" → Ouvrir un .excalidraw d'abord

---

## 📝 Notes de Session

### Demandes Utilisateur Traitées
1. ✅ "demander le type d'objet à créer : OBJET, ETAT, WORKFLOW, PROCESSUS"
2. ✅ "le template doit lire depuis obsidian"
3. ✅ "demander la couleur"
4. ✅ "créer dans le dossier du fichier actif"
5. ✅ "charger template AVANT de poser questions"

### Feedback Utilisateur
- A identifié que le vault est dans Google Drive
- Script v1.1.1 génère erreur "Template non trouvé"
- Comprend maintenant la différence entre les versions

---

## 🚀 Commande de Reprise

Pour reprendre exactement où nous en sommes :

1. **Vérifier la configuration** Settings Obsidian
2. **Tester le script v2.0.0** avec un fichier Excalidraw
3. **Observer les logs** dans la console (F12)
4. **Ajuster si nécessaire** selon les résultats

---

**État de la Todo List Finale :**
- [x] Analyser les scripts existants
- [x] Créer template OBJET
- [x] Créer script v2.0.0
- [x] Créer templates ÉTAT, COMPORTEMENT, ACTION, VUE, PROCESSUS, WORKFLOW
- [ ] Tester le système complet
- [ ] Créer script transformation canvas→markdown

---

**Fin du contexte de session**