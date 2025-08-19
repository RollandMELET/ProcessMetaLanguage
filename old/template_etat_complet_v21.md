# Template de Base - ÉTAT ProcessMetaLanguage v2.1 COMPLET

**Version** : 2.1  
**Date** : 2025-08-17  
**Auteur** : Rolland MELET  
**Évolutions** : 
- v2.0 : COMPORTEMENT DE BASE transféré au niveau PROCESS
- v2.1 : Ajout TEMPLATES de COMPORTEMENT SPÉCIFIQUE réutilisables

---

## 📊 DIAGRAMMES DE RÉFÉRENCE

### Diagramme 1 : Vue d'Ensemble du Template ÉTAT (Version 2.1)

```
                         📷 OBJET 📷
                    ┌─────────────────────┐
                    │ • ID unique         │
                    │ • Nom               │
                    │ • Identifiants GS1  │
                    │ • Propriétés static │
                    │ • Propriétés dynam  │
                    │ • Historique        │
                    │ • PROCESS (opt)     │
                    └─────────┬───────────┘
                              │
                              ▼
                      📄 PROCESS 📄
             ┌─────────────────────────────────────┐
             │ • ID unique                         │
             │ • Nom descriptif                   │
             │ • COMPORTEMENT DE BASE (GLOBAL)     │
             │ • Configuration interface standard │
             │ • Menu hamburger paramétrable      │
             │ • Règles contextuelles             │
             └─────────┬───────────────────────────┘
                       │
                       ▼
                 🏳️ ÉTAT ACTUEL 🏳️
             ┌─────────────────────────────────────┐
             │ • ID unique                         │
             │ • Nom descriptif                   │
             │ • Lien vers OBJET parent           │
             │ • Hérite COMPORTEMENT DE BASE      │
             │ • COMPORTEMENT PARTICULIER (opt)   │
             │ • Actions spécifiques à l'état     │
             │ 🆕 UTILISE TEMPLATE (nouveau)      │
             └─────────┬───────────────────────────┘
                       │
                       ▼
               ⚙️ COMPORTEMENTS ⚙️
        ┌─────────────────────────────────────────┐
        │                                         │
        ▼                                         ▼
🎯 HÉRITÉ DU PROCESS    📋 TEMPLATE RÉUTILISABLE    📘 ACTIONS
(COMPORTEMENT BASE)     (NOUVEAU v2.1)           DISPONIBLES
                                │
                                ▼
                    📚 BIBLIOTHÈQUE TEMPLATES 📚
                    ┌─────────────────────────┐
                    │ • Templates globaux     │
                    │ • Schémas paramètres    │ 
                    │ • Scripts génération    │
                    │ • Exemples usage        │
                    └─────────────────────────┘
```

### Diagramme 2 : Flux avec Héritage PROCESS + Templates v2.1

#### **Phase 1 : Résolution du Comportement Enrichie**
```
Utilisateur demande consultation OBJET
                    ↓
        📄 RÉSOLUTION COMPORTEMENT v2.1
                    ↓
    ┌─ Identifier PROCESS actuel ──────┐
    │  • Objet.process_id              │
    │  • Récupérer config PROCESS      │
    │  • Charger COMPORTEMENT DE BASE  │
    └─────────────┬────────────────────┘
                  ↓
    ┌─ Identifier ÉTAT actuel ─────────┐
    │  • Objet.etat_actuel_id         │
    │  • Vérifier mode comportement    │
    │  •   - Manuel (v2.0 classique)  │
    │  •   - Template (v2.1 nouveau)  │
    └─────────────┬────────────────────┘
                  ↓
    ┌─ 🆕 Si TEMPLATE utilisé ─────────┐
    │  • Charger depuis bibliothèque   │
    │  • Valider paramètres vs schéma  │
    │  • Générer interface dynamique   │
    │  • Appliquer surcharges état     │
    └─────────────┬────────────────────┘
                  ↓
    ┌─ Déterminer COMPORTEMENT FINAL ──┐
    │  • BASE (du PROCESS)             │
    │  • + TEMPLATE paramétré (v2.1)   │
    │  • + PARTICULIER (de l'ÉTAT)     │
    │  • Selon CONTEXTE utilisateur    │
    └─────────────┬────────────────────┘
                  ↓
            AFFICHAGE INTERFACE
```

#### **Phase 2 : Affichage Interface (Visible par Utilisateur)**
```
        📱 INTERFACE DÉTERMINÉE 📱
                    ↓
    ┌─ COMPORTEMENT DE BASE ───────────┐
    │  🔧 Interface Standard          │
    │  ├─ Identité OBJET              │
    │  ├─ État actuel                 │
    │  ├─ Propriétés statiques        │
    │  ├─ Propriétés dynamiques       │
    │  └─ Actions disponibles         │
    │                                 │
    │  🍔 Menu Hamburger PARAMÉTRABLE │
    │  ├─ Historique complet      ✓/✗ │
    │  ├─ Données techniques      ✓/✗ │
    │  ├─ Processus global        ✓/✗ │
    │  ├─ Audit trail (*)         ✓/✗ │
    │  ├─ Export données          ✓/✗ │
    │  └─ API Documentation       ✓/✗ │
    └─────────────────────────────────┘
                    OU
    ┌─ 🆕 COMPORTEMENT TEMPLATE ──────┐
    │  📋 Interface Générée Template  │
    │  ├─ Sections paramétrées        │
    │  ├─ Données filtrées/sélection  │
    │  ├─ Actions template+état       │
    │  └─ Format optimisé dynamique   │
    │                                 │
    │  🍔 Menu Hamburger ENRICHI      │
    │  ├─ 🔵 VUE STANDARD (AUTO)      │
    │  ├─ 📋 VUE TEMPLATE ORIGINAL    │
    │  ├─ Historique complet      ✓/✗ │
    │  ├─ Données techniques      ✓/✗ │
    │  ├─ Processus global        ✓/✗ │
    │  ├─ Audit trail (*)         ✓/✗ │
    │  ├─ Export données          ✓/✗ │
    │  └─ API Documentation       ✓/✗ │
    └─────────────────────────────────┘
                    OU
    ┌─ COMPORTEMENT PARTICULIER ──────┐
    │  🎨 Interface Spécifique        │
    │  ├─ Sections personnalisées     │
    │  ├─ Données métier mises avant  │
    │  ├─ Actions prioritaires        │
    │  └─ Format optimisé             │
    │                                 │
    │  🍔 Menu Hamburger ENRICHI      │
    │  ├─ 🔵 VUE STANDARD (AUTO)      │
    │  ├─ Historique complet      ✓/✗ │
    │  ├─ Données techniques      ✓/✗ │
    │  ├─ Processus global        ✓/✗ │
    │  ├─ Audit trail (*)         ✓/✗ │
    │  ├─ Export données          ✓/✗ │
    │  └─ API Documentation       ✓/✗ │
    └─────────────────────────────────┘

(*) Audit trail = Journal des modifications
    ├─ Qui a fait quoi sur l'objet
    ├─ Quand et pourquoi  
    ├─ Valeurs avant/après
    └─ Traçabilité réglementaire
```

### Diagramme 3 : Architecture Templates v2.1

```
    📚 BIBLIOTHÈQUE TEMPLATES 📚
                    ↓
    ┌─ Template Checklist ─────────────┐
    │  • Schema paramètres             │
    │  • Script génération interface   │
    │  • Exemples usage               │
    └─────────────┬────────────────────┘
                  │
    ┌─ Template Dashboard ─────────────┐
    │  • Métriques configurables      │
    │  • Formats visualisation        │
    │  • Seuils alerte paramètres     │
    └─────────────┬────────────────────┘
                  │
    ┌─ Template Formulaire ────────────┐
    │  • Champs dynamiques            │
    │  • Validations paramètrables    │
    │  • Actions soumission config    │
    └─────────────┬────────────────────┘
                  ↓
            UTILISÉ PAR ÉTATS
                  ↓
    ┌─ État A ─┐  ┌─ État B ─┐  ┌─ État C ─┐
    │Template  │  │Template  │  │Template  │
    │Checklist │  │Checklist │  │Dashboard│
    │Sect[1,2] │  │Sect[3,4] │  │KPI[T,P] │
    └──────────┘  └──────────┘  └─────────┘
```

### Diagramme 4 : Gestion du CONTEXTE (Inchangé v2.0)

```
    👤 UTILISATEUR          🕐 TEMPORALITÉ
    ├─ ID unique            ├─ Timestamp accès
    ├─ Rôles               ├─ Shift (M/A/N)
    ├─ Permissions         ├─ Heures ouvrables
    └─ Département         └─ Période

    💻 TECHNIQUE            🏭 MÉTIER  
    ├─ Device type         ├─ Niveau urgence
    ├─ Résolution          ├─ Mode maintenance
    ├─ Mode offline        ├─ Audit actif
    └─ Capabilities        └─ Contraintes spé.
                    ↓
            🎯 COMPORTEMENT ADAPTÉ
            ├─ Interfaces exposées
            ├─ Actions disponibles
            ├─ Niveau détail
            └─ Format présentation
```

### Diagramme 5 : Structure des ACTIONS (Enrichie v2.1)

```
        📘 ACTION UTILISATEUR 📘
                    ↓
    ┌─ Configuration ──────────────────┐
    │  • ID unique                   │
    │  • Nom descriptif              │
    │  • Conditions visibilité       │
    │  • Configuration logging       │
    │    ├─ Historique interne ✓/✗   │
    │    ├─ Log externe ✓/✗          │
    │    ├─ Fichier log spécifique   │
    │    └─ Niveau détail            │
    └──────────────┬──────────────────┘
                   ↓
    ┌─ Définition ────────────────────┐
    │  📝 Langage naturel            │
    │  ├─ Étapes en français         │
    │  └─ Description processus      │
    │                                │
    │  💻 Script JavaScript          │
    │  ├─ Vérifications préalables   │
    │  ├─ Exécution logique          │
    │  ├─ Modifications données      │
    │  ├─ Logging automatique        │
    │  └─ Détermination nouvel état  │
    │                                │
    │  🆕 PROVENANCE v2.1            │
    │  ├─ Du PROCESS (communes)      │
    │  ├─ Du TEMPLATE (générées)     │
    │  └─ De l'ÉTAT (spécifiques)    │
    └──────────────┬──────────────────┘
                   ↓
    ┌─ Interface Saisie ─────────────┐
    │  📝 Formulaire dynamique       │
    │  ├─ Champs typés               │
    │  ├─ Validations                │
    │  └─ Conditions obligatoires    │
    └──────────────┬──────────────────┘
                   ↓
    ┌─ Résultat Obligatoire ─────────┐
    │  📄 Transition ÉTAT            │
    │  ├─ Nouvel état cible          │
    │  ├─ Conditions transition      │
    │  └─ Même état (boucle) OK      │
    └─────────────────────────────────┘
```

### Diagramme 6 : Système de Logging Intégré (Inchangé v2.0)

