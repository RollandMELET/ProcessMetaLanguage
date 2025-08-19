# Template de Base - ÉTAT ProcessMetaLanguage v2.2 COMPLET

**Version** : 2.2  
**Date** : 2025-08-17  
**Auteur** : Rolland MELET  
**Évolutions** : 
- v2.0 : COMPORTEMENT DE BASE transféré au niveau PROCESSUS
- v2.1 : Ajout TEMPLATES de COMPORTEMENT SPÉCIFIQUE réutilisables
- v2.2 : **Ajout WORKFLOW (global) + Clarification PROCESSUS (objet)**

---

## 📊 DIAGRAMMES DE RÉFÉRENCE

### Diagramme 1 : Vue d'Ensemble du Template ÉTAT v2.2 (Architecture Complète)

```
                        🏗️ WORKFLOW 🏗️
                   ┌─────────────────────────────┐
                   │ • ID unique                 │
                   │ • Nom descriptif            │
                   │ • ORCHESTRATION multi-objets│
                   │ • Synchronisation processus │
                   │ • Règles dépendances        │
                   │ • Planning & jalons         │
                   │ • Vue d'ensemble projet     │
                   │ • Métriques globales        │
                   └─────────┬───────────────────┘
                             │ (orchestration)
           ┌─────────────────▼─────────────────┐
           │         PROCESSUS INCLUS          │
           │ ┌─────────┐ ┌─────────┐ ┌───────┐ │
           │ │PROCES 1 │ │PROCES 2 │ │ ... N │ │
           │ └─────────┘ └─────────┘ └───────┘ │
           └─────────────────┬─────────────────┘
                             │ (spécialisation)
                             ▼
                      📄 PROCESSUS 📄
             ┌─────────────────────────────────────────┐
             │ • ID unique                             │
             │ • Nom spécifique objet                  │
             │ • COMPORTEMENT DE BASE (GLOBAL)         │
             │ • Workflow parent (optionnel)           │
             │ • Configuration interface standard      │
             │ • Menu hamburger paramétrable           │
             │ • Règles contextuelles objet            │
             │ • États cycle de vie                    │
             └─────────┬───────────────────────────────┘
                       │ (état actuel)
                       ▼
                 🏳️ ÉTAT ACTUEL 🏳️
             ┌─────────────────────────────────────────┐
             │ • ID unique                             │
             │ • Nom descriptif                       │
             │ • Lien vers PROCESSUS parent            │
             │ • Hérite COMPORTEMENT DE BASE           │
             │ • COMPORTEMENT PARTICULIER (opt)        │
             │ • Actions spécifiques à l'état          │
             │ 🆕 UTILISE TEMPLATE (nouveau v2.1)      │
             │ 🆕 CONTEXTE WORKFLOW enrichi (v2.2)     │
             └─────────┬───────────────────────────────┘
                       │ (comportement)
                       ▼
               ⚙️ COMPORTEMENTS ⚙️
        ┌─────────────────────────────────────────────────┐
        │                                                 │
        ▼                         ▼                       ▼
🎯 HÉRITÉ DU PROCESSUS    📋 TEMPLATE RÉUTILISABLE    📘 ACTIONS
(COMPORTEMENT BASE)     (NOUVEAU v2.1)           DISPONIBLES
                                │
                                ▼
                    📚 BIBLIOTHÈQUE TEMPLATES 📚
                    ┌─────────────────────────────┐
                    │ • Templates globaux         │
                    │ • Schémas paramètres        │ 
                    │ • Scripts génération        │
                    │ • Exemples usage            │
                    └─────────────────────────────┘
```

### Diagramme 2 : Flux avec Héritage PROCESSUS + Templates + Workflow v2.2

#### **Phase 1 : Résolution du Comportement Enrichie Workflow**
```
Utilisateur demande consultation OBJET
                    ↓
        📄 RÉSOLUTION COMPORTEMENT v2.2
                    ↓
    ┌─ 🆕 Identifier WORKFLOW parent ─────────┐
    │  • Objet.workflow_parent_id             │
    │  • Récupérer contexte WORKFLOW          │
    │  • Charger contraintes globales         │
    │  • Vérifier dépendances autres processus│
    │  • Calculer avancement global           │
    └──────────────────┬────────────────────────┘
                      ↓
    ┌─ Identifier PROCESSUS objet ─────────────┐
    │  • Objet.processus_objet_id              │
    │  • Récupérer config PROCESSUS objet     │
    │  • Charger COMPORTEMENT DE BASE         │
    │  • Appliquer contexte workflow          │
    └──────────────────┬────────────────────────┘
                      ↓
    ┌─ Identifier ÉTAT actuel ──────────────────┐
    │  • Objet.etat_actuel_id                  │
    │  • Vérifier mode comportement            │
    │  •   - Manuel (v2.0 classique)          │
    │  •   - Template (v2.1 nouveau)          │
    │  •   - Mixte (v2.1 hybride)             │
    └──────────────────┬────────────────────────┘
                      ↓
    ┌─ 🆕 Si TEMPLATE utilisé ──────────────────┐
    │  • Charger depuis bibliothèque           │
    │  • Valider paramètres vs schéma          │
    │  • Générer interface dynamique           │
    │  • Appliquer surcharges état             │
    │  • Enrichir avec contexte workflow       │
    └──────────────────┬────────────────────────┘
                      ↓
    ┌─ Déterminer COMPORTEMENT FINAL ──────────┐
    │  • BASE (du PROCESSUS)                   │
    │  • + CONTEXTE WORKFLOW (v2.2)            │
    │  • + TEMPLATE paramétré (v2.1)           │
    │  • + PARTICULIER (de l'ÉTAT)             │
    │  • Selon CONTEXTE utilisateur enrichi    │
    └──────────────────┬────────────────────────┘
                      ↓
            AFFICHAGE INTERFACE ENRICHIE
```

#### **Phase 2 : Affichage Interface Enrichie Workflow (Visible par Utilisateur)**
```
        📱 INTERFACE DÉTERMINÉE v2.2 📱
                    ↓
    ┌─ COMPORTEMENT DE BASE ENRICHI ──────────────┐
    │  🔧 Interface Standard                      │
    │  ├─ Identité OBJET                          │
    │  ├─ État actuel                             │
    │  ├─ 🆕 Contexte WORKFLOW (si applicable)    │
    │  │   └─ Workflow parent, Avancement global  │
    │  ├─ Propriétés statiques                    │
    │  ├─ Propriétés dynamiques                   │
    │  └─ Actions disponibles                     │
    │                                             │
    │  📋 Menu Hamburger PARAMÉTRABLE ENRICHI     │
    │  ├─ Historique complet              ✓/✗    │
    │  ├─ Données techniques              ✓/✗    │
    │  ├─ Processus global                ✓/✗    │
    │  ├─ 🆕 Vue Workflow Global          ✓/✗    │
    │  ├─ 🆕 Planning Projet             ✓/✗    │
    │  ├─ 🆕 Dépendances                 ✓/✗    │
    │  ├─ Audit trail (*)                ✓/✗    │
    │  ├─ Export données                 ✓/✗    │
    │  └─ API Documentation              ✓/✗    │
    └─────────────────────────────────────────────┘
                    OU
    ┌─ 🆕 COMPORTEMENT TEMPLATE ENRICHI ──────────┐
    │  📋 Interface Générée Template              │
    │  ├─ Sections paramétrées                    │
    │  ├─ Données filtrées/sélection              │
    │  ├─ 🆕 Contexte workflow intégré            │
    │  ├─ Actions template+état                   │
    │  └─ Format optimisé dynamique               │
    │                                             │
    │  📋 Menu Hamburger ENRICHI WORKFLOW         │
    │  ├─ 🔵 VUE STANDARD (AUTO)                  │
    │  ├─ 📋 VUE TEMPLATE ORIGINAL                │
    │  ├─ 🆕 VUE WORKFLOW GLOBAL                  │
    │  ├─ 🆕 PLANNING PROJET                      │
    │  ├─ 🆕 AUTRES PROCESSUS                     │
    │  ├─ Historique complet              ✓/✗    │
    │  ├─ Données techniques              ✓/✗    │
    │  ├─ Processus global                ✓/✗    │
    │  ├─ Audit trail (*)                ✓/✗    │
    │  ├─ Export données                 ✓/✗    │
    │  └─ API Documentation              ✓/✗    │
    └─────────────────────────────────────────────┘
                    OU
    ┌─ COMPORTEMENT PARTICULIER ENRICHI ──────────┐
    │  🎨 Interface Spécifique                    │
    │  ├─ Sections personnalisées                 │
    │  ├─ Données métier mises avant              │
    │  ├─ 🆕 Intégration workflow native          │
    │  ├─ Actions prioritaires                    │
    │  └─ Format optimisé                         │
    │                                             │
    │  📋 Menu Hamburger ENRICHI COMPLET          │
    │  ├─ 🔵 VUE STANDARD (AUTO)                  │
    │  ├─ 🆕 VUE WORKFLOW GLOBAL                  │
    │  ├─ 🆕 DÉPENDANCES ACTIVES                  │
    │  ├─ Historique complet              ✓/✗    │
    │  ├─ Données techniques              ✓/✗    │
    │  ├─ Processus global                ✓/✗    │
    │  ├─ Audit trail (*)                ✓/✗    │
    │  ├─ Export données                 ✓/✗    │
    │  └─ API Documentation              ✓/✗    │
    └─────────────────────────────────────────────┘

(*) Audit trail = Journal des modifications + Historique workflow
    ├─ Qui a fait quoi sur l'objet
    ├─ Quand et pourquoi  
    ├─ Valeurs avant/après
    ├─ 🆕 Impact sur workflow global
    ├─ 🆕 Dépendances affectées
    └─ Traçabilité réglementaire complète
```

### Diagramme 3 : Architecture Workflow v2.2 (Nouveau)

```
    🏗️ WORKFLOW - ORCHESTRATION GLOBALE 🏗️
                         ↓
    ┌─ Workflow Simple (Groupement) ────────────┐
    │  • Type: "groupement"                     │
    │  • Processus sans dépendances             │
    │  • Métriques agrégées simples             │
    │  • Exemple: Formation Opérateurs Q3       │
    └────────────────────────────────────────────┘
                         OU
    ┌─ Workflow Complexe (Orchestration) ───────┐
    │  • Type: "orchestration_complete"         │
    │  • Dépendances inter-processus            │
    │  • Planning avec jalons                   │
    │  • Gestion ressources partagées           │
    │  • Exemple: Fabrication Moteur BMW        │
    └──────────────────┬─────────────────────────┘
                       │
                       ▼
            ORCHESTRATION DÉTAILLÉE
                       ↓
    ┌─ Dépendances ──────────────────────────────┐
    │  • Prérequis: [ProcessusA.État] →         │
    │  • Déclenche: ProcessusB.Action           │
    │  • Synchronisation: ET/OU logique         │
    │  • Exemple: Bloc ET Culasse → Assemblage  │
    └────────────────────────────────────────────┘
                       ↓
    ┌─ Jalons & Planning ───────────────────────┐
    │  • Jalons: Dates clés avec prérequis      │
    │  • Planning: Début/Fin/Livraison          │
    │  • Ressources: Machines/Opérateurs        │
    │  • Contraintes: Qualité/Coûts/Délais      │
    └────────────────────────────────────────────┘
                       ↓
    ┌─ Métriques Globales ──────────────────────┐
    │  • Avancement: Processus terminés/total   │
    │  • Retard: Jours vs planning              │
    │  • Ressources: Utilisation/Disponibilité  │
    │  • Qualité: KPI agrégés                   │
    └────────────────────────────────────────────┘
```

### Diagramme 4 : Gestion du CONTEXTE Enrichi Workflow v2.2

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

    🆕 PROCESSUS OBJET      🆕 WORKFLOW GLOBAL
    ├─ Processus ID        ├─ Workflow ID
    ├─ Avancement processus├─ Avancement global
    ├─ État précédent      ├─ Processus terminés
    ├─ Temps dans état     ├─ Processus en cours
    └─ Règles contexte     ├─ Contraintes planning
                          ├─ Dépendances actives
                          ├─ Ressources partagées
                          └─ Jalons critiques
                    ↓
            🎯 COMPORTEMENT ADAPTÉ v2.2
            ├─ Interfaces exposées
            ├─ Actions disponibles (objet+workflow)
            ├─ Niveau détail
            ├─ Format présentation
            ├─ 🆕 Contexte workflow visible
            ├─ 🆕 Dépendances affichées
            └─ 🆕 Planning intégré
```

### Diagramme 5 : Structure des ACTIONS Enrichie Workflow v2.2

```
        📘 ACTION UTILISATEUR v2.2 📘
                    ↓
    ┌─ Configuration Enrichie ─────────────────────┐
    │  • ID unique                                │
    │  • Nom descriptif                          │
    │  • Conditions visibilité                   │
    │  • 🆕 Impact workflow (optionnel)           │
    │  • Configuration logging                   │
    │    ├─ Historique interne ✓/✗               │
    │    ├─ Log externe ✓/✗                      │
    │    ├─ 🆕 Log workflow ✓/✗                  │
    │    ├─ Fichier log spécifique               │
    │    └─ Niveau détail                        │
    └──────────────────┬──────────────────────────┘
                       ↓
    ┌─ Définition Enrichie ────────────────────────┐
    │  📋 Langage naturel                         │
    │  ├─ Étapes en français                      │
    │  └─ Description processus                   │
    │                                             │
    │  💻 Script JavaScript                       │
    │  ├─ Vérifications préalables                │
    │  ├─ 🆕 Vérification dépendances workflow    │
    │  ├─ Exécution logique                       │
    │  ├─ Modifications données                   │
    │  ├─ 🆕 Notification workflow (si impact)    │
    │  ├─ Logging automatique                     │
    │  └─ Détermination nouvel état               │
    │                                             │
    │  🆕 PROVENANCE v2.2                         │
    │  ├─ Du WORKFLOW (orchestration)             │
    │  ├─ Du PROCESSUS (communes objet)           │
    │  ├─ Du TEMPLATE (générées)                  │
    │  └─ De l'ÉTAT (spécifiques)                 │
    └──────────────────┬──────────────────────────┘
                       ↓
    ┌─ Interface Saisie Enrichie ─────────────────┐
    │  📋 Formulaire dynamique                    │
    │  ├─ Champs typés                            │
    │  ├─ Validations                             │
    │  ├─ 🆕 Contraintes workflow                 │
    │  └─ Conditions obligatoires                 │
    └──────────────────┬──────────────────────────┘
                       ↓
    ┌─ Résultat Obligatoire Enrichi ──────────────┐
    │  📄 Transition ÉTAT                         │
    │  ├─ Nouvel état cible                       │
    │  ├─ Conditions transition                   │
    │  ├─ Même état (boucle) OK                   │
    │  └─ 🆕 Impact workflow propagé              │
    │                                             │
    │  🆕 IMPACT WORKFLOW                         │
    │  ├─ Mise à jour métriques globales          │
    │  ├─ Vérification dépendances                │
    │  ├─ Déclenchement processus liés            │
    │  ├─ Recalcul planning si nécessaire         │
    │  └─ Notification équipes concernées         │
    └─────────────────────────────────────────────┘
```

---

## 1. STRUCTURE FONDAMENTALE RÉVISÉE v2.2 (Workflow + Processus)

### 1.1 Hiérarchie WORKFLOW → PROCESSUS → ÉTAT (Nouvelle Architecture)
```yaml
# 🆕 NIVEAU WORKFLOW (Global, Multi-objets)
workflow:
  id: "uuid-unique-workflow"
  nom: "Fabrication_Moteur_BMW_X3"
  description: "Fabrication complète moteur pour commande client"
  type: "orchestration_complete" # ou "groupement"
  
  # Processus inclus dans le workflow
  processus_inclus:
    - processus_id: "production_bloc_moteur"
      objet_id: "bloc_moteur_001"
      criticite: "haute"
      
    - processus_id: "usinage_culasse"
      objet_id: "culasse_001" 
      criticite: "haute"
      
    - processus_id: "production_pistons"
      objets_ids: ["piston_001", "piston_002", "piston_003", "piston_004"]
      criticite: "moyenne"
  
  # Orchestration complexe
  orchestration:
    dependances:
      - prerequis: ["production_bloc_moteur.Termine", "usinage_culasse.Termine"]
        declenche: "assemblage_moteur.Demarrage"
        
      - prerequis: ["production_pistons.Tous_Termines"]
        declenche: "assemblage_final.Possible"
    
    jalons:
      - nom: "Pieces_Principales_Terminees"
        date_cible: "2025-09-10"
        prerequis: ["bloc_termine", "culasse_terminee"]
        
      - nom: "Assemblage_Complete"
        date_cible: "2025-09-12"
        prerequis: ["assemblage_moteur.Termine"]
    
    metriques_globales:
      avancement_global: 0.65
      pieces_terminees: 3
      pieces_total: 6
      retard_planning: 0 # jours
  
  # Planning global
  planning:
    date_debut: "2025-08-15"
    date_fin_prevue: "2025-09-15"
    date_livraison_client: "2025-09-15"
    
  # Ressources globales
  ressources_globales:
    machines_allouees: ["CNC_001", "CNC_002", "ASSEMBLAGE_001"]
    operateurs_assignes: ["op_001", "op_002", "op_003"]
    matieres_premieres: ["acier_A", "aluminium_B"]

# OBJET avec références enrichies workflow
objet:
  id: "uuid-unique-objet"
  nom: "Bloc_Moteur_BMW_001"
  processus_objet_id: "uuid-processus" # 🔄 RENOMMÉ pour clarté
  etat_actuel_id: "uuid-etat"
  workflow_parent_id: "uuid-workflow" # 🆕 RÉFÉRENCE workflow parent

# PROCESSUS (Niveau Objet, ex-"process")
processus:
  id: "uuid-unique-processus"
  nom: "Production_Bloc_Moteur"
  description: "Processus de production spécifique au bloc moteur"
  workflow_parent_id: "uuid-workflow" # 🆕 RÉFÉRENCE workflow parent
  
  # COMPORTEMENT DE BASE (commun à tous les états de ce processus)
  comportement_base:
    interface_standard: {...}
    menu_hamburger: {...}
    regles_contexte: {...}
    action_automatique_consultation: {...}
    
  # 🆕 Enrichissement contexte workflow
  contexte_workflow:
    contraintes_planning: "Héritées du workflow parent"
    dependances_autres_processus: "Calculées dynamiquement"
    ressources_partagees: "Machines et opérateurs disponibles"
    metriques_contribution: "Impact sur avancement global"
    
