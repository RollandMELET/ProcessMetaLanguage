---
excalidraw-plugin: parsed
type: "template-action"
template_version: "1.0.0"
processmetalanguage_version: "2.3"
tags: [excalidraw, processmetalanguage, action, template]

property_definitions:
  # === PROPRIÉTÉS SYSTÈME ===
  id:
    source: "system"
    dynamic: false
    description: "UUID généré automatiquement"
    
  uid:
    source: "system"
    dynamic: false
    description: "Identifiant unique ACTYYYYMMDDXXX"
    
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
  nom_action:
    source: "user"
    dynamic: false
    required: true
    prompt: "Nom de l'action (ex: Valider, Envoyer, Calculer):"
    
  description:
    source: "user"
    dynamic: true
    required: false
    prompt: "Description de l'action (optionnelle):"
    
  comportement_parent_id:
    source: "user"
    dynamic: false
    required: false
    prompt: "ID du comportement parent (optionnel):"
    
  type_declencheur:
    source: "user"
    dynamic: false
    required: true
    prompt: "Type de déclencheur:"
    options: ["bouton", "automatique", "condition", "timer", "webhook", "api"]
    
  proprietes_modifiees:
    source: "user"
    dynamic: true
    required: false
    prompt: "Propriétés à modifier (séparées par virgules):"
    validation: "Vérifier que les propriétés sont dynamic:true"
    
  script_modification:
    source: "user"
    dynamic: true
    required: false
    type: "javascript"
    prompt: "Script de modification (JavaScript optionnel):"
    default_value: "// return modifiedProperties;"
    
  conditions_execution:
    source: "user"
    dynamic: true
    required: false
    type: "json"
    prompt: "Conditions d'exécution (JSON optionnel):"
    default_value: "{}"
    
  # === PROPRIÉTÉS PAR DÉFAUT ===
  parametres_action:
    source: "default"
    dynamic: true
    default_value: "{}"
    update_triggers: ["update_parameters"]
    
  resultat_attendu:
    source: "default"
    dynamic: true
    default_value: "{ \"type\": \"void\", \"format\": null }"
    update_triggers: ["define_output"]
    
  actions_suivantes:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["chain_action", "unchain_action"]
    
  log_modifications:
    source: "default"
    dynamic: false
    default_value: true
    type: "boolean"
    
  async:
    source: "default"
    dynamic: false
    default_value: false
    type: "boolean"
    
  timeout:
    source: "default"
    dynamic: true
    default_value: 5000
    update_triggers: ["set_timeout"]
    
  retry_config:
    source: "default"
    dynamic: true
    default_value: "{ \"max_attempts\": 3, \"delay\": 1000 }"
    update_triggers: ["configure_retry"]
    
  statut:
    source: "default"
    dynamic: true
    default_value: "actif"
    options: ["actif", "inactif", "test", "erreur"]
    
  statistiques:
    source: "default"
    dynamic: true
    default_value: "{ \"executions\": 0, \"succes\": 0, \"echecs\": 0, \"temps_moyen\": null }"
    update_triggers: ["action_executed"]
    
  historique:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["*"]
---

# ⚙️ Action : ${nom_action}

## 📋 Informations Générales

- **UID**: ${uid}
- **Type de Déclencheur**: ${type_declencheur}
- **Comportement Parent**: ${comportement_parent_id}
- **Statut**: ${statut}
- **Asynchrone**: ${async}
- **Créé le**: ${created_at}
- **Version**: ${version}

## 📝 Description

${description}

## 🎯 Configuration d'Exécution

### Conditions d'Exécution
```json
${conditions_execution}
```

### Propriétés Modifiées
${proprietes_modifiees}

### Script de Modification
```javascript
${script_modification}
```

## ⚙️ Paramètres

```json
${parametres_action}
```

### Résultat Attendu
```json
${resultat_attendu}
```

## 🔄 Chaînage

### Actions Suivantes
${actions_suivantes}

## ⏱️ Performance

- **Timeout**: ${timeout}ms
- **Configuration Retry**: 
```json
${retry_config}
```

## 📊 Statistiques

- **Exécutions**: ${statistiques.executions}
- **Succès**: ${statistiques.succes}
- **Échecs**: ${statistiques.echecs}
- **Temps Moyen**: ${statistiques.temps_moyen}ms

## 🔧 Options

- **Log des Modifications**: ${log_modifications}

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
			"type": "diamond",
			"x": -75,
			"y": -75,
			"width": 150,
			"height": 150,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#E3F2FD",
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
			"text": "⚙️ ${nom_action}",
			"fontSize": 14,
			"fontFamily": 1,
			"textAlign": "center",
			"verticalAlign": "middle"
		},
		{
			"type": "text",
			"x": -60,
			"y": 20,
			"width": 120,
			"height": 20,
			"text": "[${type_declencheur}]",
			"fontSize": 12,
			"fontFamily": 1,
			"textAlign": "center",
			"verticalAlign": "middle",
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