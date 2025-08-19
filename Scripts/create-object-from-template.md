/*
excalidraw-script-icon: 📦
Script Excalidraw pour créer un OBJET ProcessMetaLanguage depuis template BackOfTheCard
Basé sur embed-object-from-template.md v1.3.0
Version: 1.1.1
Date: 2025-01-18
Auteur: Rolland MELET & Claude Code

Ce script:
- Utilise le template-objet-backofcard.md
- Collecte les propriétés selon leur type (USER/SYSTEM/DEFAULT)
- Respecte les trois types d'objets workflow (RESSOURCE/CONSOMMABLE/PRODUIT)
- Crée un embed dans Excalidraw avec la bonne couleur selon le type

Changelog v1.1.1:
- Correction syntaxe manque virgule après tags

Changelog v1.1.0:
- Ajout titre dans les popups
- Rappel du type choisi dans les prompts suivants
- Correction bug catégorie qui affichait "false"
- Logging détaillé dans la console
- Meilleur chemin du template

```javascript
*/

// === CONFIGURATION ===
const TEMPLATE_PATH = "/Users/rollandmelet/Library/CloudStorage/GoogleDrive-rm@360sc.io/Mon Drive/OBSIDIAN/CHATTERS/002 - Projets/PERSO/ProcessMetaLanguage/Templates/BackOfCard/template-objet-backofcard.md";
const OBJECTS_FOLDER = "ProcessMetaLanguage/Objets";
const STORAGE_KEY = "processmetalanguage-object-preferences";
const DEBUG = true; // Active les logs détaillés

// === INTERNATIONALIZATION ===
const getUserLanguage = () => {
    const lang = moment?.locale() || 
                 navigator.language || 
                 app.locale || 
                 'en';
    return lang.startsWith('fr') ? 'fr' : 'en';
};

const lang = getUserLanguage();

// Translation dictionary
const i18n = {
    fr: {
        // Titres popups
        popupTitleType: "Choisir le type de l'objet",
        popupTitleName: "Nom de l'objet [TYPE]",
        popupTitleDescription: "Description de l'objet [TYPE]",
        popupTitleCategory: "Catégorie métier de l'objet [TYPE]",
        
        // Sélection type
        selectObjectType: "Quel type d'objet voulez-vous créer?",
        typeResource: "🔄 RESSOURCE - Objet réutilisable (moule, équipement)",
        typeConsumable: "📦 CONSOMMABLE - Élément consommé (matière, composant)",
        typeProduct: "🏭 PRODUIT - Objet produit (voussoir, PAC)",
        
        // Propriétés générales
        objectName: "Nom de l'objet ([TYPE]):",
        objectDescription: "Description ([TYPE]) - optionnelle:",
        objectCategory: "Catégorie métier ([TYPE]):",
        businessProperties: "Propriétés métier JSON ([TYPE]) - optionnel:",
        tags: "Tags ([TYPE]) - séparés par virgules:",
        
        // Propriétés CONSOMMABLE
        stockQuantity: "Quantité en stock:",
        batchNumber: "Numéro de lot:",
        expiryDate: "Date de péremption (AAAA-MM-JJ, optionnel):",
        
        // Messages
        creatingObject: "Création de l'objet en cours...",
        objectCreated: "✅ Objet",
        createdWith: "créé avec type",
        error: "Erreur",
        cancelled: "Création annulée",
        noActiveFile: "Aucun fichier Excalidraw actif",
        errorCreatingFile: "Impossible de créer le fichier",
        fileExists: "existe déjà. Choisir:",
        enterCustomName: "💡 Entrer un autre nom...",
        suggestionsAvailable: "Suggestions disponibles:",
        enterNewName: "Nouveau nom:",
        templateNotFound: "Template non trouvé",
        invalidJson: "JSON invalide, utilisation de {}",
        scriptStarting: "Création d'objet ProcessMetaLanguage v1.0.0",
        nameAlreadyExists: "(nom existant)"
    },
    en: {
        // Popup titles
        popupTitleType: "Choose the object type",
        popupTitleName: "Object name [TYPE]",
        popupTitleDescription: "Object description [TYPE]",
        popupTitleCategory: "Business category [TYPE]",
        
        // Type selection
        selectObjectType: "What type of object do you want to create?",
        typeResource: "🔄 RESOURCE - Reusable object (mold, equipment)",
        typeConsumable: "📦 CONSUMABLE - Consumed element (material, component)",
        typeProduct: "🏭 PRODUCT - Produced object (segment, PAC)",
        
        // General properties
        objectName: "Object name ([TYPE]):",
        objectDescription: "Description ([TYPE]) - optional:",
        objectCategory: "Business category ([TYPE]):",
        businessProperties: "Business properties JSON ([TYPE]) - optional:",
        tags: "Tags ([TYPE]) - comma-separated:",
        
        // CONSUMABLE properties
        stockQuantity: "Stock quantity:",
        batchNumber: "Batch number:",
        expiryDate: "Expiry date (YYYY-MM-DD, optional):",
        
        // Messages
        creatingObject: "Creating object...",
        objectCreated: "✅ Object",
        createdWith: "created with type",
        error: "Error",
        cancelled: "Creation cancelled",
        noActiveFile: "No active Excalidraw file",
        errorCreatingFile: "Unable to create file",
        fileExists: "already exists. Choose:",
        enterCustomName: "💡 Enter another name...",
        suggestionsAvailable: "Available suggestions:",
        enterNewName: "New name:",
        templateNotFound: "Template not found",
        invalidJson: "Invalid JSON, using {}",
        scriptStarting: "ProcessMetaLanguage Object Creation v1.0.0",
        nameAlreadyExists: "(name exists)"
    }
};

