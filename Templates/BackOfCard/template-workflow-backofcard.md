---
excalidraw-plugin: parsed
type: "template-workflow"
template_version: "1.0.0"
processmetalanguage_version: "2.3"
tags: [excalidraw, processmetalanguage, workflow, template]

property_definitions:
  # === PROPRIÉTÉS SYSTÈME ===
  id:
    source: "system"
    dynamic: false
    description: "UUID généré automatiquement"
    
  uid:
    source: "system"
    dynamic: false
    description: "Identifiant unique WFYYYYMMDDXXX"
    
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
  nom_workflow:
    source: "user"
    dynamic: false
    required: true
    prompt: "Nom du workflow (ex: Fabrication Moteur BMW, Gestion Commande):"
    
  description:
    source: "user"
    dynamic: true
    required: false
    prompt: "Description du workflow (optionnelle):"
    
  objectif:
    source: "user"
    dynamic: true
    required: false
    prompt: "Objectif principal du workflow:"
    
  domaine_metier:
    source: "user"
    dynamic: false
    required: false
    prompt: "Domaine métier (ex: Production, Logistique, RH):"
    
  responsable:
    source: "user"
    dynamic: true
    required: false
    prompt: "Responsable du workflow:"
    
  # === PROPRIÉTÉS PAR DÉFAUT ===
  processus_orchestres:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_process", "remove_process", "reorder_processes"]
    
  points_synchronisation:
    source: "default"
    dynamic: true
    type: "sync_points"
    default_value: "[]"
    update_triggers: ["add_sync_point", "remove_sync_point", "update_sync_point"]
    
  propagation_modifications:
    source: "default"
    dynamic: true
    type: "rules_engine"
    default_value: "{ \"rules\": [], \"mode\": \"cascade\" }"
    update_triggers: ["add_propagation_rule", "remove_propagation_rule"]
    
  objets_geres:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["register_object", "unregister_object"]
    
  orchestration_config:
    source: "default"
    dynamic: true
    default_value: "{ \"mode\": \"sequential\", \"parallelism\": 1, \"error_handling\": \"stop\" }"
    update_triggers: ["configure_orchestration"]
    
  declencheurs_workflow:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_trigger", "remove_trigger"]
    
  contraintes_globales:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_constraint", "remove_constraint"]
    
  instances_workflow:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["start_workflow", "complete_workflow", "abort_workflow"]
    
  kpis_workflow:
    source: "default"
    dynamic: true
    default_value: "{ \"duree_moyenne\": null, \"taux_completion\": null, \"instances_total\": 0, \"processus_moyens\": null }"
    update_triggers: ["update_workflow_kpis"]
    
  statut:
    source: "default"
    dynamic: true
    default_value: "actif"
    options: ["actif", "suspendu", "maintenance", "archive"]
    
  monitoring:
    source: "default"
    dynamic: true
    default_value: "{ \"alertes\": [], \"seuils\": {}, \"notifications\": [] }"
    update_triggers: ["configure_monitoring", "add_alert", "clear_alerts"]
    
  historique:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["*"]
---

# 🏗️ Workflow : ${nom_workflow}

## 📋 Informations Générales

- **UID**: ${uid}
- **Domaine Métier**: ${domaine_metier}
- **Responsable**: ${responsable}
- **Statut**: ${statut}
- **Créé le**: ${created_at}
- **Version**: ${version}

## 📝 Description & Objectif

### Description
${description}

### Objectif Principal
${objectif}

## 🔄 Orchestration

### Configuration d'Orchestration
```json
${orchestration_config}
```

### Processus Orchestrés
${processus_orchestres}

### Points de Synchronisation
```json
${points_synchronisation}
```

## 🎯 Règles & Contraintes

### Propagation des Modifications
```json
${propagation_modifications}
```

### Contraintes Globales
${contraintes_globales}

## 🚀 Déclencheurs

${declencheurs_workflow}

## 📦 Objets Gérés

${objets_geres}

## 📊 Instances & Performance

### Instances Actives
${instances_workflow}

### KPIs du Workflow
- **Durée Moyenne**: ${kpis_workflow.duree_moyenne}
- **Taux de Complétion**: ${kpis_workflow.taux_completion}%
- **Total d'Instances**: ${kpis_workflow.instances_total}
- **Processus Moyens par Instance**: ${kpis_workflow.processus_moyens}

## 🔔 Monitoring

```json
${monitoring}
```

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
			"x": -300,
			"y": -150,
			"width": 600,
			"height": 300,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#FFF3E0",
			"fillStyle": "solid",
			"strokeWidth": 4,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"roundness": {"type": 3}
		},
		{
			"type": "rectangle",
			"x": -290,
			"y": -140,
			"width": 580,
			"height": 50,
			"angle": 0,
			"strokeColor": "#E65100",
			"backgroundColor": "#FFE0B2",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100
		},
		{
			"type": "text",
			"x": -280,
			"y": -125,
			"width": 560,
			"height": 30,
			"text": "🏗️ WORKFLOW: ${nom_workflow}",
			"fontSize": 24,
			"fontFamily": 1,
			"textAlign": "center",
			"verticalAlign": "middle",
			"strokeColor": "#E65100"
		},
		{
			"type": "line",
			"x": -290,
			"y": -90,
			"width": 580,
			"height": 0,
			"strokeColor": "#FF9800",
			"strokeWidth": 2,
			"strokeStyle": "dashed"
		},
		{
			"type": "text",
			"x": -270,
			"y": -80,
			"width": 540,
			"height": 200,
			"text": "🎯 Objectif: ${objectif}\n\n📄 Processus: ${processus_orchestres.length}\n📦 Objets gérés: ${objets_geres.length}\n🔄 Mode: ${orchestration_config.mode}\n📊 Instances: ${instances_workflow.length}\n\n🚦 Statut: ${statut}",
			"fontSize": 16,
			"fontFamily": 1,
			"textAlign": "left",
			"verticalAlign": "top"
		},
		{
			"type": "ellipse",
			"x": -50,
			"y": 100,
			"width": 100,
			"height": 40,
			"angle": 0,
			"strokeColor": "#FF9800",
			"backgroundColor": "#FFE0B2",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100
		},
		{
			"type": "text",
			"x": -40,
			"y": 110,
			"width": 80,
			"height": 20,
			"text": "ORCHESTRATION",
			"fontSize": 10,
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