```
        🎯 ÉVÉNEMENT (Action/Consultation) 🎯
                         ↓
    ┌─ Switch Logging Historique Interne ─┐
    │                                     │
    │  ✓ SI ACTIVÉ                        │
    │     ↓                               │
    │  📚 OBJET.historique.add()          │
    │  ├─ Événement structuré             │
    │  ├─ Métadonnées complètes           │
    │  ├─ Accessible via API              │
    │  └─ Intégré aux données objet       │
    └─────────────────────────────────────┘
                         ↓
    ┌─ Switch Logging Externe ────────────┐
    │                                     │
    │  ✓ SI ACTIVÉ                        │
    │     ↓                               │
    │  📄 logToFile(fichier.log)          │
    │  ├─ Fichier spécialisé par type    │
    │  ├─ Format JSON/CSV/TXT             │
    │  ├─ Rotation automatique            │
    │  └─ Analyse indépendante            │
    └─────────────────────────────────────┘
                         ↓
    ┌─ Niveaux de Détail ────────────────┐
    │  📊 MINIMAL                         │
    │  └─ timestamp, action, user, objet  │
    │                                     │
    │  📈 STANDARD                        │
    │  └─ + résultats, durée, contexte    │
    │                                     │
    │  📝 DÉTAILLÉ                        │
    │  └─ + params, étapes, erreurs, full │
    └─────────────────────────────────────┘
```

### Diagramme 7 : Métriques et Cycles de Consultation (Inchangé v2.0)

```
    👤 Utilisateur consulte OBJET
                ↓
    📊 Mise à jour métriques OBJET
    ├─ compteur_consultations++
    ├─ derniere_consultation = now
    └─ type_consultation = détecté
                ↓
    ❓ Type consultation détecté ?
                ↓
    ┌─ PREMIÈRE ──────────────────────┐
    │  • consultations_vides = 0      │
    │  • Métriques initialisées       │
    └─────────┬────────────────────────┘
              │
    ┌─ APRÈS ACTION ──────────────────┐
    │  • consultations_vides = 0      │  
    │  • Cycle normal                 │
    └─────────┬────────────────────────┘
              │
    ┌─ SANS ACTION (RE-CONSULTATION) ─┐
    │  • consultations_vides++        │
    │  • Vérification seuils          │
    │  • Alertes possibles            │
    └─────────┬────────────────────────┘
              ↓
    🚨 Si seuil atteint (ex: 5 consultations vides)
    ├─ Proposer aide contextuelle
    ├─ Action spéciale visible
    └─ Log alerte comportementale
```

### Diagramme 8 : Exposition API pour Systèmes Tiers (Enrichie v2.1)

```
    🔌 API REST Standard 🔌
              ↓
    GET /api/v1/objets/{id}/etats/{etat_id}
    └─ Retourne vue comportement (base/template/particulier)
              ↓
    GET /api/v1/objets/{id}/etats/{etat_id}/actions  
    └─ Liste actions (process+template+état) selon contexte
              ↓
    POST /api/v1/objets/{id}/etats/{etat_id}/actions/{action_id}
    └─ Exécute action + transition état
              ↓
    GET /api/v1/objets/{id}/etats/{etat_id}/live
    └─ WebSocket données temps réel
              ↓
    GET /api/v1/objets/{id}/etats/{etat_id}/historique
    └─ Historique complet avec pagination
              ↓
    🆕 GET /api/v1/templates/{template_id}
    └─ Informations template + schéma paramètres
              ↓
    🆕 POST /api/v1/templates/{template_id}/preview
    └─ Prévisualisation template avec paramètres test
```

### Diagramme 9 : Résumé Template en Une Vue v2.1

```
📷 OBJET (données + métriques) 
    ↓
📄 PROCESS (comportement de base)
    ↓
🏳️ ÉTAT (hérite + template optionnel + particulier optionnel)
    ↓ (pré-affichage)
🎯 ACTION AUTO CONSULTATION (logging + métriques)  
    ↓
📱 INTERFACE (standard/template/particulière)
    ↓ (interaction)
📘 ACTIONS UTILISATEUR (process+template+état ou aucune)
    ↓
📄 TRANSITION ÉTAT (même état OK) + LOGGING
    ↓
🔄 CYCLE RECOMMENCE...

🆕 📚 BIBLIOTHÈQUE TEMPLATES (globale, réutilisable)
```

---

## 1. STRUCTURE FONDAMENTALE RÉVISÉE v2.1

### 1.1 Hiérarchie OBJET → PROCESS → ÉTAT (Enrichie Templates)
```yaml
objet:
  id: "uuid-unique-objet"
  nom: "Nom_Objet"
  process_actuel_id: "uuid-process" # Référence au PROCESS actuel
  etat_actuel_id: "uuid-etat" # Référence à l'ÉTAT actuel
  
process:
  id: "uuid-unique-process"
  nom: "Nom_Process"
  description: "Description du processus métier"
  
  # COMPORTEMENT DE BASE (commun à tous les états du process)
  comportement_base:
    interface_standard: {...}
    menu_hamburger: {...}
    regles_contexte: {...}
    action_automatique_consultation: {...}
    
etat:
  id: "uuid-unique-etat"
  nom: "Nom_État"
  process_id: "uuid-process-parent" # OBLIGATOIRE - Référence au PROCESS
  objet_id: "uuid-objet-parent" # Référence à l'OBJET
  
  # COMPORTEMENT PARTICULIER v2.1 (3 modes possibles)
  comportement_particulier:
    actif: false
    mode: "manuel" # manuel|utilise_template|mixte
    
    # MODE MANUEL (v2.0 classique)
    interface_specifique: {...}
    
    # 🆕 MODE TEMPLATE (v2.1 nouveau)
    template_utilise:
      template_id: "nom_template"
      template_version: "1.0"
      parametres: {...}
      surcharges: {...}
    
    # MODE MIXTE (template + spécifications manuelles)
    interface_mixte:
      base_template: {...}
      ajouts_manuels: {...}
```

### 1.2 🆕 Bibliothèque Templates Comportement
```yaml
bibliotheque_templates:
  templates:
    - id: "affichage_checklist"
      nom: "Affichage Checklist Paramétrable"
      version: "1.0"
      description: "Template pour afficher sections de checklist"
      schema_parametres: {...}
      comportement_template: {...}
      exemples_usage: {...}
      
    - id: "dashboard_metriques"
      nom: "Dashboard Métriques Configurable"
      version: "1.0"
      description: "Template dashboard avec métriques sélectionnables"
      schema_parametres: {...}
      comportement_template: {...}
      
    - id: "formulaire_dynamique"
      nom: "Formulaire Dynamique Paramétrable"
      version: "1.0"
      description: "Template formulaire avec champs configurables"
      schema_parametres: {...}
      comportement_template: {...}
```

### 1.3 Résolution du Comportement v2.1
```yaml
resolution_comportement_v21:
  ordre_priorite:
    1. "Chargement COMPORTEMENT DE BASE depuis PROCESS"
    2. "SI mode=utilise_template: Chargement TEMPLATE + application PARAMÈTRES"
    3. "SI mode=manuel: Application COMPORTEMENT PARTICULIER classique"
    4. "SI mode=mixte: Fusion TEMPLATE + AJOUTS MANUELS"
    5. "Application des SURCHARGES niveau ÉTAT"
    6. "Application des règles CONTEXTUELLES finales"
    
  regles_fusion:
    - "CONTEXTE > SURCHARGES ÉTAT > TEMPLATE/PARTICULIER > COMPORTEMENT BASE"
    - "Menu hamburger enrichi automatiquement selon mode"
    - "Actions fusionnées : PROCESS + TEMPLATE + ÉTAT"
    - "Validation paramètres template obligatoire"
```

---

## 🆕 2. BIBLIOTHÈQUE TEMPLATES COMPORTEMENT

