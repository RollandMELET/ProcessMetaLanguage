# Script Excalidraw - Création Template Avatar 360SmartConnect

## 🎯 Objectif
Créer un script Excalidraw qui transforme un dessin existant en template réutilisable pour la création d'objets Avatar dans 360SmartConnect.

## 📋 Spécifications Fonctionnelles

### Input
- **Dessin Excalidraw existant** sélectionné par l'utilisateur
- **Documentation API 360SmartConnect** (objet Avatar) pour valeurs par défaut

### Output
- **Template enrichi** avec métadonnées BackOfCard
- **Fichier template** prêt pour script de création d'objets embedded

## 🏗️ Workflow du Script

### Étape 1 : Initialisation
1. **Détecter** le dessin Excalidraw sélectionné
2. **Charger** la documentation Avatar (propriétés obligatoires/optionnelles)
3. **Créer** zone BackOfCard pour métadonnées

### Étape 2 : Configuration Propriétés Avatar (Minimales)
Le script présente **une par une** les propriétés essentielles :

#### Propriétés Obligatoires
- **`name`** (string) - Nom de l'avatar
- **`avatarType`** (IRI) - Type d'avatar (relation obligatoire)

#### Propriétés Recommandées
- **`serialNumber`** (string) - Numéro de série
- **`company`** (IRI) - Société propriétaire
- **`status`** (integer) - Statut opérationnel

### Étape 3 : Validation Interactive par Propriété

Pour chaque propriété, l'utilisateur choisit :

#### Options de Configuration
1. **🤖 Auto 360SC** - Valeur renseignée automatiquement par 360SmartConnect
2. **📝 Valeur Template** - Saisie utilisateur maintenant (fixe dans template)
3. **✏️ Valeur Embedded** - Saisie utilisateur lors création objet embedded
4. **⏭️ Ignorer** - Passer à la propriété suivante

#### Interface Simplifiée
```
Propriété: name (string) - Nom de l'avatar
Valeur par défaut suggérée: "Nouvel Avatar"

Choisir mode:
[1] Auto 360SC
[2] Valeur Template → [Saisir maintenant]
[3] Valeur Embedded
[4] Ignorer

Votre choix: _
```

### Étape 4 : Génération Template
1. **Sauvegarder** métadonnées dans BackOfCard
2. **Structurer** données pour script création embedded
3. **Valider** cohérence template généré

## 📄 Structure BackOfCard Générée

```json
{
  "templateType": "Avatar360SC",
  "version": "1.0",
  "properties": {
    "name": {
      "mode": "embedded",
      "defaultValue": "",
      "required": true
    },
    "avatarType": {
      "mode": "template",
      "value": "/api/avatar_types/123",
      "required": true
    },
    "serialNumber": {
      "mode": "auto",
      "required": false
    },
    "company": {
      "mode": "template",
      "value": "/api/companies/456",
      "required": false
    },
    "status": {
      "mode": "embedded",
      "defaultValue": 1,
      "required": false
    }
  }
}
```

## 🎯 Contraintes Techniques

### Simplicité Maximale
- **Interface texte simple** (pas GUI complexe)
- **Validation une propriété à la fois**
- **Choix numérotés** (1, 2, 3, 4)
- **Messages clairs** pour chaque étape

### Gestion Erreurs
- **Valeurs obligatoires** : Redemander si mode "ignorer" choisi
- **Formats invalides** : Proposer re-saisie avec exemple
- **Annulation** : Possibilité d'annuler à tout moment

### Intégration Excalidraw
- **Préserver dessin original** intact
- **Zone BackOfCard** non visible sur canvas principal
- **Métadonnées** stockées dans propriétés Excalidraw

## 🔄 Flux de Données

```
Dessin Excalidraw
    ↓
Script Template
    ↓
Validation Propriétés
    ↓
BackOfCard Enrichi
    ↓
Template Prêt
    ↓
(Script Création Embedded)
```

## 📝 Livrables Attendus

### Script Principal
- **Fichier .js** pour Excalidraw
- **Interface console** simple
- **Validation robuste** des entrées

### Documentation
- **README** utilisation script
- **Exemples** de templates générés
- **Guide** intégration avec script embedded

## 🚀 Version Initiale (MVP)

### Scope Réduit
- **Uniquement objet Avatar** (pas autres objets API)
- **5 propriétés maximum** pour première version
- **Interface console** uniquement (pas GUI)
- **Pas de validation API** (format local seulement)

### Extensions Futures
- Support autres objets API (AvatarType, etc.)
- Interface graphique améliorée
- Validation temps réel avec API 360SC
- Templates prédéfinis par secteur

---
**Fichier Spécification - Script Création Template Avatar**  
**Version MVP - Focus Simplicité Maximale**