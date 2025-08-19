---
excalidraw-plugin: parsed
tags: [excalidraw, template, processmetalanguage, objet]

# Métadonnées du Template
template_type: "objet"
template_version: "1.0.0"
processmetalanguage_version: "2.3"
template_created: "2025-01-18"
template_author: "Rolland MELET & Claude Code"

# Définition des Propriétés de l'Objet
property_definitions:
  # === PROPRIÉTÉS SYSTÈME (générées automatiquement) ===
  id:
    source: "system"
    dynamic: false
    description: "UUID unique généré par 360SmartConnect"
    example: "550e8400-e29b-41d4-a716-446655440000"
    
  uid:
    source: "system"
    dynamic: false
    description: "Identifiant court unique"
    format: "OBJ{YYYYMMDD}{INDEX}"
    example: "OBJ20250118001"
    
  url_propre:
    source: "system"
    dynamic: false
    description: "URL permanente de l'objet"
    format: "/objects/{uid}"
    example: "/objects/OBJ20250118001"
    
  created_at:
    source: "system"
    dynamic: false
    description: "Timestamp de création ISO 8601"
    example: "2025-01-18T10:30:00Z"
    
  created_by:
    source: "system"
    dynamic: false
    description: "Utilisateur créateur"
    
  # === PROPRIÉTÉS UTILISATEUR (saisies à la création) ===
  nom:
    source: "user"
    dynamic: false
    required: true
    prompt: "Nom de l'objet:"
    validation: "Min 3 caractères"
    example: "Bloc Moteur BMW B48"
    
  description:
    source: "user"
    dynamic: true
    required: false
    prompt: "Description de l'objet (optionnelle):"
    multiline: true
    update_triggers: ["update_description", "enrich_metadata"]
    
  type_objet_workflow:
    source: "user"
    dynamic: false
    required: true
    prompt: "Type d'objet dans le workflow:"
    options: ["RESSOURCE", "CONSOMMABLE", "PRODUIT"]
    description: |
      RESSOURCE: Objets réutilisables qui cyclent dans le processus (moule, commande)
      CONSOMMABLE: Éléments consommés par le workflow (armature, étuvage, TIPI)
      PRODUIT: Objet produit par le workflow (voussoir, PAC)
    validation: "Doit être l'un des trois types"
    
  categorie:
    source: "user"
    dynamic: false
    required: false
    prompt: "Catégorie métier de l'objet:"
    options: ["Composant", "Document", "Matériel", "Matière première", "Service", "Autre"]
    
  proprietes_metier:
    source: "user"
    dynamic: true
    required: false
    type: "json"
    prompt: "Propriétés métier spécifiques (format JSON):"
    example: '{"reference": "BMW-B48-001", "poids": 120, "materiau": "aluminium"}'
    update_triggers: ["update_properties", "enrich_metadata"]
    
  tags:
    source: "user"
    dynamic: true
    required: false
    type: "array"
    prompt: "Tags (séparés par des virgules):"
    example: "moteur, bmw, production"
    update_triggers: ["add_tag", "remove_tag"]
    
  # === PROPRIÉTÉS SPÉCIFIQUES SELON TYPE ===
  # Pour RESSOURCE
  cycles_utilisation:
    source: "default"
    dynamic: true
    default_value: 0
    description: "Nombre de cycles d'utilisation (pour RESSOURCE)"
    update_triggers: ["increment_cycle", "reset_cycles"]
    applicable_to: ["RESSOURCE"]
    
  disponibilite:
    source: "default"
    dynamic: true
    default_value: "disponible"
    options: ["disponible", "en_utilisation", "maintenance", "indisponible"]
    description: "État de disponibilité (pour RESSOURCE)"
    update_triggers: ["change_availability"]
    applicable_to: ["RESSOURCE"]
    
  # Pour CONSOMMABLE
  quantite_stock:
    source: "user"
    dynamic: true
    default_value: 0
    type: "number"
    prompt: "Quantité en stock (pour CONSOMMABLE):"
    description: "Quantité disponible en stock"
    update_triggers: ["consume", "restock"]
    applicable_to: ["CONSOMMABLE"]
    
  lot_numero:
    source: "user"
    dynamic: false
    prompt: "Numéro de lot (pour CONSOMMABLE):"
    description: "Référence du lot de fabrication"
    applicable_to: ["CONSOMMABLE"]
    
  date_peremption:
    source: "user"
    dynamic: false
    type: "date"
    prompt: "Date de péremption (pour CONSOMMABLE, optionnel):"
    applicable_to: ["CONSOMMABLE"]
    
  # Pour PRODUIT
  date_production:
    source: "default"
    dynamic: true
    default_value: null
    type: "date"
    description: "Date de production (pour PRODUIT)"
    update_triggers: ["start_production", "complete_production"]
    applicable_to: ["PRODUIT"]
    
  conformite:
    source: "default"
    dynamic: true
    default_value: null
    options: ["conforme", "non_conforme", "en_controle", null]
    description: "Statut de conformité (pour PRODUIT)"
    update_triggers: ["quality_check", "approve", "reject"]
    applicable_to: ["PRODUIT"]
    
  traçabilite_composants:
    source: "default"
    dynamic: true
    default_value: []
    type: "array"
    description: "Liste des composants/consommables utilisés (pour PRODUIT)"
    update_triggers: ["add_component", "link_consumable"]
    applicable_to: ["PRODUIT"]
    
  # === PROPRIÉTÉS PAR DÉFAUT (initialisées à null) ===
  processus_objet_id:
    source: "default"
    dynamic: true
    default_value: null
    description: "ID du processus assigné à l'objet"
    update_triggers: ["assign_to_process", "remove_from_process"]
    validation: "Doit référencer un processus existant"
    
  etat_actuel_id:
    source: "default"
    dynamic: true
    default_value: null
    description: "ID de l'état actuel dans le processus"
    update_triggers: ["state_transition"]
    validation: "Si processus_objet_id existe, etat_actuel_id est OBLIGATOIRE"
    
  etat_actuel_nom:
    source: "default"
    dynamic: true
    default_value: null
    description: "Nom lisible de l'état actuel"
    update_triggers: ["state_transition"]
    
  workflow_parent_id:
    source: "default"
    dynamic: true
    default_value: null
    description: "ID du workflow parent"
    update_triggers: ["attach_to_workflow", "detach_from_workflow"]
    
  priorite:
    source: "default"
    dynamic: true
    default_value: "normale"
    options: ["basse", "normale", "haute", "critique"]
    update_triggers: ["change_priority"]
    
  statut:
    source: "default"
    dynamic: true
    default_value: "actif"
    options: ["actif", "inactif", "archive", "supprime"]
    update_triggers: ["change_status"]
    
  # === PROPRIÉTÉS SYSTÈME DYNAMIQUES ===
  modified_at:
    source: "system"
    dynamic: true
    description: "Dernière modification"
    update_triggers: ["*"]
    
  modified_by:
    source: "system"
    dynamic: true
    description: "Dernier utilisateur modificateur"
    update_triggers: ["*"]
    
  version:
    source: "system"
    dynamic: true
    default_value: 1
    description: "Version de l'objet"
    update_triggers: ["*"]
    
  historique:
    source: "system"
    dynamic: true
    default_value: []
    type: "array"
    description: "Historique complet des modifications"
    update_triggers: ["*"]
    structure:
      - timestamp: "ISO 8601"
      - property: "Propriété modifiée"
      - old_value: "Ancienne valeur"
      - new_value: "Nouvelle valeur"
      - action: "Action déclenchante"
      - user: "Utilisateur"
      
  liens:
    source: "default"
    dynamic: true
    default_value: []
    type: "array"
    description: "Liens vers d'autres objets"
    update_triggers: ["create_link", "remove_link"]
    
  fichiers_attaches:
    source: "default"
    dynamic: true
    default_value: []
    type: "array"
    description: "Fichiers attachés à l'objet"
    update_triggers: ["attach_file", "detach_file"]