# ÉTAT inchangé v2.1 (avec enrichissement contextuel workflow)
etat:
  id: "uuid-unique-etat"
  nom: "En_Usinage"
  processus_parent_id: "uuid-processus" # Référence processus objet
  
  # COMPORTEMENT PARTICULIER v2.1 (3 modes possibles)
  comportement_particulier:
    actif: true
    mode: "utilise_template" # manuel|utilise_template|mixte
    
    # 🆕 Enrichissement workflow dans template
    template_utilise:
      template_id: "dashboard_metriques"
      template_version: "1.0"
      parametres: {...}
      surcharges: {...}
      
      # 🆕 Intégration contexte workflow
      contexte_workflow_integre:
        afficher_avancement_global: true
        afficher_dependances: true
        alertes_planning: true
        ressources_temps_reel: true
```

### 1.2 🆕 Bibliothèque Templates Enrichie Workflow
```yaml
bibliotheque_templates_v22:
  templates:
    - id: "affichage_checklist_workflow"
      nom: "Affichage Checklist avec Contexte Workflow"
      version: "2.2"
      description: "Template checklist enrichi informations workflow"
      
      # 🆕 Paramètres workflow intégrés
      schema_parametres_enrichi:
        # Paramètres existants v2.1
        - nom: "document_source"
          type: "string"
          obligatoire: true
          
        # 🆕 Nouveaux paramètres workflow
        - nom: "afficher_contexte_workflow"
          type: "boolean"
          defaut: true
          description: "Afficher informations workflow dans interface"
          
        - nom: "dependances_workflow_visibles"
          type: "boolean"
          defaut: false
          description: "Afficher dépendances avec autres processus"
          
        - nom: "planning_integre"
          type: "boolean" 
          defaut: false
          description: "Intégrer jalons workflow dans checklist"
      
      # 🆕 Comportement template enrichi workflow
      comportement_template_enrichi:
        interface_specifique:
          nom: "Interface_Checklist_Workflow"
          
          # Sections existantes v2.1
          sections_dynamiques:
            - nom: "Checklist Filtrée"
              contenu: "generer_checklist_standard"
              
            # 🆕 Nouvelles sections workflow
            - nom: "Contexte Workflow"
              contenu: "afficher_contexte_workflow"
              visible_si: "parametres.afficher_contexte_workflow"
              format: "panneau_workflow"
              
            - nom: "Dépendances Actives"
              contenu: "lister_dependances_workflow"
              visible_si: "parametres.dependances_workflow_visibles"
              format: "liste_dependances"
              
            - nom: "Planning Intégré"
              contenu: "jalons_workflow_pertinents"
              visible_si: "parametres.planning_integre"
              format: "timeline_compact"
              
    - id: "dashboard_metriques_workflow"
      nom: "Dashboard Métriques avec Workflow"
      version: "2.2"
      description: "Dashboard métrique enrichi contexte workflow"
      
      schema_parametres_enrichi:
        # Paramètres existants v2.1
        - nom: "metriques_a_afficher"
          type: "array"
          obligatoire: true
          
        # 🆕 Paramètres workflow
        - nom: "metriques_workflow"
          type: "array"
          defaut: ["avancement_global", "retard_planning"]
          description: "Métriques workflow à inclure"
          
        - nom: "comparaison_autres_processus"
          type: "boolean"
          defaut: false
          description: "Comparer avec autres processus du workflow"
```

### 1.3 Résolution du Comportement v2.2 (Enrichie Workflow)
```yaml
resolution_comportement_v22:
  ordre_priorite:
    1. "🆕 Chargement CONTEXTE WORKFLOW depuis workflow parent"
    2. "Chargement COMPORTEMENT DE BASE depuis PROCESSUS objet"
    3. "SI mode=utilise_template: Chargement TEMPLATE + PARAMèTRES + CONTEXTE WORKFLOW"
    4. "SI mode=manuel: Application COMPORTEMENT PARTICULIER classique"
    5. "SI mode=mixte: Fusion TEMPLATE + AJOUTS MANUELS + WORKFLOW"
    6. "Application des SURCHARGES niveau ÉTAT"
    7. "🆕 Application des CONTRAINTES WORKFLOW globales"
    8. "Application des règles CONTEXTUELLES finales"
    
  regles_fusion_v22:
    - "CONTEXTE > CONTRAINTES WORKFLOW > SURCHARGES ÉTAT > TEMPLATE/PARTICULIER > COMPORTEMENT BASE"
    - "Menu hamburger enrichi automatiquement avec options workflow"
    - "Actions fusionnées : WORKFLOW + PROCESSUS + TEMPLATE + ÉTAT"
    - "🆕 Validation contraintes workflow obligatoire"
    - "🆕 Propagation impacts workflow automatique"
```

---

## 🆕 2. WORKFLOW - ORCHESTRATION GLOBALE (Nouveau v2.2)

### 2.1 Structure d'un Workflow Complet
```yaml
workflow_complet:
  # Métadonnées
  id: "fabrication_moteur_bmw_x3_cmd12345"
  nom: "Fabrication Moteur BMW X3 - Commande #12345"
  description: "Fabrication complète moteur pour commande client BMW"
  type: "orchestration_complete"
  version: "1.0"
  
  # Classification
  domaine: "production_automobile"
  criticite: "haute" # haute|moyenne|basse
  complexite: "elevee" # simple|moyenne|elevee
  
  # Processus participants
  processus_inclus:
    processus_primaires:
      - processus_id: "production_bloc_moteur"
        objet_id: "bloc_moteur_001"
        criticite: "critique"
        duree_estimee: 8 # heures
        prerequis: ["matiere_premiere_disponible"]
        
      - processus_id: "usinage_culasse"
        objet_id: "culasse_001"
        criticite: "critique"
        duree_estimee: 6 # heures
        prerequis: ["machine_cnc_disponible"]
        
    processus_secondaires:
      - processus_id: "production_pistons"
        objets_ids: ["piston_001", "piston_002", "piston_003", "piston_004"]
        criticite: "importante"
        duree_estimee: 4 # heures par piston
        parallelisable: true
        
    processus_support:
      - processus_id: "controle_qualite"
        objets_ids: ["tous_composants"]
        criticite: "obligatoire"
        duree_estimee: 2 # heures
        declenche_par: "completion_piece"
  
  # Orchestration détaillée
  orchestration:
    # Règles de dépendances
    dependances:
      synchrones:
        - nom: "Assemblage Possible"
          condition: "ET"
          prerequis: ["production_bloc_moteur.Termine", "usinage_culasse.Termine"]
          declenche: "assemblage_moteur.Autorisation"
          delai_max: 2 # heures entre prérequis et déclenchement
          
        - nom: "Pistons Nécessaires"
          condition: "OU"
          prerequis: ["production_pistons.75_Pourcent_Complete"]
          declenche: "preparation_assemblage_pistons.Demarrage"
          
      asynchrones:
        - nom: "Contrôle Parallèle"
          condition: "CHAQUE"
          prerequis: ["*.Piece_Produite"]
          declenche: "controle_qualite.Nouveau_Controle"
          parallelisme: true
    
    # Jalons clés
    jalons:
      - id: "jalon_pieces_principales"
        nom: "Pièces Principales Terminées"
        description: "Bloc moteur et culasse terminés et validés"
        date_cible: "2025-09-10T18:00:00Z"
        prerequis: ["production_bloc_moteur.Valide", "usinage_culasse.Valide"]
        criticite: "bloquant"
        
      - id: "jalon_assemblage_complet"
        nom: "Assemblage Moteur Complet"
        description: "Moteur assemblé et testé"
        date_cible: "2025-09-12T16:00:00Z"
        prerequis: ["assemblage_moteur.Termine", "test_fonctionnel.Valide"]
        criticite: "livraison"
        
      - id: "jalon_livraison"
        nom: "Livraison Client"
        description: "Moteur emballé et expédié"
        date_cible: "2025-09-15T12:00:00Z"
        prerequis: ["controle_final.Valide", "emballage.Termine"]
        criticite: "contractuel"
    
    # Métriques globales
    metriques_globales:
      avancement:
        processus_termines: 2
        processus_total: 8
        pourcentage_global: 0.65
        
      planning:
        date_debut_reel: "2025-08-15T08:00:00Z"
        date_fin_prevue: "2025-09-15T12:00:00Z"
        retard_actuel: 0 # jours
        jalons_respectes: 1
        jalons_total: 3
        
      qualite:
        pieces_validees: 8
        pieces_rejetees: 1
        taux_qualite: 0.89
        controles_passes: 12
        
      ressources:
        utilisation_machines: 0.75
        disponibilite_operateurs: 0.80
        consommation_matieres: 0.85
  
  # Planning détaillé
  planning:
    temporalite:
      date_creation: "2025-08-10T14:00:00Z"
      date_debut_prevue: "2025-08-15T08:00:00Z"
      date_debut_reelle: "2025-08-15T08:30:00Z" # 30min retard
      date_fin_prevue: "2025-09-15T12:00:00Z"
      date_livraison_client: "2025-09-15T18:00:00Z"
      
    phases:
      - nom: "Phase 1 - Préparation"
        debut: "2025-08-15T08:00:00Z"
        fin: "2025-08-16T18:00:00Z"
        processus: ["preparation_machines", "controle_matieres"]
        
      - nom: "Phase 2 - Production Primaire"
        debut: "2025-08-17T08:00:00Z"
        fin: "2025-08-25T18:00:00Z"
        processus: ["production_bloc_moteur", "usinage_culasse"]
        
      - nom: "Phase 3 - Production Secondaire"
        debut: "2025-08-20T08:00:00Z"
        fin: "2025-08-28T18:00:00Z"
        processus: ["production_pistons"]
        parallelisme: "phase_2"
        
      - nom: "Phase 4 - Assemblage"
        debut: "2025-08-26T08:00:00Z"
        fin: "2025-09-05T18:00:00Z"
        processus: ["assemblage_moteur"]
        
      - nom: "Phase 5 - Contrôle Final"
        debut: "2025-09-06T08:00:00Z"
        fin: "2025-09-12T18:00:00Z"
        processus: ["test_fonctionnel", "controle_final"]
        
      - nom: "Phase 6 - Livraison"
        debut: "2025-09-13T08:00:00Z"
        fin: "2025-09-15T12:00:00Z"
        processus: ["emballage", "expedition"]
  
  # Gestion des ressources
  ressources_globales:
    machines:
      - id: "CNC_001"
        type: "Centre usinage"
        capacite: "24h/jour"
        allocation: ["production_bloc_moteur", "usinage_culasse"]
        disponibilite_actuelle: 0.75
        
      - id: "CNC_002"
        type: "Centre usinage"
        capacite: "24h/jour" 
        allocation: ["production_pistons"]
        disponibilite_actuelle: 0.90
        
      - id: "ASSEMBLAGE_001"
        type: "Poste assemblage"
        capacite: "16h/jour"
        allocation: ["assemblage_moteur"]
        disponibilite_actuelle: 1.0
    
    operateurs:
      - id: "op_001"
        nom: "Jean Dupont"
        competences: ["usinage_cnc", "controle_qualite"]
        allocation: ["production_bloc_moteur"]
        charge_actuelle: 0.80
        
      - id: "op_002"
        nom: "Marie Martin"
        competences: ["usinage_cnc", "assemblage"]
        allocation: ["usinage_culasse", "assemblage_moteur"]
        charge_actuelle: 0.75
    
    matieres_premieres:
      - reference: "ACIER_A_GRADE_X"
        quantite_allouee: 50 # kg
        quantite_consommee: 30 # kg
        stock_restant: 20 # kg
        
      - reference: "ALUMINIUM_B_ALLOY_Y"
        quantite_allouee: 30 # kg
        quantite_consommee: 25 # kg
        stock_restant: 5 # kg
  
  # Contraintes globales
  contraintes_globales:
    qualite:
      taux_defaut_max: 0.02 # 2%
      controles_obligatoires: ["dimensionnel", "fonctionnel"]
      certifications_requises: ["ISO_9001", "IATF_16949"]
      
    planning:
      date_livraison_ferme: "2025-09-15T18:00:00Z"
      penalites_retard: "1000_euros_par_jour"
      avance_max_autorisee: 2 # jours
      
    couts:
      budget_total: 50000 # euros
      cout_actuel: 32000 # euros
      marge_prevue: 0.15 # 15%
      seuil_alerte: 0.90 # 90% budget
      
    securite:
      habilitations_requises: ["conduite_machines", "manipulation_pieces_lourdes"]
      equipements_obligatoires: ["casque", "gants", "chaussures_securite"]
      procedures_urgence: ["arret_urgence", "evacuation"]
  
  # Actions workflow (niveau global)
  actions_workflow:
    - id: "recalculer_planning"
      nom: "Recalculer Planning Global"
      description: "Recalcul automatique planning suite modification processus"
      
      script_javascript: |
        async function recalculerPlanningGlobal(workflow, contexte) {
          // Récupération état tous processus
          const etatProcessus = await getAllProcessusStatus(workflow.processus_inclus);
          
          // Recalcul dépendances
          const nouveauxJalons = await calculerNouveauxJalons(etatProcessus, workflow.dependances);
          
          // Mise à jour planning
          workflow.planning.jalons = nouveauxJalons;
          workflow.metriques_globales.retard_planning = calculerRetard(nouveauxJalons);
          
          // Notifications équipes
          await notifierEquipes("planning_modifie", workflow.id);
          
          return {
            planning_mis_a_jour: true,
            jalons_modifies: nouveauxJalons.length,
            retard_calcule: workflow.metriques_globales.retard_planning
          };
        }
      
      declencheurs:
        - "processus.retard_detecte"
        - "processus.termine_en_avance"
        - "ressource.indisponible"
        
    - id: "reallouer_ressources"
      nom: "Réallouer Ressources"
      description: "Réallocation dynamique ressources selon priorités"
      
      script_javascript: |
        async function reallouerRessources(workflow, ressource_id, contexte) {
          // Analyse priorités processus
          const priorites = calculerPriorites(workflow.processus_inclus);
          
          // Réallocation optimale
          const nouvelleAllocation = optimiserAllocation(workflow.ressources_globales, priorites);
          
          // Application changements
          await appliquerNouvelleAllocation(nouvelleAllocation);
          
          return {
            reallocation_effectuee: true,
            processus_impactes: nouvelleAllocation.processus_impactes
          };
        }
```

### 2.2 Types de Workflows
```yaml
types_workflows:
  # Workflow simple (groupement)
  workflow_simple:
    type: "groupement"
    description: "Regroupement logique sans orchestration complexe"
    caracteristiques:
      - "Pas de dépendances inter-processus"
      - "Métriques agrégées simples"
      - "Planning basique"
      - "Ressources non partagées"
    
    exemple:
      nom: "Formation Opérateurs Q3 2025"
      processus: ["formation_op_001", "formation_op_002", "formation_op_003"]
      orchestration: null
      metriques: "formations_completees / formations_total"
  
  # Workflow complexe (orchestration)
  workflow_complexe:
    type: "orchestration_complete"
    description: "Orchestration complète avec dépendances et ressources partagées"
    caracteristiques:
      - "Dépendances complexes entre processus"
      - "Planning avec jalons critiques"
      - "Gestion ressources partagées"
      - "Contraintes globales"
    
    exemple:
      nom: "Production Série 1000 Pièces Type A"
      processus: ["preparation", "production_lots", "controle", "conditionnement"]
      orchestration: "dependances_complexes + jalons + ressources"
      metriques: "avancement_global + qualite + planning + couts"
  
  # Workflow adaptatif (futur v2.3)
  workflow_adaptatif:
    type: "adaptatif_intelligent"
    description: "Workflow auto-adaptatif selon contexte et performance"
    caracteristiques:
      - "Adaptation automatique dépendances"
      - "Optimisation ressources temps réel"
      - "Apprentissage automatique patterns"
      - "Prédiction et prévention problèmes"