### 2.1 Structure d'un Template Comportement
```yaml
template_comportement:
  # Métadonnées
  id: "affichage_checklist"
  nom: "Affichage Checklist Paramétrable"
  description: "Template pour afficher des sections de checklist selon paramètres"
  version: "1.0"
  auteur: "équipe_processus"
  date_creation: "2025-08-17"
  derniere_modification: "2025-08-17"
  
  # Documentation
  documentation:
    description_longue: |
      Ce template permet d'afficher des sections spécifiques d'un document 
      checklist avec différents formats d'affichage et actions configurables.
    cas_usage:
      - "États de préparation avec contrôles préalables"
      - "États de validation avec contrôles finaux"
      - "États de maintenance avec checklist sécurité"
    
  # Schéma des paramètres (OBLIGATOIRE)
  schema_parametres:
    - nom: "document_source"
      type: "string"
      description: "Chemin vers le document checklist source"
      obligatoire: true
      exemple: "checklist_qualite_v2.yaml"
      validation: "file_exists"
      
    - nom: "sections_a_afficher"
      type: "array"
      element_type: "integer"
      description: "Liste des numéros de sections à afficher"
      obligatoire: true
      exemple: [1, 3, 5]
      validation: "min_length:1"
      
    - nom: "format_affichage"
      type: "enum"
      valeurs: ["liste_cochable", "tableau", "cards", "accordeon"]
      defaut: "liste_cochable"
      description: "Format d'affichage des éléments"
      
    - nom: "titre_personnalise"
      type: "string"
      obligatoire: false
      description: "Titre personnalisé pour cette instance"
      max_length: 100
      
    - nom: "actions_par_element"
      type: "array"
      element_type: "string"
      obligatoire: false
      description: "Actions disponibles sur chaque élément"
      exemple: ["valider", "commenter", "rejeter"]
      validation: "valid_actions"
      
    - nom: "couleur_theme"
      type: "string"
      format: "color_hex"
      defaut: "#1976D2"
      description: "Couleur du thème pour cette instance"
      
    - nom: "icone_section"
      type: "string"
      defaut: "📋"
      description: "Icône affichée pour chaque section"
  
  # Comportement du template
  comportement_template:
    interface_specifique:
      nom: "Interface_Checklist_Parametree"
      type: "template_genere"
      
      sections_dynamiques:
        - nom: "Titre Section"
          contenu: "parametres.titre_personnalise OR 'Checklist'"
          format: "h2"
          style: "color: parametres.couleur_theme"
          
        - nom: "Checklist Filtrée"
          source_donnees: "charger_document(parametres.document_source)"
          filtre: "sections IN parametres.sections_a_afficher"
          format: "parametres.format_affichage"
          icone: "parametres.icone_section"
          
        - nom: "Actions Disponibles"
          contenu: "generer_actions(parametres.actions_par_element)"
          format: "boutons_contextuels"
          visible_si: "parametres.actions_par_element.length > 0"
    
    # Script de génération interface (OBLIGATOIRE)
    script_generation: |
      async function genererInterface(objet, parametres, contexte) {
        // 1. Validation des paramètres obligatoire
        const validationResult = validateParameters(parametres, this.schema_parametres);
        if (!validationResult.valid) {
          throw new Error(`Paramètres invalides: ${validationResult.errors.join(', ')}`);
        }
        
        // 2. Chargement du document source
        const document = await chargerDocument(parametres.document_source);
        if (!document) {
          throw new Error(`Document source introuvable: ${parametres.document_source}`);
        }
        
        // 3. Filtrage des sections selon paramètres
        const sectionsAffichees = document.sections.filter(section => 
          parametres.sections_a_afficher.includes(section.numero)
        );
        
        // 4. Génération de l'interface dynamique
        const interface = {
          titre: parametres.titre_personnalise || "Checklist",
          couleur_theme: parametres.couleur_theme || "#1976D2",
          icone: parametres.icone_section || "📋",
          sections: sectionsAffichees.map(section => ({
            numero: section.numero,
            nom: section.nom,
            elements: section.elements.map(element => ({
              id: element.id,
              texte: element.texte,
              obligatoire: element.obligatoire,
              type: element.type,
              actions: parametres.actions_par_element || [],
              format: parametres.format_affichage
            }))
          })),
          actions_globales: this.genererActionsGlobales(parametres),
          metadata: {
            template_id: this.id,
            timestamp_generation: Date.now(),
            contexte_utilisateur: contexte.utilisateur.id
          }
        };
        
        return interface;
      }
      
      function genererActionsGlobales(parametres) {
        const actions = [];
        
        // Action automatique de validation globale si checklist complète
        if (parametres.actions_par_element.includes('valider')) {
          actions.push({
            id: 'valider_checklist_complete',
            nom: 'Valider Checklist Complète',
            visible_si: 'tous_elements_valides',
            script: 'validerChecklistComplete'
          });
        }
        
        // Action d'export si demandée
        actions.push({
          id: 'exporter_checklist',
          nom: 'Exporter Checklist',
          script: 'exporterChecklist'
        });
        
        return actions;
      }
    
    # Menu hamburger enrichi automatiquement
    menu_hamburger_enrichi:
      ajout_automatique_vue_template: true
      
      vue_template_original:
        nom: "Vue Template Complète"
        id: "vue_template_complet"
        action: "afficher_template_sans_filtrage"
        description: "Voir le template complet sans paramétrage des sections"
        icone: "📋"
      
      vue_document_source:
        nom: "Document Source"
        id: "vue_document_source"
        action: "afficher_document_brut"
        description: "Voir le document source complet"
        icone: "📄"
  
  # Actions spécifiques au template
  actions_template:
    - id: "valider_element_checklist"
      nom: "Valider Élément"
      description: "Valider un élément spécifique de la checklist"
      disponible_sur: "elements_checklist"
      
      script_javascript: |
        async function validerElement(objet, element_id, contexte) {
          const timestamp = Date.now();
          
          // Mise à jour de l'objet
          if (!objet.proprietesDynamiques.checklist_validations) {
            objet.proprietesDynamiques.checklist_validations = {};
          }
          
          objet.proprietesDynamiques.checklist_validations[element_id] = {
            valide: true,
            timestamp: timestamp,
            utilisateur: contexte.utilisateur.id,
            commentaire: contexte.parametres.commentaire || null
          };
          
          // Vérifier si checklist complète
          const checklistComplete = this.verifierChecklistComplete(objet);
          
          return {
            nouvel_etat: checklistComplete ? "Checklist_Complete" : contexte.etat_actuel,
            donnees_maj: objet.proprietesDynamiques,
            message: `Élément ${element_id} validé avec succès`
          };
        }
      
      logging:
        historique_interne: true
        log_externe: true
        fichier_log: "validations_checklist.log"
        niveau_detail: "standard"
    
    - id: "commenter_element_checklist"
      nom: "Commenter Élément"
      description: "Ajouter un commentaire à un élément"
      disponible_sur: "elements_checklist"
      
      interface_saisie:
        formulaire:
          - nom: "commentaire"
            type: "textarea"
            obligatoire: true
            max_chars: 500
            placeholder: "Votre commentaire sur cet élément..."
      
      script_javascript: |
        async function commenterElement(objet, element_id, parametres, contexte) {
          const timestamp = Date.now();
          
          if (!objet.proprietesDynamiques.checklist_commentaires) {
            objet.proprietesDynamiques.checklist_commentaires = {};
          }
          
          if (!objet.proprietesDynamiques.checklist_commentaires[element_id]) {
            objet.proprietesDynamiques.checklist_commentaires[element_id] = [];
          }
          
          objet.proprietesDynamiques.checklist_commentaires[element_id].push({
            commentaire: parametres.commentaire,
            timestamp: timestamp,
            utilisateur: contexte.utilisateur.id
          });
          
          return {
            nouvel_etat: contexte.etat_actuel, // Boucle sur même état
            donnees_maj: objet.proprietesDynamiques,
            message: "Commentaire ajouté avec succès"
          };
        }
  
  # Exemples d'utilisation concrets
  exemples_usage:
    - nom: "État Préparation Production"
      description: "Affichage des contrôles préparatoires"
      parametres:
        document_source: "checklist_qualite_v2.yaml"
        sections_a_afficher: [1, 2]
        titre_personnalise: "Contrôles Préparatoires Production"
        format_affichage: "liste_cochable"
        actions_par_element: ["valider", "commenter"]
        couleur_theme: "#2E7D32"
        icone_section: "🔧"
      resultat_attendu: "Interface avec sections 1 et 2, format liste cochable verte"
      
    - nom: "État Contrôle Final"
      description: "Affichage des contrôles finaux avec validation"
      parametres:
        document_source: "checklist_qualite_v2.yaml"
        sections_a_afficher: [4, 5, 6]
        titre_personnalise: "Contrôles Finaux Qualité"
        format_affichage: "tableau"
        actions_par_element: ["valider", "rejeter", "reporter"]
        couleur_theme: "#D32F2F"
        icone_section: "✅"
      resultat_attendu: "Interface tableau rouge avec actions validation/rejet"
      
    - nom: "État Maintenance Sécurité"
      description: "Checklist sécurité en format cards"
      parametres:
        document_source: "checklist_securite_v1.yaml"
        sections_a_afficher: [1, 3, 7]
        titre_personnalise: "Contrôles Sécurité Obligatoires"
        format_affichage: "cards"
        actions_par_element: ["valider"]
        couleur_theme: "#FF9800"
        icone_section: "⚠️"
      resultat_attendu: "Cards orange sécurité avec validation obligatoire"
  
  # Métadonnées de maintenance
  maintenance:
    versions:
      - version: "1.0"
        date: "2025-08-17"
        changements: ["Création initiale du template"]
        compatible_avec: ["ProcessMetaLanguage v2.1"]
    
    dependances:
      - "function: charger_document"
      - "function: validateParameters"
      - "function: genererActionsGlobales"
    
    tests_validation:
      - nom: "Validation paramètres obligatoires"
        description: "Vérifier que les paramètres obligatoires sont présents"
        
      - nom: "Génération interface valide"
        description: "Vérifier que l'interface générée est cohérente"
        
      - nom: "Actions template fonctionnelles"
        description: "Vérifier que les actions du template s'exécutent correctement"
```

### 2.2 Template Dashboard Métriques
```yaml
template_dashboard_metriques:
  id: "dashboard_metriques"
  nom: "Dashboard Métriques Configurable"
  description: "Template pour afficher des métriques avec visualisations paramètrables"
  version: "1.0"
  
  schema_parametres:
    - nom: "metriques_a_afficher"
      type: "array"
      element_type: "string"
      description: "Liste des métriques à inclure dans le dashboard"
      obligatoire: true
      exemple: ["temperature", "pression", "vitesse_production"]
      
    - nom: "format_visualisation" 
      type: "enum"
      valeurs: ["gauges", "graphiques_ligne", "graphiques_barre", "tableaux", "mixte"]
      defaut: "gauges"
      description: "Format de visualisation des métriques"
      
    - nom: "seuils_alerte"
      type: "object"
      description: "Seuils d'alerte pour chaque métrique"
      exemple: {"temperature": {"min": 10, "max": 80}, "pression": {"max": 150}}
      
    - nom: "periode_affichage"
      type: "enum"
      valeurs: ["temps_reel", "1h", "24h", "7j", "30j"]
      defaut: "temps_reel"
      
    - nom: "mise_a_jour_frequence"
      type: "integer"
      defaut: 5000
      unite: "millisecondes"
      description: "Fréquence de mise à jour des données"
      
    - nom: "disposition_layout"
      type: "enum"
      valeurs: ["grille_2x2", "grille_3x3", "ligne_unique", "colonne_unique", "dashboard"]
      defaut: "grille_2x2"
  
  comportement_template:
    interface_specifique:
      nom: "Interface_Dashboard_Metriques"
      
      sections_dynamiques:
        - nom: "Dashboard Temps Réel"
          contenu: "generer_dashboard(parametres.metriques_a_afficher)"
          format: "parametres.format_visualisation"
          layout: "parametres.disposition_layout"
          
        - nom: "Alertes Actives"
          contenu: "detecter_alertes(parametres.seuils_alerte)"
          format: "bandeau_alerte"
          visible_si: "alertes_detectees"
    
    script_generation: |
      async function genererInterface(objet, parametres, contexte) {
        validateParameters(parametres, this.schema_parametres);
        
        const metriques = await chargerMetriques(objet, parametres.metriques_a_afficher);
        const alertes = detecterAlertes(metriques, parametres.seuils_alerte);
        
        return {
          dashboard: {
            metriques: metriques,
            format: parametres.format_visualisation,
            layout: parametres.disposition_layout,
            frequence_maj: parametres.mise_a_jour_frequence
          },
          alertes: alertes,
          timestamp: Date.now()
        };
      }
```

### 2.3 Template Formulaire Dynamique
```yaml
template_formulaire_dynamique:
  id: "formulaire_dynamique"
  nom: "Formulaire Dynamique Paramétrable"
  description: "Template pour créer des formulaires avec champs configurables"
  version: "1.0"
  
  schema_parametres:
    - nom: "champs_configuration"
      type: "array"
      description: "Configuration des champs du formulaire"
      obligatoire: true
      exemple: [
        {"nom": "temperature", "type": "number", "min": 0, "max": 100, "obligatoire": true},
        {"nom": "commentaire", "type": "textarea", "obligatoire": false, "max_chars": 500}
      ]
      
    - nom: "validation_regles"
      type: "object"
      description: "Règles de validation supplémentaires"
      exemple: {"temperature": "required|numeric|min:0|max:100"}
      
    - nom: "action_soumission"
      type: "string"
      description: "Action à exécuter lors de la soumission"
      defaut: "valider_formulaire"
      
    - nom: "titre_formulaire"
      type: "string"
      description: "Titre du formulaire"
      defaut: "Saisie de Données"
      
    - nom: "layout_formulaire"
      type: "enum"
      valeurs: ["colonne_simple", "deux_colonnes", "accordeon", "onglets"]
      defaut: "colonne_simple"
  
  comportement_template:
    interface_specifique:
      nom: "Interface_Formulaire_Dynamique"
      
      sections_dynamiques:
        - nom: "Formulaire Principal"
          contenu: "generer_formulaire(parametres.champs_configuration)"
          format: "parametres.layout_formulaire"
          validation: "parametres.validation_regles"
```

---

## 3. COMPORTEMENT DE BASE (NIVEAU PROCESS) - Inchangé v2.0

