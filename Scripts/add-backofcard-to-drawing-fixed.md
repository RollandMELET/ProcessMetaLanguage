```javascript
// === Add BackOfTheCard to Drawing ===
// Version: 1.0.1 - Fixed
// Date: 2025-01-19
// Auteur: Rolland MELET & Claude Code

// Vérification préalable
if (!ea.verifyMinimumPluginVersion("2.0.0")) {
    new Notice("⚠️ Version Excalidraw Plugin insuffisante. Minimum requis: 2.0.0");
    return;
}

// Configuration et constantes
const CONFIG = {
    version: "1.0.1",
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
function log(message, level) {
    if (CONFIG.debug) {
        const timestamp = new Date().toLocaleTimeString();
        console.log("[" + timestamp + "] BackOfTheCard: " + message);
    }
}

// Fonction principale
async function main() {
    try {
        log("=== DÉMARRAGE DU SCRIPT ===");
        log("Add BackOfTheCard to Drawing v" + CONFIG.version);

        // Étape 1: Vérifications préalables
        const activeFile = app.workspace.getActiveFile();
        if (!activeFile || !activeFile.name.endsWith(".excalidraw")) {
            new Notice("❌ Veuillez ouvrir un fichier Excalidraw");
            return;
        }

        log("Fichier actif: " + activeFile.path);

        // Vérifier qu'il y a exactement un objet dans le dessin
        const elements = ea.getViewElements();
        if (elements.length === 0) {
            new Notice("❌ Aucun élément dans le dessin. Créez d'abord un objet.");
            return;
        }
        
        if (elements.length > 1) {
            new Notice("⚠️ Le dessin doit contenir exactement UN objet. Sélectionnez ou supprimez les autres éléments.");
            return;
        }

        log("Objet trouvé dans le dessin");

        // Étape 2: Demander le nom du template
        const nomTemplate = await askForTemplateName();
        if (!nomTemplate) return;

        log("Nom du template: " + nomTemplate);

        // Étape 3: Demander le type d'objet
        const typeObjet = await selectObjectType();
        if (!typeObjet) return;

        log("Type d'objet: " + typeObjet);

        // Étape 4: Générer le template BackOfTheCard
        const templateContent = generateTemplate(nomTemplate, typeObjet);

        // Étape 5: Créer le fichier template
        const templateFile = await createTemplateFile(nomTemplate, templateContent);
        if (!templateFile) return;

        // Étape 6: Ajouter le texte du type dans le dessin
        addTypeToDrawing(typeObjet, nomTemplate);

        // Étape 7: Créer l'embed
        await createEmbed(templateFile);

        log("✅ BackOfTheCard ajouté avec succès!");
        new Notice("✅ Template \"" + nomTemplate + "\" créé et ajouté au dessin");

    } catch (error) {
        log("❌ ERREUR: " + error.message, "error");
        new Notice("❌ Erreur: " + error.message);
    }
}

// Demander le nom du template
async function askForTemplateName() {
    const result = await utils.inputPrompt(
        "Nom du Template",
        "Entrez le nom du template à créer :",
        "",
        [{
            caption: "Créer",
            action: function() { return true; }
        }]
    );
    
    if (!result || result.trim() === "") {
        new Notice("❌ Nom du template requis");
        return null;
    }
    
    return result.trim();
}

// Sélection du type d'objet
async function selectObjectType() {
    const result = await utils.suggester(
        function(item) { return item; },
        TYPES_OBJETS,
        false,
        "Sélectionnez le type d'objet ProcessMetaLanguage :"
    );
    
    return result;
}

// Générateur de template avec exemples (version simplifiée)
function generateTemplate(nom, type) {
    const uid = generateUID(type);
    const date = new Date().toISOString().split('T')[0];
    
    // Construction par concaténation simple pour éviter les conflits
    var content = "---\n";
    content += "excalidraw-plugin: parsed\n";
    content += "type: \"template-" + type.toLowerCase() + "\"\n";
    content += "template_version: \"1.0.0\"\n";
    content += "processmetalanguage_version: \"2.3\"\n";
    content += "uid: \"" + uid + "\"\n";
    content += "created: \"" + date + "\"\n";
    content += "author: \"" + CONFIG.author + "\"\n";
    content += "tags: [processmetalanguage, " + type.toLowerCase() + ", template]\n";
    content += "---\n\n";
    
    content += "# " + type + ": " + nom + "\n\n";
    
    content += "## 📋 Métadonnées\n\n";
    content += "- **UID**: " + uid + "\n";
    content += "- **Type**: " + type + "\n";
    content += "- **Créé**: " + date + "\n";
    content += "- **Statut**: Brouillon\n";
    content += "- **Version**: 1.0.0\n\n";
    
    content += "## 📝 Description\n\n";
    content += "[Décrivez ici l'objectif et le rôle de cet élément]\n\n";
    content += "**Exemple**: " + getDescriptionExample(type) + "\n\n";
    
    content += "## ⚖️ Rules (Règles de Gestion)\n\n";
    content += "### Règles Métier\n";
    content += "- [ ] " + getRuleExample(type, "business") + "\n";
    content += "- [ ] " + getRuleExample(type, "constraint") + "\n";
    content += "- [ ] " + getRuleExample(type, "validation") + "\n\n";
    
    content += "### Règles Techniques\n";
    content += "- [ ] " + getRuleExample(type, "technical") + "\n";
    content += "- [ ] " + getRuleExample(type, "performance") + "\n\n";
    
    content += "### Règles de Transition\n";
    content += "- [ ] " + getRuleExample(type, "transition") + "\n\n";
    
    content += "## 🏗️ Propriétés\n\n";
    content += "### Propriétés de Base\n";
    content += getPropertiesExample(type) + "\n\n";
    
    content += "### Propriétés Spécifiques\n";
    content += getSpecificPropertiesExample(type) + "\n\n";
    
    content += "## 🔗 Relations\n\n";
    content += "### Relations Entrantes\n";
    content += getRelationsExample(type, "incoming") + "\n\n";
    
    content += "### Relations Sortantes\n";
    content += getRelationsExample(type, "outgoing") + "\n\n";
    
    content += "## 🔄 États et Transitions\n\n";
    content += "### États Possibles\n";
    content += getStatesExample(type) + "\n\n";
    
    content += "### Conditions de Transition\n";
    content += getTransitionsExample(type) + "\n\n";
    
    content += "## 📊 Métriques et KPIs\n\n";
    content += "### Indicateurs de Performance\n";
    content += getMetricsExample(type) + "\n\n";
    
    content += "## 🔍 Points de Contrôle\n\n";
    content += "### Validations Requises\n";
    content += getValidationExample(type) + "\n\n";
    
    content += "## 📚 Documentation\n\n";
    content += "### Références\n";
    content += "- [Lien vers documentation métier]\n";
    content += "- [Lien vers spécifications techniques]\n\n";
    
    content += "### Exemples d'Usage\n";
    content += getUsageExample(type) + "\n\n";
    
    content += "## 📝 Notes\n\n";
    content += "### À Compléter\n";
    content += "- [ ] Finaliser les règles métier\n";
    content += "- [ ] Valider les propriétés\n";
    content += "- [ ] Tester les transitions\n";
    content += "- [ ] Documenter les cas d'usage\n\n";
    
    content += "### Historique\n";
    content += "- " + date + ": Création du template\n\n";
    
    content += "---\n";
    content += "**Template généré par ProcessMetaLanguage v" + CONFIG.version + "**\n";
    
    return content;
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
    return rules[ruleType] || "Règle à définir";
}

function getPropertiesExample(type) {
    return "- **nom**: Nom de l'élément\n" +
           "- **description**: Description détaillée\n" +
           "- **statut**: État courant (actif, inactif, suspendu)\n" +
           "- **propriétaire**: Responsable de l'élément\n" +
           "- **date_creation**: Date de création\n" +
           "- **date_modification**: Dernière modification";
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
    return relations[direction] || "- **relation**: [Type] - Description";
}

function getStatesExample(type) {
    const states = {
        "OBJET": "- **Créé**: État initial après création\n- **En traitement**: Objet en cours de traitement\n- **Validé**: Objet approuvé\n- **Terminé**: État final",
        "PROCESSUS": "- **Inactif**: Processus non démarré\n- **Actif**: Processus en cours d'exécution\n- **Suspendu**: Processus temporairement arrêté\n- **Terminé**: Processus achevé"
    };
    return states[type] || "- **État_1**: Description\n- **État_2**: Description";
}

function getTransitionsExample(type) {
    return "- **Condition_1**: Si [condition] alors [action]\n" +
           "- **Condition_2**: Quand [événement] alors [transition]\n" +
           "- **Règle_métier**: Selon [règle] autoriser [passage]";
}

function getMetricsExample(type) {
    return "- **Temps_moyen**: Durée moyenne de traitement\n" +
           "- **Taux_succès**: Pourcentage de succès\n" +
           "- **Nombre_instances**: Instances actives";
}

function getValidationExample(type) {
    return "- **Validation_1**: Vérifier l'intégrité des données\n" +
           "- **Validation_2**: Contrôler les permissions\n" +
           "- **Validation_3**: Valider les règles métier";
}

function getUsageExample(type) {
    return "### Scénario Type\n" +
           "1. **Déclenchement**: [Événement déclencheur]\n" +
           "2. **Traitement**: [Étapes de traitement]\n" +
           "3. **Résultat**: [Résultat attendu]\n\n" +
           "### Cas Particuliers\n" +
           "- **Exception_1**: [Gestion du cas particulier]\n" +
           "- **Exception_2**: [Alternative ou contournement]";
}

// Générer un UID unique
function generateUID(type) {
    const prefix = type.substring(0, 3).toUpperCase();
    const date = new Date().toISOString().slice(0, 10).replace(/-/g, '');
    const random = Math.floor(Math.random() * 1000).toString().padStart(3, '0');
    return prefix + date + random;
}

// Créer le fichier template
async function createTemplateFile(nom, content) {
    try {
        const activeFile = app.workspace.getActiveFile();
        const targetFolder = activeFile.parent;
        const fileName = "template-" + nom.replace(/\s+/g, '-').toLowerCase() + ".md";
        const filePath = targetFolder.path + "/" + fileName;
        
        log("Création du fichier: " + filePath);
        
        // Vérifier si le fichier existe déjà
        const existingFile = app.vault.getAbstractFileByPath(filePath);
        if (existingFile) {
            const overwrite = await utils.inputPrompt(
                "Fichier existant",
                "Le fichier \"" + fileName + "\" existe déjà. Voulez-vous l'écraser ? (oui/non)",
                "non"
            );
            
            if (overwrite && overwrite.toLowerCase() !== "oui") {
                new Notice("❌ Création annulée");
                return null;
            }
        }
        
        const file = await app.vault.create(filePath, content);
        log("✅ Fichier créé: " + fileName);
        
        return file;
        
    } catch (error) {
        log("❌ Erreur création fichier: " + error.message, "error");
        throw new Error("Impossible de créer le fichier: " + error.message);
    }
}

// Ajouter le type dans le dessin
function addTypeToDrawing(type, nom) {
    try {
        log("Ajout du texte dans le dessin");
        
        // Obtenir les éléments existants pour positionner le texte
        const elements = ea.getViewElements();
        const firstElement = elements[0];
        
        // Position du texte (au-dessus de l'élément existant)
        const textX = firstElement.x;
        const textY = firstElement.y - 60;
        
        // Créer le texte du type
        const typeText = ea.addText(textX, textY, type + ": {{" + nom + "}}", {
            fontSize: 16,
            fontFamily: 1,
            textAlign: "left",
            verticalAlign: "middle",
            strokeColor: COULEURS[type] || "#333333"
        });
        
        log("✅ Texte ajouté: " + type + ": {{" + nom + "}}");
        
    } catch (error) {
        log("⚠️ Erreur ajout texte: " + error.message, "error");
        // Ne pas bloquer le processus pour cette erreur
    }
}

// Créer l'embed
async function createEmbed(templateFile) {
    try {
        log("Création de l'embed");
        
        // Obtenir les éléments pour positionner l'embed
        const elements = ea.getViewElements();
        const firstElement = elements[0];
        
        // Position de l'embed (à droite de l'élément)
        const embedX = firstElement.x + firstElement.width + 50;
        const embedY = firstElement.y;
        
        // Créer l'embed
        const embed = ea.addEmbeddable(embedX, embedY, 400, 300, templateFile.path);
        
        // Finaliser le dessin
        await ea.addElementsToView();
        
        log("✅ Embed créé et ajouté au dessin");
        
    } catch (error) {
        log("❌ Erreur création embed: " + error.message, "error");
        throw new Error("Impossible de créer l'embed: " + error.message);
    }
}

// === EXÉCUTION ===
main().catch(function(error) {
    console.error("Erreur fatale:", error);
    new Notice("❌ Erreur fatale du script");
});
```