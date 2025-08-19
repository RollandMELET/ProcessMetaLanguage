# Plan de Développement - ProcessMetaLanguage Templates & Scripts

**Version** : 1.0.0  
**Date** : 2025-01-18  
**Auteur** : Rolland MELET & Claude Code  
**Objectif** : Créer un système de templates BackOfTheCard pour enrichir les dessins Excalidraw avec des métadonnées ProcessMetaLanguage v2.3

---

## 🎯 Vue d'Ensemble

### Contexte
Enrichir des dessins Excalidraw existants avec des fichiers embedded standardisés qui décrivent de façon structurée les éléments d'un workflow selon le ProcessMetaLanguage v2.3.

### Concept "BackOfTheCard"
- **Face Avant** : Élément visuel dessiné par l'utilisateur dans Excalidraw
- **Face Arrière** : Fichier markdown embarqué avec métadonnées structurées

### Hiérarchie ProcessMetaLanguage v2.3
```
🏗️ WORKFLOW (orchestration globale)
    └── 📄 PROCESSUS (cycle de vie d'UN objet)
        └── 🏳️ ÉTAT (situation instantanée)
            └── 🎭 COMPORTEMENT (patterns réutilisables)
                └── ⚙️ ACTION (déclencheur de transition)
                    └── 📱 VUE/AFFICHAGE (UI/wireframe)
```

---

## 🔑 Principe Fondamental : Gestion des Propriétés

### Types de Propriétés

#### 1. SYSTEM (Créées automatiquement par 360SmartConnect)
```yaml
source: "system"
dynamic: false
exemples: [id, uid, url_propre, created_at]
```

#### 2. USER (Saisies par l'utilisateur)
```yaml
source: "user"
dynamic: true|false
required: true|false
prompt: "Message pour l'utilisateur"
```

#### 3. DEFAULT (Valeurs par défaut)
```yaml
source: "default"
dynamic: true|false
default_value: null|value
update_triggers: ["action1", "action2"]
```

#### 4. DYNAMIC (Modifiables par actions)
```yaml
dynamic: true
update_triggers: ["*"] # ou liste spécifique
validation: "règles de validation"
```

### Structure de Métadonnées pour Chaque Propriété
```yaml
property_metadata:
  source: "system|user|default"
  dynamic: true|false
  required: true|false
  default_value: null|value
  update_triggers: []
  prompt: "Question à poser"
  validation: "Règles"
```

---

## 📦 PHASE 1 : TEMPLATE OBJET (Semaine 1)

### Objectif
Créer le template de base pour les objets avec gestion complète des propriétés dynamiques.

### Template : template-objet-backofcard.md

#### Structure YAML Front Matter
```yaml
---
excalidraw-plugin: parsed
type: "template-objet"
template_version: "1.0.0"
processmetalanguage_version: "2.3"

property_definitions:
  # PROPRIÉTÉS SYSTÈME
  id:
    source: "system"
    dynamic: false
    description: "UUID généré par 360SmartConnect"
    
  uid:
    source: "system"
    dynamic: false
    description: "Identifiant unique court"
    
  url_propre:
    source: "system"
    dynamic: false
    description: "URL permanente /objects/{uid}"
    
  created_at:
    source: "system"
    dynamic: false
    
  # PROPRIÉTÉS UTILISATEUR
  nom:
    source: "user"
    dynamic: false
    required: true
    prompt: "Nom de l'objet:"
    
  description:
    source: "user"
    dynamic: true
    required: false
    prompt: "Description (optionnelle):"
    
  proprietes_metier:
    source: "user"
    dynamic: true
    required: false
    type: "json"
    prompt: "Propriétés métier spécifiques"
    
  # PROPRIÉTÉS PAR DÉFAUT
  processus_objet_id:
    source: "default"
    dynamic: true
    default_value: null
    update_triggers: ["assign_to_process"]
    
  etat_actuel_id:
    source: "default"
    dynamic: true
    default_value: null
    update_triggers: ["state_transition"]
    validation: "Si processus_objet_id existe, obligatoire"
    
  workflow_parent_id:
    source: "default"
    dynamic: true
    default_value: null
    update_triggers: ["attach_to_workflow"]
    
  historique:
    source: "default"
    dynamic: true
    default_value: []
    update_triggers: ["*"]
---
```

