/*
 * === Add BackOfTheCard to Drawing ===
 * Version: 1.0.0
 * Date: 2025-01-19
 * Auteur: Rolland MELET & Claude Code
 * 
 * Description: Script qui génère automatiquement un template BackOfTheCard 
 * à partir d'un dessin Excalidraw existant contenant un seul objet.
 * 
 * Le script:
 * 1. Demande le nom du template
 * 2. Génère un fichier template avec exemples inspirants
 * 3. Ajoute visuellement le type d'objet dans le dessin
 * 4. Crée l'embed BackOfTheCard

```javascript
 */

// Vérification préalable et logs de démarrage
console.log("🔧 === VÉRIFICATION ENVIRONNEMENT ===");
console.log("📦 ExcalidrawAutomate (ea):", typeof ea, ea ? "✅" : "❌");
console.log("🛠️ Utils:", typeof utils, utils ? "✅" : "❌");
console.log("🏠 App object:", typeof app, app ? "✅" : "❌");

if (!ea.verifyMinimumPluginVersion("2.0.0")) {
    console.error("❌ Version Excalidraw Plugin insuffisante");
    new Notice("⚠️ Version Excalidraw Plugin insuffisante. Minimum requis: 2.0.0");
    return;
}
console.log("✅ Version plugin vérifiée");

// Configuration et constantes
const CONFIG = {
    version: "1.0.0",
    author: "Rolland MELET & Claude Code",
    debug: true
};

const TYPES_OBJETS = [
    "OBJET", "WORKFLOW", "PROCESSUS", "ÉTAT", "ACTION", 
    "COMPORTEMENT", "VUE", "RÈGLE", "DÉCISION"
];

const COULEURS = {
    "OBJET": "#4CAF50",
    "WORKFLOW": "#FF9800", 
    "PROCESSUS": "#2196F3",
    "ÉTAT": "#9C27B0",
    "ACTION": "#F44336",
    "COMPORTEMENT": "#00BCD4",
    "VUE": "#E91E63",
    "RÈGLE": "#795548",
    "DÉCISION": "#FFC107"
};

// Fonction de logging
function log(message, level = "info") {
    if (CONFIG.debug) {
        const timestamp = new Date().toLocaleTimeString();
        console.log(`[${timestamp}] BackOfTheCard: ${message}`);
    }
}