```

---

## 3. COMPORTEMENT DE BASE (NIVEAU PROCESSUS) - Enrichi Workflow v2.2

### 3.1 Structure - Propriété du PROCESSUS Enrichie
```yaml
processus:
  id: "production_bloc_moteur_001"
  nom: "Production Bloc Moteur"
  description: "Processus de production spécifique au bloc moteur BMW"
  workflow_parent_id: "fabrication_moteur_bmw_x3" # 🆕 RÉFÉRENCE workflow
  
  # COMPORTEMENT DE BASE - Commun à TOUS les états de ce processus
  comportement_base:
    action_automatique_consultation:
      actif: true
      description: "Action déclenchée automatiquement avant affichage de l'état"
      execution: "pre_affichage"
      
      script_javascript: |
        async function onConsultationObjet(objet, contexte) {
          const currentTimestamp = Date.now();
          
          // Logique commune à tous les états du processus
          const consultationEvent = {
            timestamp: currentTimestamp,
            utilisateur_id: contexte.utilisateur.id,
            processus_id: this.processus.id,
            etat_id: this.etat.id,
            objet_id: objet.id
          };
          
          // 🆕 Enrichissement contexte workflow
          if (this.processus.workflow_parent_id) {
            const workflowContext = await getWorkflowContext(this.processus.workflow_parent_id);
            consultationEvent.workflow_data = {
              workflow_id: workflowContext.id,
              avancement_global: workflowContext.avancement_global,
              dependances_actives: workflowContext.dependances_actives,
              contraintes_planning: workflowContext.contraintes_planning,
              ressources_partagees: workflowContext.ressources_partagees
            };
          }
          
          // Mise à jour métriques communes au processus
          await this.processus.updateMetriques(consultationEvent);
          
          // 🆕 Mise à jour métriques workflow si applicable
          if (consultationEvent.workflow_data) {
            await updateWorkflowMetriques(consultationEvent.workflow_data.workflow_id, consultationEvent);
          }
          
          // Détermination comportement selon état + workflow
          const comportement = await this.determinerComportement(objet, contexte);
          
          return { 
            comportement_a_afficher: comportement,
            donnees_supplementaires: consultationEvent
          };
        }
      
      logging:
        historique_interne: true
        log_externe: true
        fichier_log: "consultations_processus.log"
        fichier_log_workflow: "consultations_workflow.log" # 🆕
        niveau_detail: "standard"

    interface_standard:
      type: "vue_objet_complete_workflow" # 🆕 Enrichie workflow
      affichage: "obligatoire"
      access_method: "direct"
      
      sections:
        - nom: "Identité Objet"
          contenu: ["id_objet", "nom_objet", "identifiants_gs1"]
          
        - nom: "État Actuel"
          contenu: ["nom_etat", "date_entree_etat", "duree_dans_etat"]
          
        - nom: "Processus"
          contenu: ["nom_processus", "etape_actuelle", "progression_processus"]
          
        # 🆕 NOUVELLE SECTION WORKFLOW
        - nom: "Contexte Workflow"
          contenu: ["nom_workflow", "avancement_global", "dependances_actives"]
          visible_si: "workflow_parent_id != null"
          format: "panneau_workflow"
          mise_a_jour: "temps_reel"
          
        - nom: "Propriétés Statiques"
          contenu: "toutes_proprietes_statiques"
          format: "tableau_cle_valeur"
          
        - nom: "Propriétés Dynamiques"
          contenu: "toutes_proprietes_dynamiques"
          format: "tableau_cle_valeur_horodatage"
          mise_a_jour: "temps_reel"
          
        - nom: "Actions Disponibles"
          contenu: "actions_workflow + actions_processus + actions_etat + actions_template" # 🆕 Workflow en premier
          format: "boutons_action"

    menu_hamburger:
      access_method: "hamburger_menu"
      parametrage_visibilite: true
      
      options_disponibles:
        # Options existantes v2.1
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
          
        # 🆕 NOUVELLES OPTIONS WORKFLOW
        - nom: "Vue Workflow Global"
          id: "vue_workflow_global"
          action: "afficher_workflow_complet"
          visible_si: "workflow_parent_id != null"
          visible_par_defaut: true
          parametrable: true
          description: "Vue d'ensemble du workflow complet"
          icone: "🏗️"
          
        - nom: "Planning Projet"
          id: "planning_projet"
          action: "afficher_planning_workflow"
          visible_si: "workflow.planning != null"
          visible_par_defaut: false
          parametrable: true
          description: "Jalons et planning du projet"
          icone: "📅"
          
        - nom: "Dépendances"
          id: "dependances_workflow"
          action: "afficher_dependances_workflow"
          visible_si: "workflow.dependances.length > 0"
          visible_par_defaut: false
          parametrable: true
          description: "Dépendances avec autres processus"
          icone: "🔗"
          
        - nom: "Autres Processus"
          id: "autres_processus"
          action: "afficher_autres_processus_workflow"
          visible_si: "workflow.processus_inclus.length > 1"
          visible_par_defaut: false
          parametrable: true
          description: "États autres processus du workflow"
          icone: "📊"
          
        # Options existantes v2.1 (inchangées)
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
      # Règles existantes v2.1
      - condition: "utilisateur.roles.contains('operateur')"
        modifications:
          - masquer: ["donnees_techniques"]
          - mettre_en_avant: ["actions.controler_qualite"]
          
      - condition: "processus.mode_maintenance == true"
        modifications:
          - afficher_alerte: "Processus en mode maintenance"
          - options_supplementaires: ["logs_systeme", "debug_processus"]
      
      # 🆕 NOUVELLES RÈGLES WORKFLOW
      - condition: "workflow.retard_planning > 0"
        modifications:
          - afficher_alerte: "Retard planning détecté - Actions prioritaires requises"
          - mettre_en_avant: ["actions.accelerer_processus"]
          - couleur_theme: "orange_alerte"
          
      - condition: "workflow.dependances_bloquantes.length > 0"
        modifications:
          - afficher_alerte: "Dépendances bloquantes actives"
          - afficher_section: "dependances_bloquantes"
          - actions_supplementaires: ["contacter_equipes_dependantes"]
          
      - condition: "workflow.ressources_critiques == true"
        modifications:
          - afficher_alerte: "Ressources critiques - Optimisation requise"
          - mettre_en_avant: ["planning_projet", "autres_processus"]
          - actions_supplementaires: ["demander_ressources_supplementaires"]
  
  # 🆕 Contexte workflow intégré
  contexte_workflow:
    source_donnees: "workflow_parent_id"
    
    donnees_exposees:
      avancement_global: "Pourcentage avancement workflow complet"
      dependances_actives: "Dépendances en attente pour ce processus"
      contraintes_planning: "Contraintes de planning héritées du workflow"
      ressources_partagees: "Machines et opérateurs disponibles"
      autres_processus: "État des autres processus du workflow"
      jalons_critiques: "Jalons workflow impactant ce processus"
      
    regles_affichage:
      - condition: "workflow.type == 'groupement'"
        affichage: "simplifie" # Métriques simples uniquement
        
      - condition: "workflow.type == 'orchestration_complete'"
        affichage: "complet" # Toutes les informations workflow
        
      - condition: "utilisateur.role == 'operateur'"
        affichage: "operationnel" # Focus sur tâches immédiates
        
      - condition: "utilisateur.role == 'chef_projet'"
        affichage: "gestion" # Focus sur planning et ressources
```

---

## 4. COMPORTEMENT PARTICULIER v2.2 (NIVEAU ÉTAT) - Enrichi Workflow

### 4.1 Mode Template avec Workflow - Nouveau v2.2
```yaml
etat:
  id: "etat_usinage_bloc_moteur"
  nom: "En_Usinage"
  processus_parent_id: "production_bloc_moteur"
  
  # COMPORTEMENT PARTICULIER v2.2 - Mode Template enrichi Workflow
  comportement_particulier:
    actif: true
    mode: "utilise_template" # 🆕 ENRICHI avec contexte workflow
    
    # Référence au template + paramètres enrichis workflow
    template_utilise:
      template_id: "dashboard_metriques"
      template_version: "2.2" # Version enrichie workflow
      
      # Paramètres spécifiques à cet état
      parametres:
        metriques_a_afficher: ["temperature", "vitesse_broche", "avancement_piece"]
        format_visualisation: "gauges"
        seuils_alerte: {"temperature": {"max": 80}, "vitesse_broche": {"max": 3000}}
        couleur_theme: "#1976D2" # Bleu production
        mise_a_jour_frequence: 3000 # 3 secondes
        
        # 🆕 PARAMÈTRES WORKFLOW INTÉGRÉS
        afficher_contexte_workflow: true
        metriques_workflow: ["avancement_global", "retard_planning", "dependances_actives"]
        comparaison_autres_processus: true
        planning_integre: true
        alertes_workflow_actives: true
      
      # Surcharges optionnelles (priorité sur template)
      surcharges:
        sections_supplementaires:
          # 🆕 SECTIONS WORKFLOW
          - nom: "Impact Workflow"
            contenu: "afficher_impact_usinage_sur_workflow"
            format: "panneau_impact"
            position: "before_dashboard"
            visible_si: "workflow_parent_id != null"
            donnees: ["contribution_planning", "dependances_declenchees", "ressources_optimisees"]
          
          - nom: "Coordination Équipes"
            contenu: "afficher_coordination_autres_processus"
            format: "liste_processus_paralleles"
            position: "after_dashboard"
            visible_si: "workflow.processus_paralleles.length > 0"
          
          # Sections existantes v2.1
          - nom: "Actions Spéciales Usinage"
            contenu: "actions_etat_specifiques"
            position: "after_workflow"
        
        menu_hamburger:
          options_supplementaires:
            # 🆕 OPTIONS WORKFLOW
            - nom: "Synchronisation Workflow"
              action: "synchroniser_avec_workflow"
              icone: "🔄"
              description: "Synchroniser état avec workflow global"
              
            - nom: "Impact Planning"
              action: "analyser_impact_planning"
              icone: "⏱️"
              description: "Analyser impact sur planning global"
              
            # Options existantes v2.1
            - nom: "Guide Usinage"
              action: "afficher_guide_usinage"
              icone: "📖"
        
        # 🆕 Règles contextuelles workflow spécifiques
        regles_contexte_workflow:
          - condition: "workflow.retard_planning > 2" # Plus de 2 jours de retard
            modifications:
              - afficher_alerte: "URGENT - Retard workflow critique"
              - augmenter_frequence_maj: 1000 # 1 seconde
              - mettre_en_avant: ["vitesse_broche", "avancement_piece"]
              - actions_prioritaires: ["accelerer_usinage", "notifier_chef_projet"]
              
          - condition: "workflow.dependances_bloquantes.includes('assemblage_moteur')"
            modifications:
              - afficher_alerte: "Assemblage en attente - Finaliser usinage rapidement"
              - couleur_theme: "#FF9800" # Orange urgence
              - ajouter_metriques: ["temps_restant_estime", "qualite_piece_actuelle"]
    
    # 🆕 Actions supplémentaires enrichies workflow
    actions_supplementaires:
      - id: "terminer_usinage_avec_workflow"
        nom: "Terminer Usinage"
        description: "Terminer usinage et déclencher actions workflow"
        visible_si: "piece_usinage_complete == true"
        
        # 🆕 Prérequis workflow
        prerequis_workflow:
          workflow_id: "fabrication_moteur_bmw_x3"
          conditions:
            - "qualite_usinage_validee == true"
            - "tolerances_respectees == true"
            - "machine_arretee_proprement == true"
          contraintes_workflow:
            - "pas_de_conflit_ressources"
            - "dependances_satisfaites"
        
        script_javascript: |
          async function terminerUsinageAvecWorkflow(objet, contexte) {
            const timestamp = Date.now();
            
            // 1. Vérifications processus standard
            const usinageComplete = await verifierUsinageComplete(objet);
            if (!usinageComplete.valide) {
              throw new Error(`Usinage incomplet: ${usinageComplete.raisons.join(', ')}`);
            }
            
            // 🆕 2. Vérifications contraintes workflow
            if (contexte.workflow_parent_id) {
              const workflowConstraints = await verifierContraintesWorkflow(
                contexte.workflow_parent_id, 
                this.processus.id,
                "terminer_usinage"
              );
              
              if (!workflowConstraints.respectees) {
                throw new Error(`Contraintes workflow non respectées: ${workflowConstraints.violations.join(', ')}`);
              }
            }
            
            // 3. Finalisation usinage
            objet.proprietesDynamiques.usinage_termine = true;
            objet.proprietesDynamiques.timestamp_fin_usinage = timestamp;
            objet.proprietesDynamiques.operateur_finition = contexte.utilisateur.id;
            
            // 🆕 4. Mise à jour workflow
            if (contexte.workflow_parent_id) {
              const workflowUpdate = {
                processus_id: this.processus.id,
                action: "usinage_termine",
                timestamp: timestamp,
                qualite_validee: usinageComplete.qualite,
                donnees_techniques: usinageComplete.mesures
              };
              
              // Propagation au workflow
              const workflowResult = await updateWorkflowProgress(
                contexte.workflow_parent_id,
                workflowUpdate
              );
              
              // 🆕 5. Déclenchement dépendances automatiques
              if (workflowResult.dependances_declenchees.length > 0) {
                await declencherDependancesWorkflow(workflowResult.dependances_declenchees);
                
                // Notification équipes concernées
                await notifierEquipesDependances(workflowResult.dependances_declenchees);
              }
              
              // 🆕 6. Mise à jour métriques globales
              await updateMetriquesGlobalesWorkflow(contexte.workflow_parent_id);
            }
            
            // 7. Logging enrichi workflow
            const logData = {
              action: "terminer_usinage_avec_workflow",
              timestamp: timestamp,
              objet_id: objet.id,
              processus_id: this.processus.id,
              workflow_id: contexte.workflow_parent_id,
              utilisateur: contexte.utilisateur.id,
              donnees_usinage: usinageComplete,
              workflow_impact: workflowResult || null
            };
            
            // Historique interne
            objet.historique.add("fin_usinage_workflow", logData);
            
            // 🆕 Log workflow spécialisé
            if (contexte.workflow_parent_id) {
              await logToFile("workflow_progress.log", logData);
            }
            
            return {
              nouvel_etat: "Controle_Qualite",
              donnees_maj: objet.proprietesDynamiques,
              message: "Usinage terminé - Workflow mis à jour",
              workflow_impact: {
                dependances_declenchees: workflowResult?.dependances_declenchees || [],
                metriques_mises_a_jour: workflowResult?.metriques || {},
                processus_impactes: workflowResult?.processus_impactes || []
              }
            };
          }
        
        # Interface saisie enrichie workflow
        interface_saisie:
          formulaire:
            - nom: "qualite_finale"
              type: "select"
              obligatoire: true
              options: ["Conforme", "Conforme avec réserves", "Non conforme"]
              
            - nom: "mesures_finales"
              type: "object"
              obligatoire: true
              champs:
                - "tolerance_dimensionnelle"
                - "etat_surface"
                - "durete_materiau"
                
            # 🆕 Informations workflow
            - nom: "impact_planning"
              type: "textarea"
              obligatoire: false
              max_chars: 200
              placeholder: "Impact sur planning workflow (optionnel)..."
              visible_si: "workflow_parent_id != null"
        
        # Transition vers contrôle qualité
        transition: "Controle_Qualite"
        
        # 🆕 Impact workflow automatique
        workflow_actions:
          - trigger: "dependances_assemblage"
            condition: "usinage_termine && qualite_conforme"
            action: "autoriser_assemblage_moteur"
            
          - trigger: "update_planning"
            condition: "always"
            action: "recalculer_planning_global"
        
        # Logging spécialisé workflow
        logging:
          historique_interne: true
          log_externe: true
          fichier_log: "fin_usinage.log"
          fichier_log_workflow: "workflow_progress.log" # 🆕
          niveau_detail: "detaille"
          donnees_supplementaires: ["donnees_usinage", "workflow_impact"]
```

### 4.2 Mode Mixte avec Workflow - Enrichi v2.2
```yaml
etat:
  id: "etat_controle_qualite_mixte"
  nom: "Controle_Qualite"
  processus_parent_id: "production_bloc_moteur"
  
  # COMPORTEMENT PARTICULIER v2.2 - Mode Mixte enrichi Workflow
  comportement_particulier:
    actif: true
    mode: "mixte" # 🆕 Mode hybride template + manuel + workflow
    
    # Base template pour structure commune
    base_template:
      template_id: "affichage_checklist"
      parametres:
        document_source: "checklist_controle_qualite_v3.yaml"
        sections_a_afficher: [4, 5] # Contrôle dimensionnel + Contrôle surface
        titre_personnalise: "Contrôle Qualité Bloc Moteur"
        format_affichage: "tableau"
        actions_par_element: ["valider", "rejeter", "reporter", "commenter"]
        couleur_theme: "#4CAF50" # Vert contrôle
        
        # 🆕 Paramètres workflow intégrés
        afficher_contexte_workflow: true
        dependances_workflow_visibles: true
        planning_integre: true
        impact_autres_processus: true
    
    # Ajouts manuels spécifiques enrichis workflow
    ajouts_manuels:
      sections_supplementaires:
        # 🆕 SECTION WORKFLOW DÉDIÉE
        - nom: "Impact Contrôle sur Workflow"
          contenu: "analyser_impact_controle_workflow"
          format: "dashboard_impact"
          position: "before_checklist"
          donnees: ["processus_en_attente", "delai_impact", "alternatives_possibles"]
          
        - nom: "Coordination Assemblage"
          contenu: "etat_preparation_assemblage"
          format: "statut_dependances"
          position: "sidebar_right"
          visible_si: "workflow.dependances.assemblage_moteur.active"
          
        # Sections manuelles existantes
        - nom: "Mesures Précises"
          contenu: "interface_mesures_manuelles"
          format: "formulaire_mesures"
          position: "after_checklist"
          
        - nom: "Décision Qualité"
          contenu: "actions_decision_qualite_finale"
          format: "boutons_decision"
          position: "bottom"
          visible_si: "checklist_controle_complete"
      
      # 🆕 Actions workflow intégrées
      actions_workflow_supplementaires:
        - id: "expedier_assemblage_si_conforme"
          nom: "Expédier vers Assemblage"
          description: "Validation et expédition immédiate vers assemblage"
          visible_si: "controle_conforme && assemblage_disponible"
          
          prerequis_workflow:
            workflow_id: "fabrication_moteur_bmw_x3"
            conditions:
              - "assemblage_moteur.etat == 'Attente_Pieces'"
              - "culasse.controle_qualite == 'Conforme'"
            delai_max: 30 # minutes
          
          script_javascript: |
            async function expedierAssemblageConform(objet, contexte) {
              // Vérification contrôle qualité complet
              const controleResult = await verifierControleQualiteComplet(objet);
              
              // 🆕 Vérification synchronisation workflow
              const workflowSync = await verifierSynchronisationWorkflow(
                contexte.workflow_parent_id,
                ["assemblage_moteur"]
              );
              
              if (controleResult.conforme && workflowSync.pret) {
                // Marquage pièce prête
                objet.proprietesDynamiques.pret_assemblage = true;
                objet.proprietesDynamiques.timestamp_validation = Date.now();
                
                // 🆕 Déclenchement automatique assemblage
                await declencherProcessusWorkflow(
                  contexte.workflow_parent_id,
                  "assemblage_moteur",
                  "demarrer_assemblage_bloc_culasse"
                );
                
                return {
                  nouvel_etat: "Pret_Assemblage",
                  workflow_action: "assemblage_declenche",
                  message: "Pièce validée et assemblage démarré automatiquement"
                };
              }
              
              throw new Error("Conditions assemblage non réunies");
            }
        
        - id: "reporter_avec_impact_workflow"
          nom: "Reporter avec Analyse Impact"
          description: "Report avec analyse complète impact workflow"
          
          interface_saisie:
            formulaire:
              - nom: "motif_report_detaille"
                type: "select"
                obligatoire: true
                options: ["Défaut dimensionnel", "Défaut surface", "Outillage défaillant", "Formation requise"]
                
              - nom: "duree_correction_estimee"
                type: "number"
                obligatoire: true
                unite: "heures"
                min: 0.5
                max: 48
                
              # 🆕 Analyse impact workflow
              - nom: "impact_workflow_analyse"
                type: "readonly"
                contenu: "calculer_impact_report_sur_workflow"
                description: "Impact calculé automatiquement sur workflow"
                
              - nom: "alternatives_proposees"
                type: "checkbox_multiple"
                obligatoire: false
                options_dynamiques: "generer_alternatives_workflow"
                description: "Alternatives pour minimiser impact workflow"
          
          script_javascript: |
            async function reporterAvecImpactWorkflow(objet, parametres, contexte) {
              const timestamp = Date.now();
              
              // 🆕 Calcul impact workflow détaillé
              const impactAnalysis = await analyserImpactReportWorkflow(
                contexte.workflow_parent_id,
                parametres.duree_correction_estimee,
                parametres.motif_report_detaille
              );
              
              // Sauvegarde report avec analyse
              objet.proprietesDynamiques.controle_reporte = {
                timestamp: timestamp,
                motif: parametres.motif_report_detaille,
                duree_estimee: parametres.duree_correction_estimee,
                operateur: contexte.utilisateur.id,
                impact_workflow: impactAnalysis
              };
              
              // 🆕 Recalcul planning workflow
              if (impactAnalysis.impact_critique) {
                await recalculerPlanningWorkflow(
                  contexte.workflow_parent_id,
                  impactAnalysis.nouveau_planning
                );
                
                // Notification équipes impactées
                await notifierEquipesImpactPlanifng(impactAnalysis.equipes_concernees);
              }
              
              return {
                nouvel_etat: "Controle_Reporte",
                workflow_impact: impactAnalysis,
                message: `Contrôle reporté - Impact workflow: ${impactAnalysis.niveau}`
              };
            }