### Script : create-object-backofcard.md

#### Fonctionnalités
1. Collecte des propriétés USER via prompts interactifs
2. Génération automatique des propriétés SYSTEM
3. Application des valeurs DEFAULT
4. Création du fichier embedded avec validation
5. Historique des modifications

### Tests Phase 1
- [ ] Créer 3 objets différents
- [ ] Vérifier génération des UIDs uniques
- [ ] Valider les métadonnées YAML
- [ ] Tester la règle processus→état

---

## 🏳️ PHASE 2 : TEMPLATE ÉTAT (Semaine 2)

### Objectif
Gérer les états avec classification et transitions dynamiques.

### Template : template-etat-backofcard.md

#### Propriétés Spécifiques
```yaml
property_definitions:
  nom_etat:
    source: "user"
    required: true
    prompt: "Nom de l'état:"
    
  type_etat:
    source: "user"
    required: true
    prompt: "Type d'état:"
    options: ["initial", "intermediaire", "final"]
    
  processus_parent_id:
    source: "user"
    dynamic: false
    required: true
    
  transitions:
    source: "default"
    dynamic: true
    default_value:
      predecesseurs: []
      successeurs: []
    update_triggers: ["connect_states"]
    
  conditions_entree:
    source: "user"
    dynamic: true
    
  actions_sortie:
    source: "default"
    dynamic: true
    default_value: []
    update_triggers: ["add_exit_action"]
```

### Script : add-state-to-object.md

#### Fonctionnalités
1. Sélection objet existant via suggester
2. Classification automatique selon position
3. Mise à jour objet.etat_actuel_id
4. Connexion visuelle par flèches
5. Validation cohérence transitions

### Tests Phase 2
- [ ] Ajouter état initial à un objet
- [ ] Créer chaîne de 3 états
- [ ] Vérifier transitions bidirectionnelles
- [ ] Tester état final

---

## 🎭 PHASE 3 : TEMPLATES COMPORTEMENT & ACTION (Semaine 3)

### Template : template-comportement-backofcard.md

#### Propriétés Comportement
```yaml
property_definitions:
  nom_comportement:
    source: "user"
    required: true
    
  sections_ui:
    source: "user"
    dynamic: true
    type: "array"
    prompt: "Sections d'interface:"
    
  actions_disponibles:
    source: "default"
    dynamic: true
    default_value: []
    update_triggers: ["add_action", "remove_action"]
    
  est_template_reutilisable:
    source: "user"
    type: "boolean"
    prompt: "Template réutilisable?"
```

### Template : template-action-backofcard.md

#### Propriétés Action
```yaml
property_definitions:
  nom_action:
    source: "user"
    required: true
    
  declencheur:
    source: "user"
    required: true
    options: ["bouton", "automatique", "condition"]
    
  proprietes_modifiees:
    source: "user"
    prompt: "Propriétés à modifier:"
    validation: "Vérifier dynamic:true"
    
  script_modification:
    source: "user"
    dynamic: true
    type: "javascript"
    
  log_modifications:
    source: "default"
    default_value: true
```

### Scripts
- attach-behavior-to-state.md
- add-action-to-behavior.md

### Tests Phase 3
- [ ] Créer comportement avec 2 actions
- [ ] Tester modification propriété dynamique
- [ ] Valider logging des transitions
- [ ] Vérifier template réutilisable

---

## 📱 PHASE 4 : TEMPLATE VUE/AFFICHAGE (Semaine 3)

### Template : template-vue-backofcard.md

