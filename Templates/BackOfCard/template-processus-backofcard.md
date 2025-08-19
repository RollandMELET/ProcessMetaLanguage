---
excalidraw-plugin: parsed
type: "template-processus"
template_version: "1.0.0"
processmetalanguage_version: "2.3"
tags: [excalidraw, processmetalanguage, processus, template]

property_definitions:
  # === PROPRIÉTÉS SYSTÈME ===
  id:
    source: "system"
    dynamic: false
    description: "UUID généré automatiquement"
    
  uid:
    source: "system"
    dynamic: false
    description: "Identifiant unique PROCYYYYMMDDXXX"
    
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
  nom_processus:
    source: "user"
    dynamic: false
    required: true
    prompt: "Nom du processus (ex: Fabrication Pièce, Validation Commande):"
    
  description:
    source: "user"
    dynamic: true
    required: false
    prompt: "Description du processus (optionnelle):"
    
  objet_cible_id:
    source: "user"
    dynamic: false
    required: true
    prompt: "ID de l'objet cible de ce processus:"
    validation: "L'objet doit exister"
    
  workflow_parent_id:
    source: "user"
    dynamic: false
    required: false
    prompt: "ID du workflow parent (optionnel):"
    
  duree_estimee:
    source: "user"
    dynamic: true
    required: false
    prompt: "Durée estimée du processus (ex: 2h, 3j, 1s):"
    
  priorite:
    source: "user"
    dynamic: true
    required: false
    prompt: "Priorité du processus:"
    options: ["basse", "normale", "haute", "critique"]
    default_value: "normale"
    
  # === PROPRIÉTÉS PAR DÉFAUT ===
  etats_processus:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_state", "remove_state", "reorder_states"]
    
  etat_initial_id:
    source: "default"
    dynamic: true
    default_value: null
    update_triggers: ["set_initial_state"]
    
  etats_finaux_ids:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_final_state", "remove_final_state"]
    
  regles_transition:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_transition_rule", "remove_transition_rule"]
    
  proprietes_modifiables:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["define_modifiable_properties"]
    validation: "Toutes doivent être dynamic:true"
    
  instances_actives:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["start_instance", "complete_instance", "abort_instance"]
    
  points_controle:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["add_checkpoint", "remove_checkpoint"]
    
  kpis:
    source: "default"
    dynamic: true
    default_value: "{ \"temps_moyen\": null, \"taux_succes\": null, \"instances_total\": 0 }"
    update_triggers: ["update_kpis"]
    
  statut:
    source: "default"
    dynamic: true
    default_value: "actif"
    options: ["actif", "suspendu", "archive", "test"]
    
  historique:
    source: "default"
    dynamic: true
    default_value: "[]"
    update_triggers: ["*"]
---

# 📄 Processus : ${nom_processus}

## 📋 Informations Générales

- **UID**: ${uid}
- **Objet Cible**: ${objet_cible_id}
- **Workflow Parent**: ${workflow_parent_id}
- **Priorité**: ${priorite}
- **Durée Estimée**: ${duree_estimee}
- **Statut**: ${statut}
- **Créé le**: ${created_at}
- **Version**: ${version}

## 📝 Description

${description}

## 🔄 États du Processus

### État Initial
${etat_initial_id}

### États du Processus
${etats_processus}

### États Finaux
${etats_finaux_ids}

## 🎯 Règles de Transition

```json
${regles_transition}
```

## 🔧 Configuration

### Propriétés Modifiables
${proprietes_modifiables}

### Points de Contrôle
${points_controle}

## 📊 Instances

### Instances Actives
${instances_actives}

## 📈 KPIs

- **Temps Moyen**: ${kpis.temps_moyen}
- **Taux de Succès**: ${kpis.taux_succes}%
- **Total d'Instances**: ${kpis.instances_total}

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
			"x": -250,
			"y": -100,
			"width": 500,
			"height": 200,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#E8F5E9",
			"fillStyle": "solid",
			"strokeWidth": 3,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"roundness": {"type": 3}
		},
		{
			"type": "text",
			"x": -200,
			"y": -80,
			"width": 400,
			"height": 30,
			"text": "📄 PROCESSUS: ${nom_processus}",
			"fontSize": 20,
			"fontFamily": 1,
			"textAlign": "center",
			"verticalAlign": "top",
			"strokeColor": "#2E7D32"
		},
		{
			"type": "line",
			"x": -250,
			"y": -40,
			"width": 500,
			"height": 0,
			"strokeColor": "#4CAF50",
			"strokeWidth": 2,
			"strokeStyle": "solid"
		},
		{
			"type": "text",
			"x": -230,
			"y": -30,
			"width": 460,
			"height": 100,
			"text": "🎯 Objet: ${objet_cible_id}\n⏱️ Durée: ${duree_estimee}\n📊 États: ${etats_processus.length}\n🚦 Statut: ${statut}",
			"fontSize": 16,
			"fontFamily": 1,
			"textAlign": "left",
			"verticalAlign": "middle"
		},
		{
			"type": "arrow",
			"x": -250,
			"y": 0,
			"width": 500,
			"height": 0,
			"points": [[0,0], [500,0]],
			"strokeColor": "#4CAF50",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"startArrowhead": "dot",
			"endArrowhead": "arrow"
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