# Configuration d'affichage selon type
display_config:
  RESSOURCE:
    couleur: "#52C41A"  # Vert - objets réutilisables
    icone: "🔄"         # Cycle
    forme: "rectangle"
    description: "Objet réutilisable qui cycle dans le processus"
  CONSOMMABLE:
    couleur: "#FA8C16"  # Orange - consommables
    icone: "📦"         # Paquet
    forme: "rectangle"
    description: "Élément consommé par le workflow"
  PRODUIT:
    couleur: "#1890FF"  # Bleu - produit final
    icone: "🏭"         # Usine/Production
    forme: "rectangle"
    description: "Objet produit par le workflow"
  taille_defaut:
    width: 200
    height: 100

---

==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==

# ${nom}

> **Type d'objet**: ${type_objet_workflow}  
> **UID**: ${uid}  
> **URL**: ${url_propre}  
> **Créé le**: ${created_at}  
> **Créé par**: ${created_by}  

## 📋 Informations Générales

| Propriété | Valeur |
|-----------|--------|
| **Nom** | ${nom} |
| **Type dans Workflow** | ${type_objet_workflow} |
| **Description** | ${description} |
| **Catégorie** | ${categorie} |
| **Tags** | ${tags} |
| **Priorité** | ${priorite} |
| **Statut** | ${statut} |