const t = (key) => i18n[lang][key] || i18n['en'][key];

// === LOGGING FUNCTION ===
function log(message, data = null) {
    if (DEBUG) {
        const timestamp = new Date().toISOString().split('T')[1].substring(0, 8);
        if (data) {
            console.log(`[${timestamp}] ProcessMetaLanguage: ${message}`, data);
        } else {
            console.log(`[${timestamp}] ProcessMetaLanguage: ${message}`);
        }
    }
}

// === HELPER FUNCTIONS ===

// Generate unique file ID (40 chars)
function generateFileId() {
    const chars = 'abcdef0123456789';
    let result = '';
    for (let i = 0; i < 40; i++) {
        result += chars[Math.floor(Math.random() * chars.length)];
    }
    return result;
}

// Generate UUID v4
function generateUUID() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        const r = Math.random() * 16 | 0;
        const v = c === 'x' ? r : (r & 0x3 | 0x8);
        return v.toString(16);
    });
}

// Generate UID for object
function generateUID() {
    const date = moment().format('YYYYMMDD');
    const index = Math.floor(Math.random() * 1000).toString().padStart(3, '0');
    return `OBJ${date}${index}`;
}

// Get template content
async function getTemplateContent() {
    try {
        log("Recherche du template:", TEMPLATE_PATH);
        
        // Essayer plusieurs chemins possibles
        const possiblePaths = [
            TEMPLATE_PATH,
            "Templates/BackOfCard/template-objet-backofcard.md",
            "ProcessMetaLanguage/Templates/BackOfCard/template-objet-backofcard.md"
        ];
        
        let templateFile = null;
        let foundPath = null;
        
        for (const path of possiblePaths) {
            log(`Tentative avec le chemin: ${path}`);
            templateFile = app.vault.getAbstractFileByPath(path);
            if (templateFile) {
                foundPath = path;
                break;
            }
        }
        
        if (!templateFile) {
            log("ERREUR: Template non trouvé dans aucun des chemins testés");
            new Notice(t('templateNotFound'));
            return null;
        }
        
        log(`Template trouvé à: ${foundPath}`);
        const content = await app.vault.read(templateFile);
        log(`Template chargé, taille: ${content.length} caractères`);
        return content;
        
    } catch (e) {
        log("ERREUR lors de la lecture du template:", e);
        console.error("Error reading template:", e);
        return null;
    }
}