```

---

## 5. ACTIONS (RÉPARTITION WORKFLOW/PROCESSUS/TEMPLATE/ÉTAT) - v2.2

### 5.1 🆕 Actions au Niveau WORKFLOW (Nouveau v2.2)
```yaml
workflow:
  actions_orchestration:
    # Actions globales de coordination
    - id: "synchroniser_processus"
      nom: "Synchroniser Processus"
      description: "Synchronisation manuelle entre processus du workflow"
      disponible_dans: "tous_processus_workflow"
      niveau: "coordination"
      
      interface_saisie:
        formulaire:
          - nom: "processus_a_synchroniser"
            type: "checkbox_multiple"
            options_dynamiques: "lister_processus_workflow"
            obligatoire: true
            
          - nom: "mode_synchronisation"
            type: "select"
            options: ["attendre_tous", "declencher_suivants", "forcer_alignement"]
            defaut: "attendre_tous"
      
      script_javascript: |
        async function synchroniserProcessus(workflow, parametres, contexte) {
          const timestamp = Date.now();
          
          // Récupération état tous processus sélectionnés
          const etatProcessus = await getEtatProcessusMultiples(parametres.processus_a_synchroniser);
          
          // Application mode synchronisation
          let resultats = [];
          switch (parametres.mode_synchronisation) {
            case "attendre_tous":
              resultats = await attendreTousProcessus(etatProcessus);
              break;
            case "declencher_suivants":
              resultats = await declencherProcessusSuivants(etatProcessus, workflow.dependances);
              break;
            case "forcer_alignement":
              resultats = await forcerAlignementProcessus(etatProcessus);
              break;
          }
          
          // Mise à jour métriques workflow
          await updateMetriquesWorkflow(workflow.id, resultats);
          
          return {
            processus_synchronises: resultats.length,
            dependances_mises_a_jour: resultats.filter(r => r.dependances_impactees).length,
            message: `Synchronisation ${parametres.mode_synchronisation} effectuée`
          };
        }
      
      logging:
        historique_interne: false # Action workflow = pas d'historique objet
        log_externe: true
        fichier_log: "workflow_synchronisation.log"
        niveau_detail: "detaille"
    
    - id: "recalculer_planning_global"
      nom: "Recalculer Planning Global"
      description: "Recalcul complet planning workflow avec nouvelles contraintes"
      disponible_dans: "tous_processus_workflow"
      niveau: "gestion_projet"
      
      prerequis:
        - "utilisateur.permissions.includes('gestion_planning')"
        - "workflow.type == 'orchestration_complete'"
      
      script_javascript: |
        async function recalculerPlanningGlobal(workflow, contexte) {
          // Récupération état actuel tous processus
          const etatGlobal = await getEtatGlobalWorkflow(workflow.id);
          
          // Recalcul optimisé planning
          const nouveauPlanning = await optimiserPlanningWorkflow(
            workflow.processus_inclus,
            workflow.contraintes_globales,
            etatGlobal
          );
          
          // Application nouveau planning
          workflow.planning = nouveauPlanning.planning;
          workflow.metriques_globales.retard_planning = nouveauPlanning.retard_calcule;
          
          // Notification équipes des changements
          if (nouveauPlanning.changements_significatifs) {
            await notifierChangementsPlannings(nouveauPlanning.changements);
          }
          
          return {
            planning_mis_a_jour: true,
            jalons_modifies: nouveauPlanning.jalons_modifies,
            retard_global: nouveauPlanning.retard_calcule,
            equipes_notifiees: nouveauPlanning.changements_significatifs
          };
        }
    
    - id: "reallouer_ressources_globales"
      nom: "Réallouer Ressources Globales"
      description: "Réallocation optimale des ressources partagées entre processus"
      disponible_dans: "tous_processus_workflow"
      niveau: "gestion_ressources"
      
      interface_saisie:
        formulaire:
          - nom: "critere_optimisation"
            type: "select"
            options: ["minimiser_duree", "maximiser_qualite", "equilibrer_charge", "respecter_priorites"]
            defaut: "respecter_priorites"
            
          - nom: "ressources_concernees"
            type: "checkbox_multiple"
            options_dynamiques: "lister_ressources_workflow"
            defaut: "toutes"
      
      script_javascript: |
        async function reallouerRessourcesGlobales(workflow, parametres, contexte) {
          // Analyse utilisation actuelle ressources
          const utilisationActuelle = await analyserUtilisationRessources(workflow.ressources_globales);
          
          // Calcul allocation optimale
          const nouvelleAllocation = await calculerAllocationOptimale(
            workflow.processus_inclus,
            workflow.ressources_globales,
            parametres.critere_optimisation,
            parametres.ressources_concernees
          );
          
          // Application nouvelle allocation
          await appliquerNouvelleAllocation(nouvelleAllocation);
          
          // Mise à jour planning si impact significatif
          if (nouvelleAllocation.impact_planning) {
            await recalculerPlanningGlobal(workflow, contexte);
          }
          
          return {
            reallocation_effectuee: true,
            ressources_reallouees: nouvelleAllocation.ressources_modifiees,
            processus_impactes: nouvelleAllocation.processus_impactes,
            impact_planning: nouvelleAllocation.impact_planning
          };
        }
```

### 5.2 Actions au Niveau PROCESSUS (Enrichies Workflow v2.2)
```yaml
processus:
  actions_communes:
    # Actions existantes v2.1 enrichies
    - id: "consulter_processus_avec_workflow"
      nom: "Consulter Processus"
      description: "Action de consultation enrichie contexte workflow"
      disponible_dans: "tous_etats"
      
      script_javascript: |
        async function consulterProcessusAvecWorkflow(objet, contexte) {
          // Logique commune processus
          const donneesProcessus = this.processus.getDonnees(objet);
          
          // 🆕 Enrichissement workflow
          let donneesWorkflow = null;
          if (contexte.workflow_parent_id) {
            donneesWorkflow = await getWorkflowContext(contexte.workflow_parent_id);
            
            // Calcul impact processus sur workflow
            const impactWorkflow = await calculerImpactProcessusSurWorkflow(
              contexte.workflow_parent_id,
              this.processus.id
            );
            
            donneesWorkflow.impact_processus = impactWorkflow;
          }
          
          return {
            nouvel_etat: contexte.etat_actuel, // Pas de changement d'état
            donnees_exposees: {
              processus: donneesProcessus,
              workflow: donneesWorkflow
            }
          };
        }
      
      logging:
        historique_interne: false # Consultation = pas d'historique
        log_externe: true
        fichier_log: "consultations_processus.log"
        fichier_log_workflow: "consultations_workflow.log" # 🆕
    
    # 🆕 NOUVELLES ACTIONS WORKFLOW AU NIVEAU PROCESSUS
    - id: "signaler_blocage_workflow"
      nom: "Signaler Blocage Workflow"
      description: "Signaler un blocage impactant le workflow global"
      disponible_dans: "tous_etats"
      
      interface_saisie:
        formulaire:
          - nom: "type_blocage"
            type: "select"
            obligatoire: true
            options: ["Ressource indisponible", "Défaut qualité", "Panne équipement", "Formation manquante", "Approvisionnement"]
            
          - nom: "duree_estimee_blocage"
            type: "number"
            obligatoire: true
            unite: "heures"
            min: 0.5
            max: 168 # 1 semaine max
            
          - nom: "impact_autres_processus"
            type: "checkbox_multiple"
            options_dynamiques: "lister_processus_dependants"
            description: "Processus impactés par ce blocage"
            
          - nom: "actions_correctives_proposees"
            type: "textarea"
            obligatoire: false
            max_chars: 500
      
      script_javascript: |
        async function signalerBlocageWorkflow(objet, parametres, contexte) {
          const timestamp = Date.now();
          
          // Enregistrement blocage au niveau processus
          objet.proprietesDynamiques.blocage_actif = {
            timestamp: timestamp,
            type: parametres.type_blocage,
            duree_estimee: parametres.duree_estimee_blocage,
            operateur: contexte.utilisateur.id,
            actions_correctives: parametres.actions_correctives_proposees
          };
          
          // 🆕 Propagation au workflow
          if (contexte.workflow_parent_id) {
            const impactWorkflow = await analyserImpactBlocageSurWorkflow(
              contexte.workflow_parent_id,
              this.processus.id,
              parametres
            );
            
            // Mise à jour métriques workflow
            await updateBlocageWorkflow(contexte.workflow_parent_id, impactWorkflow);
            
            // Recalcul planning si impact critique
            if (impactWorkflow.impact_critique) {
              await recalculerPlanningWorkflow(contexte.workflow_parent_id);
            }
            
            // Notification équipes concernées
            await notifierBlocageWorkflow(impactWorkflow);
          }
          
          return {
            nouvel_etat: "Processus_Bloque",
            message: `Blocage signalé: ${parametres.type_blocage}`,
            workflow_impact: impactWorkflow || null
          };
        }
      
      transition: "Processus_Bloque"
      
      logging:
        historique_interne: true
        log_externe: true
        fichier_log: "blocages_processus.log"
        fichier_log_workflow: "blocages_workflow.log" # 🆕
        niveau_detail: "detaille"
```

### 5.3 🆕 Actions Template Enrichies Workflow v2.2
```yaml
# Actions générées automatiquement par les templates enrichies workflow
actions_templates_v22:
  # Du template "affichage_checklist" enrichi workflow
  checklist_workflow:
    - id: "valider_element_avec_impact_workflow"
      nom: "Valider Élément (Impact Workflow)"
      source: "template_affichage_checklist_workflow"
      version: "2.2"
      
      script_javascript: |
        async function validerElementAvecImpactWorkflow(objet, element_id, contexte) {
          const timestamp = Date.now();
          
          // Validation élément standard
          const validationResult = await validerElementStandard(objet, element_id, contexte);
          
          // 🆕 Analyse impact sur workflow
          if (contexte.workflow_parent_id && validationResult.succes) {
            const progressionChecklist = await getProgressionChecklist(objet);
            
            // Vérification si validation déclenche actions workflow
            const actionsWorkflow = await verifierActionsWorkflowDeclenchees(
              contexte.workflow_parent_id,
              progressionChecklist
            );
            
            if (actionsWorkflow.length > 0) {
              // Exécution actions workflow automatiques
              for (const action of actionsWorkflow) {
                await executerActionWorkflow(action);
              }
              
              validationResult.workflow_actions = actionsWorkflow;
            }
          }
          
          return validationResult;
        }
      
      parametre: true # Action paramétrée par l'état
      workflow_integration: true # 🆕 Intégration workflow native
      
    - id: "completer_checklist_avec_workflow"
      nom: "Compléter Checklist (Workflow)"
      source: "template_affichage_checklist_workflow"
      description: "Compléter checklist et déclencher actions workflow"
      
      prerequis_template:
        progression_minimale: 1.0 # 100% checklist
        elements_obligatoires: "tous_valides"
        
      prerequis_workflow:
        dependances_satisfaites: true
        ressources_disponibles: true
        
      script_javascript: |
        async function completerChecklistAvecWorkflow(objet, contexte) {
          // Vérification checklist complète
          const checklistComplete = await verifierChecklistComplete(objet);
          if (!checklistComplete.valide) {
            throw new Error("Checklist incomplète");
          }
          
          // 🆕 Déclenchement automatique processus suivants workflow
          if (contexte.workflow_parent_id) {
            const processusSuivants = await identifierProcessusSuivants(
              contexte.workflow_parent_id,
              this.processus.id
            );
            
            for (const processus of processusSuivants) {
              await declencherProcessusWorkflow(processus.id);
            }
          }
          
          return {
            checklist_complete: true,
            processus_suivants_declenches: processusSuivants?.length || 0
          };
        }
  
  # Du template "dashboard_metriques" enrichi workflow
  dashboard_workflow:
    - id: "actualiser_metriques_avec_workflow"
      nom: "Actualiser Métriques (Workflow)"
      source: "template_dashboard_metriques_workflow"
      
      script_javascript: |
        async function actualiserMetriquesAvecWorkflow(objet, contexte) {
          // Actualisation métriques objet standard
          const metriquesObjet = await actualiserMetriquesObjet(objet);
          
          // 🆕 Enrichissement métriques workflow
          let metriquesWorkflow = null;
          if (contexte.workflow_parent_id) {
            metriquesWorkflow = await getMetriquesWorkflow(contexte.workflow_parent_id);
            
            // Calcul contribution objet aux métriques globales
            const contribution = await calculerContributionWorkflow(
              objet,
              metriquesWorkflow
            );
            
            metriquesWorkflow.contribution_objet = contribution;
          }
          
          return {
            metriques_objet: metriquesObjet,
            metriques_workflow: metriquesWorkflow,
            timestamp_maj: Date.now()
          };
        }
```

### 5.4 Actions au Niveau ÉTAT (Enrichies Workflow v2.2)
```yaml
etat:
  actions_specifiques:
    # Actions existantes enrichies workflow
    - id: "demarrer_production_avec_workflow"
      nom: "Démarrer Production"
      description: "Démarrage spécifique avec coordination workflow"
      disponible_dans: "etat_preparation_production_uniquement"
      
      # Prérequis template inchangés v2.1
      prerequis_template:
        template_utilise: "affichage_checklist"
        condition: "checklist_complete == true"
        message_erreur: "Checklist de préparation incomplète"
      
      # 🆕 Prérequis workflow
      prerequis_workflow:
        workflow_id: "fabrication_moteur_bmw_x3"
        conditions:
          - "ressources.machine_cnc.disponible == true"
          - "matieres_premieres.stock_suffisant == true"
          - "operateur.habilite_production == true"
        dependances:
          - "aucun_processus_bloquant_actif"
        contraintes_planning:
          - "pas_de_retard_critique"
          - "jalons_respectables"
      
      script_javascript: |
        async function demarrerProductionAvecWorkflow(objet, contexte) {
          // Vérifications standard v2.1
          const templateResult = await verifierPrerequisTemplate(objet, contexte);
          if (!templateResult.valide) {
            throw new Error(templateResult.erreur);
          }
          
          // 🆕 Vérifications workflow
          if (contexte.workflow_parent_id) {
            const workflowChecks = await verifierPrerequisWorkflow(
              contexte.workflow_parent_id,
              this.processus.id,
              "demarrer_production"
            );
            
            if (!workflowChecks.valide) {
              throw new Error(`Contraintes workflow: ${workflowChecks.violations.join(', ')}`);
            }
            
            // Réservation ressources workflow
            await reserverRessourcesWorkflow(
              contexte.workflow_parent_id,
              this.processus.id,
              workflowChecks.ressources_requises
            );
          }
          
          // Démarrage production
          objet.proprietesDynamiques.production_demarree = true;
          objet.proprietesDynamiques.timestamp_demarrage = Date.now();
          objet.proprietesDynamiques.operateur_demarrage = contexte.utilisateur.id;
          
          // 🆕 Mise à jour workflow
          if (contexte.workflow_parent_id) {
            await updateProgressionWorkflow(
              contexte.workflow_parent_id,
              this.processus.id,
              "production_demarree"
            );
            
            // Notification processus dépendants
            await notifierProcessusDependants(
              contexte.workflow_parent_id,
              this.processus.id,
              "production_demarree"
            );
          }
          
          return {
            nouvel_etat: "En_Production",
            message: "Production démarrée avec coordination workflow",
            workflow_coordonne: contexte.workflow_parent_id != null
          };
        }
      
      transition:
        type: "conditionnel"
        conditions:
          - si: "template_valide && workflow_valide"
            nouvel_etat: "En_Production"
          - defaut: "Preparation_Production"
      
      # 🆕 Actions workflow déclenchées automatiquement
      workflow_triggers:
        - condition: "production_demarree"
          action: "update_metriques_globales"
          
        - condition: "production_demarree && processus_critique"
          action: "notifier_chef_projet"
          
        - condition: "production_demarree"
          action: "verifier_dependances_suivantes"
```

---

## 6. RÉSOLUTION ET FUSION DES COMPORTEMENTS v2.2 (Enrichie Workflow)

### 6.1 Algorithme de Résolution Enrichi Workflow
```yaml
algorithme_resolution_v22:
  etape_0_chargement_workflow:
    source: "WORKFLOW.contexte_global" # 🆕 NOUVELLE ÉTAPE
    elements_charges:
      - orchestration_globale
      - dependances_inter_processus
      - contraintes_planning
      - ressources_partagees
      - metriques_globales
      - jalons_critiques
  
  etape_1_chargement_processus:
    source: "PROCESSUS.comportement_base" # Niveau objet
    elements_charges:
      - interface_standard
      - menu_hamburger
      - actions_communes
      - regles_contexte_objet
      
    # 🆕 Enrichissement workflow
    enrichissement_workflow:
      - contexte_workflow_integre
      - contraintes_heritees
      - dependances_calculees
      - ressources_disponibles
  
  etape_2_verification_etat:
    condition_1: "ETAT.comportement_particulier.mode == 'utilise_template'"
    si_template:
      actions:
        - charger: "TEMPLATE depuis bibliothèque selon template_id"
        - valider: "parametres ETAT vs schema_parametres TEMPLATE"
        - generer: "interface dynamique avec parametres ETAT"
        - enrichir: "🆕 contexte workflow dans template"
        - appliquer: "surcharges ETAT sur résultat template"
        - enrichir: "menu_hamburger avec vue_template + vue_workflow"
    
    condition_2: "ETAT.comportement_particulier.mode == 'manuel'"
    si_manuel:
      actions:
        - charger: "ETAT.comportement_particulier.interface_specifique"
        - enrichir: "🆕 intégration workflow native"
        - enrichir: "menu_hamburger avec vue_standard + vue_workflow"
    
    condition_3: "ETAT.comportement_particulier.mode == 'mixte'"
    si_mixte:
      actions:
        - charger: "TEMPLATE base selon base_template"
        - generer: "interface template avec parametres"
        - enrichir: "🆕 contexte workflow dans template"
        - fusionner: "avec ajouts_manuels ETAT + workflow"
        - enrichir: "menu_hamburger avec vue_standard + vue_template + vue_workflow"
  
  etape_3_fusion_actions:
    sources_actions:
      1. "🆕 WORKFLOW.actions_orchestration (coordination globale)"
      2. "PROCESSUS.actions_communes (niveau objet)"
      3. "TEMPLATE.actions_template (si template utilisé)"
      4. "ETAT.actions_specifiques (niveau opérationnel)"
      5. "🆕 ETAT.actions_workflow_supplementaires (intégration workflow)"
    
    regles_fusion_v22:
      - "Actions WORKFLOW ont priorité coordination globale"
      - "Actions ETAT peuvent avoir prérequis WORKFLOW + TEMPLATE"
      - "Actions TEMPLATE sont enrichies contexte WORKFLOW"
      - "Propagation automatique impacts WORKFLOW"
      - "Validation contraintes WORKFLOW obligatoire"
  
  etape_4_enrichissement_menu:
    menu_hamburger_final_v22:
      options_automatiques:
        - toujours: "options héritées du PROCESSUS"
        - si_workflow: "🆕 Vue Workflow Global, Planning, Dépendances"
        - si_template: "Vue Template Original"
        - si_particulier_ou_template: "Vue Standard Processus"
      
      options_surcharges:
        - source: "🆕 WORKFLOW.options_globales"
        - source: "PROCESSUS.menu_hamburger"
        - source: "TEMPLATE.menu_hamburger_enrichi"
        - source: "ETAT.surcharges.menu_hamburger"
  
  etape_5_application_contexte:
    sources_regles_v22:
      1. "🆕 WORKFLOW.contraintes_globales (niveau projet)"
      2. "PROCESSUS.regles_contexte (niveau objet)"
      3. "TEMPLATE.regles_contexte (si template utilisé)"
      4. "ETAT.regles_contexte (niveau opérationnel)"
      5. "ETAT.surcharges.regles_contexte_workflow"
    
    ordre_application: "WORKFLOW → PROCESSUS → TEMPLATE → ETAT → CONTEXTE_UTILISATEUR"
    priorite_finale: "CONTEXTE_UTILISATEUR > ETAT > TEMPLATE > PROCESSUS > WORKFLOW"
