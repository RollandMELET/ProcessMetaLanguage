# Flux d'Exécution - Script create-object-from-template.md

**Version** : 1.1.1  
**Date** : 2025-01-18  
**Auteur** : Rolland MELET & Claude Code  
**Description** : Diagramme de flux détaillé du script de création d'objets ProcessMetaLanguage

---

## 📊 Vue d'Ensemble du Flux

```mermaid
graph TD
    Start([Démarrage du Script]) --> CheckFile{Fichier Excalidraw<br/>actif?}
    
    CheckFile -->|Non| ErrorNoFile[❌ Erreur: Aucun fichier actif]
    CheckFile -->|Oui| SelectType[📋 Sélection du Type d'Objet]
    
    SelectType --> TypePopup{Popup: Choisir Type}
    TypePopup --> TypeResource[🔄 RESSOURCE]
    TypePopup --> TypeConsumable[📦 CONSOMMABLE]
    TypePopup --> TypeProduct[🏭 PRODUIT]
    
    TypeResource --> CollectProps
    TypeConsumable --> CollectProps
    TypeProduct --> CollectProps
    
    CollectProps[📝 Collecte des Propriétés] --> PropName[Nom de l'objet]
    PropName --> PropDesc[Description optionnelle]
    PropDesc --> PropCategory{Sélection Catégorie}
    
    PropCategory --> Categories[Composant<br/>Document<br/>Matériel<br/>Matière première<br/>Service<br/>Autre]
    
    Categories --> TypeSpecific{Propriétés<br/>spécifiques au type?}
    
    TypeSpecific -->|CONSOMMABLE| ConsProps[Quantité stock<br/>Numéro lot<br/>Date péremption]
    TypeSpecific -->|Autres| SkipCons[Passer]
    
    ConsProps --> CommonProps
    SkipCons --> CommonProps
    
    CommonProps[Propriétés métier JSON<br/>Tags] --> GenerateSystem[🔧 Génération Propriétés SYSTEM]
    
    GenerateSystem --> SystemGen[ID: UUID<br/>UID: OBJ20250118xxx<br/>URL: /objects/xxx<br/>Timestamps<br/>Version: 1]
    
    SystemGen --> ApplyDefaults[📌 Application Valeurs DEFAULT]
    
    ApplyDefaults --> DefaultVals[processus_objet_id: null<br/>etat_actuel_id: null<br/>priorite: normale<br/>statut: actif<br/>historique: vide<br/>Plus Spécifiques au type]
    
    DefaultVals --> GetFileName[📁 Obtenir nom fichier unique]
    
    GetFileName --> CheckExists{Fichier existe?}
    CheckExists -->|Oui| SuggestNames[Suggérer alternatives:<br/>nom_1<br/>nom_v2<br/>nom_copy<br/>Saisie manuelle]
    CheckExists -->|Non| CreateFile
    
    SuggestNames --> CreateFile[📄 Créer fichier MD]
    
    CreateFile --> LoadTemplate{Charger Template}
    LoadTemplate -->|Succès| ReplaceVars[Remplacer variables ${xxx}]
    LoadTemplate -->|Échec| TryPaths[Essayer chemins alternatifs]
    
    TryPaths -->|Trouvé| ReplaceVars
    TryPaths -->|Non trouvé| ErrorTemplate[❌ Template non trouvé]
    
    ReplaceVars --> SaveFile[💾 Sauvegarder fichier]
    
    SaveFile --> CreateEmbed[🎨 Créer Embed Excalidraw]
    
    CreateEmbed --> GenFileId[Générer FileID 40 chars]
    GenFileId --> ReadExcalidraw[Lire contenu Excalidraw]
    ReadExcalidraw --> ParseJSON[Parser JSON du drawing]
    
    ParseJSON --> CalcPosition[📍 Calculer position]
    CalcPosition --> CheckEmbeds{Embeds existants?}
    
    CheckEmbeds -->|Oui| Cascade[Décaler position 30px]
    CheckEmbeds -->|Non| NoOffset[Position centre vue]
    
    Cascade --> CreateElements
    NoOffset --> CreateElements
    
    CreateElements[🎨 Créer éléments visuels] --> CreateRect[Rectangle coloré<br/>selon type]
    CreateRect --> CreateText[Texte avec icône<br/>et nom objet]
    CreateText --> CreateImage[Image element<br/>pour embed]
    
    CreateImage --> AddToDrawing[Ajouter au drawing JSON]
    
    AddToDrawing --> AddEmbedEntry[📎 Ajouter entrée Embedded Files]
    
    AddEmbedEntry --> CheckEmbedSection{Section<br/>Embedded Files<br/>existe?}
    
    CheckEmbedSection -->|Oui| AppendEntry[Ajouter à la fin]
    CheckEmbedSection -->|Non| CreateSection[Créer section]
    
    AppendEntry --> UpdateJSON
    CreateSection --> UpdateJSON
    
    UpdateJSON[🔄 Mettre à jour JSON] --> SaveExcalidraw[💾 Sauvegarder Excalidraw]
    
    SaveExcalidraw --> ForceWrite[Forcer écriture disque]
    ForceWrite --> Wait[Attendre 1s]
    
    Wait --> RefreshView[🔄 Rafraîchir vue]
    
    RefreshView --> TryToggle{Vue Excalidraw?}
    TryToggle -->|Oui| ToggleMode[Toggle source/preview]
    TryToggle -->|Non| Skip[Passer]
    
    ToggleMode --> Success
    Skip --> Success
    
    Success[✅ Succès - Notification utilisateur]
    
    ErrorNoFile --> End([Fin])
    ErrorTemplate --> End
    Success --> End
    
    style Start fill:#e1f5e1
    style Success fill:#c8e6c9
    style ErrorNoFile fill:#ffcdd2
    style ErrorTemplate fill:#ffcdd2
    style End fill:#f5f5f5
    style TypeResource fill:#52C41A
    style TypeConsumable fill:#FA8C16
    style TypeProduct fill:#1890FF
```