// Fonction principale
async function main() {
    try {
        log("=== DÉMARRAGE DU SCRIPT ===");
        log(`Add BackOfTheCard to Drawing v${CONFIG.version}`);

        // Étape 1: Vérifications préalables
        log("🔍 Recherche du fichier actif...");
        const activeFile = app.workspace.getActiveFile();
        
        log(`📁 activeFile found: ${activeFile ? 'YES' : 'NO'}`);
        if (activeFile) {
            log(`📄 File name: ${activeFile.name}`);
            log(`🔗 File extension: ${activeFile.extension}`);
        }
        
        if (!activeFile) {
            log("❌ STOP: Aucun fichier ouvert");
            new Notice("❌ Veuillez ouvrir un fichier");
            return;
        }

        // Vérifier si c'est un fichier Excalidraw 
        log("🔍 Vérification si fichier Excalidraw...");
        
        // Méthode 1: Utiliser la fonction intégrée d'EA
        const isExcalidrawEA = ea.isExcalidrawFile(activeFile);
        log(`🎨 EA.isExcalidrawFile: ${isExcalidrawEA ? 'YES' : 'NO'}`);
        
        // Méthode 2: Vérifier le frontmatter
        const fileContent = await app.vault.read(activeFile);
        const hasExcalidrawProperty = fileContent.includes("excalidraw-plugin: parsed");
        log(`📋 Frontmatter excalidraw-plugin: ${hasExcalidrawProperty ? 'YES' : 'NO'}`);
        
        const isExcalidrawFile = isExcalidrawEA || hasExcalidrawProperty;
        log(`✅ Final check - Is Excalidraw: ${isExcalidrawFile ? 'YES' : 'NO'}`);
        
        if (!isExcalidrawFile) {
            log("❌ STOP: Ce n'est pas un fichier Excalidraw");
            new Notice("❌ Veuillez ouvrir un fichier Excalidraw (avec 'excalidraw-plugin: parsed')");
            return;
        }

        log(`📁 Fichier actif trouvé: ${activeFile.name}`);
        log(`📂 Chemin complet: ${activeFile.path}`);

        // Vérifier qu'il y a exactement un objet dans le dessin
        const elements = ea.getViewElements();
        log(`🔍 Nombre d'éléments trouvés: ${elements.length}`);
        
        if (elements.length === 0) {
            log("❌ STOP: Aucun élément dans le dessin");
            new Notice("❌ Aucun élément dans le dessin. Créez d'abord un objet.");
            return;
        }
        
        if (elements.length > 1) {
            log(`❌ STOP: Trop d'éléments (${elements.length}) - doit être exactement 1`);
            new Notice("⚠️ Le dessin doit contenir exactement UN objet. Sélectionnez ou supprimez les autres éléments.");
            return;
        }

        log(`✅ Objet unique trouvé dans le dessin`);

        // Étape 2: Demander le nom du template
        const nomTemplate = await askForTemplateName();
        if (!nomTemplate) return;

        log(`Nom du template: ${nomTemplate}`);

        // Étape 3: Demander le type d'objet
        const typeObjet = await selectObjectType();
        if (!typeObjet) return;

        log(`Type d'objet: ${typeObjet}`);

        // Étape 4: Générer le template BackOfTheCard
        const templateContent = generateTemplate(nomTemplate, typeObjet);

        // Étape 5: Créer le fichier template
        const templateFile = await createTemplateFile(nomTemplate, templateContent);
        if (!templateFile) return;

        // Étape 6: Ajouter le texte du type dans le dessin actuel
        await addTypeToCurrentDrawing(typeObjet, nomTemplate);

        // Étape 7: Créer l'embed dans le dessin actuel
        await addEmbedToCurrentDrawing(templateFile);

        log("✅ BackOfTheCard ajouté avec succès!");
        new Notice(`✅ Template "${nomTemplate}" créé et ajouté au dessin`);

    } catch (error) {
        log(`❌ ERREUR: ${error.message}`, "error");
        new Notice(`❌ Erreur: ${error.message}`);
    }
}

// Demander le nom du template
async function askForTemplateName() {
    log("📝 Demande du nom du template...");
    const result = await utils.inputPrompt(
        "Nom du Template",
        "Entrez le nom du template à créer :",
        "",
        [{
            caption: "Créer",
            action: () => true
        }]
    );
    
    if (!result || result.trim() === "") {
        log("❌ STOP: Nom du template vide ou annulé");
        new Notice("❌ Nom du template requis");
        return null;
    }
    
    log(`✅ Nom du template saisi: ${result.trim()}`);
    return result.trim();
}

// Sélection du type d'objet
async function selectObjectType() {
    log("🎯 Demande du type d'objet...");
    const result = await utils.suggester(
        (item) => item,
        TYPES_OBJETS,
        false,
        "Sélectionnez le type d'objet ProcessMetaLanguage :"
    );
    
    if (!result) {
        log("❌ STOP: Aucun type d'objet sélectionné");
        return null;
    }
    
    log(`✅ Type d'objet sélectionné: ${result}`);
    return result;
}