```

### 6.2 Exemple de Fusion Complète v2.2
```yaml
exemple_fusion_complete_v22:
  # Configuration initiale enrichie workflow
  configuration_initiale:
    workflow: "fabrication_moteur_bmw_x3"
    processus: "production_bloc_moteur"
    etat: "en_usinage"
    template: "dashboard_metriques"
    contexte: "operateur_shift_matin_workflow_actif"
  
  configuration_finale:
    source_interface: "WORKFLOW + PROCESSUS + TEMPLATE dashboard_metriques + PARAMÈTRES état + SURCHARGES"
    source_menu: "WORKFLOW + PROCESSUS.menu_hamburger + vue standard + vue template + vue workflow"
    source_actions: "WORKFLOW.orchestration + PROCESSUS.actions_communes + TEMPLATE.actions + ÉTAT.actions"
    
    interface_affichee:
      type: "Interface_Dashboard_Metriques_Workflow"
      titre: "Usinage Bloc Moteur - Dashboard Production"
      couleur_theme: "#1976D2" # Bleu workflow
      
      sections_visibles:
        # 🆕 Du contexte workflow
        - nom: "Contexte Workflow Global"
          contenu: "Fabrication Moteur BMW X3 - Commande #12345"
          avancement_global: "65%"
          dependances_actives: ["Attente fin usinage culasse pour assemblage"]
          jalons_critiques: ["Assemblage possible le 10/09"]
          format: "panneau_workflow_compact"
          
        # Du template paramétré avec enrichissement workflow
        - nom: "Métriques Production Temps Réel"
          elements: 
            - "Température: 45°C (Normal)"
            - "Vitesse broche: 2800 rpm (Optimal)" 
            - "Avancement pièce: 75% (Conforme planning)"
            - "🆕 Impact workflow: +2% avancement global"
          format: "dashboard_gauges"
          mise_a_jour: "3 secondes"
          
        # De la surcharge état
        - nom: "Actions Usinage Prioritaires"
          contenu: ["Finaliser Usinage", "Contrôle Intermédiaire", "🆕 Synchroniser Workflow"]
          visible_si: "usinage_avancement > 70%"
          
      metadata:
        workflow_id: "fabrication_moteur_bmw_x3"
        processus_id: "production_bloc_moteur" 
        template_id: "dashboard_metriques"
        etat_id: "en_usinage"
        timestamp_generation: "2025-08-17T14:30:00Z"
        contexte_workflow: "actif_avec_dependances"
        
    menu_hamburger_final:
      options:
        # 🆕 Ajoutées automatiquement workflow
        - nom: "🏗️ Vue Workflow Global"
          action: "afficher_workflow_complet"
          position: "premier"
          description: "Vue d'ensemble fabrication moteur BMW X3"
          
        - nom: "📅 Planning Projet"
          action: "afficher_planning_workflow"  
          position: "second"
          description: "Jalons et planning global"
          
        - nom: "🔗 Dépendances Actives"
          action: "afficher_dependances_workflow"
          position: "third"
          description: "Coordination avec usinage culasse"
          badge: "1" # 1 dépendance active
          
        - nom: "📊 Autres Processus"
          action: "afficher_autres_processus_workflow"
          position: "fourth"
          description: "État usinage culasse et production pistons"
          
        # Ajoutées automatiquement template + processus
        - nom: "🔵 Vue Standard Processus"
          action: "afficher_comportement_base_processus"
          position: "cinquieme"
          description: "Vue standard production bloc moteur"
          
        - nom: "📋 Vue Template Original"
          action: "afficher_dashboard_complet"
          position: "sixieme"
          description: "Dashboard métriques complet"
          
        # Héritées du processus (conservées)
        - nom: "📚 Historique Complet"
          action: "afficher_historique_objet"
          visible: true
          
        - nom: "📤 Export Données"
          action: "exporter_donnees_objet"
          formats: ["json", "csv", "pdf", "🆕workflow_report"]
        
    actions_disponibles:
      du_workflow:
        - id: "synchroniser_avec_workflow"
          nom: "Synchroniser Workflow"
          origine: "workflow_fabrication_moteur_bmw_x3"
          description: "Synchronisation manuelle avec workflow global"
          
      du_processus:
        - id: "consulter_processus_avec_workflow"
          nom: "Consulter Processus"
          origine: "processus_production_bloc_moteur"
          enrichissement: "contexte_workflow_integre"
          
      du_template:
        - id: "actualiser_metriques_avec_workflow"
          nom: "Actualiser Métriques"
          origine: "template_dashboard_metriques_workflow_v22"
          enrichissement: "metriques_workflow_integrees"
          
      de_letat:
        - id: "finaliser_usinage_avec_workflow"
          nom: "Finaliser Usinage"
          origine: "etat_en_usinage"
          prerequis: ["usinage_complete", "🆕workflow_contraintes_respectees"]
          workflow_impact: "declenchement_controle_qualite + assemblage_preparation"
    
    regles_contexte_appliquees:
      # 🆕 Du workflow (priorité projet)
      - condition: "workflow.retard_planning > 0"
        afficher_alerte: "Retard planning workflow détecté"
        mettre_en_avant: ["actions.accelerer_usinage"]
        origine: "workflow"
        
      # Du processus (niveau objet)
      - condition: "utilisateur.role == 'operateur'"
        masquer: ["donnees_techniques_avancees"]
        origine: "processus"
        
      # De l'état (niveau opérationnel)
      - condition: "usinage.avancement > 70%"
        afficher_section: "actions_finalisation"
        origine: "etat"
        
      # Du contexte final enrichi
      - condition: "contexte.workflow_actif && shift == 'matin'"
        mise_en_avant: ["workflow.dependances_actives", "actions.synchroniser_workflow"]
        origine: "contexte_final_enrichi"
```

---

## 7. GESTION AVANCÉE DU LOGGING ET CYCLES DE CONSULTATION v2.2 (Enrichie Workflow)

### 7.1 Types de Consultations Détectées Enrichies Workflow
```yaml
types_consultations_v22:
  premiere:
    description: "Première consultation de l'objet"
    metriques: ["timestamp", "utilisateur", "device"]
    template_info: "template_utilise si applicable"
    workflow_info: "🆕 workflow_context si applicable"
    
  consultation_apres_action:
    description: "Consultation après qu'une action ait été exécutée"
    metriques: ["timestamp", "utilisateur", "action_precedente", "duree_depuis_action"]
    action_origine: "workflow|processus|template|etat" # 🆕 Workflow ajouté
    workflow_impact: "🆕 impact_sur_workflow si action workflow"
    
  re_consultation_sans_action:
    description: "Nouvelle consultation sans action intermédiaire"
    metriques: ["timestamp", "utilisateur", "nombre_consultations_consecutives", "duree_session"]
    template_progression: "progression_template si applicable"
    workflow_progression: "🆕 progression_workflow si applicable"
    alerte_possible: true
    
  consultation_changement_template:
    description: "🆕 Consultation après changement de paramètres template"
    metriques: ["timestamp", "utilisateur", "ancien_template", "nouveau_template", "parametres_changes"]
    workflow_impact: "🆕 impact_changement_sur_workflow"
    type: "nouveau_v21"
    
  # 🆕 NOUVEAUX TYPES WORKFLOW v2.2
  consultation_synchronisation_workflow:
    description: "Consultation suite à synchronisation workflow"
    metriques: ["timestamp", "utilisateur", "action_synchronisation", "processus_synchronises"]
    workflow_data: ["dependances_mises_a_jour", "planning_impacte", "ressources_reallouees"]
    type: "nouveau_v22"
    
  consultation_alerte_workflow:
    description: "Consultation déclenchée par alerte workflow"
    metriques: ["timestamp", "utilisateur", "type_alerte", "severite"]
    alerte_data: ["retard_planning", "dependance_bloquante", "ressource_critique"]
    action_requise: true
    type: "nouveau_v22"
    
  consultation_workflow_global:
    description: "Consultation de la vue workflow global"
    metriques: ["timestamp", "utilisateur", "workflow_id", "section_consultee"]
    workflow_data: ["avancement_global", "processus_consultes", "jalons_analyses"]
    niveau: "coordination_globale"
    type: "nouveau_v22"
```

### 7.2 Métriques de Comportement Utilisateur Enrichies Workflow
```yaml
metriques_comportement_v22:
  # Tracking patterns existants v2.1
  patterns_consultation:
    consultations_totales: "compteur_global"
    consultations_sans_action: "compteur_consultations_vides"
    ratio_action_consultation: "pourcentage_efficacite"
    duree_moyenne_session: "temps_moyen_avant_action_ou_fermeture"
    
    # Métriques templates v2.1
    templates_utilises: "liste_templates_consultes"
    progression_templates: "avancement_par_template"
    efficacite_par_template: "ratio_completion_par_template"
    parametres_modifies: "historique_changements_parametres"
    
    # 🆕 MÉTRIQUES WORKFLOW v2.2
    workflows_consultes: "liste_workflows_impliques"
    consultations_workflow: "consultations_vue_workflow_global"
    actions_coordination: "actions_synchronisation_executees"
    efficacite_workflow: "ratio_actions_workflow_reussies"
    impact_workflow_moyen: "impact_moyen_actions_sur_workflow"
    
  # Seuils d'alerte enrichis workflow
  seuils_alerte_v22:
    # Seuils existants v2.1
    consultations_vides_consecutives: 5
    duree_session_anormale: 3600000 # 1 heure
    ratio_action_faible: 0.2 # Moins de 20%
    template_incomplet_seuil: 0.5 # Template complété à moins de 50%
    changements_parametres_excessifs: 3
    templates_abandonnes_seuil: 2
    
    # 🆕 SEUILS WORKFLOW v2.2
    retard_workflow_critique: 24 # heures
    dependances_bloquantes_seuil: 3 # nombre max
    consultations_workflow_sans_action: 5 # consultations workflow sans coordination
    desynchronisation_processus_seuil: 2 # heures entre processus interdépendants
    inefficacite_coordination_seuil: 0.3 # Moins de 30% actions coordination réussies
    
  # Actions automatiques enrichies workflow
  actions_automatiques_v22:
    # Actions existantes v2.1
    - condition: "consultations_vides_consecutives >= 5"
      action: "proposer_aide_contextuelle"
      
    - condition: "template_incomplet_seuil < 0.5"
      action: "proposer_assistance_template"
      
    # 🆕 ACTIONS WORKFLOW v2.2
    - condition: "retard_workflow_critique > 24"
      action: "alerte_retard_critique_workflow"
      message: "Retard critique workflow - Intervention requise"
      notification: ["chef_projet", "operateurs_concernes"]
      
    - condition: "dependances_bloquantes_seuil > 3"
      action: "proposer_deblocage_automatique"
      message: "Dépendances bloquantes détectées - Solutions disponibles"
      
    - condition: "desynchronisation_processus_seuil > 2"
      action: "suggerer_synchronisation_workflow"
      message: "Processus désynchronisés - Synchronisation recommandée"
      
    - condition: "inefficacite_coordination_seuil < 0.3"
      action: "proposer_formation_coordination"
      message: "Formation coordination workflow disponible"
```

### 7.3 Gestion des Sessions Enrichie Workflow v2.2
```yaml
gestion_sessions_v22:
  # Propriétés dynamiques ajoutées à l'OBJET (enrichies workflow)
  proprietes_dynamiques_consultation:
    derniere_consultation:
      timestamp: "2025-08-17T14:30:00Z"
      utilisateur_id: "user_123"
      etat: "En_Usinage"
      template_utilise: "dashboard_metriques" 
      parametres_template: {...}
      workflow_parent_id: "fabrication_moteur_bmw_x3" # 🆕
      contexte_workflow: {...} # 🆕
      
    derniere_action:
      timestamp: "2025-08-17T14:25:00Z"
      action_id: "actualiser_metriques_avec_workflow"
      origine_action: "template" # workflow|processus|template|etat
      utilisateur_id: "user_123"
      resultat: "Metriques_Mises_A_Jour"
      donnees_template: {...}
      workflow_impact: {...} # 🆕
      
    session_actuelle:
      debut_session: "2025-08-17T14:30:00Z"
      actions_executees: []
      consultations_dans_session: 3
      utilisateur_session: "user_123"
      
      # Données templates session v2.1
      templates_utilises_session: ["dashboard_metriques"]
      progression_templates: {"dashboard_metriques": 0.8}
      parametres_modifies_session: []
      
      # 🆕 DONNÉES WORKFLOW SESSION v2.2
      workflows_impliques_session: ["fabrication_moteur_bmw_x3"]
      actions_workflow_session: ["synchroniser_avec_workflow"]
      consultations_workflow_session: 2
      coordination_effectuee: true
      impact_workflow_session: {"avancement_contribue": 0.05}
      
    metriques_globales:
      compteur_consultations: 25
      compteur_actions: 18
      ratio_efficacite: 0.72
      derniere_consultation_sans_action: "2025-08-17T13:45:00Z"
      
      # Métriques templates globales v2.1
      templates_completes: ["dashboard_metriques", "formulaire_dynamique"]
      templates_en_cours: ["affichage_checklist"]
      efficacite_templates: {"affichage_checklist": 0.8, "dashboard_metriques": 0.9}
      
      # 🆕 MÉTRIQUES WORKFLOW GLOBALES v2.2
      workflows_participants: ["fabrication_moteur_bmw_x3"]
      actions_workflow_total: 12
      actions_workflow_reussies: 10
      efficacite_workflow: 0.83
      contribution_workflow_moyenne: 0.03 # Contribution moyenne à avancement workflow
      coordination_reussie_ratio: 0.85
```

### 7.4 Logs Enrichis Workflow v2.2
```json
// Exemple de log consultation avec workflow
{
  "timestamp": "2025-08-17T14:30:00Z",
  "type_event": "consultation_workflow_enrichie",
  "objet_id": "bloc_moteur_001",
  "utilisateur": "operateur_123",
  "etat": "En_Usinage",
  "type_consultation": "apres_action_workflow",
  "template_data": {
    "template_id": "dashboard_metriques",
    "template_version": "2.2",
    "parametres": {
      "metriques_a_afficher": ["temperature", "vitesse_broche"],
      "afficher_contexte_workflow": true
    },
    "progression": 0.8
  },
  "workflow_data": {
    "workflow_id": "fabrication_moteur_bmw_x3",
    "workflow_type": "orchestration_complete",
    "avancement_global": 0.67,
    "processus_actuel": "production_bloc_moteur",
    "dependances_actives": [
      {
        "attente": "fin_usinage_culasse",
        "pour_declencher": "assemblage_moteur"
      }
    ],
    "contraintes_planning": {
      "retard_actuel": 0,
      "jalon_critique": "assemblage_possible_10_09"
    },
    "ressources_partagees": {
      "machines_utilisees": ["CNC_001"],
      "operateurs_assignes": ["op_123"]
    }
  },
  "session_data": {
    "debut_session": "2025-08-17T14:30:00Z",
    "consultations_consecutives": 1,
    "actions_executees_session": 2,
    "templates_utilises_session": ["dashboard_metriques"],
    "workflows_impliques_session": ["fabrication_moteur_bmw_x3"],
    "coordination_workflow_active": true
  },
  "contexte": {
    "device": "webapp",
    "shift": "apres_midi",
    "formation_workflow": true,
    "role_coordination": "operateur_principal"
  }
}

// Exemple de log action workflow
{
  "timestamp": "2025-08-17T14:35:00Z",
  "type_event": "action_workflow_coordination",
  "action": "synchroniser_avec_workflow",
  "objet_id": "bloc_moteur_001",
  "utilisateur": "operateur_123",
  "workflow_data": {
    "workflow_id": "fabrication_moteur_bmw_x3",
    "processus_synchronises": ["production_bloc_moteur", "usinage_culasse"],
    "mode_synchronisation": "attendre_tous",
    "dependances_verifiees": [
      {
        "prerequis": "usinage_bloc_75_pourcent",
        "etat": "satisfait"
      }
    ]
  },
  "resultat": {
    "synchronisation_reussie": true,
    "processus_impactes": 2,
    "dependances_mises_a_jour": 1,
    "avancement_workflow_avant": 0.65,
    "avancement_workflow_apres": 0.67,
    "contribution_action": 0.02
  },
  "impact": {
    "planning_modifie": false,
    "ressources_reallouees": false,
    "notifications_envoyees": ["equipe_assemblage"],
    "actions_declenchees": ["preparation_assemblage_automatique"]
  },
  "performance": {
    "duree_execution": 1200,
    "processus_analyses": 4,
    "dependances_evaluees": 8,
    "optimisations_appliquees": ["cache_metriques", "calcul_parallele"]
  }
}