#### Propriétés Vue
```yaml
property_definitions:
  type_vue:
    source: "user"
    required: true
    options: ["formulaire", "dashboard", "liste", "wireframe"]
    
  proprietes_affichees:
    source: "user"
    dynamic: true
    validation: "Vérifier existence dans objet"
    
  proprietes_editables:
    source: "user"
    dynamic: true
    validation: "Vérifier dynamic:true"
    
  layout:
    source: "user"
    type: "wireframe"
    prompt: "Dessiner le wireframe"
```

### Script : link-view-to-behavior.md

### Tests Phase 4
- [ ] Créer vue formulaire
- [ ] Lier vue à comportement
- [ ] Tester propriétés éditables
- [ ] Valider wireframe

---

## 📋 PHASE 5 : TEMPLATE PROCESSUS (Semaine 4)

### Template : template-processus-backofcard.md

#### Propriétés Processus
```yaml
property_definitions:
  nom_processus:
    source: "user"
    required: true
    
  objet_cible_id:
    source: "user"
    required: true
    validation: "Objet doit exister"
    
  etats_processus:
    source: "default"
    dynamic: true
    default_value: []
    update_triggers: ["add_state", "remove_state"]
    
  regles_transition:
    source: "user"
    dynamic: true
    
  proprietes_modifiables:
    source: "user"
    validation: "Toutes doivent être dynamic:true"
```

### Script : create-process-for-object.md

### Tests Phase 5
- [ ] Créer processus avec 5 états
- [ ] Valider enchaînement logique
- [ ] Tester règles transition
- [ ] Vérifier cohérence globale

---

## 🏗️ PHASE 6 : TEMPLATE WORKFLOW (Semaine 4)

### Template : template-workflow-backofcard.md

#### Propriétés Workflow
```yaml
property_definitions:
  nom_workflow:
    source: "user"
    required: true
    
  processus_orchestres:
    source: "default"
    dynamic: true
    default_value: []
    update_triggers: ["add_process", "remove_process"]
    
  points_synchronisation:
    source: "user"
    dynamic: true
    type: "sync_points"
    
  propagation_modifications:
    source: "user"
    dynamic: true
    type: "rules_engine"
```

### Script : create-workflow-orchestration.md

### Tests Phase 6
- [ ] Orchestrer 3 processus
- [ ] Définir points synchronisation
- [ ] Tester propagation modifications
- [ ] Valider orchestration complète

---

## 📝 PHASE 7 : TRANSFORMATION CANVAS→MARKDOWN (Semaine 5)

### Script : canvas-to-processmetalanguage.md

#### Fonctionnalités
1. Parser hiérarchie complète du canvas
2. Extraire toutes les métadonnées
3. Générer documentation ProcessMetaLanguage v2.3
4. Export Dataview-ready
5. Validation contraintes et règles

### Structure Export
```markdown
# Documentation ProcessMetaLanguage v2.3

## Workflow : {nom}
### Processus orchestrés
- Processus 1
  - États
  - Transitions
- Processus 2

## Objets
### Objet 1
- Propriétés
- État actuel
- Historique

## Requêtes Dataview
\`\`\`dataview
TABLE nom, etat_actuel_id, processus_objet_id
FROM "ProcessMetaLanguage"
WHERE type = "objet"
\`\`\`
```

### Tests Phase 7
- [ ] Transformer canvas simple
- [ ] Exporter workflow complet
- [ ] Valider structure markdown
- [ ] Tester requêtes Dataview

---

## 📁 Structure Finale des Fichiers

```
ProcessMetaLanguage/
├── Templates/
│   └── BackOfCard/
│       ├── template-objet-backofcard.md
│       ├── template-etat-backofcard.md
│       ├── template-comportement-backofcard.md
│       ├── template-action-backofcard.md
│       ├── template-vue-backofcard.md
│       ├── template-processus-backofcard.md
│       └── template-workflow-backofcard.md
│
├── Scripts/
│   ├── create-object-backofcard.md
│   ├── add-state-to-object.md
│   ├── attach-behavior-to-state.md
│   ├── add-action-to-behavior.md
│   ├── link-view-to-behavior.md
│   ├── create-process-for-object.md
│   ├── create-workflow-orchestration.md
│   └── canvas-to-processmetalanguage.md
│
├── Examples/
│   ├── workflow-fabrication-moteur-bmw.md
│   └── objet-bloc-moteur-exemple.md
│
└── Tests/
    ├── test-phase1-objets.md
    ├── test-phase2-etats.md
    ├── test-phase3-comportements.md
    ├── test-phase4-vues.md
    ├── test-phase5-processus.md
    ├── test-phase6-workflows.md
    └── test-phase7-transformation.md
```