// Générateur de template avec exemples
function generateTemplate(nom, type) {
    const uid = generateUID(type);
    const date = new Date().toISOString().split('T')[0];
    
    // Construction du contenu par concaténation pour éviter les conflits de template literals
    const templateHeader = `---
excalidraw-plugin: parsed
type: "template-${type.toLowerCase()}"
template_version: "1.0.0"
processmetalanguage_version: "2.3"
uid: "${uid}"
created: "${date}"
author: "${CONFIG.author}"
tags: [processmetalanguage, ${type.toLowerCase()}, template]
---`;

    const templateBody = `
# ${type}: ${nom}

## 📋 Métadonnées

- **UID**: ${uid}
- **Type**: ${type}
- **Créé**: ${date}
- **Statut**: Brouillon
- **Version**: 1.0.0

## 📝 Description

[Décrivez ici l'objectif et le rôle de cet élément]

**Exemple**: ` + getDescriptionExample(type) + `

## back of the Card

[Cette section contient les informations détaillées et techniques de l'élément - le "verso" de la carte qui complète la représentation visuelle du "recto"]

### Contexte d'utilisation
- **Quand l'utiliser** : [Situations appropriées]
- **Pré-requis** : [Conditions nécessaires]
- **Impact** : [Conséquences de son utilisation]

### Détails techniques
- **Implémentation** : [Comment c'est réalisé techniquement]
- **Dépendances** : [Autres éléments nécessaires]
- **Configuration** : [Paramètres spécifiques]

## ⚖️ Rules (Règles de Gestion)

### Règles Métier
- [ ] ` + getRuleExample(type, "business") + `
- [ ] ` + getRuleExample(type, "constraint") + `  
- [ ] ` + getRuleExample(type, "validation") + `

### Règles Techniques
- [ ] ` + getRuleExample(type, "technical") + `
- [ ] ` + getRuleExample(type, "performance") + `

### Règles de Transition
- [ ] ` + getRuleExample(type, "transition") + `

## 🏗️ Propriétés

### Propriétés de Base
` + getPropertiesExample(type) + `

### Propriétés Spécifiques
` + getSpecificPropertiesExample(type) + `

## 🔗 Relations

### Relations Entrantes
` + getRelationsExample(type, "incoming") + `

### Relations Sortantes
` + getRelationsExample(type, "outgoing") + `

## 🔄 États et Transitions

### États Possibles
` + getStatesExample(type) + `

### Conditions de Transition
` + getTransitionsExample(type) + `

## 📊 Métriques et KPIs

### Indicateurs de Performance
` + getMetricsExample(type) + `

## 🔍 Points de Contrôle

### Validations Requises
` + getValidationExample(type) + `

## 📚 Documentation

### Références
- [Lien vers documentation métier]
- [Lien vers spécifications techniques]

### Exemples d'Usage
` + getUsageExample(type) + `

## 📝 Notes

### À Compléter
- [ ] Finaliser les règles métier
- [ ] Valider les propriétés
- [ ] Tester les transitions
- [ ] Documenter les cas d'usage

### Historique
- ${date}: Création du template

---
**Template généré par ProcessMetaLanguage v${CONFIG.version}**
`;

    return templateHeader + templateBody;
}

// Exemples contextuels selon le type
function getDescriptionExample(type) {
    const examples = {
        "OBJET": "Cet objet représente une entité métier (ex: Commande, Produit, Client) qui suit un processus défini.",
        "WORKFLOW": "Ce workflow orchestre plusieurs processus pour atteindre un objectif business (ex: Traitement Commande Complète).",
        "PROCESSUS": "Ce processus décrit le cycle de vie complet d'un objet de son état initial à son état final.",
        "ÉTAT": "Cet état représente une situation spécifique dans laquelle peut se trouver un objet à un moment donné.",
        "ACTION": "Cette action déclenche une transition d'état et peut modifier les propriétés de l'objet.",
        "COMPORTEMENT": "Ce comportement définit une logique réutilisable applicable dans différents contextes.",
        "VUE": "Cette vue présente l'information de manière adaptée à un rôle utilisateur spécifique.",
        "RÈGLE": "Cette règle définit une contrainte métier qui doit être respectée en permanence.",
        "DÉCISION": "Ce point de décision oriente le flux selon des critères définis."
    };
    return examples[type] || "Décrivez l'objectif de cet élément";
}

function getRuleExample(type, ruleType) {
    const rules = {
        "business": "Respecter la logique métier définie",
        "constraint": "Valider les contraintes d'intégrité",
        "validation": "Vérifier les conditions d'entrée",
        "technical": "Respecter les standards techniques",
        "performance": "Maintenir les performances acceptables",
        "transition": "Autoriser le passage selon les conditions"
    };
    return rules[ruleType];
}

function getPropertiesExample(type) {
    return `- **nom**: Nom de l'élément
- **description**: Description détaillée
- **statut**: État courant (actif, inactif, suspendu)
- **propriétaire**: Responsable de l'élément
- **date_creation**: Date de création
- **date_modification**: Dernière modification`;
}