// Exemple de log workflow global
{
  "timestamp": "2025-08-17T14:40:00Z",
  "type_event": "workflow_progression_globale",
  "workflow_id": "fabrication_moteur_bmw_x3",
  "utilisateur": "chef_projet_456",
  "evenement_declencheur": "fin_usinage_bloc_moteur",
  "etat_workflow": {
    "avancement_global": 0.70,
    "avancement_precedent": 0.67,
    "progression_periode": 0.03,
    "processus_termines": 3,
    "processus_total": 6,
    "jalons_atteints": 2,
    "jalons_total": 4
  },
  "processus_details": [
    {
      "processus_id": "production_bloc_moteur",
      "etat": "Termine",
      "contribution": 0.20,
      "duree_reelle": 8.5,
      "duree_prevue": 8.0,
      "ecart": 0.5
    },
    {
      "processus_id": "usinage_culasse", 
      "etat": "En_Cours",
      "avancement": 0.85,
      "contribution": 0.17,
      "duree_estimee_restante": 1.2
    }
  ],
  "dependances_actives": [
    {
      "prerequis_satisfaits": ["production_bloc_moteur.Termine"],
      "prerequis_en_attente": ["usinage_culasse.Termine"],
      "action_declenchee": "preparation_assemblage_moteur",
      "delai_estime": 1.5
    }
  ],
  "planning": {
    "retard_global": 0.5,
    "jalons_impactes": [],
    "date_livraison_maintenue": true,
    "ressources_critiques": ["operateur_assemblage"]
  },
  "actions_automatiques": [
    {
      "action": "notifier_equipe_assemblage",
      "raison": "bloc_moteur_pret",
      "destinataires": ["op_assemblage_001", "chef_equipe_assemblage"]
    },
    {
      "action": "optimiser_allocation_ressources",
      "raison": "liberation_cnc_001",
      "nouveau_processus_prioritaire": "usinage_culasse"
    }
  ]
}
```

---

## 8. LOGGING CONFIGURATION v2.2 (Enrichie Workflow)

### 8.1 Configuration Logging Workflow
```yaml
logging_configuration_v22:
  # Historique interne enrichi workflow
  historique_interne:
    description: "Stockage dans objet.historique avec données templates + workflow"
    avantages: ["Accessible via API", "Intégré données objet", "Requêtable", "Historique workflow complet"]
    format: "structured_json_with_templates_and_workflow"
    retention: "illimitee"
    
    # Données templates v2.1
    donnees_templates:
      templates_utilises: "liste chronologique des templates"
      parametres_historique: "évolution des paramètres par template"
      progressions: "historique des progressions templates"
      completions: "timestamps des complétions templates"
    
    # 🆕 DONNÉES WORKFLOW v2.2
    donnees_workflow:
      workflows_participants: "liste workflows impliqués dans historique objet"
      contributions_workflow: "contribution objet aux avancements workflow"
      coordination_historique: "actions de coordination effectuées"
      dependances_historique: "dépendances satisfaites et impacts"
      planning_impacts: "modifications planning causées par objet"
    
  # Log externe enrichi workflow
  log_externe:
    description: "Écriture fichiers logs externes avec séparation workflow"
    fichiers_specialises:
      # Fichiers existants v2.1
      - "consultations_templates.log"
      - "actions_templates.log" 
      - "completions_templates.log"
      - "erreurs_templates.log"
      - "consultations_processus.log"
      - "actions_processus.log"
      
      # 🆕 FICHIERS WORKFLOW v2.2
      - "workflow_consultations.log"
      - "workflow_coordination.log"
      - "workflow_progression.log"
      - "workflow_planning.log"
      - "workflow_ressources.log"
      - "workflow_dependances.log"
      - "workflow_erreurs.log"
    
    formats_supportes: ["json", "csv", "plain_text"]
    rotation: "quotidienne"
    compression: true
    retention: "365_jours"
    
    # 🆕 Configuration spécialisée workflow
    workflow_logging:
      niveau_detail_workflow: "standard" # minimal|standard|detaille
      aggregation_metriques: true # Agrégation automatique métriques workflow
      alertes_temps_reel: true # Alertes workflow temps réel
      correlation_processus: true # Corrélation automatique entre processus
```

### 8.2 Niveaux de Détail Enrichis Workflow v2.2
```yaml
niveaux_detail_v22:
  minimal:
    donnees: ["timestamp", "action", "utilisateur_id", "objet_id"]
    templates: ["template_id", "progression"]
    workflow: ["🆕 workflow_id", "🆕 contribution_globale"] # NOUVEAU
    cas_usage: "Audit basique, performance, métriques workflow"
    
  standard:
    donnees: ["timestamp", "action", "utilisateur_id", "objet_id", "resultats", "duree"]
    templates: ["template_id", "parametres", "progression", "elements_modifies"]
    workflow: ["🆕 workflow_id", "🆕 dependances", "🆕 coordination", "🆕 impact_planning"]
    cas_usage: "Monitoring opérationnel avec workflow et templates"
    
  detaille:
    donnees: ["timestamp", "action", "utilisateur_id", "objet_id", "parametres_entree", "etapes_intermediaires", "resultats", "erreurs", "contexte_complet"]
    templates: ["template_complet", "schema_validation", "parametres_historique", "erreurs_template"]
    workflow: ["🆕 workflow_complet", "🆕 orchestration_details", "🆕 ressources_impact", "🆕 planning_recalculs"]
    cas_usage: "Debug workflow, analyse poussée coordination, conformité workflow"
```

### 8.3 Actions Systémiques de Logging Enrichies Workflow v2.2
```yaml
actions_systemiques_v22:
  # Actions existantes v2.1 enrichies
  consultation_objet:
    logging:
      historique_interne: true
      log_externe: false
      niveau_detail: "minimal"
      fichier_specialise: "consultations_templates.log" # si template utilisé
      fichier_workflow: "🆕 workflow_consultations.log" # si workflow parent
      
  consultation_template:
    logging:
      historique_interne: true
      log_externe: true
      niveau_detail: "standard"
      fichier_specialise: "consultations_templates.log"
      fichier_workflow: "🆕 workflow_consultations.log" # si workflow impact
      
  action_template:
    logging:
      historique_interne: true
      log_externe: true
      niveau_detail: "standard"
      fichier_specialise: "actions_templates.log"
      fichier_workflow: "🆕 workflow_coordination.log" # si impact workflow
      
  # 🆕 NOUVELLES ACTIONS WORKFLOW v2.2
  consultation_workflow:
    logging:
      historique_interne: false # Pas d'historique objet pour consultation workflow global
      log_externe: true
      niveau_detail: "standard"
      fichier_specialise: "workflow_consultations.log"
      alerte_temps_reel: false
      
  action_workflow_coordination:
    logging:
      historique_interne: true # Impact sur objets participants
      log_externe: true
      niveau_detail: "detaille"
      fichier_specialise: "workflow_coordination.log"
      alerte_temps_reel: true # Coordination = critique
      
  progression_workflow:
    logging:
      historique_interne: false # Niveau workflow, pas objet
      log_externe: true
      niveau_detail: "standard"
      fichier_specialise: "workflow_progression.log"
      alerte_temps_reel: true # Progression = information importante
      aggregation_automatique: true
      
  recalcul_planning_workflow:
    logging:
      historique_interne: false
      log_externe: true
      niveau_detail: "detaille"
      fichier_specialise: "workflow_planning.log"
      alerte_temps_reel: true # Planning = critique
      notification_equipes: true
      
    reallocation_ressources_workflow:
    logging:
      historique_interne: false
      log_externe: true
      niveau_detail: "detaille"
      fichier_specialise: "workflow_ressources.log"
      alerte_temps_reel: true # Ressources = impact immédiat
      notification_equipes: true
      
  erreur_workflow:
    logging:
      historique_interne: true # Erreur peut impacter objets
      log_externe: true
      niveau_detail: "detaille"
      fichier_specialise: "workflow_erreurs.log"
      alerte_temps_reel: true # Erreur = critique
      escalade_automatique: true
```

---

## 9. EXEMPLE COMPLET v2.2 - WORKFLOW FABRICATION MOTEUR BMW

### 9.1 Workflow Global - Fabrication Moteur BMW X3
```yaml
workflow_fabrication_moteur_bmw_complet:
  # Métadonnées workflow
  id: "fabrication_moteur_bmw_x3_cmd12345"
  nom: "Fabrication Moteur BMW X3 - Commande #12345"
  description: "Fabrication complète moteur 2.0L turbo pour commande client BMW"
  type: "orchestration_complete"
  version: "1.0"
  
  # Client et commande
  commande:
    numero: "BMW-2025-12345"
    client: "BMW Manufacturing Munich"
    moteur_type: "B48 2.0L Turbo"
    quantite: 1
    date_livraison_contractuelle: "2025-09-15T18:00:00Z"
    specifications: "Moteur série production avec options sport"
  
  # Processus participants
  processus_inclus:
    processus_critiques:
      - processus_id: "production_bloc_moteur_001"
        objet_id: "bloc_moteur_bmw_001"
        nom: "Production Bloc Moteur"
        criticite: "critique"
        duree_estimee: 8 # heures
        prerequis: ["aluminium_disponible", "cnc_001_libre"]
        
      - processus_id: "usinage_culasse_001"
        objet_id: "culasse_bmw_001"
        nom: "Usinage Culasse"
        criticite: "critique"
        duree_estimee: 6 # heures
        prerequis: ["fonte_disponible", "cnc_002_libre"]
        
    processus_importants:
      - processus_id: "production_pistons_001"
        objets_ids: ["piston_001", "piston_002", "piston_003", "piston_004"]
        nom: "Production 4 Pistons"
        criticite: "importante"
        duree_estimee: 4 # heures par piston
        parallelisable: true
        prerequis: ["aluminium_forge_disponible"]
        
      - processus_id: "usinage_vilebrequin_001"
        objet_id: "vilebrequin_bmw_001"
        nom: "Usinage Vilebrequin"
        criticite: "importante"
        duree_estimee: 10 # heures
        prerequis: ["acier_forge_disponible", "cnc_precision_libre"]
        
    processus_support:
      - processus_id: "controle_qualite_001"
        objets_ids: ["tous_composants"]
        nom: "Contrôle Qualité Global"
        criticite: "obligatoire"
        duree_estimee: 2 # heures par composant
        declenche_par: "completion_composant"
        
      - processus_id: "assemblage_moteur_001"
        objet_id: "moteur_complet_bmw_001"
        nom: "Assemblage Moteur Final"
        criticite: "finale"
        duree_estimee: 12 # heures
        prerequis: ["tous_composants_valides"]
  
  # Orchestration complexe
  orchestration:
    # Dépendances critiques
    dependances:
      synchrones:
        - nom: "Assemblage Bloc-Culasse Possible"
          condition: "ET"
          prerequis: [
            "production_bloc_moteur.Etat = 'Controle_Qualite_Valide'",
            "usinage_culasse.Etat = 'Controle_Qualite_Valide'"
          ]
          declenche: "assemblage_moteur.Phase_1_Autorisation"
          delai_max: 4 # heures
          criticite: "bloquant"
          
        - nom: "Assemblage Complet Possible"
          condition: "ET"
          prerequis: [
            "assemblage_moteur.Phase_1 = 'Complete'",
            "production_pistons.Tous_Valides = true",
            "usinage_vilebrequin.Controle_Qualite = 'Valide'"
          ]
          declenche: "assemblage_moteur.Phase_2_Autorisation"
          delai_max: 2 # heures
          criticite: "finale"
          
      asynchrones:
        - nom: "Contrôle Qualité Continu"
          condition: "CHAQUE"
          prerequis: ["*.Composant_Produit"]
          declenche: "controle_qualite.Nouveau_Controle_Composant"
          parallelisme: true
          delai_max: 1 # heure
          
        - nom: "Préparation Assemblage Anticipée"
          condition: "OU"
          prerequis: [
            "production_bloc_moteur.Avancement >= 0.75",
            "usinage_culasse.Avancement >= 0.75"
          ]
          declenche: "assemblage_moteur.Preparation_Poste"
          anticipation: true
    
    # Jalons critiques
    jalons:
      - id: "jalon_composants_principaux"
        nom: "Composants Principaux Terminés"
        description: "Bloc moteur et culasse terminés et validés qualité"
        date_cible: "2025-09-08T18:00:00Z"
        prerequis: [
          "production_bloc_moteur.Etat = 'Controle_Qualite_Valide'",
          "usinage_culasse.Etat = 'Controle_Qualite_Valide'"
        ]
        criticite: "bloquant"
        impact_retard: "Report assemblage impossible"
        
      - id: "jalon_tous_composants"
        nom: "Tous Composants Prêts"
        description: "Tous composants moteur produits et validés"
        date_cible: "2025-09-10T18:00:00Z"
        prerequis: [
          "production_pistons.Tous_Valides = true",
          "usinage_vilebrequin.Controle_Qualite = 'Valide'",
          "autres_composants.Tous_Valides = true"
        ]
        criticite: "importante"
        impact_retard: "Assemblage retardé"
        
      - id: "jalon_assemblage_complet"
        nom: "Assemblage Moteur Complet"
        description: "Moteur assemblé, testé et validé"
        date_cible: "2025-09-13T18:00:00Z"
        prerequis: [
          "assemblage_moteur.Phase_2 = 'Complete'",
          "test_fonctionnel.Resultat = 'Conforme'",
          "controle_final.Validation = 'Approuvee'"
        ]
        criticite: "contractuel"
        impact_retard: "Pénalités client"
        
      - id: "jalon_livraison"
        nom: "Livraison Client"
        description: "Moteur emballé et expédié vers client BMW"
        date_cible: "2025-09-15T12:00:00Z"
        prerequis: [
          "emballage.Etat = 'Termine'",
          "documentation.Complete = true",
          "transport.Organise = true"
        ]
        criticite: "contractuel"
        impact_retard: "Pénalités + impact relation client"
    
    # Métriques globales temps réel
    metriques_globales:
      avancement:
        processus_termines: 2 # bloc moteur + culasse
        processus_en_cours: 3 # pistons + vilebrequin + contrôle
        processus_total: 8
        pourcentage_global: 0.68
        
      planning:
        date_debut_reel: "2025-08-15T08:30:00Z" # 30min retard initial
        date_fin_prevue: "2025-09-15T15:00:00Z" # 3h avance prévue
        retard_actuel: 0 # jours (rattrapé)
        jalons_respectes: 2
        jalons_total: 4
        marge_securite: 3 # heures
        
      qualite:
        composants_produits: 6
        composants_valides: 5
        composants_rejetes: 1 # 1 piston refait
        taux_qualite: 0.83 # 83% (acceptable > 80%)
        controles_passes: 15
        non_conformites: 2 # mineures
        
      ressources:
        machines_allouees: 4
        machines_utilisees: 3
        utilisation_machines: 0.78
        operateurs_assignes: 5
        operateurs_actifs: 4
        disponibilite_operateurs: 0.85
        matieres_consommees: 0.82 # % du stock alloué
  
  # Planning détaillé avec phases
  planning:
    temporalite:
      date_creation_workflow: "2025-08-10T14:00:00Z"
      date_debut_prevue: "2025-08-15T08:00:00Z"
      date_debut_reelle: "2025-08-15T08:30:00Z"
      date_fin_prevue: "2025-09-15T15:00:00Z"
      date_livraison_contractuelle: "2025-09-15T18:00:00Z"
      marge_contractuelle: 3 # heures
      
    phases_execution:
      - nom: "Phase 1 - Préparation Globale"
        periode: "2025-08-15 → 2025-08-16"
        processus: ["preparation_machines", "controle_matieres", "briefing_equipes"]
        statut: "Complete"
        duree_reelle: 26 # heures
        duree_prevue: 24 # heures
        ecart: +2 # heures
        
      - nom: "Phase 2 - Production Composants Critiques"
        periode: "2025-08-17 → 2025-08-25"
        processus: ["production_bloc_moteur", "usinage_culasse"]
        statut: "Complete"
        parallelisme: "Complet"
        duree_reelle: 62 # heures
        duree_prevue: 64 # heures
        ecart: -2 # heures (avance)
        
      - nom: "Phase 3 - Production Composants Secondaires"
        periode: "2025-08-20 → 2025-08-30"
        processus: ["production_pistons", "usinage_vilebrequin"]
        statut: "En_Cours"
        parallelisme: "Phase_2 + autonome"
        avancement: 0.75
        duree_prevue: 80 # heures
        duree_estimee_restante: 18 # heures
        
      - nom: "Phase 4 - Assemblage Principal"
        periode: "2025-08-26 → 2025-09-08"
        processus: ["assemblage_moteur_phase_1"]
        statut: "Planifie"
        prerequis: ["phase_2_complete"]
        duree_prevue: 48 # heures
        
      - nom: "Phase 5 - Assemblage Final"
        periode: "2025-09-09 → 2025-09-12"
        processus: ["assemblage_moteur_phase_2", "test_fonctionnel"]
        statut: "Planifie"
        prerequis: ["phase_3_complete", "phase_4_complete"]
        duree_prevue: 32 # heures
        
      - nom: "Phase 6 - Finalisation Livraison"
        periode: "2025-09-13 → 2025-09-15"
        processus: ["controle_final", "emballage", "expedition"]
        statut: "Planifie"
        duree_prevue: 24 # heures
  
  # Gestion ressources globales
  ressources_globales:
    machines:
      - id: "CNC_001_BLOC_MOTEUR"
        type: "Centre usinage 5 axes"
        capacite: "24h/jour"
        allocation_actuelle: "production_bloc_moteur"
        planning: ["production_bloc_moteur: 08/17-08/21", "usinage_pieces_secondaires: 08/22-08/25"]
        utilisation_actuelle: 0.85
        maintenance_prevue: "2025-08-26T02:00:00Z"
        
      - id: "CNC_002_CULASSE"
        type: "Centre usinage haute précision"
        capacite: "24h/jour"
        allocation_actuelle: "usinage_culasse"
        planning: ["usinage_culasse: 08/18-08/23", "finition_vilebrequin: 08/24-08/26"]
        utilisation_actuelle: 0.78
        
      - id: "CNC_PRECISION_VILEBREQUIN"
        type: "Tour haute précision"
        capacite: "24h/jour"
        allocation_actuelle: "usinage_vilebrequin"
        specialisation: "Vilebrequins et arbres"
        utilisation_actuelle: 0.90
        
      - id: "POSTE_ASSEMBLAGE_001"
        type: "Poste assemblage moteur"
        capacite: "16h/jour" # 2 équipes
        allocation_future: "assemblage_moteur"
        equipements: ["pont roulant", "outils couple", "banc test"]
        disponibilite: 1.0
    
    operateurs:
      - id: "op_cnc_001"
        nom: "Jean Dupont"
        competences: ["usinage_cnc_5_axes", "controle_qualite", "reglage_machines"]
        allocation_actuelle: "production_bloc_moteur"
        charge_actuelle: 0.90
        shift: "2x8" # Matin + Après-midi
        habilitations: ["conduite_cnc", "controle_dimensionnel"]
        
      - id: "op_cnc_002"
        nom: "Marie Martin"
        competences: ["usinage_precision", "programmation_cnc", "metrologie"]
        allocation_actuelle: "usinage_culasse"
        charge_actuelle: 0.80
        shift: "3x8" # Continu
        specialisation: "Culasses et pièces complexes"
        
      - id: "op_assemblage_001"
        nom: "Pierre Durand"
        competences: ["assemblage_moteur", "test_fonctionnel", "diagnostic"]
        allocation_future: "assemblage_moteur"
        charge_prevue: 0.95
        certifications: ["assemblage_bmw", "test_banc"]
        
      - id: "controleur_qualite_001"
        nom: "Sophie Leblanc"
        competences: ["controle_dimensionnel", "controle_materiau", "validation_qualite"]
        allocation: "transversale_tous_processus"
        charge_actuelle: 0.75
        equipements: ["MMT", "durometre", "banc_test"]
    
    matieres_premieres:
      - reference: "ALUMINIUM_A356_BLOC"
        designation: "Aluminium coulé pour blocs moteur"
        quantite_allouee: 80 # kg
        quantite_consommee: 65 # kg
        quantite_restante: 15 # kg
        fournisseur: "Aluminium France"
        lot: "AF-2025-0847"
        
      - reference: "FONTE_GS_CULASSE"
        designation: "Fonte à graphite sphéroïdal"
        quantite_allouee: 45 # kg
        quantite_consommee: 40 # kg
        quantite_restante: 5 # kg
        fournisseur: "Fonderie Lorraine"
        lot: "FL-2025-1203"
        
      - reference: "ACIER_FORGE_VILEBREQUIN"
        designation: "Acier forgé haute résistance"
        quantite_allouee: 25 # kg
        quantite_consommee: 20 # kg
        quantite_restante: 5 # kg
        fournisseur: "Forge Industrielle"
        lot: "FI-2025-0952"
        certification: "IATF_16949"
  
  # Contraintes globales
  contraintes_globales:
    qualite:
      standards_applicables: ["ISO_9001", "IATF_16949", "BMW_GS95003"]
      taux_defaut_max: 0.05 # 5% max (BMW standard)
      controles_obligatoires: ["dimensionnel", "metallurgique", "fonctionnel"]
      points_critiques: ["etancheite", "couple_serrage", "equilibrage"]
      traçabilite_complete: true
      
    planning:
      date_livraison_ferme: "2025-09-15T18:00:00Z"
      penalites_retard: "5000_euros_par_jour"
      bonus_avance: "2000_euros_par_jour" # max 3 jours
      jalons_contractuels: ["jalon_assemblage_complet", "jalon_livraison"]
      marge_securite_minimale: 2 # heures
      
    couts:
      budget_total_alloue: 75000 # euros
      cout_actuel_consomme: 52000 # euros
      pourcentage_consomme: 0.69 # 69%
      marge_prevue: 0.12 # 12%
      seuil_alerte_budget: 0.85 # 85%
      cout_par_heure_retard: 500 # euros
      
    securite:
      habilitations_obligatoires: ["conduite_machines_cnc", "manipulation_pieces_lourdes", "travail_hauteur"]
      equipements_protection: ["casque", "gants_anti_coupure", "chaussures_securite", "lunettes"]
      procedures_urgence: ["arret_urgence_machines", "evacuation_atelier", "premier_secours"]
      formation_continue: "8h_par_trimestre"
  
  # Actions workflow niveau global
  actions_workflow_globales:
    - id: "recalculer_planning_complet"
      nom: "Recalcul Planning Complet"
      description: "Recalcul automatique planning complet workflow"
      
      declencheurs:
        - "processus.retard_significatif > 4h"
        - "processus.termine_en_avance > 2h"
        - "ressource.panne_critique"
        - "qualite.non_conformite_majeure"
        
      script_javascript: |
        async function recalculerPlanningComplet(workflow, evenement, contexte) {
          // Récupération état complet tous processus
          const etatGlobal = await getEtatCompletWorkflow(workflow.id);
          
          // Analyse impact événement déclencheur
          const impactAnalysis = await analyserImpactEvenement(evenement, etatGlobal);
          
          // Recalcul optimisé avec contraintes
          const nouveauPlanning = await optimiserPlanningAvecContraintes(
            workflow.processus_inclus,
            workflow.contraintes_globales,
            etatGlobal,
            impactAnalysis
          );
          
          // Validation contraintes contractuelles
          const validationContractuelle = await validerContraintesContractuelles(
            nouveauPlanning,
            workflow.contraintes_globales
          );
          
          if (!validationContractuelle.respectee) {
            // Escalade automatique si contraintes violées
            await escaladerProblemeContractuel(validationContractuelle.violations);
          }
          
          // Application nouveau planning
          workflow.planning = nouveauPlanning.planning_optimise;
          workflow.metriques_globales.retard_planning = nouveauPlanning.retard_calcule;
          
          // Notifications équipes impactées
          await notifierEquipesChangementPlanning(nouveauPlanning.changements_significatifs);
          
          return {
            planning_recalcule: true,
            processus_impactes: nouveauPlanning.processus_modifies.length,
            retard_global: nouveauPlanning.retard_calcule,
            contraintes_respectees: validationContractuelle.respectee
          };
        }