### 3.1 Structure - Propriété du PROCESS
```yaml
process:
  id: "process_production_industrielle"
  nom: "Production Industrielle"
  description: "Processus de production avec contrôles qualité"
  
  # COMPORTEMENT DE BASE - Commun à TOUS les états de ce process
  comportement_base:
    action_automatique_consultation:
      actif: true
      description: "Action déclenchée automatiquement avant affichage de l'état"
      execution: "pre_affichage"
      
      script_javascript: |
        async function onConsultationObjet(objet, contexte) {
          const currentTimestamp = Date.now();
          
          // Logique commune à tous les états du process
          const consultationEvent = {
            timestamp: currentTimestamp,
            utilisateur_id: contexte.utilisateur.id,
            process_id: this.process.id,
            etat_id: this.etat.id,
            objet_id: objet.id
          };
          
          // Mise à jour métriques communes au process
          await this.process.updateMetriques(consultationEvent);
          
          // Détermination comportement selon état
          const comportement = await this.determinerComportement(objet, contexte);
          
          return { 
            comportement_a_afficher: comportement,
            donnees_supplementaires: consultationEvent
          };
        }
      
      logging:
        historique_interne: true
        log_externe: true
        fichier_log: "consultations_process.log"
        niveau_detail: "standard"

    interface_standard:
      type: "vue_objet_complete"
      affichage: "obligatoire"
      access_method: "direct"
      
      sections:
        - nom: "Identité Objet"
          contenu: ["id_objet", "nom_objet", "identifiants_gs1"]
          
        - nom: "État Actuel"
          contenu: ["nom_etat", "date_entree_etat", "duree_dans_etat"]
          
        - nom: "Processus"
          contenu: ["nom_process", "etape_actuelle", "progression_globale"]
          
        - nom: "Propriétés Statiques"
          contenu: "toutes_proprietes_statiques"
          format: "tableau_cle_valeur"
          
        - nom: "Propriétés Dynamiques"
          contenu: "toutes_proprietes_dynamiques"
          format: "tableau_cle_valeur_horodatage"
          mise_a_jour: "temps_reel"
          
        - nom: "Actions Disponibles"
          contenu: "actions_process + actions_etat + actions_template"
          format: "boutons_action"

    menu_hamburger:
      access_method: "hamburger_menu"
      parametrage_visibilite: true
      
      options_disponibles:
        - nom: "Historique Complet"
          id: "historique_complet"
          action: "afficher_historique_objet"
          visible_par_defaut: true
          parametrable: true
          
        - nom: "Vue Processus"
          id: "vue_processus"
          action: "afficher_processus_complet"
          visible_par_defaut: true
          parametrable: true
          description: "Vue complète du processus avec tous les états"
          
        - nom: "Données Techniques"
          id: "donnees_techniques"
          action: "afficher_donnees_brutes"
          visible_par_defaut: false
          parametrable: true
          
        - nom: "Audit Trail"
          id: "audit_trail"
          action: "afficher_audit_trail"
          visible_par_defaut: true
          parametrable: true
          
        - nom: "Export Données"
          id: "export_donnees"
          action: "exporter_donnees_objet"
          formats: ["json", "csv", "pdf"]
          visible_par_defaut: true
          parametrable: true

    regles_contexte:
      - condition: "utilisateur.roles.contains('operateur')"
        modifications:
          - masquer: ["donnees_techniques"]
          - mettre_en_avant: ["actions.controler_qualite"]
          
      - condition: "process.mode_maintenance == true"
        modifications:
          - afficher_alerte: "Processus en mode maintenance"
          - options_supplementaires: ["logs_systeme", "debug_process"]
```

---

## 4. COMPORTEMENT PARTICULIER v2.1 (NIVEAU ÉTAT)

### 4.1 Mode Template - Nouveau v2.1
```yaml
etat:
  id: "etat_preparation_production"
  nom: "Préparation_Production"
  process_id: "process_production_industrielle"
  
  # COMPORTEMENT PARTICULIER v2.1 - Mode Template
  comportement_particulier:
    actif: true
    mode: "utilise_template" # 🆕 NOUVEAU MODE
    
    # Référence au template + paramètres
    template_utilise:
      template_id: "affichage_checklist"
      template_version: "1.0"
      
      # Paramètres spécifiques à cet état
      parametres:
        document_source: "checklist_qualite_v2.yaml"
        sections_a_afficher: [1, 2] # Sections préparatoires
        titre_personnalise: "Contrôles Préparatoires Production"
        format_affichage: "liste_cochable"
        actions_par_element: ["valider", "commenter"]
        couleur_theme: "#2E7D32" # Vert production
        icone_section: "🔧"
      
      # Surcharges optionnelles (priorité sur template)
      surcharges:
        sections:
          - nom: "Actions Spéciales"
            contenu: "actions_etat_specifiques"
            position: "after_checklist"
        
        menu_hamburger:
          options_supplementaires:
            - nom: "Guide Préparation"
              action: "afficher_guide_preparation"
              icone: "📖"
        
        # Règles contextuelles spécifiques à cet état
        regles_contexte_etat:
          - condition: "shift == 'nuit'"
            modifications:
              - afficher_alerte: "Contrôles renforcés équipe de nuit"
              - ajouter_sections: ["controles_supplementaires_nuit"]
    
    # Actions supplémentaires spécifiques à l'état
    actions_supplementaires:
      - id: "demarrer_production"
        nom: "Démarrer Production"
        description: "Démarrer la production après validation checklist"
        visible_si: "checklist_complete == true"
        
        script_javascript: |
          async function demarrerProduction(objet, contexte) {
            // Vérifier que la checklist est complète
            const checklistComplete = verifierChecklistComplete(objet);
            if (!checklistComplete) {
              throw new Error("Checklist de préparation incomplète");
            }
            
            // Démarrer la production
            objet.proprietesDynamiques.production_demarree = true;
            objet.proprietesDynamiques.timestamp_demarrage = Date.now();
            
            return {
              nouvel_etat: "En_Production",
              message: "Production démarrée avec succès"
            };
          }
        
        transition: "En_Production"
        
        logging:
          historique_interne: true
          log_externe: true
          fichier_log: "demarrage_production.log"
          niveau_detail: "detaille"
```

### 4.2 Mode Manuel - Classique v2.0
```yaml
etat:
  id: "etat_controle_manuel"
  nom: "Contrôle_Manuel"
  process_id: "process_production_industrielle"
  
  # COMPORTEMENT PARTICULIER v2.0 - Mode Manuel Classique
  comportement_particulier:
    actif: true
    mode: "manuel" # Mode classique v2.0
    remplace_comportement_base: true
    
    interface_specifique:
      nom: "Interface_Controle_Manuel"
      description: "Interface spécialisée pour contrôles manuels"
      
      sections_personnalisees:
        - nom: "Zone de Contrôle"
          proprietes_selectionnees:
            - "piece_en_cours"
            - "outils_controle"
            - "tolerances_acceptees"
          format: "panneau_controle"
          
        - nom: "Résultats Mesures"
          proprietes_selectionnees:
            - "mesures_effectuees"
            - "ecarts_detectes"
          format: "tableau_resultats"
          
        - nom: "Actions Contrôle"
          actions_mises_en_avant:
            - "effectuer_mesure"
            - "valider_piece"
            - "rejeter_piece"
          format: "boutons_principaux"
    
    # Menu hamburger enrichi automatiquement
    menu_hamburger_enrichi:
      ajout_automatique_vue_standard: true
      
      vue_standard:
        nom: "Vue Standard Process"
        id: "vue_comportement_base_process"
        action: "afficher_comportement_base_process"
        position: "premier"
        description: "Retour à la vue standard du processus"
```

### 4.3 Mode Mixte - Hybride v2.1
```yaml
etat:
  id: "etat_controle_mixte"
  nom: "Contrôle_Mixte"
  process_id: "process_production_industrielle"
  
  # COMPORTEMENT PARTICULIER v2.1 - Mode Mixte
  comportement_particulier:
    actif: true
    mode: "mixte" # 🆕 Mode hybride template + manuel
    
    # Base template pour structure commune
    base_template:
      template_id: "dashboard_metriques"
      parametres:
        metriques_a_afficher: ["temperature", "pression"]
        format_visualisation: "gauges"
        seuils_alerte: {"temperature": {"max": 80}, "pression": {"max": 150}}
    
    # Ajouts manuels spécifiques
    ajouts_manuels:
      sections_supplementaires:
        - nom: "Contrôles Visuels"
          contenu: "controles_visuels_manuels"
          format: "checklist_simple"
          position: "after_dashboard"
          
        - nom: "Commentaires Qualité"
          contenu: "champ_commentaires_libres"
          format: "textarea_large"
          position: "bottom"
      
      actions_supplementaires:
        - id: "rapport_controle_complet"
          nom: "Générer Rapport Complet"
          description: "Génère un rapport incluant métriques et contrôles visuels"
```

---

## 5. ACTIONS (RÉPARTITION PROCESS/TEMPLATE/ÉTAT)

### 5.1 Actions au Niveau PROCESS (Inchangé v2.0)
```yaml
process:
  actions_communes:
    # Actions disponibles dans TOUS les états du process
    - id: "consulter_processus"
      nom: "Consulter Processus"
      description: "Action de consultation standard pour tous les états"
      disponible_dans: "tous_etats"
      
      script_javascript: |
        async function consulterProcessus(objet, contexte) {
          // Logique commune à tous les états
          return {
            nouvel_etat: contexte.etat_actuel, // Pas de changement d'état
            donnees_exposees: this.process.getDonnees(objet)
          };
        }
      
      logging:
        historique_interne: false # Consultation = pas d'historique
        log_externe: true
        fichier_log: "consultations_process.log"
```

### 5.2 🆕 Actions au Niveau TEMPLATE
```yaml
# Actions générées automatiquement par les templates
actions_templates:
  # Du template "affichage_checklist"
  checklist:
    - id: "valider_element_checklist"
      nom: "Valider Élément"
      source: "template_affichage_checklist"
      parametre: true # Action paramétrée par l'état
      
    - id: "commenter_element_checklist"
      nom: "Commenter Élément"
      source: "template_affichage_checklist"
      parametre: true
  
  # Du template "dashboard_metriques"
  dashboard:
    - id: "actualiser_metriques"
      nom: "Actualiser Métriques"
      source: "template_dashboard_metriques"
      
    - id: "configurer_seuils"
      nom: "Configurer Seuils"
      source: "template_dashboard_metriques"
  
  # Du template "formulaire_dynamique"
  formulaire:
    - id: "valider_formulaire"
      nom: "Valider Formulaire"
      source: "template_formulaire_dynamique"
      
    - id: "sauvegarder_brouillon"
      nom: "Sauvegarder Brouillon"
      source: "template_formulaire_dynamique"
```

### 5.3 Actions au Niveau ÉTAT (Enrichi v2.1)
```yaml
etat:
  actions_specifiques:
    # Actions spécifiques à cet état uniquement
    - id: "demarrer_production_preparee"
      nom: "Démarrer Production"
      description: "Démarrage spécifique après préparation complète"
      disponible_dans: "etat_preparation_production_uniquement"
      
      # 🆕 Interaction avec template
      prerequis_template:
        template_utilise: "affichage_checklist"
        condition: "checklist_complete == true"
        message_erreur: "Checklist de préparation incomplète"
      
      script_javascript: |
        async function demarrerProductionPreparee(objet, contexte) {
          // Vérification prérequis template
          if (contexte.template_utilise) {
            const checklistComplete = await verifierChecklistTemplate(objet, contexte.template_utilise);
            if (!checklistComplete) {
              throw new Error("Checklist de préparation incomplète");
            }
          }
          
          // Logique spécifique au démarrage
          objet.proprietesDynamiques.production_demarree = true;
          objet.proprietesDynamiques.timestamp_demarrage = Date.now();
          
          // Récupération des données du template pour historique
          const donneesTemplate = await extraireDonneesTemplate(objet, contexte.template_utilise);
          
          return {
            nouvel_etat: "En_Production",
            donnees_template: donneesTemplate,
            message: "Production démarrée avec checklist validée"
          };
        }
      
      transition:
        type: "conditionnel"
        conditions:
          - si: "checklist_complete == true"
            nouvel_etat: "En_Production"
          - defaut: "Preparation_Production" # Boucle si prérequis non remplis
```