---

## 🔍 Détail des Étapes Principales

### 1️⃣ **Initialisation et Vérifications**

```
├── Vérifier fichier Excalidraw actif
├── Charger configuration (chemins, dossiers)
└── Initialiser système de logging (DEBUG = true)
```

### 2️⃣ **Sélection du Type d'Objet**

```
├── Afficher popup avec titre "Choisir le type de l'objet"
├── Options disponibles:
│   ├── 🔄 RESSOURCE (vert #52C41A)
│   ├── 📦 CONSOMMABLE (orange #FA8C16)
│   └── 🏭 PRODUIT (bleu #1890FF)
└── Log: Type sélectionné
```

### 3️⃣ **Collecte des Propriétés USER**

```
├── Nom (obligatoire)
│   └── Prompt avec rappel [TYPE]
├── Description (optionnelle)
│   └── Prompt avec rappel [TYPE]
├── Catégorie métier
│   └── Suggester avec options prédéfinies
├── Propriétés spécifiques au type:
│   └── Si CONSOMMABLE:
│       ├── Quantité stock
│       ├── Numéro de lot
│       └── Date péremption
├── Propriétés métier (JSON)
└── Tags (séparés par virgules)
```

### 4️⃣ **Génération Automatique SYSTEM**

```yaml
Propriétés générées:
  id: UUID v4
  uid: OBJ{YYYYMMDD}{INDEX}
  url_propre: /objects/{uid}
  created_at: ISO 8601
  created_by: "User"
  modified_at: ISO 8601
  modified_by: "User"
  version: 1
```

### 5️⃣ **Application des Valeurs DEFAULT**

```yaml
Valeurs par défaut:
  processus_objet_id: "Non défini"
  etat_actuel_id: "Non défini"
  priorite: "normale"
  statut: "actif"
  historique: [création]
  
Spécifiques par type:
  RESSOURCE:
    cycles_utilisation: 0
    disponibilite: "disponible"
  CONSOMMABLE:
    (déjà collectées en USER)
  PRODUIT:
    date_production: "Non défini"
    conformite: "Non défini"
```

### 6️⃣ **Gestion du Nom de Fichier**

```
├── Vérifier existence
├── Si existe, proposer:
│   ├── nom(1), nom(2), nom(3)
│   ├── nom_v2
│   ├── nom_copy
│   └── Saisie manuelle
└── Créer dans: ProcessMetaLanguage/Objets/
```