## 🎯 Propriétés Spécifiques (${type_objet_workflow})

### Pour RESSOURCE
| Propriété | Valeur |
|-----------|--------|
| **Cycles d'utilisation** | ${cycles_utilisation} |
| **Disponibilité** | ${disponibilite} |

### Pour CONSOMMABLE
| Propriété | Valeur |
|-----------|--------|
| **Quantité en stock** | ${quantite_stock} |
| **Numéro de lot** | ${lot_numero} |
| **Date de péremption** | ${date_peremption} |

### Pour PRODUIT
| Propriété | Valeur |
|-----------|--------|
| **Date de production** | ${date_production} |
| **Conformité** | ${conformite} |
| **Composants tracés** | ${traçabilite_composants} |

## 🔄 État dans le Processus

| Propriété | Valeur |
|-----------|--------|
| **Processus** | ${processus_objet_id} |
| **État Actuel** | ${etat_actuel_nom} (${etat_actuel_id}) |
| **Workflow Parent** | ${workflow_parent_id} |

## 📊 Propriétés Métier

```yaml
${proprietes_metier}
```

## 🔗 Relations

### Liens vers autres objets
${liens}

### Fichiers attachés
${fichiers_attaches}

## 📝 Historique des Modifications

```dataview
TABLE 
  timestamp as "Date/Heure",
  property as "Propriété",
  old_value as "Ancienne Valeur",
  new_value as "Nouvelle Valeur",
  action as "Action",
  user as "Utilisateur"
FROM this.historique
SORT timestamp DESC
```

## 🎨 Zone de Dessin Excalidraw

*L'utilisateur dessine ici la représentation visuelle de l'objet*

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
			"version": 1,
			"versionNonce": 1,
			"isDeleted": false,
			"id": "template-object-rect",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 0,
			"y": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#4A90E2",
			"width": 200,
			"height": 100,
			"seed": 1,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1737207600000,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1,
			"versionNonce": 1,
			"isDeleted": false,
			"id": "template-object-text",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 10,
			"y": 35,
			"strokeColor": "#ffffff",
			"backgroundColor": "transparent",
			"width": 180,
			"height": 30,
			"seed": 2,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1737207600000,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "📦 OBJET\n${nom}",
			"rawText": "📦 OBJET\n${nom}",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": null,
			"originalText": "📦 OBJET\n${nom}",
			"lineHeight": 1.25,
			"baseline": 25
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#1e1e1e",
		"currentItemBackgroundColor": "#4A90E2",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 2,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 1,
		"currentItemFontSize": 20,
		"currentItemTextAlign": "center",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 0,
		"scrollY": 0,
		"zoom": {
			"value": 1
		},
		"currentItemRoundness": "round",
		"gridSize": null,
		"gridColor": {
			"Bold": "#C9C9C9FF",
			"Regular": "#EDEDEDFF"
		},
		"currentStrokeOptions": null,
		"previousGridSize": null,
		"frameRendering": {
			"enabled": true,
			"clip": true,
			"name": true,
			"outline": true
		}
	},
	"files": {}
}
```
%%