function getSpecificPropertiesExample(type) {
    const specific = {
        "OBJET": "- **processus_id**: Identifiant du processus associé\n- **etat_courant**: État actuel de l'objet\n- **proprietes_metier**: Données spécifiques au domaine",
        "WORKFLOW": "- **processus_orchestres**: Liste des processus coordonnés\n- **declencheurs**: Événements qui initient le workflow\n- **points_synchronisation**: Moments de coordination",
        "PROCESSUS": "- **objet_cible**: Type d'objet géré par ce processus\n- **etats_possibles**: Liste des états du processus\n- **etat_initial**: Point de départ\n- **etats_finaux**: Points d'arrivée",
        "ÉTAT": "- **type_etat**: initial, intermédiaire, final\n- **conditions_entree**: Conditions pour entrer dans cet état\n- **conditions_sortie**: Conditions pour quitter cet état\n- **actions_entree**: Actions à exécuter à l'entrée\n- **actions_sortie**: Actions à exécuter à la sortie"
    };
    return specific[type] || "- **propriete_specifique**: Valeur spécifique au type";
}

function getRelationsExample(type, direction) {
    const relations = {
        "incoming": "- **predecesseur_1**: [Type] - Description de la relation\n- **parent**: [Type] - Relation hiérarchique",
        "outgoing": "- **successeur_1**: [Type] - Description de la relation\n- **enfants**: [Type] - Relations hiérarchiques"
    };
    return relations[direction];
}

function getStatesExample(type) {
    const states = {
        "OBJET": "- **Créé**: État initial après création\n- **En traitement**: Objet en cours de traitement\n- **Validé**: Objet approuvé\n- **Terminé**: État final",
        "PROCESSUS": "- **Inactif**: Processus non démarré\n- **Actif**: Processus en cours d'exécution\n- **Suspendu**: Processus temporairement arrêté\n- **Terminé**: Processus achevé"
    };
    return states[type] || "- **État_1**: Description\n- **État_2**: Description";
}

function getTransitionsExample(type) {
    return `- **Condition_1**: Si [condition] alors [action]
- **Condition_2**: Quand [événement] alors [transition]
- **Règle_métier**: Selon [règle] autoriser [passage]`;
}

function getMetricsExample(type) {
    return `- **Temps_moyen**: Durée moyenne de traitement
- **Taux_succès**: Pourcentage de succès
- **Nombre_instances**: Instances actives`;
}

function getValidationExample(type) {
    return `- **Validation_1**: Vérifier l'intégrité des données
- **Validation_2**: Contrôler les permissions
- **Validation_3**: Valider les règles métier`;
}

function getUsageExample(type) {
    return `### Scénario Type
1. **Déclenchement**: [Événement déclencheur]
2. **Traitement**: [Étapes de traitement]
3. **Résultat**: [Résultat attendu]

### Cas Particuliers
- **Exception_1**: [Gestion du cas particulier]
- **Exception_2**: [Alternative ou contournement]`;
}

// Générer un UID unique
function generateUID(type) {
    const prefix = type.substring(0, 3).toUpperCase();
    const date = new Date().toISOString().slice(0, 10).replace(/-/g, '');
    const random = Math.floor(Math.random() * 1000).toString().padStart(3, '0');
    return `${prefix}${date}${random}`;
}

// Créer le fichier template
async function createTemplateFile(nom, content) {
    try {
        const activeFile = app.workspace.getActiveFile();
        const targetFolder = activeFile.parent;
        const fileName = `template-${nom.replace(/\s+/g, '-').toLowerCase()}.md`;
        const filePath = `${targetFolder.path}/${fileName}`;
        
        log(`Création du fichier: ${filePath}`);
        
        // Vérifier si le fichier existe déjà
        const existingFile = app.vault.getAbstractFileByPath(filePath);
        if (existingFile) {
            const overwrite = await utils.inputPrompt(
                "Fichier existant",
                `Le fichier "${fileName}" existe déjà. Voulez-vous l'écraser ? (oui/non)`,
                "non"
            );
            
            if (overwrite?.toLowerCase() !== "oui") {
                log("❌ STOP: Création du template annulée par l'utilisateur");
                new Notice("❌ Création annulée");
                return null;
            }
        }
        
        const file = await app.vault.create(filePath, content);
        log(`✅ Fichier créé: ${fileName}`);
        
        return file;
        
    } catch (error) {
        log(`❌ Erreur création fichier: ${error.message}`, "error");
        throw new Error(`Impossible de créer le fichier: ${error.message}`);
    }
}