### 7️⃣ **Création du Fichier depuis Template**

```
Chemins testés dans l'ordre:
1. Chemin absolu complet (TEMPLATE_PATH)
2. "Templates/BackOfCard/template-objet-backofcard.md"
3. "ProcessMetaLanguage/Templates/BackOfCard/template-objet-backofcard.md"
```

### 8️⃣ **Création de l'Embed Excalidraw**

```javascript
Structure de l'embed:
├── Rectangle coloré (selon type)
│   ├── width: 400
│   ├── height: 100
│   └── backgroundColor: couleur du type
├── Texte (icône + type + nom)
│   └── text: "{icon} {TYPE}\n{nom}"
└── Image element (embed fichier)
    ├── width: 400
    ├── height: 300
    ├── status: "saved"
    └── fileId: ID unique 40 chars
```

### 9️⃣ **Mise à jour du Fichier Excalidraw**

```
1. Parser JSON du drawing
2. Ajouter les 3 éléments
3. Ajouter entrée dans "# Embedded files"
   Format: {fileId}: [[{nom}|100%]]
4. Remplacer le JSON
5. Sauvegarder et forcer écriture
6. Attendre 1 seconde
7. Rafraîchir la vue (toggle source/preview)
```

---

## 🛠️ Points de Personnalisation

### Configuration Initiale

```javascript
const TEMPLATE_PATH = "..."; // Chemin du template
const OBJECTS_FOLDER = "ProcessMetaLanguage/Objets";
const DEBUG = true; // Active/désactive les logs
```

### Ajout d'un Nouveau Type d'Objet

1. Ajouter dans les options de sélection
2. Définir couleur dans `getColorByType()`
3. Définir icône dans `getIconByType()`
4. Ajouter propriétés spécifiques dans collecte
5. Ajouter valeurs par défaut spécifiques

### Modification des Catégories

```javascript
const categories = [
    "Composant",
    "Document", 
    "Matériel",
    "Matière première",
    "Service",
    "Autre"
    // Ajouter ici de nouvelles catégories
];
```

### Personnalisation de la Position

```javascript
// Centre de vue actuel
let x = viewCenter.x - 200;
let y = viewCenter.y - 150;

// Décalage cascade (modifiable)
const offset = (embedElements.length % 5) * 30;
```

### Dimensions des Éléments

```javascript
// Rectangle header
width: 400,  // Modifiable
height: 100, // Modifiable

// Embed image
width: 400,  // Modifiable
height: 300, // Modifiable
```

---

## 🐛 Points de Débogage

### Logs Critiques à Surveiller

1. **Template non trouvé** : Vérifier les chemins testés
2. **Structure Excalidraw non reconnue** : Vérifier le format du fichier
3. **Erreur création fichier** : Vérifier permissions et chemins
4. **Catégorie "false"** : Corrigé en v1.1.1

### Console Logs Structure

```text
[HH:MM:SS] ProcessMetaLanguage: === DÉMARRAGE DU SCRIPT ===
[HH:MM:SS] ProcessMetaLanguage: Fichier actif: {path}
[HH:MM:SS] ProcessMetaLanguage: Type sélectionné: {TYPE}
[HH:MM:SS] ProcessMetaLanguage: Template trouvé à: {path}
[HH:MM:SS] ProcessMetaLanguage: Fichier créé: {path}
[HH:MM:SS] ProcessMetaLanguage: === SUCCÈS ===
```

---

## 📈 Améliorations Possibles

1. **Validation avancée** :
   - Vérifier format JSON des propriétés métier
   - Valider dates (péremption)
   - Contrôler unicité des UIDs

2. **Gestion des erreurs** :
   - Rollback si échec création embed
   - Sauvegarde état avant modifications

3. **Optimisations** :
   - Cache du template en mémoire
   - Batch des modifications Excalidraw

4. **Fonctionnalités** :
   - Import/export de configurations
   - Templates de propriétés métier
   - Historique des créations

---

## 🔗 Fichiers Liés

- **Script** : `/Scripts/create-object-from-template.md`
- **Template** : `/Templates/BackOfCard/template-objet-backofcard.md`
- **Script Original** : `/Scripts/embed-object-from-template.md`
- **Plan Projet** : `/plan.md`

---

**Fin du document de flux**