// Get current view center
function getViewCenter(excalidrawContent) {
    try {
        const appStateMatch = excalidrawContent.match(/"appState":\s*{([^}]*)}/);
        if (appStateMatch) {
            const appStateStr = `{${appStateMatch[1]}}`;
            const scrollXMatch = appStateStr.match(/"scrollX":\s*([-\d.]+)/);
            const scrollYMatch = appStateStr.match(/"scrollY":\s*([-\d.]+)/);
            const zoomMatch = appStateStr.match(/"zoom":\s*{[^}]*"value":\s*([\d.]+)/);
            
            if (scrollXMatch && scrollYMatch) {
                const scrollX = parseFloat(scrollXMatch[1]);
                const scrollY = parseFloat(scrollYMatch[1]);
                const zoom = zoomMatch ? parseFloat(zoomMatch[1]) : 1;
                
                const viewportWidth = 800;
                const viewportHeight = 600;
                
                const centerX = -scrollX + (viewportWidth / 2) / zoom;
                const centerY = -scrollY + (viewportHeight / 2) / zoom;
                
                return { x: centerX, y: centerY };
            }
        }
    } catch (e) {
        // Return default on error
    }
    return { x: 0, y: 0 };
}

// Generate unique filename
async function getUniqueFileName(baseName, folder) {
    let fileName = `${baseName}.md`;
    let filePath = folder ? `${folder.path}/${fileName}` : fileName;
    
    if (!app.vault.getAbstractFileByPath(filePath)) {
        return { name: baseName, fileName: fileName, filePath: filePath };
    }
    
    const choices = [];
    
    // Windows style: name(1), name(2)
    for (let i = 1; i <= 3; i++) {
        const suggestion = `${baseName}(${i})`;
        const suggestionPath = folder ? 
            `${folder.path}/${suggestion}.md` : 
            `${suggestion}.md`;
        
        if (!app.vault.getAbstractFileByPath(suggestionPath)) {
            choices.push(suggestion);
            if (choices.length >= 2) break;
        }
    }
    
    // Version style
    const versionName = `${baseName}_v2`;
    const versionPath = folder ? 
        `${folder.path}/${versionName}.md` : 
        `${versionName}.md`;
    if (!app.vault.getAbstractFileByPath(versionPath)) {
        choices.push(versionName);
    }
    
    choices.push(t('enterCustomName'));
    
    // Selection menu
    let selected;
    try {
        if (typeof utils !== 'undefined' && utils.suggester) {
            const displayLabels = choices.map(choice => {
                if (choice === t('enterCustomName')) {
                    return choice;
                }
                return `✓ ${choice}`;
            });
            
            selected = await utils.suggester(
                displayLabels,
                choices,
                false,
                `"${baseName}.md" ${t('fileExists')}`
            );
        } else {
            selected = choices[0];
            new Notice(`${t('fileCreatedAs')} "${selected}.md" ${t('nameAlreadyExists')}`);
        }
    } catch (e) {
        console.error("Error in suggester:", e);
        selected = choices[0];
    }
    
    if (!selected) {
        return null;
    }
    
    if (selected === t('enterCustomName')) {
        let customName = await utils.inputPrompt(
            `"${baseName}.md" ${t('fileExists')}\n\n` +
            `${t('suggestionsAvailable')} ${choices.slice(0, -1).join(", ")}\n\n` +
            `${t('enterNewName')}`,
            baseName + "_new"
        );
        
        if (!customName) {
            return null;
        }
        
        return await getUniqueFileName(customName, folder);
    }
    
    const selectedFileName = `${selected}.md`;
    const selectedFilePath = folder ? 
        `${folder.path}/${selectedFileName}` : 
        selectedFileName;
    
    return { 
        name: selected, 
        fileName: selectedFileName, 
        filePath: selectedFilePath 
    };
}

// Get color by object type
function getColorByType(type) {
    const colors = {
        "RESSOURCE": "#52C41A",   // Vert
        "CONSOMMABLE": "#FA8C16",  // Orange  
        "PRODUIT": "#1890FF"       // Bleu
    };
    return colors[type] || "#4A90E2";
}

