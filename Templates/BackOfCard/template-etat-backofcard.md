---
excalidraw-plugin: parsed
type: "template-etat"
template_version: "1.0.0"
processmetalanguage_version: "2.3"
tags: [excalidraw, processmetalanguage, etat, template]

property_definitions:
  # === PROPRIÉTÉS SYSTÈME ===
  id:
    source: "system"
    dynamic: false
    description: "UUID généré automatiquement"
    
  uid:
    source: "system"
    dynamic: false
    description: "Identifiant unique ETATYYYYMMDDXXX"
    
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
  nom_etat:
    source: "user"
    dynamic: false
    required: true
    prompt: "Nom de l'état (ex: En attente, En cours, Terminé):"
    
  description:
    source: "user"
    dynamic: true
    required: false
    prompt: "Description de l'état (optionnelle):"
    
  type_etat:
    source: "user"
    dynamic: false
    required: true
    prompt: "Type d'état:"
    options: ["initial", "intermediaire", "final", "erreur", "annulation"]
    
  processus_parent_id:
    source: "user"
    dynamic: false
    required: false
    prompt: "ID du processus parent (optionnel):"
    
  conditions_entree:
    source: "user"
    dynamic: true
    required: false
    type: "json"
    prompt: "Conditions d'entrée dans cet état (JSON optionnel):"
    default_value: "{}"
    
  conditions_sortie:
    source: "user"
    dynamic: true
    required: false
    type: "json"
    prompt: "Conditions de sortie de cet état (JSON optionnel):"
    default_value: "{}"
    
  duree_max:
    source: "user"
    dynamic: true
    required: false
    prompt: "Durée maximale dans cet état (ex: 24h, 3j):"
    
  # === PROPRIÉTÉS PAR DÉFAUT ===
  transitions:
    source: "default"
    dynamic: true
    default_value: "{ \"predecesseurs\": [], \"successeurs\": [] }"
    update_triggers: ["connect_states", "disconnect_states"]
    
  actions_entree:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_entry_action", "remove_entry_action"]
    
  actions_sortie:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_exit_action", "remove_exit_action"]
    
  comportements_associes:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["attach_behavior", "detach_behavior"]
    
  objets_dans_etat:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["object_enters_state", "object_leaves_state"]
    
  couleur_visuelle:
    source: "default"
    dynamic: true
    default_value: "#4CAF50"
    update_triggers: ["change_color"]
    
  statut:
    source: "default"
    dynamic: true
    default_value: "actif"
    options: ["actif", "inactif", "archive"]
    
  metriques:
    source: "default"
    dynamic: true
    default_value: "{ \"nombre_passages\": 0, \"temps_moyen\": null, \"derniere_activation\": null }"
    update_triggers: ["state_activated", "metrics_update"]
    
  historique:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["*"]
---

# 🏳️ État : ${nom_etat}

## 📋 Informations Générales

- **UID**: ${uid}
- **Type**: ${type_etat}
- **Processus Parent**: ${processus_parent_id}
- **Statut**: ${statut}
- **Créé le**: ${created_at}
- **Version**: ${version}

## 📝 Description

${description}

## ⚙️ Configuration

### Conditions d'Entrée
```json
${conditions_entree}
```

### Conditions de Sortie
```json
${conditions_sortie}
```

### Durée Maximale
${duree_max}

## 🔄 Transitions

### Prédécesseurs
${transitions.predecesseurs}

### Successeurs
${transitions.successeurs}

## 🎬 Actions

### Actions d'Entrée
${actions_entree}

### Actions de Sortie
${actions_sortie}

## 🎭 Comportements Associés
${comportements_associes}

## 📊 Métriques

- **Nombre de passages**: ${metriques.nombre_passages}
- **Temps moyen**: ${metriques.temps_moyen}
- **Dernière activation**: ${metriques.derniere_activation}

## 📦 Objets Actuellement dans cet État
${objets_dans_etat}

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
			"type": "ellipse",
			"x": -100,
			"y": -50,
			"width": 200,
			"height": 100,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "${couleur_visuelle}",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100
		},
		{
			"type": "text",
			"x": -50,
			"y": -10,
			"width": 100,
			"height": 25,
			"text": "${nom_etat}",
			"fontSize": 20,
			"fontFamily": 1,
			"textAlign": "center",
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