```

### 9.2 Processus Bloc Moteur avec Workflow
```yaml
processus_bloc_moteur_workflow:
  # Identité processus
  id: "production_bloc_moteur_001"
  nom: "Production Bloc Moteur BMW"
  description: "Production complète bloc moteur 2.0L turbo avec intégration workflow"
  workflow_parent_id: "fabrication_moteur_bmw_x3_cmd12345"
  
  # Objet associé
  objet_associe:
    id: "bloc_moteur_bmw_001"
    nom: "Bloc Moteur BMW B48 2.0L"
    identifiants_gs1: "01234567890123"
    specifications_bmw: "B48B20A Turbo"
    
  # Comportement de base enrichi workflow
  comportement_base:
    action_automatique_consultation:
      actif: true
      enrichissement_workflow: true
      
      script_javascript: |
        async function onConsultationBlocMoteurWorkflow(objet, contexte) {
          const currentTimestamp = Date.now();
          
          // Consultation standard processus
          const consultationEvent = await consultationProcessusStandard(objet, contexte);
          
          // Enrichissement workflow BMW
          const workflowContext = await getWorkflowContext("fabrication_moteur_bmw_x3_cmd12345");
          consultationEvent.workflow_bmw = {
            commande_numero: "BMW-2025-12345",
            avancement_global: workflowContext.avancement_global,
            dependances_assemblage: workflowContext.dependances_actives.filter(d => 
              d.pour_declencher.includes("assemblage")
            ),
            contraintes_bmw: workflowContext.contraintes_globales.qualite,
            delai_livraison: workflowContext.contraintes_globales.planning.date_livraison_ferme
          };
          
          // Vérification jalons critiques BMW
          const jalonsImpactes = await verifierJalonsCritiques(
            workflowContext.jalons,
            objet.etat_actuel
          );
          
          if (jalonsImpactes.length > 0) {
            consultationEvent.workflow_bmw.jalons_critiques = jalonsImpactes;
          }
          
          return { 
            comportement_a_afficher: "production_bloc_moteur_workflow",
            donnees_supplementaires: consultationEvent
          };
        }

    interface_standard:
      type: "vue_objet_workflow_bmw"
      sections:
        - nom: "Identité Bloc Moteur"
          contenu: ["id_objet", "specifications_bmw", "commande_client"]
          
        - nom: "État Production"
          contenu: ["etat_actuel", "avancement_production", "etape_usinage"]
          
        - nom: "Contexte Workflow BMW"
          contenu: ["commande_bmw", "avancement_moteur_complet", "dependances_assemblage"]
          format: "panneau_workflow_bmw"
          mise_a_jour: "temps_reel"
          
        - nom: "Spécifications BMW"
          contenu: ["tolerances_bmw", "materiaux_certifies", "standards_qualite"]
          format: "tableau_specifications"
          
        - nom: "Actions Production"
          contenu: "actions_workflow + actions_processus + actions_etat"
          format: "boutons_action_bmw"

    menu_hamburger:
      options_disponibles:
        # Options workflow BMW spécifiques
        - nom: "🏗️ Workflow Moteur BMW"
          action: "afficher_workflow_moteur_bmw"
          visible_par_defaut: true
          description: "Vue complète fabrication moteur BMW X3"
          
        - nom: "📋 Commande BMW #12345"
          action: "afficher_details_commande_bmw"
          visible_par_defaut: true
          description: "Détails commande client BMW Munich"
          
        - nom: "🔗 Assemblage Dépendances"
          action: "afficher_dependances_assemblage"
          visible_par_defaut: true
          description: "Coordination avec culasse pour assemblage"
          badge_dependances: "1_active"
          
        # Options standard
        - nom: "📊 Métriques Production"
          action: "afficher_metriques_production"
          
        - nom: "🛠️ Machine CNC-001"
          action: "afficher_etat_machine_cnc_001"
          
        - nom: "📚 Historique Complet"
          action: "afficher_historique_objet"
          
        - nom: "📄 Export Données BMW"
          action: "exporter_donnees_format_bmw"
          formats: ["json", "csv", "bmw_report"]

    regles_contexte:
      # Règles workflow BMW
      - condition: "workflow.commande.client == 'BMW' && workflow.retard_planning > 0"
        modifications:
          - afficher_alerte: "URGENT - Retard commande BMW détecté"
          - couleur_theme: "#FF5722" # Rouge BMW urgent
          - mettre_en_avant: ["actions.accelerer_production"]
          - notification_automatique: ["chef_projet_bmw", "responsable_production"]
          
      - condition: "workflow.jalons.assemblage_possible.risque == 'retard'"
        modifications:
          - afficher_alerte: "Risque retard assemblage - Production prioritaire"
          - actions_supplementaires: ["coordination_urgente_assemblage"]
          
      # Règles qualité BMW spécifiques
      - condition: "standards_bmw.iatf_16949 == 'obligatoire'"
        modifications:
          - afficher_section: "controles_iatf_specifiques"
          - mettre_en_avant: ["tolerances_bmw", "tracabilite_complete"]
```

### 9.3 État En_Usinage avec Template Dashboard Workflow
```yaml
etat_usinage_workflow_bmw:
  # Identité état
  id: "etat_usinage_bloc_moteur_bmw"
  nom: "En_Usinage_CNC"
  description: "Usinage bloc moteur BMW sur centre CNC avec dashboard workflow"
  processus_parent_id: "production_bloc_moteur_001"
  
  # Comportement template enrichi workflow BMW
  comportement_particulier:
    actif: true
    mode: "utilise_template"
    
    template_utilise:
      template_id: "dashboard_metriques_workflow"
      template_version: "2.2"
      
      # Paramètres spécifiques usinage BMW
      parametres:
        metriques_a_afficher: [
          "temperature_usinage",
          "vitesse_broche", 
          "avancement_piece",
          "pression_lubrifiant",
          "precision_dimensionnelle"
        ]
        format_visualisation: "gauges_temps_reel"
        seuils_alerte: {
          "temperature_usinage": {"min": 15, "max": 85},
          "vitesse_broche": {"min": 1000, "max": 4000},
          "pression_lubrifiant": {"min": 2.0, "max": 8.0}
        }
        couleur_theme: "#1976D2" # Bleu BMW
        mise_a_jour_frequence: 2000 # 2 secondes
        
        # Paramètres workflow BMW intégrés
        afficher_contexte_workflow: true
        metriques_workflow: [
          "avancement_moteur_global",
          "retard_planning_bmw", 
          "dependances_assemblage_actives",
          "jalons_critiques_bmw"
        ]
        comparaison_autres_processus: true
        planning_integre_bmw: true
        alertes_workflow_bmw: true
        specifications_bmw_visibles: true
      
      # Surcharges BMW spécifiques
      surcharges:
        sections_supplementaires:
          # Section spéciale BMW
          - nom: "Standards BMW"
            contenu: "afficher_standards_bmw_temps_reel"
            format: "panneau_standards_bmw"
            position: "before_dashboard"
            donnees: [
              "conformite_iatf_16949",
              "tolerances_bmw_respectees",
              "tracabilite_lot_matiere"
            ]
          
          # Impact workflow sur assemblage
          - nom: "Impact Assemblage Moteur"
            contenu: "calculer_impact_usinage_sur_assemblage"
            format: "panneau_impact_assemblage"
            position: "sidebar_right"
            donnees: [
              "avancement_vs_culasse",
              "preparation_assemblage_possible",
              "delai_optimise_coordination"
            ]
            mise_a_jour: 5000 # 5 secondes
          
          # Métriques qualité BMW temps réel
          - nom: "Qualité BMW Temps Réel"
            contenu: "metriques_qualite_bmw_live"
            format: "dashboard_qualite"
            position: "after_dashboard"
            donnees: [
              "conformite_instantanee",
              "predictions_qualite_finale",
              "alertes_derive_tolerances"
            ]
        
        menu_hamburger:
          options_supplementaires:
            - nom: "🔄 Synchronisation BMW"
              action: "synchroniser_avec_workflow_bmw"
              description: "Synchronisation avec planning moteur BMW"
              
            - nom: "⚡ Optimisation BMW"
              action: "optimiser_pour_standards_bmw"
              description: "Optimisation paramètres pour standards BMW"
              
            - nom: "📊 Dashboard BMW Complet"
              action: "afficher_dashboard_bmw_complet"
              description: "Vue complète métriques BMW"
        
        # Règles contextuelles BMW workflow
        regles_contexte_bmw_workflow:
          - condition: "workflow_bmw.retard_planning > 1 && usinage.avancement < 0.75"
            modifications:
              - afficher_alerte: "CRITIQUE - Accélérer usinage pour respecter planning BMW"
              - augmenter_frequence_maj: 1000 # 1 seconde
              - couleur_alertes: "#D32F2F" # Rouge critique
              - actions_prioritaires: ["optimiser_vitesse_usinage", "alerter_chef_equipe"]
              
          - condition: "workflow_bmw.dependances.assemblage_en_attente == true"
            modifications:
              - afficher_alerte: "Assemblage en attente - Finaliser rapidement"
              - mettre_en_avant: ["avancement_piece", "precision_dimensionnelle"]
              - ajouter_section: "preparation_transfert_assemblage"
              
          - condition: "standards_bmw.derive_tolerances > seuil_bmw"
            modifications:
              - afficher_alerte: "Dérive tolérances BMW détectée"
              - actions_correctives: ["ajuster_parametres", "controler_outils"]
              - notification_qualite: true
    
    # Actions spécifiques usinage BMW workflow
    actions_supplementaires:
      - id: "finaliser_usinage_bmw_workflow"
        nom: "Finaliser Usinage BMW"
        description: "Finalisation usinage avec validation BMW et coordination workflow"
        visible_si: "usinage_completion >= 0.95 && qualite_bmw_conforme == true"
        
        # Prérequis BMW stricts
        prerequis_bmw:
          standards: ["IATF_16949_respecte", "tolerances_bmw_conformes"]
          qualite: ["precision_dimensionnelle_validee", "etat_surface_conforme"]
          tracabilite: ["lot_matiere_enregistre", "parametres_usinage_sauvegardes"]
          
        # Prérequis workflow
        prerequis_workflow:
          workflow_id: "fabrication_moteur_bmw_x3_cmd12345"
          conditions:
            - "machine_cnc_001.arret_propre_possible == true"
            - "controle_qualite.disponible == true"
            - "assemblage.preparation_possible == true"
          contraintes_bmw:
            - "delai_livraison_respecte"
            - "standards_qualite_maintenus"
        
        script_javascript: |
          async function finaliserUsinageBMWWorkflow(objet, contexte) {
            const timestamp = Date.now();
            
            // 1. Vérifications BMW obligatoires
            const validationBMW = await verifierStandardsBMW(objet);
            if (!validationBMW.conforme) {
              throw new Error(`Standards BMW non respectés: ${validationBMW.ecarts.join(', ')}`);
            }
            
            // 2. Vérifications workflow
            const workflowValidation = await verifierContraintesWorkflowBMW(
              contexte.workflow_parent_id,
              "finaliser_usinage_bloc_moteur"
            );
            
            if (!workflowValidation.respectees) {
              throw new Error(`Contraintes workflow BMW: ${workflowValidation.violations.join(', ')}`);
            }
            
            // 3. Finalisation technique usinage
            objet.proprietesDynamiques.usinage_bmw_termine = {
              timestamp: timestamp,
              operateur: contexte.utilisateur.id,
              standards_bmw: validationBMW,
              machine_utilisee: "CNC_001",
              parametres_finaux: await getMachineParametres("CNC_001"),
              qualite_finale: validationBMW.qualite_finale
            };
            
            // 4. Coordination workflow BMW
            const workflowImpact = await coordonnerFinalisationAvecWorkflowBMW(
              contexte.workflow_parent_id,
              objet,
              validationBMW
            );
            
            // 5. Déclenchement automatique contrôle qualité
            await declencherControleQualiteBMW(objet, validationBMW);
            
            // 6. Vérification assemblage possible
            const assemblageStatus = await verifierPossibiliteAssemblage(
              contexte.workflow_parent_id
            );
            
            if (assemblageStatus.possible) {
              await preparerAssemblageBMW(assemblageStatus.processus_prets);
            }
            
            // 7. Logging complet BMW
            const logDataBMW = {
              action: "finaliser_usinage_bmw_workflow",
              timestamp: timestamp,
              objet_id: objet.id,
              commande_bmw: "BMW-2025-12345",
              standards_bmw: validationBMW,
              workflow_impact: workflowImpact,
              assemblage_impact: assemblageStatus
            };
            
            await logToFile("bmw_production.log", logDataBMW);
            await logToFile("workflow_coordination_bmw.log", logDataBMW);
            
            return {
              nouvel_etat: "Controle_Qualite_BMW",
              message: "Usinage BMW terminé - Standards respectés - Workflow coordonné",
              standards_bmw: validationBMW.conforme,
              assemblage_preparation: assemblageStatus.possible,
              workflow_impact: workflowImpact
            };
          }
        
        # Transition vers contrôle qualité BMW
        transition: "Controle_Qualite_BMW"
        
        # Actions workflow automatiques
        workflow_actions_bmw:
          - trigger: "usinage_termine_bmw"
            condition: "standards_bmw_respectes"
            action: "declencher_controle_qualite_bmw"
            
          - trigger: "coordination_assemblage"
            condition: "bloc_et_culasse_prets"
            action: "preparer_assemblage_moteur_bmw"
            
          - trigger: "mise_a_jour_planning_bmw"
            condition: "always"
            action: "recalculer_planning_livraison_bmw"
        
        # Logging spécialisé BMW
        logging:
          historique_interne: true
          log_externe: true
          fichier_log: "usinage_bmw.log"
          fichier_log_workflow: "workflow_bmw_coordination.log"
          niveau_detail: "detaille"
          donnees_supplementaires: ["standards_bmw", "workflow_impact", "assemblage_preparation"]
          certification_bmw: true # Logging certifié BMW
