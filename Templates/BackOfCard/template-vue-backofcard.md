---
excalidraw-plugin: parsed
type: "template-vue"
template_version: "1.0.0"
processmetalanguage_version: "2.3"
tags: [excalidraw, processmetalanguage, vue, template]

property_definitions:
  # === PROPRIÉTÉS SYSTÈME ===
  id:
    source: "system"
    dynamic: false
    description: "UUID généré automatiquement"
    
  uid:
    source: "system"
    dynamic: false
    description: "Identifiant unique VUEYYYYMMDDXXX"
    
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
  nom_vue:
    source: "user"
    dynamic: false
    required: true
    prompt: "Nom de la vue (ex: Dashboard Principal, Formulaire Contact):"
    
  description:
    source: "user"
    dynamic: true
    required: false
    prompt: "Description de la vue (optionnelle):"
    
  type_vue:
    source: "user"
    dynamic: false
    required: true
    prompt: "Type de vue:"
    options: ["formulaire", "dashboard", "liste", "detail", "wireframe", "kanban", "calendrier", "graphique"]
    
  comportement_parent_id:
    source: "user"
    dynamic: false
    required: false
    prompt: "ID du comportement parent (optionnel):"
    
  proprietes_affichees:
    source: "user"
    dynamic: true
    required: false
    prompt: "Propriétés à afficher (séparées par virgules):"
    validation: "Vérifier existence dans objet"
    
  proprietes_editables:
    source: "user"
    dynamic: true
    required: false
    prompt: "Propriétés éditables (séparées par virgules):"
    validation: "Vérifier que les propriétés sont dynamic:true"
    
  responsive:
    source: "user"
    dynamic: false
    type: "boolean"
    prompt: "Vue responsive (adaptative mobile/desktop)?"
    default_value: true
    
  # === PROPRIÉTÉS PAR DÉFAUT ===
  layout:
    source: "default"
    dynamic: true
    type: "wireframe"
    default_value: "{ \"type\": \"grid\", \"columns\": 12, \"rows\": \"auto\" }"
    update_triggers: ["update_layout"]
    
  composants_ui:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_component", "remove_component", "reorder_components"]
    
  styles:
    source: "default"
    dynamic: true
    default_value: "{ \"theme\": \"default\", \"custom_css\": \"\" }"
    update_triggers: ["update_styles"]
    
  bindings:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["bind_data", "unbind_data"]
    
  evenements:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_event_handler", "remove_event_handler"]
    
  permissions:
    source: "default"
    dynamic: true
    default_value: "{ \"view\": [\"all\"], \"edit\": [\"admin\"] }"
    update_triggers: ["update_permissions"]
    
  cache_config:
    source: "default"
    dynamic: true
    default_value: "{ \"enabled\": false, \"ttl\": 300 }"
    update_triggers: ["configure_cache"]
    
  statut:
    source: "default"
    dynamic: true
    default_value: "actif"
    options: ["actif", "inactif", "maintenance", "archive"]
    
  metriques_usage:
    source: "default"
    dynamic: true
    default_value: "{ \"vues\": 0, \"interactions\": 0, \"temps_moyen\": null }"
    update_triggers: ["view_accessed", "user_interaction"]
    
  historique:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["*"]
---

# 📱 Vue : ${nom_vue}

## 📋 Informations Générales

- **UID**: ${uid}
- **Type de Vue**: ${type_vue}
- **Comportement Parent**: ${comportement_parent_id}
- **Responsive**: ${responsive}
- **Statut**: ${statut}
- **Créé le**: ${created_at}
- **Version**: ${version}

## 📝 Description

${description}

## 🎨 Layout & Design

### Configuration Layout
```json
${layout}
```

### Styles
```json
${styles}
```

## 📊 Données

### Propriétés Affichées
${proprietes_affichees}

### Propriétés Éditables
${proprietes_editables}

### Bindings de Données
```json
${bindings}
```

## 🧩 Composants UI

${composants_ui}

## 🎯 Événements

${evenements}

## 🔒 Permissions

```json
${permissions}
```

## ⚡ Performance

### Configuration Cache
```json
${cache_config}
```

## 📈 Métriques d'Usage

- **Nombre de Vues**: ${metriques_usage.vues}
- **Interactions**: ${metriques_usage.interactions}
- **Temps Moyen**: ${metriques_usage.temps_moyen}ms

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
			"x": -200,
			"y": -150,
			"width": 400,
			"height": 300,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#F3E5F5",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"roundness": {"type": 3}
		},
		{
			"type": "rectangle",
			"x": -190,
			"y": -140,
			"width": 380,
			"height": 40,
			"angle": 0,
			"strokeColor": "#666666",
			"backgroundColor": "#E1BEE7",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100
		},
		{
			"type": "text",
			"x": -180,
			"y": -130,
			"width": 360,
			"height": 25,
			"text": "📱 ${nom_vue} [${type_vue}]",
			"fontSize": 18,
			"fontFamily": 1,
			"textAlign": "center",
			"verticalAlign": "middle"
		},
		{
			"type": "line",
			"x": -190,
			"y": -100,
			"width": 380,
			"height": 0,
			"strokeColor": "#666666",
			"strokeWidth": 1,
			"strokeStyle": "dashed"
		},
		{
			"type": "text",
			"x": -180,
			"y": -90,
			"width": 360,
			"height": 220,
			"text": "[ Zone de contenu ]\n\nPropriétés affichées:\n${proprietes_affichees}\n\nPropriétés éditables:\n${proprietes_editables}",
			"fontSize": 14,
			"fontFamily": 1,
			"textAlign": "left",
			"verticalAlign": "top",
			"strokeColor": "#666666"
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