---

## 6. RÉSOLUTION ET FUSION DES COMPORTEMENTS v2.1

### 6.1 Algorithme de Résolution Enrichi
```yaml
algorithme_resolution_v21:
  etape_1_chargement_process:
    source: "PROCESS.comportement_base"
    elements_charges:
      - interface_standard
      - menu_hamburger
      - actions_communes
      - regles_contexte
      - logging_configuration
  
  etape_2_verification_etat:
    condition_1: "ETAT.comportement_particulier.mode == 'utilise_template'"
    si_template:
      actions:
        - charger: "TEMPLATE depuis bibliothèque selon template_id"
        - valider: "parametres ETAT vs schema_parametres TEMPLATE"
        - generer: "interface dynamique avec parametres ETAT"
        - appliquer: "surcharges ETAT sur résultat template"
        - enrichir: "menu_hamburger avec vue_template_original"
    
    condition_2: "ETAT.comportement_particulier.mode == 'manuel'"
    si_manuel:
      actions:
        - charger: "ETAT.comportement_particulier.interface_specifique"
        - enrichir: "menu_hamburger avec vue_standard"
    
    condition_3: "ETAT.comportement_particulier.mode == 'mixte'"
    si_mixte:
      actions:
        - charger: "TEMPLATE base selon base_template"
        - generer: "interface template avec parametres"
        - fusionner: "avec ajouts_manuels ETAT"
        - enrichir: "menu_hamburger avec vue_standard + vue_template"
    
    condition_4: "ETAT.comportement_particulier.actif == false"
    si_inactif:
      actions:
        - utiliser: "comportement_base_process uniquement"
  
  etape_3_fusion_actions:
    sources_actions:
      1. "PROCESS.actions_communes"
      2. "TEMPLATE.actions_template (si template utilisé)"
      3. "ETAT.actions_specifiques"
      4. "ETAT.actions_supplementaires (si template utilisé)"
    
    regles_fusion:
      - "Toutes les actions sont disponibles simultanément"
      - "Actions ETAT peuvent avoir prérequis sur actions TEMPLATE"
      - "Actions TEMPLATE sont paramétrées selon ETAT"
      - "Contexte s'applique à toutes les actions"
  
  etape_4_enrichissement_menu:
    menu_hamburger_final:
      options_automatiques:
        - si_template: "Vue Template Original"
        - si_particulier_ou_template: "Vue Standard Process"
        - toujours: "options héritées du PROCESS"
      
      options_surcharges:
        - source: "ETAT.surcharges.menu_hamburger"
        - source: "TEMPLATE.menu_hamburger_enrichi"
        - source: "ETAT.ajouts_manuels.menu_hamburger"
  
  etape_5_application_contexte:
    sources_regles:
      1. "PROCESS.regles_contexte"
      2. "TEMPLATE.regles_contexte (si template utilisé)"
      3. "ETAT.regles_contexte"
      4. "ETAT.surcharges.regles_contexte_etat"
    
    ordre_application: "PROCESS → TEMPLATE → ETAT → CONTEXTE_UTILISATEUR"
    priorite_finale: "CONTEXTE_UTILISATEUR > ETAT > TEMPLATE > PROCESS"
```

### 6.2 Exemple de Fusion Concrète v2.1
```yaml
exemple_fusion_complete_v21:
  # État "Préparation Production" utilisant template "affichage_checklist"
  configuration_initiale:
    process: "process_production_industrielle"
    etat: "etat_preparation_production"
    template: "affichage_checklist"
    contexte: "operateur_shift_matin"
  
  configuration_finale:
    source_interface: "TEMPLATE affichage_checklist + PARAMÈTRES état + SURCHARGES"
    source_menu: "PROCESS.menu_hamburger + vue standard + vue template original"
    source_actions: "PROCESS.actions_communes + TEMPLATE.actions + ÉTAT.actions"
    
    interface_affichee:
      type: "Interface_Checklist_Parametree"
      titre: "Contrôles Préparatoires Production"
      couleur_theme: "#2E7D32" # Vert de la surcharge état
      
      sections_visibles:
        # Du template paramétré
        - nom: "Section 1: Vérification Matière Première"
          elements: ["Certificat MP", "Dimensions entrantes"]
          format: "liste_cochable"
          actions: ["valider", "commenter"] # Paramètres état
          
        - nom: "Section 2: Contrôle Machine"
          elements: ["Température fonctionnement"]
          format: "liste_cochable"
          actions: ["valider", "commenter"]
        
        # De la surcharge état
        - nom: "Actions Spéciales"
          contenu: "actions_etat_specifiques"
          elements: ["Démarrer Production"]
          visible_si: "checklist_complete"
      
      metadata:
        template_id: "affichage_checklist"
        etat_id: "etat_preparation_production"
        timestamp_generation: "2025-08-17T10:30:00Z"
        
    menu_hamburger_final:
      options:
        # Ajoutées automatiquement
        - nom: "Vue Standard Process"
          action: "afficher_comportement_base_process"
          position: "premier"
          icone: "🔧"
          description: "Vue standard du processus"
          
        - nom: "Vue Template Original"
          action: "afficher_template_complet"
          position: "second"
          icone: "📋"
          description: "Checklist complète sans filtrage"
          
        # Héritées du process
        - nom: "Historique Complet"
          action: "afficher_historique_objet"
          icone: "📚"
          visible: true # Paramétrage process
          
        - nom: "Vue Processus"
          action: "afficher_processus_complet"
          icone: "🔄"
          visible: true
          mise_en_avant: true # Surcharge état
          
        - nom: "Export Données"
          action: "exporter_donnees_objet"
          icone: "📤"
          formats: ["json", "csv", "pdf"]
          
        # De la surcharge état
        - nom: "Guide Préparation"
          action: "afficher_guide_preparation"
          icone: "📖"
          description: "Guide détaillé de préparation"
        
    actions_disponibles:
      du_process:
        - id: "consulter_processus"
          nom: "Consulter Processus"
          origine: "process_production_industrielle"
          
      du_template:
        - id: "valider_element_checklist"
          nom: "Valider Élément"
          origine: "template_affichage_checklist"
          parametres: ["element_id"]
          
        - id: "commenter_element_checklist"
          nom: "Commenter Élément"
          origine: "template_affichage_checklist"
          parametres: ["element_id", "commentaire"]
          
      de_letat:
        - id: "demarrer_production"
          nom: "Démarrer Production"
          origine: "etat_preparation_production"
          prerequis: "checklist_complete"
          visible_si: "checklist_complete == true"
    
    regles_contexte_appliquees:
      # Du process
      - condition: "utilisateur.role == 'operateur'"
        masquer: ["donnees_techniques"]
        origine: "process"
        
      # Du template (aucune dans cet exemple)
      
      # De l'état
      - condition: "shift == 'nuit'"
        afficher_alerte: "Contrôles renforcés équipe de nuit"
        origine: "etat"
        
      # Du contexte final
      - condition: "contexte.temporalite.shift == 'matin'"
        mise_en_avant: ["actions.demarrer_production"]
        origine: "contexte_final"

---

## 7. GESTION AVANCÉE DU LOGGING ET CYCLES DE CONSULTATION v2.1

### 7.1 Types de Consultations Détectées (Enrichi Templates)
```yaml
types_consultations_v21:
  premiere:
    description: "Première consultation de l'objet"
    metriques: ["timestamp", "utilisateur", "device"]
    template_info: "template_utilise si applicable"
    
  consultation_apres_action:
    description: "Consultation après qu'une action ait été exécutée"
    metriques: ["timestamp", "utilisateur", "action_precedente", "duree_depuis_action"]
    action_origine: "process|template|etat" # 🆕 Origine de l'action
    
  re_consultation_sans_action:
    description: "Nouvelle consultation sans action intermédiaire"
    metriques: ["timestamp", "utilisateur", "nombre_consultations_consecutives", "duree_session"]
    template_progression: "progression_template si applicable" # 🆕
    alerte_possible: true
    
  consultation_changement_template:
    description: "🆕 Consultation après changement de paramètres template"
    metriques: ["timestamp", "utilisateur", "ancien_template", "nouveau_template", "parametres_changes"]
    type: "nouveau_v21"
    
  consultation_multi_utilisateur:
    description: "Consultation par utilisateur différent du précédent"
    metriques: ["timestamp", "utilisateur_actuel", "utilisateur_precedent", "changement_contexte"]
    template_heritage: "heritage_contexte_template" # 🆕
```

### 7.2 Métriques de Comportement Utilisateur Enrichies
```yaml
metriques_comportement_v21:
  # Tracking des patterns de consultation enrichi templates
  patterns_consultation:
    consultations_totales: "compteur_global"
    consultations_sans_action: "compteur_consultations_vides"
    ratio_action_consultation: "pourcentage_efficacite"
    duree_moyenne_session: "temps_moyen_avant_action_ou_fermeture"
    
    # 🆕 Métriques templates spécifiques
    templates_utilises: "liste_templates_consultes"
    progression_templates: "avancement_par_template"
    efficacite_par_template: "ratio_completion_par_template"
    parametres_modifies: "historique_changements_parametres"
    
  # Seuils d'alerte enrichis
  seuils_alerte_v21:
    consultations_vides_consecutives: 5
    duree_session_anormale: 3600000 # 1 heure
    ratio_action_faible: 0.2 # Moins de 20% des consultations aboutissent à une action
    
    # 🆕 Seuils templates
    template_incomplet_seuil: 0.5 # Template complété à moins de 50%
    changements_parametres_excessifs: 3 # Plus de 3 changements de paramètres
    templates_abandonnes_seuil: 2 # Plus de 2 templates commencés et abandonnés
    
  # Actions automatiques selon métriques
  actions_automatiques_v21:
    - condition: "consultations_vides_consecutives >= 5"
      action: "proposer_aide_contextuelle"
      
    - condition: "template_incomplet_seuil < 0.5"
      action: "proposer_assistance_template"
      message: "Besoin d'aide pour compléter ce template ?"
      
    - condition: "changements_parametres_excessifs > 3"
      action: "suggerer_formation_template"
      message: "Formation sur l'utilisation des templates disponible"
      
    - condition: "templates_abandonnes_seuil > 2"
      action: "analyser_ux_templates"
      message: "Analyse UX des templates nécessaire"