```

---

## 10. VALIDATION ET CONTRAINTES v2.2 (Enrichies Workflow)

### 10.1 Règles de Validation Enrichies Workflow
```yaml
validation_v22:
  # Contraintes obligatoires enrichies workflow
  contraintes_obligatoires:
    # Contraintes existantes v2.1
    - "Un état doit toujours avoir un comportement (base/template/particulier)"
    - "Si template utilisé, paramètres doivent respecter schéma template"
    - "Actions template générées doivent être cohérentes avec paramètres"
    - "Transitions d'état doivent être définies même avec templates"
    - "Template référencé doit exister dans bibliothèque"
    
    # 🆕 CONTRAINTES WORKFLOW v2.2
    - "Si workflow parent défini, processus doit être déclaré dans workflow.processus_inclus"
    - "Actions workflow doivent respecter contraintes globales workflow"
    - "Transitions d'état avec workflow doivent vérifier dépendances"
    - "Métriques workflow doivent être cohérentes entre processus"
    - "Planning workflow doit être respecté sauf exception justifiée"
    
  # Cohérence données enrichie workflow
  coherence:
    # Cohérence existante v2.1
    - "Paramètres template doivent respecter schema_parametres"
    - "Sections template référencées doivent exister dans document source"
    - "Actions template doivent être compatibles avec données objet"
    - "Surcharges état ne doivent pas casser fonctionnement template"
    - "Menu hamburger enrichi doit être cohérent"
    
    # 🆕 COHÉRENCE WORKFLOW v2.2
    - "Dépendances workflow doivent être satisfiables"
    - "Ressources workflow ne doivent pas être sur-allouées"
    - "Jalons workflow doivent être atteignables selon planning"
    - "Contraintes globales doivent être compatibles entre processus"
    - "Métriques aggregées doivent correspondre à somme métriques processus"
    
  # Sécurité enrichie workflow
  securite:
    # Sécurité existante v2.1
    - "Validation paramètres template obligatoire avant génération"
    - "Scripts template doivent être sandboxés"
    - "Accès documents source doit être contrôlé"
    - "Logs templates doivent respecter confidentialité"
    - "Permissions utilisateur applicables aux actions template"
    
    # 🆕 SÉCURITÉ WORKFLOW v2.2
    - "Actions workflow doivent respecter permissions niveau projet"
    - "Coordination workflow ne doit pas exposer données sensibles"
    - "Accès métriques workflow selon rôle utilisateur"
    - "Logs workflow doivent respecter confidentialité client"
    - "Notifications workflow limitées aux parties autorisées"
    
  # 🆕 VALIDATION SPÉCIFIQUE WORKFLOW v2.2
  validation_workflow:
    - "Workflow doit avoir au moins un processus inclus"
    - "Dépendances workflow ne doivent pas créer de cycles"
    - "Planning workflow doit être cohérent avec contraintes temporelles"
    - "Ressources allouées doivent être disponibles"
    - "Jalons critiques doivent avoir chemins de réalisation"
    - "Contraintes globales doivent être respectables"
    - "Actions orchestration doivent avoir scripts valides"
```

### 10.2 Tests de Validation v2.2
```yaml
tests_validation_v22:
  # Tests existants v2.1
  - nom: "Affichage comportement de base"
    test: "Vérifier que toutes les sections obligatoires s'affichent"
    
  - nom: "Gestion contexte"
    test: "Vérifier que l'affichage change selon le rôle utilisateur"
    
  - nom: "Exécution actions"
    test: "Vérifier que chaque action produit une transition d'état"
    
  - nom: "API cohérence"
    test: "Vérifier que l'API expose les mêmes données que l'interface"
    
  - nom: "Validation template schema"
    test: "Vérifier que le schéma paramètres template est valide"
    
  - nom: "Génération interface template"
    test: "Vérifier que l'interface template se génère correctement"
    
  - nom: "Actions template fonctionnelles"
    test: "Vérifier que les actions template s'exécutent"
    
  - nom: "Fusion comportements"
    test: "Vérifier fusion base + template + particulier"
    
  - nom: "Performance templates"
    test: "Vérifier performance génération interface"
    
  - nom: "Sécurité templates"
    test: "Vérifier sécurité scripts template"
  
  # 🆕 TESTS WORKFLOW v2.2
  - nom: "Validation workflow orchestration"
    test: "Vérifier que l'orchestration workflow est cohérente"
    details: "Dépendances sans cycles + ressources disponibles + planning réalisable"
    
  - nom: "Coordination processus workflow"
    test: "Vérifier que la coordination entre processus fonctionne"
    details: "Synchronisation + déclenchement dépendances + mise à jour métriques"
    
  - nom: "Intégration workflow dans templates"
    test: "Vérifier que les templates intègrent correctement le contexte workflow"
    details: "Données workflow visibles + actions coordination + contraintes respectées"
    
  - nom: "Performance workflow"
    test: "Vérifier performance coordination workflow"
    details: "Calcul dépendances < 100ms + mise à jour métriques < 50ms"
    
  - nom: "Sécurité workflow"
    test: "Vérifier sécurité coordination workflow"
    details: "Permissions respectées + données sensibles protégées + logs sécurisés"
    
  - nom: "Resilience workflow"
    test: "Vérifier comportement workflow en cas d'erreur"
    details: "Gestion pannes processus + recovery automatique + escalade"
    
  - nom: "API workflow cohérence"
    test: "Vérifier cohérence API workflow avec interface"
    details: "Métriques identiques + actions disponibles + états synchronisés"
```

---

## 11. AVANTAGES ARCHITECTURE v2.2

### 11.1 🆕 Orchestration Globale (Nouveau)
- **Coordination Multi-Objets** avec dépendances intelligentes
- **Planning Intégré** avec jalons critiques et contraintes
- **Gestion Ressources Partagées** entre processus
- **Métriques Globales** agrégées et temps réel
- **Escalade Automatique** en cas de problèmes critiques

### 11.2 Réutilisabilité Maximale (Enrichie)
- **1 Template = N États** avec paramètres différents enrichis workflow
- **Bibliothèque Centralisée** de comportements + contexte workflow
- **Maintenance Template** impact automatique tous états + workflow
- **Standards UX** garantis + cohérence workflow
- **Expertise Capitalisée** dans templates + orchestration

### 11.3 Flexibilité Préservée et Enrichie
- **4 Niveaux** : Workflow (global) + Processus (objet) + État (opérationnel) + Templates (réutilisables)
- **3 Modes Comportement** : manuel, template, mixte + enrichissement workflow
- **Paramétrage Fin** des templates + contexte workflow intégré
- **Surcharges Possibles** + contraintes workflow respectées
- **Compatibilité Totale** avec architecture v2.1 + migration transparente

### 11.4 Performance et Cohérence (Optimisée)
- **Templates Mis en Cache** + métriques workflow optimisées
- **Génération Interface** dynamique + contexte workflow
- **Validation Automatique** paramètres + contraintes workflow
- **Menu Enrichi Automatiquement** selon mode + workflow
- **Logging Spécialisé** pour analyse templates + coordination workflow

### 11.5 🆕 Évolutivité et Maintenance Workflow
- **Orchestration Extensible** avec nouveaux patterns coordination
- **Workflow Adaptatifs** futurs (v2.3) avec IA intégrée
- **Monitoring Intelligent** avec prédiction et prévention
- **Tests Automatisés** validation workflow + templates
- **Documentation Intégrée** exemples usage + orchestration

---

## 12. EXPOSITION API v2.2 (Enrichie Workflow)

### 12.1 Endpoints Standard Enrichis Workflow
```yaml
api_exposition_v22:
  # Endpoints objets enrichis workflow
  base_url_objets: "/api/v1/objets/{objet_id}/etats/{etat_id}"
  
  endpoints_objets:
    # Consultation état enrichie workflow
    - method: "GET"
      path: "/"
      description: "Obtenir l'état complet avec données template + workflow"
      response_type: "vue_comportement_complet_v22"
      nouvelles_donnees: ["workflow_context", "dependances_actives", "contraintes_planning"]
      
    # Actions disponibles enrichies workflow
    - method: "GET" 
      path: "/actions"
      description: "Lister actions (workflow+processus+template+état) selon contexte"
      nouvelles_donnees: ["origine_action_workflow", "prerequis_workflow", "impact_workflow"]
      
    # Exécution action enrichie workflow
    - method: "POST"
      path: "/actions/{action_id}"
      description: "Déclencher action avec support workflow + templates"
      nouvelles_donnees: ["workflow_coordination", "dependances_declenchees"]
      
    # Endpoints templates existants v2.1
    - method: "GET"
      path: "/template"
      description: "Informations template utilisé par l'état"
      
    - method: "PUT"
      path: "/template/parametres"
      description: "Modifier paramètres template"
      
    # 🆕 ENDPOINTS WORKFLOW CONTEXTE
    - method: "GET"
      path: "/workflow"
      description: "Contexte workflow pour cet objet"
      response: "workflow_info + dependances + planning + ressources"
      
    - method: "POST"
      path: "/workflow/coordination"
      description: "Déclencher coordination workflow depuis cet objet"
      body: "type_coordination + parametres"
      
  # 🆕 ENDPOINTS WORKFLOW GLOBAUX
  workflow_endpoints:
    base_url: "/api/v1/workflows"
    
    - method: "GET"
      path: "/"
      description: "Lister tous workflows actifs"
      
    - method: "GET"
      path: "/{workflow_id}"
      description: "Détails workflow complet + orchestration"
      
    - method: "GET"
      path: "/{workflow_id}/processus"
      description: "Processus inclus dans workflow avec états actuels"
      
    - method: "GET"
      path: "/{workflow_id}/dependances"
      description: "Dépendances actives et satisfaites"
      
    - method: "GET"
      path: "/{workflow_id}/planning"
      description: "Planning détaillé avec jalons et ressources"
      
    - method: "GET"
      path: "/{workflow_id}/metriques"
      description: "Métriques globales temps réel"
      
    - method: "POST"
      path: "/{workflow_id}/actions/{action_id}"
      description: "Exécuter action coordination workflow"
      
    - method: "PUT"
      path: "/{workflow_id}/planning"
      description: "Recalculer planning workflow"
      
    - method: "POST"
      path: "/{workflow_id}/ressources/reallocation"
      description: "Réallouer ressources globales"
      
  # 🆕 ENDPOINTS TEMPLATES ENRICHIS WORKFLOW
  templates_endpoints_v22:
    base_url: "/api/v1/templates"
    
    - method: "GET"
      path: "/{template_id}/workflow-integration"
      description: "Capacités intégration workflow du template"
      
    - method: "POST"
      path: "/{template_id}/preview-workflow"
      description: "Prévisualisation template avec contexte workflow"
      body: "parametres_template + workflow_context"
```

### 12.2 Format de Réponse Standard v2.2 (Enrichi Workflow)
```json
{
  "etat": {
    "id": "uuid-etat",
    "nom": "En_Usinage",
    "timestamp_entree": "2025-08-17T14:00:00Z",
    "mode_comportement": "utilise_template"
  },
  "objet": {
    "id": "uuid-objet",
    "nom": "Bloc_Moteur_BMW_001",
    "identifiants_gs1": { ... },
    "proprietes_statiques": { ... },
    "proprietes_dynamiques": { ... }
  },
  "template_data": {
    "template_id": "dashboard_metriques_workflow",
    "template_version": "2.2",
    "parametres_appliques": {
      "metriques_a_afficher": ["temperature", "vitesse_broche"],
      "afficher_contexte_workflow": true,
      "metriques_workflow": ["avancement_global", "dependances_actives"]
    },
    "progression": {
      "elements_total": 5,
      "elements_completes": 4,
      "pourcentage": 80.0
    },
    "interface_generee": { ... }
  },
  "workflow_data": {
    "workflow_id": "fabrication_moteur_bmw_x3_cmd12345",
    "workflow_nom": "Fabrication Moteur BMW X3 - Commande #12345",
    "workflow_type": "orchestration_complete",
    "avancement_global": 0.68,
    "processus_actuel": {
      "processus_id": "production_bloc_moteur_001",
      "contribution_workflow": 0.25,
      "criticite": "critique"
    },
    "dependances_actives": [
      {
        "prerequis": "usinage_culasse.Etat = 'Controle_Qualite_Valide'",
        "pour_declencher": "assemblage_moteur.Phase_1",
        "etat_actuel": "en_attente",
        "delai_estime": 4.5
      }
    ],
    "contraintes_planning": {
      "jalon_critique": "assemblage_possible_08_09",
      "marge_restante": 6.5,
      "retard_actuel": 0
    },
    "ressources_partagees": {
      "machines_utilisees": ["CNC_001"],
      "operateurs_assignes": ["op_cnc_001"],
      "disponibilite": {
        "CNC_002": "libre_dans_2h",
        "op_assemblage_001": "disponible"
      }
    },
    "jalons_critiques": [
      {
        "id": "jalon_assemblage_complet",
        "nom": "Assemblage Moteur Complet",
        "date_cible": "2025-09-13T18:00:00Z",
        "statut": "en_cours",
        "risque": "faible"
      }
    ]
  },
  "actions_disponibles": [
    {
      "id": "actualiser_metriques_avec_workflow",
      "nom": "Actualiser Métriques",
      "origine": "template",
      "template_id": "dashboard_metriques_workflow",
      "workflow_integration": true,
      "endpoint": "/actions/actualiser_metriques_avec_workflow"
    },
    {
      "id": "finaliser_usinage_bmw_workflow",
      "nom": "Finaliser Usinage BMW",
      "origine": "etat",
      "prerequis_workflow": {
        "workflow_id": "fabrication_moteur_bmw_x3_cmd12345",
        "conditions": ["standards_bmw_respectes", "machine_arretable"]
      },
      "workflow_impact": {
        "dependances_declenchees": ["controle_qualite_bmw"],
        "processus_impactes": ["assemblage_moteur"],
        "planning_impact": "aucun_si_conforme"
      },
      "visible": true,
      "priorite": "haute"
    },
    {
      "id": "synchroniser_avec_workflow",
      "nom": "Synchroniser Workflow",
      "origine": "workflow",
      "workflow_id": "fabrication_moteur_bmw_x3_cmd12345",
      "endpoint": "/workflow/coordination",
      "permissions_requises": ["coordination_workflow"]
    }
  ],
  "contexte_applique": {
    "utilisateur_role": "operateur_cnc",
    "timestamp": "2025-08-17T14:30:00Z",
    "template_autorisations": ["consulter", "actualiser_metriques"],
    "workflow_autorisations": ["consulter", "signaler_blocage"],
    "contraintes_actives": ["standards_bmw", "planning_critique"]
  },
  "menu_hamburger": [
    {
      "nom": "🏗️ Vue Workflow Global",
      "action": "afficher_workflow_complet",
      "origine": "auto_workflow",
      "badge": "68%"
    },
    {
      "nom": "📅 Planning BMW",
      "action": "afficher_planning_workflow",
      "origine": "auto_workflow",
      "badge": "critique"
    },
    {
      "nom": "🔗 Coordination Assemblage",
      "action": "afficher_dependances_assemblage",
      "origine": "auto_workflow", 
      "badge": "1_active"
    },
    {
      "nom": "📋 Vue Template Original",
      "action": "afficher_dashboard_complet",
      "origine": "auto_template"
    },
    {
      "nom": "🔵 Vue Standard Processus",
      "action": "afficher_comportement_base",
      "origine": "auto_processus"
    }
  ]
}
```

---

## 13. RÉSUMÉ ÉVOLUTION v2.1 → v2.2

### 13.1 🆕 Nouveautés Majeures v2.2
1. **🏗️ WORKFLOW Global** - Orchestration multi-objets avec coordination intelligente
2. **📄 PROCESSUS Clarifiés** - Niveau objet distinct avec workflow parent optionnel
3. **🔗 Dépendances Intelligentes** - Synchronisation automatique entre processus
4. **📅 Planning Intégré** - Jalons critiques et contraintes temporelles
5. **⚙️ Gestion Ressources** - Allocation optimisée et réallocation dynamique
6. **📊 Métriques Globales** - Agrégation temps réel et indicateurs projet
7. **🔄 Coordination Automatique** - Actions workflow déclenchées automatiquement

### 13.2 Clarification Terminologique v2.2
- **WORKFLOW** = Global, multi-objets, orchestration projet
- **PROCESSUS** = Spécifique objet, cycle de vie, workflow parent optionnel
- **ÉTAT** = Situation instantanée, opérationnel, templates + workflow
- **TEMPLATES** = Réutilisables, enrichis contexte workflow

### 13.3 Compatibilité v2.1 → v2.2
- **100% Rétrocompatible** - États v2.1 fonctionnent sans modification
- **Migration Terminologique** - `process` → `processus` (automatique)
- **Workflow Optionnel** - Objets peuvent exister sans workflow parent
- **Templates Enrichis** - Contexte workflow ajouté sans casser v2.1

### 13.4 Bénéfices Concrets v2.2
- **Orchestration Projet** - Coordination intelligente multi-objets
- **Planning Intégré** - Suivi jalons et contraintes temps réel
- **Ressources Optimisées** - Allocation dynamique selon priorités
- **Escalade Automatique** - Gestion proactive des problèmes
- **Métriques Centralisées** - Vision globale avancement projet
- **Coordination Transparente** - Actions workflow automatiques

### 13.5 🚀 Perspectives v2.3
- **Workflow Adaptatifs** avec IA intégrée
- **Optimisation Prédictive** des ressources et planning
- **Apprentissage Automatique** des patterns de coordination
- **Intégration IoT** pour métriques temps réel enrichies

---

**Template ÉTAT ProcessMetaLanguage v2.2 COMPLET** - Architecture révolutionnaire intégrant orchestration workflow globale, processus objets clarifiés, et templates enrichis, offrant une solution complète pour la gestion de projets industriels complexes avec coordination multi-objets intelligente.