// Get icon by object type
function getIconByType(type) {
    const icons = {
        "RESSOURCE": "🔄",
        "CONSOMMABLE": "📦",
        "PRODUIT": "🏭"
    };
    return icons[type] || "📦";
}

// === MAIN SCRIPT ===
(async () => {
    log("=== DÉMARRAGE DU SCRIPT ===");
    console.log(t('scriptStarting'));
    
    // Check active file
    const activeFile = app.workspace.getActiveFile();
    log("Fichier actif:", activeFile?.path);
    
    if (!activeFile) {
        log("ERREUR: Aucun fichier actif");
        new Notice(t('noActiveFile'));
        return;
    }
    
    // Step 1: Select object type avec TITRE
    log("Étape 1: Sélection du type d'objet");
    const objectTypes = [
        { label: t('typeResource'), value: "RESSOURCE" },
        { label: t('typeConsumable'), value: "CONSOMMABLE" },
        { label: t('typeProduct'), value: "PRODUIT" }
    ];
    
    let objectType;
    if (typeof utils.suggester === 'function') {
        objectType = await utils.suggester(
            objectTypes.map(t => t.label),
            objectTypes.map(t => t.value),
            false,
            t('popupTitleType')  // Utiliser le titre de popup
        );
    } else {
        // Fallback
        objectType = "PRODUIT";
        log("Suggester non disponible, utilisation du type par défaut: PRODUIT");
    }
    
    if (!objectType) {
        log("Annulation: Aucun type sélectionné");
        new Notice(t('cancelled'));
        return;
    }
    
    log(`Type sélectionné: ${objectType}`);
    
    // Step 2: Collect object properties
    const properties = {};
    
    // === USER PROPERTIES ===
    properties.type_objet_workflow = objectType;
    
    // Nom (required) - avec rappel du type
    const namePrompt = t('objectName').replace('[TYPE]', objectType);
    log("Demande du nom de l'objet");
    properties.nom = await utils.inputPrompt(namePrompt);
    if (!properties.nom) {
        log("Annulation: Pas de nom fourni");
        new Notice(t('cancelled'));
        return;
    }
    log(`Nom saisi: ${properties.nom}`);
    
    // Description (optional) - avec rappel du type
    const descPrompt = t('objectDescription').replace('[TYPE]', objectType);
    properties.description = await utils.inputPrompt(descPrompt) || "";
    log(`Description: ${properties.description || "(vide)"}`);
    
    // Category (optional) - CORRECTION du bug
    const categories = ["Composant", "Document", "Matériel", "Matière première", "Service", "Autre"];
    const catPrompt = t('objectCategory').replace('[TYPE]', objectType);
    
    log("Sélection de la catégorie");
    if (typeof utils.suggester === 'function') {
        const selectedCat = await utils.suggester(
            categories,
            categories,
            false,
            catPrompt
        );
        properties.categorie = selectedCat || "Autre";
    } else {
        properties.categorie = await utils.inputPrompt(catPrompt) || "Autre";
    }
    log(`Catégorie sélectionnée: ${properties.categorie}`);
    
    // Type-specific properties
    if (objectType === "CONSOMMABLE") {
        const stockStr = await utils.inputPrompt(t('stockQuantity'), "0");
        properties.quantite_stock = parseInt(stockStr) || 0;
        
        properties.lot_numero = await utils.inputPrompt(t('batchNumber')) || "";
        properties.date_peremption = await utils.inputPrompt(t('expiryDate')) || null;
    }
    
    // Business properties (JSON)
    const jsonInput = await utils.inputPrompt(t('businessProperties'), "{}");
    try {
        properties.proprietes_metier = jsonInput && jsonInput !== "{}" ? 
            JSON.stringify(JSON.parse(jsonInput), null, 2) : "{}";
    } catch (e) {
        new Notice(t('invalidJson'));
        properties.proprietes_metier = "{}";
    }
    
    // Tags
    const tagsInput = await utils.inputPrompt(t('tags')) || "";
    properties.tags = tagsInput || "";
    
    // === SYSTEM PROPERTIES ===
    const uid = generateUID();
    properties.id = generateUUID();
    properties.uid = uid;
    properties.url_propre = `/objects/${uid}`;
    properties.created_at = new Date().toISOString();
    properties.created_by = "User";
    properties.modified_at = new Date().toISOString();
    properties.modified_by = "User";
    properties.version = 1;
    
    // === DEFAULT PROPERTIES ===
    properties.processus_objet_id = "Non défini";
    properties.etat_actuel_id = "Non défini";
    properties.etat_actuel_nom = "Non défini";
    properties.workflow_parent_id = "Non défini";
    properties.priorite = "normale";
    properties.statut = "actif";
    properties.liens = "";
    properties.fichiers_attaches = "";
    properties.historique = `- ${new Date().toISOString()}: Création de l'objet`;
    
    // Type-specific defaults
    if (objectType === "RESSOURCE") {
        properties.cycles_utilisation = 0;
        properties.disponibilite = "disponible";
    } else if (objectType === "PRODUIT") {
        properties.date_production = "Non défini";
        properties.conformite = "Non défini";
        properties.traçabilite_composants = "";
    }
    
    // Fill empty consumable properties
    if (objectType === "CONSOMMABLE") {
        properties.quantite_stock = properties.quantite_stock || 0;
        properties.lot_numero = properties.lot_numero || "Non défini";
        properties.date_peremption = properties.date_peremption || "Non défini";
    } else {
        properties.quantite_stock = "N/A";
        properties.lot_numero = "N/A";
        properties.date_peremption = "N/A";
    }
    
    // Fill resource/product specific
    if (objectType !== "RESSOURCE") {
        properties.cycles_utilisation = "N/A";
        properties.disponibilite = "N/A";
    }
    if (objectType !== "PRODUIT") {
        properties.date_production = "N/A";
        properties.conformite = "N/A";
        properties.traçabilite_composants = "N/A";
    }
    
    // Step 3: Get unique filename
    log("Étape 3: Obtention d'un nom de fichier unique");
    const folder = activeFile.parent;
    log(`Dossier parent: ${folder?.path || "racine"}`);
    
    const fileInfo = await getUniqueFileName(properties.nom, folder);
    
    if (!fileInfo) {
        log("Annulation: Pas de nom de fichier sélectionné");
        new Notice(t('cancelled'));
        return;
    }
    
    const finalObjectName = fileInfo.name;
    const newFileName = fileInfo.fileName;
    const newFilePath = fileInfo.filePath;
    log(`Fichier final: ${newFilePath}`);
    
    try {
        // Step 4: Create file from template
        log("Étape 4: Création du fichier depuis le template");
        new Notice(t('creatingObject'));
        
        let templateContent = await getTemplateContent();
        if (!templateContent) {
            log("ERREUR: Template non trouvé, arrêt du script");
            return;
        }
        
        // Replace all placeholders
        log("Remplacement des placeholders dans le template");
        for (const [key, value] of Object.entries(properties)) {
            const regex = new RegExp(`\\$\\{${key}\\}`, 'g');
            templateContent = templateContent.replace(regex, value);
        }
        
        // Replace object name
        templateContent = templateContent.replace(/\${nom_objet}/g, finalObjectName);
        
        // Create file
        let newFile;
        try {
            log(`Création du fichier: ${newFilePath}`);
            newFile = await app.vault.create(newFilePath, templateContent);
            log(`✓ Fichier créé avec succès: ${newFilePath}`);
        } catch (createError) {
            log("ERREUR lors de la création du fichier:", createError);
            console.error("Error creating file:", createError);
            new Notice(`${t('error')}: ${t('errorCreatingFile')} "${newFileName}"`);
            return;
        }
        
        // Wait for indexing
        log("Attente de l'indexation du fichier...");
        await new Promise(resolve => setTimeout(resolve, 200));
        
        // Step 5: Create embed in Excalidraw
        log("Étape 5: Création de l'embed dans Excalidraw");
        const fileId = generateFileId();
        log(`FileID généré: ${fileId}`);
        
        // Read current Excalidraw content
        log("Lecture du contenu Excalidraw actuel");
        let excalidrawContent = await app.vault.read(activeFile);
        log(`Taille du fichier Excalidraw: ${excalidrawContent.length} caractères`);
        
        // Parse drawing JSON
        const drawingMatch = excalidrawContent.match(/# Drawing\n```json\n([\s\S]*?)\n```/);
        if (!drawingMatch) {
            log("ERREUR: Structure Excalidraw non reconnue");
            new Notice("Error: Excalidraw structure not recognized");
            return;
        }
        
        log("Parsing du JSON du drawing");
        const drawingData = JSON.parse(drawingMatch[1]);
        log(`Nombre d'éléments existants: ${drawingData.elements?.length || 0}`);
        
        // Calculate position
        const viewCenter = getViewCenter(excalidrawContent);
        log(`Centre de vue calculé: x=${viewCenter.x}, y=${viewCenter.y}`);
        let x = viewCenter.x - 200;
        let y = viewCenter.y - 150;
        
        // Cascade offset if embeds exist
        if (drawingData.elements && drawingData.elements.length > 0) {
            const embedElements = drawingData.elements.filter(el => el.type === "image" && el.fileId);
            log(`Nombre d'embeds existants: ${embedElements.length}`);
            if (embedElements.length > 0) {
                const offset = (embedElements.length % 5) * 30;
                x += offset;
                y += offset;
                log(`Position ajustée avec cascade: x=${x}, y=${y}`);
            }
        }
        
        // Create colored rectangle for object type
        const backgroundColor = getColorByType(objectType);
        const icon = getIconByType(objectType);
        log(`Couleur et icône pour ${objectType}: ${backgroundColor} ${icon}`);
        
        // Add rectangle element
        const rectElement = {
            id: generateUUID().substring(0, 16),
            type: "rectangle",
            x: x,
            y: y,
            width: 400,
            height: 100,
            angle: 0,
            strokeColor: "#1e1e1e",
            backgroundColor: backgroundColor,
            fillStyle: "solid",
            strokeWidth: 2,
            strokeStyle: "solid",
            roughness: 0,
            opacity: 100,
            groupIds: [],
            frameId: null,
            roundness: { type: 3 },
            seed: Math.floor(Math.random() * 2000000000),
            versionNonce: Math.floor(Math.random() * 2000000000),
            isDeleted: false,
            boundElements: [],
            updated: Date.now(),
            link: null,
            locked: false
        };
        
        // Add text element
        const textElement = {
            id: generateUUID().substring(0, 16),
            type: "text",
            x: x + 10,
            y: y + 25,
            width: 380,
            height: 50,
            angle: 0,
            strokeColor: "#ffffff",
            backgroundColor: "transparent",
            fillStyle: "solid",
            strokeWidth: 2,
            strokeStyle: "solid",
            roughness: 0,
            opacity: 100,
            groupIds: [],
            frameId: null,
            roundness: null,
            seed: Math.floor(Math.random() * 2000000000),
            versionNonce: Math.floor(Math.random() * 2000000000),
            isDeleted: false,
            boundElements: null,
            updated: Date.now(),
            link: null,
            locked: false,
            fontSize: 20,
            fontFamily: 1,
            text: `${icon} ${objectType}\n${finalObjectName}`,
            rawText: `${icon} ${objectType}\n${finalObjectName}`,
            textAlign: "center",
            verticalAlign: "middle",
            containerId: null,
            originalText: `${icon} ${objectType}\n${finalObjectName}`,
            lineHeight: 1.25,
            baseline: 45
        };
        
        // Create image element for embed
        const imageElement = {
            id: generateUUID().substring(0, 16),
            type: "image",
            x: x,
            y: y + 120,
            width: 400,
            height: 300,
            angle: 0,
            strokeColor: "transparent",
            backgroundColor: "transparent",
            fillStyle: "hachure",
            strokeWidth: 1,
            strokeStyle: "solid",
            roughness: 1,
            opacity: 100,
            groupIds: [],
            frameId: null,
            roundness: null,
            seed: Math.floor(Math.random() * 2000000000),
            versionNonce: Math.floor(Math.random() * 2000000000),
            isDeleted: false,
            boundElements: null,
            updated: Date.now(),
            link: null,
            locked: false,
            status: "saved",
            fileId: fileId,
            scale: [1, 1]
        };
        
        // Add elements to drawing
        log("Ajout des éléments au drawing");
        drawingData.elements = drawingData.elements || [];
        drawingData.elements.push(rectElement);
        drawingData.elements.push(textElement);
        drawingData.elements.push(imageElement);
        log(`Nouveaux éléments ajoutés: rectangle, texte et image`);
        
        // Add embedded file entry
        const embedEntry = `${fileId}: [[${finalObjectName}|100%]]`;
        log(`Entrée embed créée: ${embedEntry}`);
        
        const embeddedFilesRegex = /# Embedded files\n([\s\S]*?)(?=\n#|$)/;
        const embeddedFilesMatch = excalidrawContent.match(embeddedFilesRegex);
        
        if (embeddedFilesMatch) {
            log("Section 'Embedded files' trouvée, ajout de l'entrée");
            let existingContent = embeddedFilesMatch[1].trim();
            const newContent = existingContent ? 
                `${existingContent}\n${embedEntry}` : 
                embedEntry;
            
            excalidrawContent = excalidrawContent.replace(
                embeddedFilesRegex, 
                `# Embedded files\n${newContent}\n`
            );
        } else {
            log("Section 'Embedded files' non trouvée, création de la section");
            // Add section if doesn't exist
            const textElementsIndex = excalidrawContent.indexOf("# Text Elements");
            if (textElementsIndex > -1) {
                excalidrawContent = 
                    excalidrawContent.slice(0, textElementsIndex) + 
                    `# Embedded files\n${embedEntry}\n\n` +
                    excalidrawContent.slice(textElementsIndex);
            }
        }
        
        // Replace drawing JSON
        log("Remplacement du JSON du drawing");
        const newDrawingJson = JSON.stringify(drawingData, null, "\t");
        excalidrawContent = excalidrawContent.replace(
            /# Drawing\n```json\n[\s\S]*?\n```/,
            `# Drawing\n\`\`\`json\n${newDrawingJson}\n\`\`\``
        );
        
        // Save modifications
        log("Sauvegarde des modifications dans le fichier Excalidraw");
        await app.vault.modify(activeFile, excalidrawContent);
        
        // Force save
        log("Forcer l'écriture sur disque");
        await app.vault.adapter.write(activeFile.path, excalidrawContent);
        
        // Wait for save
        log("Attente de la sauvegarde complète...");
        await new Promise(resolve => setTimeout(resolve, 1000));
        
        // Programmatic toggle for refresh (like in original script)
        log("Tentative de rafraîchissement de la vue Excalidraw");
        try {
            const activeLeaf = app.workspace.activeLeaf;
            if (activeLeaf && activeLeaf.view) {
                const view = activeLeaf.view;
                
                if (view.getViewType && (view.getViewType() === "markdown" || view.getViewType() === "excalidraw")) {
                    log("Vue Excalidraw/Markdown détectée, toggle en cours");
                    const currentState = view.getState ? view.getState() : null;
                    
                    if (view.setMode) {
                        log("Passage en mode source");
                        await view.setMode('source');
                        await new Promise(resolve => setTimeout(resolve, 200));
                        log("Retour en mode preview");
                        await view.setMode('preview');
                    } else if (view.setState && currentState) {
                        log("Utilisation de setState pour rafraîchir");
                        await view.setState(currentState, { history: false });
                    }
                }
            }
        } catch (e) {
            log("ERREUR lors du toggle:", e);
            console.error("Toggle error:", e);
        }
        
        // Success message
        log(`=== SUCCÈS === Objet "${finalObjectName}" créé avec type ${objectType}`);
        new Notice(`${t('objectCreated')} "${finalObjectName}" ${t('createdWith')} ${objectType}!`);
        console.log(`Object created: ${finalObjectName} (Type: ${objectType})`);
        
    } catch (error) {
        log("=== ERREUR CRITIQUE ===", error);
        console.error("Error:", error);
        new Notice(`${t('error')}: ${error.message}`);
    }
    
    log("=== FIN DU SCRIPT ===");
})();

/* 
```
*/