---
excalidraw-plugin: parsed
type: "template-comportement"
template_version: "1.0.0"
processmetalanguage_version: "2.3"
tags: [excalidraw, processmetalanguage, comportement, template]

property_definitions:
  # === PROPRIÉTÉS SYSTÈME ===
  id:
    source: "system"
    dynamic: false
    description: "UUID généré automatiquement"
    
  uid:
    source: "system"
    dynamic: false
    description: "Identifiant unique COMPYYYYMMDDXXX"
    
  created_at:
    source: "system"
    dynamic: false
    
  created_by:
    source: "system"
    dynamic: false
    
  modified_at:
    source: "system"
    dynamic: true
    
  modified_by:
    source: "system"
    dynamic: true
    
  version:
    source: "system"
    dynamic: true
    
  # === PROPRIÉTÉS UTILISATEUR ===
  nom_comportement:
    source: "user"
    dynamic: false
    required: true
    prompt: "Nom du comportement (ex: Validation Manager, Formulaire Saisie):"
    
  description:
    source: "user"
    dynamic: true
    required: false
    prompt: "Description du comportement (optionnelle):"
    
  etat_parent_id:
    source: "user"
    dynamic: false
    required: false
    prompt: "ID de l'état parent (optionnel):"
    
  est_template_reutilisable:
    source: "user"
    dynamic: false
    type: "boolean"
    prompt: "Ce comportement est-il un template réutilisable?"
    default_value: false
    
  sections_ui:
    source: "user"
    dynamic: true
    type: "json"
    prompt: "Sections d'interface (header, body, footer) - JSON optionnel:"
    default_value: "{ \"header\": \"\", \"body\": \"\", \"footer\": \"\" }"
    
  roles_autorises:
    source: "user"
    dynamic: true
    required: false
    prompt: "Rôles autorisés (séparés par virgules):"
    default_value: "tous"
    
  # === PROPRIÉTÉS PAR DÉFAUT ===
  actions_disponibles:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_action", "remove_action", "reorder_actions"]
    
  vues_associees:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["attach_view", "detach_view"]
    
  parametres_comportement:
    source: "default"
    dynamic: true
    default_value: "{}"
    update_triggers: ["update_parameters"]
    
  validations:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_validation", "remove_validation"]
    
  declencheurs:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_trigger", "remove_trigger"]
    
  statut:
    source: "default"
    dynamic: true
    default_value: "actif"
    options: ["actif", "inactif", "test", "archive"]
    
  instances_actives:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["instance_created", "instance_destroyed"]
    
  historique:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["*"]
---

# 🎭 Comportement : ${nom_comportement}

## 📋 Informations Générales

- **UID**: ${uid}
- **État Parent**: ${etat_parent_id}
- **Template Réutilisable**: ${est_template_reutilisable}
- **Statut**: ${statut}
- **Créé le**: ${created_at}
- **Version**: ${version}

## 📝 Description

${description}

## 🎨 Interface Utilisateur

### Sections
```json
${sections_ui}
```

## 👥 Contrôle d'Accès

- **Rôles Autorisés**: ${roles_autorises}

## ⚙️ Actions Disponibles

${actions_disponibles}

## 📱 Vues Associées

${vues_associees}

## 🔧 Paramètres

```json
${parametres_comportement}
```

## ✔️ Validations

${validations}

## 🎯 Déclencheurs

${declencheurs}

## 📊 Instances Actives

${instances_actives}

## 📜 Historique

${historique}

---

# Text Elements

# Embedded files

# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin",
	"elements": [
		{
			"type": "rectangle",
			"x": -150,
			"y": -75,
			"width": 300,
			"height": 150,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#FFE0B2",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"roundness": {"type": 3}
		},
		{
			"type": "text",
			"x": -100,
			"y": -50,
			"width": 200,
			"height": 25,
			"text": "🎭 ${nom_comportement}",
			"fontSize": 16,
			"fontFamily": 1,
			"textAlign": "center",
			"verticalAlign": "top"
		},
		{
			"type": "line",
			"x": -150,
			"y": -20,
			"width": 300,
			"height": 0,
			"strokeColor": "#1e1e1e",
			"strokeWidth": 1,
			"strokeStyle": "solid"
		},
		{
			"type": "text",
			"x": -140,
			"y": -10,
			"width": 280,
			"height": 75,
			"text": "Actions: ${actions_disponibles.length}\nVues: ${vues_associees.length}\nStatut: ${statut}",
			"fontSize": 14,
			"fontFamily": 1,
			"textAlign": "left",
			"verticalAlign": "middle"
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"gridSize": null
	},
	"files": {}
}
```
%%