```

### 7.3 Gestion des Sessions et États Enrichie Templates
```yaml
gestion_sessions_v21:
  # Propriétés dynamiques ajoutées à l'OBJET (enrichies templates)
  proprietes_dynamiques_consultation:
    derniere_consultation:
      timestamp: "2025-08-17T10:30:00Z"
      utilisateur_id: "user_123"
      etat: "En_Production"
      template_utilise: "affichage_checklist" # 🆕
      parametres_template: {...} # 🆕
      
    derniere_action:
      timestamp: "2025-08-17T10:25:00Z"
      action_id: "valider_element_checklist"
      origine_action: "template" # 🆕 process|template|etat
      utilisateur_id: "user_123"
      resultat: "Element_Valide"
      donnees_template: {...} # 🆕
      
    session_actuelle:
      debut_session: "2025-08-17T10:30:00Z"
      actions_executees: []
      consultations_dans_session: 3
      utilisateur_session: "user_123"
      
      # 🆕 Données templates session
      templates_utilises_session: ["affichage_checklist"]
      progression_templates: {"affichage_checklist": 0.6}
      parametres_modifies_session: []
      
    metriques_globales:
      compteur_consultations: 25
      compteur_actions: 18
      ratio_efficacite: 0.72
      derniere_consultation_sans_action: "2025-08-17T09:45:00Z"
      
      # 🆕 Métriques templates globales
      templates_completes: ["dashboard_metriques", "formulaire_dynamique"]
      templates_en_cours: ["affichage_checklist"]
      efficacite_templates: {"affichage_checklist": 0.8, "dashboard_metriques": 0.9}
```

### 7.4 Logs Enrichis pour Analyse Comportementale Templates
```json
// Exemple de log consultation avec template
{
  "timestamp": "2025-08-17T10:30:00Z",
  "type_event": "consultation_template",
  "objet_id": "obj_comp_001",
  "utilisateur": "operateur_123",
  "etat": "Preparation_Production",
  "type_consultation": "premiere_consultation_template",
  "template_data": {
    "template_id": "affichage_checklist",
    "template_version": "1.0",
    "parametres": {
      "sections_a_afficher": [1, 2],
      "format_affichage": "liste_cochable"
    },
    "elements_total": 5,
    "elements_completes": 0,
    "progression": 0.0
  },
  "session_data": {
    "debut_session": "2025-08-17T10:30:00Z",
    "consultations_consecutives": 1,
    "actions_executees_session": 0,
    "templates_utilises_session": ["affichage_checklist"]
  },
  "contexte": {
    "device": "webapp",
    "shift": "matin",
    "formation_templates": true
  }
}

// Exemple de log action template
{
  "timestamp": "2025-08-17T10:35:00Z",
  "type_event": "action_template",
  "action": "valider_element_checklist",
  "objet_id": "obj_comp_001",
  "utilisateur": "operateur_123",
  "template_data": {
    "template_id": "affichage_checklist",
    "element_id": "mp_001",
    "element_nom": "Vérifier certificat matière première",
    "progression_avant": 0.0,
    "progression_apres": 0.2,
    "elements_restants": 4
  },
  "resultat": {
    "valide": true,
    "commentaire": "Certificat conforme",
    "utilisateur_validation": "operateur_123",
    "timestamp_validation": "2025-08-17T10:35:00Z"
  },
  "impact": {
    "nouvel_etat": "Preparation_Production", // Boucle
    "actions_deverrouillees": [],
    "progression_globale": 0.2
  }
}

// Exemple de log template complété
{
  "timestamp": "2025-08-17T10:45:00Z",
  "type_event": "template_complete",
  "objet_id": "obj_comp_001",
  "utilisateur": "operateur_123",
  "template_data": {
    "template_id": "affichage_checklist",
    "duree_completion": 900000, // 15 minutes
    "elements_total": 5,
    "elements_valides": 5,
    "efficacite": 1.0,
    "commentaires_ajoutes": 2
  },
  "actions_deverrouillees": [
    "demarrer_production"
  ],
  "transition_possible": "En_Production",
  "session_summary": {
    "debut_session": "2025-08-17T10:30:00Z",
    "duree_totale": 900000,
    "consultations_effectuees": 6,
    "actions_executees": 5,
    "efficacite_session": 0.83
  }
}
```

---

## 8. LOGGING CONFIGURATION v2.1 (Enrichi Templates)

### 8.1 Configuration Logging Templates
```yaml
logging_configuration_v21:
  # Historique interne (dans l'objet) - enrichi templates
  historique_interne:
    description: "Stockage dans objet.historique avec données templates"
    avantages: ["Accessible via API", "Intégré données objet", "Requêtable", "Historique templates"]
    format: "structured_json_with_templates"
    retention: "illimitee"
    
    # 🆕 Données templates dans historique
    donnees_templates:
      templates_utilises: "liste chronologique des templates"
      parametres_historique: "évolution des paramètres par template"
      progressions: "historique des progressions templates"
      completions: "timestamps des complétions templates"
    
  # Log externe (fichiers) - enrichi templates
  log_externe:
    description: "Écriture dans fichiers logs externes avec séparation templates"
    fichiers_specialises:
      - "consultations_templates.log" # 🆕
      - "actions_templates.log" # 🆕
      - "completions_templates.log" # 🆕
      - "erreurs_templates.log" # 🆕
      - "consultations_process.log" # Existant
      - "actions_process.log" # Existant
    
    formats_supportes: ["json", "csv", "plain_text"]
    rotation: "quotidienne"
    compression: true
    retention: "365_jours"
```

### 8.2 Niveaux de Détail Enrichis Templates
```yaml
niveaux_detail_v21:
  minimal:
    donnees: ["timestamp", "action", "utilisateur_id", "objet_id"]
    templates: ["template_id", "progression"] # 🆕
    cas_usage: "Audit basique, performance"
    
  standard:
    donnees: ["timestamp", "action", "utilisateur_id", "objet_id", "resultats", "duree"]
    templates: ["template_id", "parametres", "progression", "elements_modifies"] # 🆕
    cas_usage: "Monitoring opérationnel avec templates"
    
  detaille:
    donnees: ["timestamp", "action", "utilisateur_id", "objet_id", "parametres_entree", "etapes_intermediaires", "resultats", "erreurs", "contexte_complet"]
    templates: ["template_complet", "schema_validation", "parametres_historique", "erreurs_template"] # 🆕
    cas_usage: "Debug templates, analyse poussée, conformité"
```

### 8.3 Actions Systémiques de Logging Enrichies
```yaml
actions_systemiques_v21:
  consultation_objet:
    logging:
      historique_interne: true
      log_externe: false
      niveau_detail: "minimal"
      fichier_specialise: "consultations_templates.log" # 🆕 si template utilisé
      
  consultation_template: # 🆕 NOUVEAU
    logging:
      historique_interne: true
      log_externe: true
      niveau_detail: "standard"
      fichier_specialise: "consultations_templates.log"
      
  action_template: # 🆕 NOUVEAU
    logging:
      historique_interne: true
      log_externe: true
      niveau_detail: "standard"
      fichier_specialise: "actions_templates.log"
      
  completion_template: # 🆕 NOUVEAU
    logging:
      historique_interne: true
      log_externe: true
      niveau_detail: "detaille"
      fichier_specialise: "completions_templates.log"
      alerte_temps_reel: true # Notification completion
      
  changement_etat:
    logging:
      historique_interne: true
      log_externe: true
      niveau_detail: "standard"
      donnees_templates: true # 🆕 Inclure données templates
      
  action_utilisateur:
    logging:
      historique_interne: true
      log_externe: true
      niveau_detail: "standard"
      origine_tracking: true # 🆕 process|template|etat
      
  erreur_systeme:
    logging:
      historique_interne: true
      log_externe: true
      niveau_detail: "detaille"
      erreurs_templates: true # 🆕 Erreurs spécifiques templates
      alerte_temps_reel: true
```

---

## 9. EXEMPLE COMPLET v2.1 - ÉTAT AVEC TEMPLATE CHECKLIST

### 9.1 Document Source Checklist
```yaml
# checklist_qualite_v2.yaml
checklist:
  nom: "Checklist Qualité Production v2.0"
  version: "2.0"
  auteur: "équipe_qualité"
  derniere_revision: "2025-08-17"
  
  sections:
    - numero: 1
      nom: "Vérification Matière Première"
      description: "Contrôles d'entrée matière première"
      obligatoire: true
      elements:
        - id: "mp_001"
          texte: "Vérifier certificat matière première"
          description: "Contrôler la conformité du certificat fournisseur"
          obligatoire: true
          type: "validation"
          criteres: ["Date validité", "Signature", "Composition"]
          
        - id: "mp_002" 
          texte: "Contrôler dimensions pièces entrantes"
          description: "Mesurer échantillon pièces selon plan"
          obligatoire: true
          type: "mesure"
          unite: "mm"
          tolerance: "±0.1"
          echantillon: 5
          
        - id: "mp_003"
          texte: "Vérifier état surface pièces"
          description: "Contrôle visuel défauts surface"
          obligatoire: true
          type: "visuel"
          defauts_recherches: ["rayures", "impacts", "corrosion"]
          
    - numero: 2
      nom: "Contrôle Machine"
      description: "Vérifications machine avant production"
      obligatoire: true
      elements:
        - id: "mc_001"
          texte: "Vérifier température de fonctionnement"
          description: "Contrôler température dans plage nominale"
          obligatoire: true
          type: "mesure"
          unite: "°C"
          plage_nominale: {"min": 20, "max": 80}
          frequence_controle: "début_production"
          
        - id: "mc_002"
          texte: "Contrôler niveau lubrifiant"
          description: "Vérifier niveau et qualité lubrifiant"
          obligatoire: true
          type: "mesure"
          unite: "L"
          niveau_mini: 2.5
          
        - id: "mc_003"
          texte: "Test cycle machine à vide"
          description: "Cycle complet machine sans pièce"
          obligatoire: true
          type: "test"
          duree_cycle: "120s"
          parametres_verification: ["vitesse", "vibrations", "bruit"]
          
    - numero: 3
      nom: "Contrôle Sécurité"
      description: "Vérifications sécurité obligatoires"
      obligatoire: true
      priorite: "critique"
      elements:
        - id: "sec_001"
          texte: "Vérifier arrêts d'urgence"
          description: "Test fonctionnement tous arrêts d'urgence"
          obligatoire: true
          type: "test"
          nombre_arrets: 4
          test_procedure: "appui_relachement"
          
        - id: "sec_002"
          texte: "Contrôler protections mobiles"
          description: "Vérifier verrouillage protections"
          obligatoire: true
          type: "test"
          protections: ["porte_avant", "porte_laterale", "capot_superieur"]
          
    - numero: 4
      nom: "Contrôle Pièce Finie"
      description: "Contrôles qualité pièce produite"
      obligatoire: true
      elements:
        - id: "pf_001"
          texte: "Mesurer dimensions finales"
          description: "Contrôle dimensionnel selon plan"
          obligatoire: true
          type: "mesure"
          points_controle: 8
          tolerance_generale: "±0.05"
          
        - id: "pf_002"
          texte: "Contrôle visuel défauts"
          description: "Inspection visuelle défauts production"
          obligatoire: true
          type: "visuel"
          zones_critiques: ["faces_usinées", "arêtes", "perçages"]
          
        - id: "pf_003"
          texte: "Test fonctionnel assemblage"
          description: "Vérifier assemblage avec pièce mâle"
          obligatoire: false
          type: "test"
          condition: "si_assemblage_requis"
          
    - numero: 5
      nom: "Documentation"
      description: "Finalisation documentation qualité"
      obligatoire: true
      elements:
        - id: "doc_001"
          texte: "Compléter fiche suivi production"
          description: "Saisir données production dans fiche"
          obligatoire: true
          type: "documentation"
          donnees_requises: ["lot_mp", "operateur", "machine", "resultats_controles"]
          
        - id: "doc_002"
          texte: "Archiver certificats contrôles"
          description: "Archiver tous certificats et PV contrôles"
          obligatoire: true
          type: "archivage"
          documents: ["certificat_mp", "pv_controle_machine", "rapport_qualite"]