// Ajouter le texte du type dans le dessin actuel
async function addTypeToCurrentDrawing(type, nom) {
    try {
        log("Ajout du texte dans le dessin courant");
        
        // Réinitialiser EA pour travailler avec le dessin actuel
        ea.reset();
        ea.setView();
        
        // Obtenir les éléments existants pour positionner le texte
        const elements = ea.getViewElements();
        if (elements.length === 0) {
            log("⚠️ Aucun élément trouvé pour positionner le texte");
            return;
        }
        
        const firstElement = elements[0];
        log(`Element de référence trouvé: ${firstElement.type} at (${firstElement.x}, ${firstElement.y})`);
        
        // Position du texte (au-dessus de l'élément existant)
        const textX = firstElement.x;
        const textY = firstElement.y - 60;
        
        // Configurer le style du texte
        ea.style.strokeColor = COULEURS[type] || "#333333";
        ea.style.fontSize = 16;
        ea.style.fontFamily = 1;
        
        // Créer le texte du type
        const typeText = ea.addText(textX, textY, `${type}: {{${nom}}}`, {
            textAlign: "left",
            verticalAlign: "middle"
        });
        
        // Appliquer les changements au dessin actuel
        await ea.addElementsToView(false, true);
        
        log(`✅ Texte ajouté au dessin: ${type}: {{${nom}}}`);
        
    } catch (error) {
        log(`❌ Erreur ajout texte: ${error.message}`, "error");
        console.error("Stack trace:", error.stack);
        // Ne pas bloquer le processus pour cette erreur
    }
}

// Ajouter l'embed dans le dessin actuel
async function addEmbedToCurrentDrawing(templateFile) {
    try {
        log("Création de l'embed dans le dessin courant");
        
        // Réinitialiser EA pour travailler avec le dessin actuel
        ea.reset();
        ea.setView();
        
        // Obtenir les éléments actuels pour positionner l'embed
        const elements = ea.getViewElements();
        if (elements.length === 0) {
            log("⚠️ Aucun élément trouvé pour positionner l'embed");
            return;
        }
        
        // Chercher l'élément le plus à droite pour positionner l'embed
        let rightmostElement = elements[0];
        for (const element of elements) {
            if (element.x + element.width > rightmostElement.x + rightmostElement.width) {
                rightmostElement = element;
            }
        }
        
        log(`Element de référence pour embed: ${rightmostElement.type} at (${rightmostElement.x}, ${rightmostElement.y})`);
        
        // Position de l'embed (à droite de l'élément le plus à droite)
        const embedX = rightmostElement.x + rightmostElement.width + 50;
        const embedY = rightmostElement.y;
        
        log(`Position embed calculée: (${embedX}, ${embedY})`);
        
        // Créer l'embed avec le lien vers le template
        const embedId = ea.addEmbeddable(embedX, embedY, 400, 300, templateFile.path);
        
        log(`Embed créé avec ID: ${embedId}`);
        log(`Lien template: ${templateFile.path}`);
        
        // Appliquer les changements au dessin actuel
        await ea.addElementsToView(false, true);
        
        log("✅ Embed ajouté au dessin courant");
        
    } catch (error) {
        log(`❌ Erreur création embed: ${error.message}`, "error");
        console.error("Stack trace:", error.stack);
        throw new Error(`Impossible de créer l'embed: ${error.message}`);
    }
}

// === EXÉCUTION ===
console.log("🚀 === DÉMARRAGE SCRIPT BackOfTheCard ===");
console.log("📋 Objects disponibles - ea:", typeof ea, "- utils:", typeof utils);
console.log("🔧 Config debug:", CONFIG.debug);

main().catch(error => {
    console.error("❌ Erreur fatale:", error);
    console.error("🔍 Stack trace:", error.stack);
    new Notice("❌ Erreur fatale du script");
});