---

## 🔧 Fonctions Utilitaires Communes

### Validation des Propriétés Dynamiques
```javascript
function validateDynamicUpdate(property, action, template) {
    const propertyDef = template.property_definitions[property];
    
    if (!propertyDef.dynamic) {
        throw new Error(`Propriété ${property} non modifiable`);
    }
    
    if (propertyDef.update_triggers && 
        !propertyDef.update_triggers.includes(action) &&
        !propertyDef.update_triggers.includes("*")) {
        throw new Error(`Action ${action} non autorisée pour ${property}`);
    }
    
    return true;
}
```

### Historique des Modifications
```javascript
function logPropertyChange(objet, property, oldValue, newValue, action) {
    objet.historique.push({
        timestamp: new Date().toISOString(),
        property: property,
        old_value: oldValue,
        new_value: newValue,
        action: action,
        user: contexte.user
    });
}
```

### Collecte des Propriétés Utilisateur
```javascript
async function collectUserProperties(template) {
    const properties = {};
    
    for (const [key, def] of Object.entries(template.property_definitions)) {
        if (def.source === "user") {
            if (def.options) {
                properties[key] = await utils.suggester(
                    def.options,
                    def.options,
                    false,
                    def.prompt
                );
            } else if (def.type === "boolean") {
                properties[key] = await utils.confirm(def.prompt);
            } else if (def.type === "json") {
                const jsonStr = await utils.inputPrompt(
                    def.prompt + "\n(Format JSON)",
                    "{}"
                );
                properties[key] = JSON.parse(jsonStr);
            } else {
                properties[key] = await utils.inputPrompt(
                    def.prompt,
                    def.default_value || ""
                );
            }
            
            if (def.required && !properties[key]) {
                throw new Error(`${key} est obligatoire`);
            }
        }
    }
    
    return properties;
}
```

---

## 🎯 Critères de Succès

### Par Phase
1. ✅ Templates créent les bons embeds
2. ✅ Métadonnées YAML conformes v2.3
3. ✅ Scripts gèrent les relations
4. ✅ Propriétés dynamiques fonctionnelles
5. ✅ Historique complet tracé
6. ✅ Export markdown structuré
7. ✅ Requêtes Dataview opérationnelles

### Global
- [ ] Respect hiérarchie ProcessMetaLanguage
- [ ] Validation règles fondamentales
- [ ] Interface utilisateur intuitive
- [ ] Documentation complète
- [ ] Tests exhaustifs

---

## 📅 Planning

| Phase | Semaine | Livrable Principal |
|-------|---------|-------------------|
| 1 | S1 | Template & Script OBJET |
| 2 | S2 | Template & Script ÉTAT |
| 3 | S3 | Templates COMPORTEMENT & ACTION |
| 4 | S3 | Template VUE/AFFICHAGE |
| 5 | S4 | Template PROCESSUS |
| 6 | S4 | Template WORKFLOW |
| 7 | S5 | Script Transformation |

---

## 🚀 Prochaines Étapes

1. **Immédiat** : Créer template-objet-backofcard.md
2. **Jour 2** : Développer create-object-backofcard.md
3. **Jour 3** : Tests et validation Phase 1
4. **Jour 4** : Commencer Phase 2

---

## 📝 Notes

- L'utilisateur dessine les éléments visuels
- Le système gère les métadonnées
- Tout est tracé dans l'historique
- Les propriétés dynamiques permettent l'évolution
- La validation est stricte mais flexible

---

**Fin du document plan.md**