```

### 9.2 État Préparation avec Template Checklist
```yaml
etat_preparation_production_complet:
  # Identité
  id: "etat_preparation_production_001"
  nom: "Préparation_Production"
  description: "État de préparation avec checklist contrôles préalables"
  process_id: "process_production_industrielle"
  
  # Comportement particulier avec template
  comportement_particulier:
    actif: true
    mode: "utilise_template"
    
    template_utilise:
      template_id: "affichage_checklist"
      template_version: "1.0"
      
      # Paramètres spécifiques préparation
      parametres:
        document_source: "checklist_qualite_v2.yaml"
        sections_a_afficher: [1, 2, 3] # MP + Machine + Sécurité
        titre_personnalise: "Contrôles Préparatoires Production"
        format_affichage: "liste_cochable"
        actions_par_element: ["valider", "commenter", "reporter"]
        couleur_theme: "#2E7D32" # Vert production
        icone_section: "🔧"
      
      # Surcharges état
      surcharges:
        sections_supplementaires:
          - nom: "État Machine"
            contenu: "afficher_statut_machine_temps_reel"
            format: "dashboard_mini"
            position: "before_checklist"
            donnees: ["temperature_actuelle", "niveau_lubrifiant", "cycles_depuis_maintenance"]
          
          - nom: "Actions Production"
            contenu: "actions_demarrage_production"
            format: "boutons_principaux"
            position: "after_checklist"
            visible_si: "checklist_preparation_complete"
        
        menu_hamburger:
          options_supplementaires:
            - nom: "Guide Préparation"
              action: "afficher_guide_preparation_detaille"
              icone: "📖"
              description: "Guide détaillé de préparation production"
              
            - nom: "Historique Machine"
              action: "afficher_historique_machine"
              icone: "🔧"
              description: "Historique maintenances et incidents machine"
        
        regles_contexte_specifiques:
          - condition: "shift == 'nuit'"
            modifications:
              - afficher_alerte: "Contrôles renforcés équipe de nuit"
              - ajouter_sections_checklist: ["controles_nuit_supplementaires"]
              - mettre_en_avant: ["sec_001", "sec_002"] # Sécurité prioritaire
              
          - condition: "machine.heures_fonctionnement > 1000"
            modifications:
              - afficher_alerte: "Machine proche révision - contrôles renforcés"
              - actions_supplementaires: ["programmer_maintenance"]
    
    # Actions spécifiques état préparation
    actions_supplementaires:
      - id: "demarrer_production_prepare"
        nom: "Démarrer Production"
        description: "Démarrer production après validation checklist complète"
        
        # Prérequis template strict
        prerequis_template:
          template_id: "affichage_checklist"
          conditions:
            - "sections_completes >= [1, 2, 3]" # MP + Machine + Sécurité obligatoires
            - "elements_critiques_valides == true" # Tous éléments obligatoires
            - "erreurs_bloquantes == 0" # Aucune erreur bloquante
          message_erreur: "Checklist préparation incomplète ou erreurs détectées"
        
        # Prérequis machine
        prerequis_machine:
          - "machine.statut == 'prete'"
          - "machine.temperature_ok == true"
          - "machine.niveaux_ok == true"
          - "machine.securite_ok == true"
        
        script_javascript: |
          async function demarrerProductionPrepare(objet, contexte) {
            const timestamp = Date.now();
            
            // 1. Vérification prérequis template OBLIGATOIRE
            const templateData = await getTemplateData(objet, "affichage_checklist");
            if (!templateData) {
              throw new Error("Template checklist non trouvé");
            }
            
            // Vérifier sections obligatoires complétées
            const sectionsObligatoires = [1, 2, 3];
            const sectionsCompletes = templateData.sections_completes || [];
            const manquantes = sectionsObligatoires.filter(s => !sectionsCompletes.includes(s));
            
            if (manquantes.length > 0) {
              throw new Error(`Sections checklist incomplètes: ${manquantes.join(', ')}`);
            }
            
            // Vérifier éléments critiques
            const elementsCritiques = await getElementsCritiques(templateData);
            const elementsNonValides = elementsCritiques.filter(e => !e.valide);
            
            if (elementsNonValides.length > 0) {
              throw new Error(`Éléments critiques non validés: ${elementsNonValides.map(e => e.id).join(', ')}`);
            }
            
            // 2. Vérification état machine
            const statutMachine = await verifierStatutMachine(objet);
            if (!statutMachine.prete) {
              throw new Error(`Machine non prête: ${statutMachine.raisons.join(', ')}`);
            }
            
            // 3. Démarrage production
            objet.proprietesDynamiques.production_demarree = true;
            objet.proprietesDynamiques.timestamp_demarrage = timestamp;
            objet.proprietesDynamiques.operateur_demarrage = contexte.utilisateur.id;
            objet.proprietesDynamiques.checklist_preparation = {
              template_id: "affichage_checklist",
              sections_validees: sectionsCompletes,
              elements_valides: templateData.elements_valides,
              timestamp_completion: templateData.timestamp_completion,
              operateur_validation: contexte.utilisateur.id
            };
            
            // 4. Logging détaillé
            const logData = {
              action: "demarrer_production_prepare",
              timestamp: timestamp,
              objet_id: objet.id,
              utilisateur: contexte.utilisateur.id,
              checklist_data: objet.proprietesDynamiques.checklist_preparation,
              machine_status: statutMachine,
              shift: contexte.temporalite.shift
            };
            
            // Historique interne
            objet.historique.add("demarrage_production", logData);
            
            // Log externe détaillé
            await logToFile("demarrage_production.log", logData);
            
            // 5. Notification équipes
            await notifierEquipes("production_demarree", {
              objet_id: objet.id,
              operateur: contexte.utilisateur.id,
              timestamp: timestamp
            });
            
            return {
              nouvel_etat: "En_Production",
              donnees_maj: objet.proprietesDynamiques,
              message: "Production démarrée avec succès - Checklist validée",
              transition_data: {
                checklist_complete: true,
                machine_prete: true,
                operateur_habilite: true
              }
            };
          }
        
        # Interface saisie optionnelle
        interface_saisie:
          formulaire:
            - nom: "commentaire_demarrage"
              type: "textarea"
              obligatoire: false
              max_chars: 200
              placeholder: "Commentaire optionnel sur le démarrage..."
        
        # Transition vers production
        transition: "En_Production"
        
        # Logging spécialisé
        logging:
          historique_interne: true
          log_externe: true
          fichier_log: "demarrage_production.log"
          niveau_detail: "detaille"
          donnees_supplementaires: ["checklist_data", "machine_status"]
      
      - id: "reporter_preparation"
        nom: "Reporter Préparation"
        description: "Reporter la préparation avec motif"
        
        interface_saisie:
          formulaire:
            - nom: "motif_report"
              type: "select"
              obligatoire: true
              options: ["Maintenance machine", "Matière première indisponible", "Formation opérateur", "Autres"]
              
            - nom: "duree_estimee_report"
              type: "number"
              obligatoire: true
              unite: "minutes"
              min: 15
              max: 480
              
            - nom: "commentaire_report"
              type: "textarea"
              obligatoire: false
              max_chars: 300
        
        script_javascript: |
          async function reporterPreparation(objet, parametres, contexte) {
            const timestamp = Date.now();
            
            // Sauvegarder progression checklist actuelle
            const progressionActuelle = await getTemplateProgression(objet, "affichage_checklist");
            
            objet.proprietesDynamiques.preparation_reportee = {
              timestamp: timestamp,
              motif: parametres.motif_report,
              duree_estimee: parametres.duree_estimee_report,
              commentaire: parametres.commentaire_report,
              operateur: contexte.utilisateur.id,
              progression_sauvegardee: progressionActuelle
            };
            
            return {
              nouvel_etat: "Preparation_Reportee",
              message: `Préparation reportée: ${parametres.motif_report}`
            };
          }
        
        transition: "Preparation_Reportee"

### 9.3 État Contrôle Final avec Même Template
```yaml
etat_controle_final_complet:
  # Identité
  id: "etat_controle_final_001"
  nom: "Contrôle_Final"
  description: "État de contrôle final avec checklist validation"
  process_id: "process_production_industrielle"
  
  # Comportement particulier avec même template, paramètres différents
  comportement_particulier:
    actif: true
    mode: "utilise_template"
    
    template_utilise:
      template_id: "affichage_checklist" # MÊME TEMPLATE
      template_version: "1.0"
      
      # Paramètres spécifiques contrôle final
      parametres:
        document_source: "checklist_qualite_v2.yaml" # MÊME SOURCE
        sections_a_afficher: [4, 5] # Pièce Finie + Documentation
        titre_personnalise: "Contrôles Finaux Qualité"
        format_affichage: "tableau"
        actions_par_element: ["valider", "rejeter", "reporter", "commenter"]
        couleur_theme: "#D32F2F" # Rouge contrôle
        icone_section: "✅"
      
      # Surcharges état contrôle final
      surcharges:
        sections_supplementaires:
          - nom: "Résultats Production"
            contenu: "afficher_resultats_production"
            format: "metriques_production"
            position: "before_checklist"
            donnees: ["pieces_produites", "temps_cycle", "defauts_detectes"]
          
          - nom: "Décision Qualité"
            contenu: "actions_decision_qualite"
            format: "boutons_decision"
            position: "after_checklist"
            visible_si: "checklist_controle_complete"
        
        regles_contexte_specifiques:
          - condition: "objet.proprietes_dynamiques.defauts_production > 0"
            modifications:
              - afficher_alerte: "Défauts détectés - Contrôles renforcés requis"
              - rendre_obligatoire: ["pf_002"] # Contrôle visuel obligatoire
              - ajouter_actions: ["analyse_defauts"]
    
    # Actions spécifiques contrôle final
    actions_supplementaires:
      - id: "valider_lot_production"
        nom: "Valider Lot Production"
        description: "Validation finale lot après contrôles"
        
        prerequis_template:
          template_id: "affichage_checklist"
          conditions:
            - "sections_completes >= [4, 5]" # Pièce Finie + Documentation
            - "tous_controles_ok == true"
            - "documentation_complete == true"
        
        script_javascript: |
          async function validerLotProduction(objet, contexte) {
            // Vérification checklist contrôle final
            const checklistData = await getTemplateData(objet, "affichage_checklist");
            
            // Validation finale
            objet.proprietesDynamiques.lot_valide = true;
            objet.proprietesDynamiques.timestamp_validation = Date.now();
            objet.proprietesDynamiques.operateur_validation = contexte.utilisateur.id;
            
            return {
              nouvel_etat: "Lot_Valide",
              message: "Lot validé - Contrôles conformes"
            };
          }
        
        transition: "Lot_Valide"
      
      - id: "rejeter_lot_production"
        nom: "Rejeter Lot"
        description: "Rejet lot suite à non-conformités"
        
        interface_saisie:
          formulaire:
            - nom: "motifs_rejet"
              type: "checkbox_multiple"
              obligatoire: true
              options: ["Dimensions hors tolérances", "Défauts visuels", "Test fonctionnel échoué", "Documentation incomplète"]
              
            - nom: "actions_correctives"
              type: "textarea"
              obligatoire: true
              max_chars: 500
              placeholder: "Décrire les actions correctives nécessaires..."
        
        transition: "Lot_Rejete"
```

---

## 10. VALIDATION ET CONTRAINTES v2.1

### 10.1 Règles de Validation Enrichies Templates
```yaml
validation_v21:
  # État obligatoire (enrichi templates)
  contraintes_obligatoires:
    - "Un état doit toujours avoir un comportement (base/template/particulier)"
    - "Si template utilisé, paramètres doivent respecter schéma template"
    - "Actions template générées doivent être cohérentes avec paramètres"
    - "Transitions d'état doivent être définies même avec templates"
    - "Template référencé doit exister dans bibliothèque"
    
  # Cohérence données (enrichie templates)
  coherence:
    - "Paramètres template doivent respecter schema_parametres"
    - "Sections template référencées doivent exister dans document source"
    - "Actions template doivent être compatibles avec données objet"
    - "Surcharges état ne doivent pas casser fonctionnement template"
    - "Menu hamburger enrichi doit être cohérent"
    
  # Sécurité (enrichie templates)
  securite:
    - "Validation paramètres template obligatoire avant génération"
    - "Scripts template doivent être sandboxés"
    - "Accès documents source doit être contrôlé"
    - "Logs templates doivent respecter confidentialité"
    - "Permissions utilisateur applicables aux actions template"
    
  # 🆕 Validation spécifique templates
  validation_templates:
    - "Template doit avoir schema_parametres valide"
    - "Script génération doit retourner structure cohérente"
    - "Actions template doivent avoir transitions définies"
    - "Exemples usage doivent être testés et validés"
    - "Versions template doivent être rétrocompatibles"
```

### 10.2 Tests de Validation v2.1
```yaml
tests_validation_v21:
  # Tests existants v2.0
  - nom: "Affichage comportement de base"
    test: "Vérifier que toutes les sections obligatoires s'affichent"
    
  - nom: "Gestion contexte"
    test: "Vérifier que l'affichage change selon le rôle utilisateur"
    
  - nom: "Exécution actions"
    test: "Vérifier que chaque action produit une transition d'état"
    
  - nom: "API cohérence"
    test: "Vérifier que l'API expose les mêmes données que l'interface"
  
  # 🆕 Tests nouveaux templates v2.1
  - nom: "Validation template schema"
    test: "Vérifier que le schéma paramètres template est valide"
    details: "JSON Schema validation + tests cas limites"
    
  - nom: "Génération interface template"
    test: "Vérifier que l'interface template se génère correctement"
    details: "Test avec paramètres valides + gestion erreurs"
    
  - nom: "Actions template fonctionnelles"
    test: "Vérifier que les actions template s'exécutent"
    details: "Test workflow complet template → action → transition"
    
  - nom: "Fusion comportements"
    test: "Vérifier fusion base + template + particulier"
    details: "Test priorités et cohérence résultat final"
    
  - nom: "Performance templates"
    test: "Vérifier performance génération interface"
    details: "Templates complexes < 500ms génération"
    
  - nom: "Sécurité templates"
    test: "Vérifier sécurité scripts template"
    details: "Sandbox + validation paramètres + accès contrôlé"
```

---

## 11. AVANTAGES ARCHITECTURE v2.1

### 11.1 Réutilisabilité Maximale (Nouveau)
- **1 Template = N États** avec paramètres différents
- **Bibliothèque centralisée** de comportements communs
- **Maintenance template** impacte automatiquement tous les états
- **Standards UX** garantis entre états similaires
- **Expertise capitalisée** dans templates réutilisables

### 11.2 Flexibilité Préservée et Enrichie
- **3 modes comportement** : manuel (v2.0), template (v2.1), mixte (v2.1)
- **Paramétrage fin** des templates par état
- **Surcharges possibles** pour spécificités état
- **Compatibilité totale** avec architecture v2.0
- **Vue standard toujours accessible** via menu hamburger

### 11.3 Performance et Cohérence
- **Templates mis en cache** pour performance optimale
- **Génération interface** dynamique et optimisée
- **Validation automatique** paramètres vs schéma
- **Menu enrichi automatiquement** selon mode
- **Logging spécialisé** pour analyse templates

### 11.4 Évolutivité et Maintenance
- **Bibliothèque extensible** avec nouveaux templates
- **Versioning templates** pour compatibilité
- **Tests automatisés** validation templates
- **Documentation intégrée** exemples usage
- **Migration progressive** v2.0 → v2.1

---

## 12. EXPOSITION API v2.1 (Enrichie Templates)

### 12.1 Endpoints Standard Enrichis
```yaml
api_exposition_v21:
  base_url: "/api/v1/objets/{objet_id}/etats/{etat_id}"
  
  endpoints:
    # Consultation état enrichie
    - method: "GET"
      path: "/"
      description: "Obtenir l'état complet avec données template si applicable"
      response_type: "vue_comportement_complet_v21"
      nouvelle_donnees: ["template_data", "parametres_appliques", "progression_template"]
      
    # Actions disponibles enrichies
    - method: "GET" 
      path: "/actions"
      description: "Lister actions (process+template+état) selon contexte"
      nouvelle_donnees: ["origine_action", "prerequis_template", "parametres_action"]
      
    # Exécution action enrichie
    - method: "POST"
      path: "/actions/{action_id}"
      description: "Déclencher action avec support templates"
      nouvelle_donnees: ["template_context", "validation_prerequis"]
      
    # 🆕 Endpoints templates spécifiques
    - method: "GET"
      path: "/template"
      description: "Informations template utilisé par l'état"
      response: "template_info + parametres + progression"
      
    - method: "PUT"
      path: "/template/parametres"
      description: "Modifier paramètres template (si autorisé)"
      body: "nouveaux_parametres"
      
    - method: "GET"
      path: "/template/progression"
      description: "Progression actuelle template (ex: checklist)"
      response: "elements_completes + restants + pourcentage"
      
    - method: "POST"
      path: "/template/reset"
      description: "Réinitialiser template (si autorisé)"
      
  # 🆕 Endpoints bibliothèque templates
  templates_endpoints:
    base_url: "/api/v1/templates"
    
    - method: "GET"
      path: "/"
      description: "Lister tous templates disponibles"
      
    - method: "GET"
      path: "/{template_id}"
      description: "Détails template + schéma paramètres"
      
    - method: "POST"
      path: "/{template_id}/preview"
      description: "Prévisualisation template avec paramètres test"
      body: "parametres_test"
      
    - method: "GET"
      path: "/{template_id}/exemples"
      description: "Exemples usage template"
```

### 12.2 Format de Réponse Standard v2.1
```json
{
  "etat": {
    "id": "uuid-etat",
    "nom": "Preparation_Production",
    "timestamp_entree": "2025-08-17T08:00:00Z",
    "mode_comportement": "utilise_template"
  },
  "objet": {
    "id": "uuid-objet",
    "nom": "Composant_C2025001",
    "identifiants_gs1": { ... },
    "proprietes_statiques": { ... },
    "proprietes_dynamiques": { ... }
  },
  "template_data": {
    "template_id": "affichage_checklist",
    "template_version": "1.0",
    "parametres_appliques": {
      "sections_a_afficher": [1, 2, 3],
      "format_affichage": "liste_cochable",
      "titre_personnalise": "Contrôles Préparatoires Production"
    },
    "progression": {
      "elements_total": 8,
      "elements_completes": 3,
      "pourcentage": 37.5,
      "sections_completes": [1],
      "sections_en_cours": [2]
    },
    "interface_generee": {
      "titre": "Contrôles Préparatoires Production",
      "sections": [
        {
          "numero": 1,
          "nom": "Vérification Matière Première",
          "elements": [
            {
              "id": "mp_001",
              "texte": "Vérifier certificat matière première",
              "statut": "valide",
              "timestamp_validation": "2025-08-17T10:15:00Z"
            }
          ]
        }
      ]
    }
  },
  "actions_disponibles": [
    {
      "id": "valider_element_checklist",
      "nom": "Valider Élément",
      "origine": "template",
      "template_id": "affichage_checklist",
      "endpoint": "/actions/valider_element_checklist",
      "parametres_requis": ["element_id"],
      "permissions_requises": ["validation_qualite"]
    },
    {
      "id": "demarrer_production",
      "nom": "Démarrer Production",
      "origine": "etat",
      "endpoint": "/actions/demarrer_production",
      "prerequis_template": {
        "template_id": "affichage_checklist",
        "conditions": ["sections_completes >= [1,2,3]"]
      },
      "visible": false,
      "raison_masquee": "Checklist préparation incomplète"
    }
  ],
  "contexte_applique": {
    "utilisateur_role": "operateur",
    "timestamp": "2025-08-17T10:30:00Z",
    "template_autorisations": ["consulter", "valider_elements"]
  },
  "menu_hamburger": [
    {
      "nom": "Vue Standard Process",
      "action": "afficher_comportement_base",
      "origine": "auto_template"
    },
    {
      "nom": "Vue Template Complète",
      "action": "afficher_template_complet",
      "origine": "auto_template"
    }
  ]
}
```

---

## 13. RÉSUMÉ ÉVOLUTION v2.0 → v2.1

### 13.1 Nouveautés Majeures v2.1
1. **📚 Bibliothèque Templates** - Comportements réutilisables avec paramétrage
2. **⚙️ 3 Modes Comportement** - Manuel (v2.0) + Template (v2.1) + Mixte (v2.1)
3. **🔧 Templates Prédéfinis** - Checklist, Dashboard, Formulaire dynamique
4. **📊 Logging Enrichi** - Suivi spécialisé templates et progressions
5. **🔄 Résolution Intelligente** - Fusion base + template + particulier + contexte
6. **🛠️ API Étendue** - Endpoints templates et gestion paramètres

### 13.2 Compatibilité v2.0
- **100% Rétrocompatible** - États v2.0 fonctionnent sans modification
- **Migration progressive** - Adoption templates par état selon besoins
- **Coexistence** - États manuels et templates dans même process
- **Conservation** - Toutes fonctionnalités v2.0 préservées

### 13.3 Bénéfices Concrets
- **Développement accéléré** - Réutilisation templates vs développement manuel
- **Cohérence UX** - Standards visuels et comportementaux garantis
- **Maintenance simplifiée** - Évolution template = tous états impactés
- **Expertise capitalisée** - Savoir-faire UX dans bibliothèque centrale
- **Flexibilité préservée** - Possibilité surcharges et personnalisations

---

**Template ÉTAT ProcessMetaLanguage v2.1 COMPLET** - Architecture révolutionnaire combinant la robustesse de la v2.0 avec la réutilisabilité des templates, offrant une flexibilité maximale pour la conception d'interfaces métier cohérentes et évolutives.