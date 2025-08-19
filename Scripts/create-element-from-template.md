/*
excalidraw-script-icon: 🎨
Script Excalidraw pour créer des éléments ProcessMetaLanguage depuis templates BackOfTheCard
Version: 2.0.0
Date: 2025-01-18
Auteur: Rolland MELET & Claude Code

Ce script:
- Charge le template approprié depuis le dossier défini dans les settings Obsidian
- Analyse le template pour identifier les propriétés à collecter
- Crée le fichier embed dans le dossier du fichier Excalidraw actif
- Utilise les éléments visuels du template
- Permet à l'utilisateur de choisir la couleur

Architecture Template-First:
1. Sélection du type d'élément
2. Chargement du template correspondant
3. Analyse des propriétés USER
4. Collecte dynamique
5. Création locale du fichier

```javascript
*/

// === CONFIGURATION ===
const CONFIG = {
    DEBUG: true,
    VERSION: "2.0.0",
    
    // Types d'éléments ProcessMetaLanguage disponibles
    ELEMENT_TYPES: {
        "objet": {
            label: "📦 OBJET - Élément manipulé par le workflow",
            templateName: "template-objet-backofcard"
        },
        "etat": {
            label: "🏳️ ÉTAT - Situation dans un processus",
            templateName: "template-etat-backofcard"
        },
        "comportement": {
            label: "🎭 COMPORTEMENT - Pattern réutilisable",
            templateName: "template-comportement-backofcard"
        },
        "action": {
            label: "⚙️ ACTION - Déclencheur de transition",
            templateName: "template-action-backofcard"
        },
        "vue": {
            label: "📱 VUE/AFFICHAGE - Interface utilisateur",
            templateName: "template-vue-backofcard"
        },
        "processus": {
            label: "📄 PROCESSUS - Cycle de vie d'un objet",
            templateName: "template-processus-backofcard"
        },
        "workflow": {
            label: "🏗️ WORKFLOW - Orchestration de processus",
            templateName: "template-workflow-backofcard"
        }
    },
    
    // Palette de couleurs disponibles
    COLORS: [
        { name: "Bleu", value: "#1890FF", emoji: "🔵" },
        { name: "Vert", value: "#52C41A", emoji: "🟢" },
        { name: "Orange", value: "#FA8C16", emoji: "🟠" },
        { name: "Rouge", value: "#F5222D", emoji: "🔴" },
        { name: "Violet", value: "#722ED1", emoji: "🟣" },
        { name: "Cyan", value: "#13C2C2", emoji: "🔵" },
        { name: "Rose", value: "#EB2F96", emoji: "🩷" },
        { name: "Jaune", value: "#FADB14", emoji: "🟡" },
        { name: "Gris", value: "#8C8C8C", emoji: "⚫" }
    ]
};

// === INTERNATIONALISATION ===
const getUserLanguage = () => {
    const lang = moment?.locale() || 
                 navigator.language || 
                 app.locale || 
                 'en';
    return lang.startsWith('fr') ? 'fr' : 'en';
};

const lang = getUserLanguage();

const i18n = {
    fr: {
        // Titres et messages principaux
        selectElementType: "Quel type d'élément ProcessMetaLanguage voulez-vous créer ?",
        selectColor: "Choisissez la couleur de l'élément",
        loadingTemplate: "Chargement du template...",
        analysingTemplate: "Analyse du template...",
        creatingElement: "Création de l'élément...",
        elementCreated: "✅ Élément créé avec succès",
        
        // Erreurs
        error: "❌ Erreur",
        noActiveFile: "Aucun fichier Excalidraw actif",
        templateNotFound: "Template non trouvé",
        invalidTemplate: "Template invalide ou incomplet",
        cancelled: "Création annulée",
        noTemplatesFolder: "Dossier de templates non configuré dans les settings",
        
        // Messages de log
        scriptStarting: "ProcessMetaLanguage Element Creator v2.0.0",
        selectedType: "Type sélectionné",
        templatePath: "Chemin du template",
        templateLoaded: "Template chargé",
        propertiesFound: "Propriétés USER trouvées",
        collectingProperty: "Collecte de la propriété",
        fileCreated: "Fichier créé",
        embedAdded: "Embed ajouté au canvas"
    },
    en: {
        // Main titles and messages
        selectElementType: "What type of ProcessMetaLanguage element do you want to create?",
        selectColor: "Choose the element color",
        loadingTemplate: "Loading template...",
        analysingTemplate: "Analyzing template...",
        creatingElement: "Creating element...",
        elementCreated: "✅ Element created successfully",
        
        // Errors
        error: "❌ Error",
        noActiveFile: "No active Excalidraw file",
        templateNotFound: "Template not found",
        invalidTemplate: "Invalid or incomplete template",
        cancelled: "Creation cancelled",
        noTemplatesFolder: "Templates folder not configured in settings",
        
        // Log messages
        scriptStarting: "ProcessMetaLanguage Element Creator v2.0.0",
        selectedType: "Selected type",
        templatePath: "Template path",
        templateLoaded: "Template loaded",
        propertiesFound: "USER properties found",
        collectingProperty: "Collecting property",
        fileCreated: "File created",
        embedAdded: "Embed added to canvas"
    }
};

const t = (key) => i18n[lang][key] || i18n['en'][key];

// === LOGGING ===
function log(message, data = null) {
    if (CONFIG.DEBUG) {
        const timestamp = new Date().toISOString().split('T')[1].substring(0, 8);
        if (data) {
            console.log(`[${timestamp}] ProcessMetaLanguage v2: ${message}`, data);
        } else {
            console.log(`[${timestamp}] ProcessMetaLanguage v2: ${message}`);
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

// Generate UID for elements
function generateUID(type) {
    const prefix = type.substring(0, 3).toUpperCase();
    const date = moment().format('YYYYMMDD');
    const index = Math.floor(Math.random() * 1000).toString().padStart(3, '0');
    return `${prefix}${date}${index}`;
}

// Get view center from Excalidraw
function getViewCenter(excalidrawContent) {
    try {
        const appStateMatch = excalidrawContent.match(/"appState":\s*{([^}]*)}/);
        if (appStateMatch) {
            const appStateStr = `{${appStateMatch[1]}}`;
            const scrollXMatch = appStateStr.match(/"scrollX":\s*([-\d.]+)/);
            const scrollYMatch = appStateStr.match(/"scrollY":\s*([-\d.]+)/);
            const zoomMatch = appStateStr.match(/"zoom":\s*{\s*"value":\s*([\d.]+)/);
            
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
        log("Erreur calcul centre vue:", e);
    }
    return { x: 0, y: 0 };
}

// === MAIN FUNCTIONS ===

// 1. Select element type
async function selectElementType() {
    log("Étape 1: Sélection du type d'élément");
    
    const options = Object.entries(CONFIG.ELEMENT_TYPES).map(([key, config]) => ({
        key: key,
        label: config.label
    }));
    
    const selected = await utils.suggester(
        options.map(o => o.label),
        options.map(o => o.key),
        false,
        t('selectElementType')
    );
    
    if (!selected) {
        throw new Error(t('cancelled'));
    }
    
    log(`${t('selectedType')}: ${selected}`);
    return selected;
}

// 2. Load template from Obsidian settings path
async function loadTemplate(elementType) {
    log("Étape 2: Chargement du template");
    
    // Récupérer le dossier de templates depuis les settings Obsidian
    const templatesFolder = app.vault.config.templatesFolder || app.vault.config.templaterFolder || "Templates";
    
    if (!templatesFolder) {
        throw new Error(t('noTemplatesFolder'));
    }
    
    const templateName = CONFIG.ELEMENT_TYPES[elementType].templateName;
    const templatePath = `${templatesFolder}/BackOfCard/${templateName}.md`;
    
    log(`${t('templatePath')}: ${templatePath}`);
    
    const templateFile = app.vault.getAbstractFileByPath(templatePath);
    if (!templateFile) {
        // Essayer sans le sous-dossier BackOfCard
        const altPath = `${templatesFolder}/${templateName}.md`;
        const altFile = app.vault.getAbstractFileByPath(altPath);
        
        if (!altFile) {
            throw new Error(`${t('templateNotFound')}: ${templatePath}`);
        }
        
        return await app.vault.read(altFile);
    }
    
    const content = await app.vault.read(templateFile);
    log(`${t('templateLoaded')}, taille: ${content.length} caractères`);
    
    return content;
}

// 3. Parse template to extract metadata
function parseTemplate(content) {
    log("Étape 3: Analyse du template");
    
    // Extraire le front matter YAML
    const frontMatterMatch = content.match(/^---\n([\s\S]*?)\n---/);
    if (!frontMatterMatch) {
        throw new Error(t('invalidTemplate') + " - No front matter");
    }
    
    // Parser le YAML manuellement (version simplifiée)
    const yaml = frontMatterMatch[1];
    const propertyDefinitions = {};
    
    // Extraire les property_definitions
    const propDefMatch = yaml.match(/property_definitions:\s*\n([\s\S]*?)(?=\n[a-z_]+:|$)/);
    if (propDefMatch) {
        const propDefSection = propDefMatch[1];
        
        // Parser chaque propriété
        const propMatches = propDefSection.matchAll(/^\s{2}(\w+):\s*\n([\s\S]*?)(?=\n\s{2}\w+:|$)/gm);
        for (const match of propMatches) {
            const propName = match[1];
            const propContent = match[2];
            
            // Extraire les attributs de la propriété
            const prop = {
                source: propContent.match(/source:\s*"([^"]+)"/)?.[1] || "default",
                dynamic: propContent.match(/dynamic:\s*(true|false)/)?.[1] === "true",
                required: propContent.match(/required:\s*(true|false)/)?.[1] === "true",
                prompt: propContent.match(/prompt:\s*"([^"]+)"/)?.[1],
                options: null,
                type: propContent.match(/type:\s*"([^"]+)"/)?.[1] || "string",
                default_value: propContent.match(/default_value:\s*"?([^"\n]+)"?/)?.[1]
            };
            
            // Extraire les options si présentes
            const optionsMatch = propContent.match(/options:\s*\[([^\]]+)\]/);
            if (optionsMatch) {
                prop.options = optionsMatch[1].split(',').map(o => o.trim().replace(/["\s]/g, ''));
            }
            
            propertyDefinitions[propName] = prop;
        }
    }
    
    // Extraire les éléments du drawing
    const drawingMatch = content.match(/# Drawing\n```json\n([\s\S]*?)\n```/);
    let drawingElements = [];
    if (drawingMatch) {
        try {
            const drawing = JSON.parse(drawingMatch[1]);
            drawingElements = drawing.elements || [];
        } catch (e) {
            log("Erreur parsing drawing JSON:", e);
        }
    }
    
    log(`${t('propertiesFound')}: ${Object.keys(propertyDefinitions).filter(k => propertyDefinitions[k].source === "user").length}`);
    
    return {
        propertyDefinitions: propertyDefinitions,
        drawingElements: drawingElements,
        templateContent: content
    };
}

// 4. Collect USER properties from template
async function collectUserProperties(template, elementType) {
    log("Étape 4: Collecte des propriétés utilisateur");
    
    const properties = {};
    
    // Générer les propriétés SYSTEM d'abord
    properties.id = generateUUID();
    properties.uid = generateUID(elementType);
    properties.created_at = new Date().toISOString();
    properties.created_by = "User";
    properties.modified_at = new Date().toISOString();
    properties.modified_by = "User";
    properties.version = 1;
    
    // Collecter les propriétés USER définies dans le template
    for (const [key, def] of Object.entries(template.propertyDefinitions)) {
        if (def.source === "user") {
            log(`${t('collectingProperty')}: ${key}`);
            
            let value = null;
            
            if (def.options && def.options.length > 0) {
                // Propriété avec options prédéfinies
                value = await utils.suggester(
                    def.options,
                    def.options,
                    false,
                    def.prompt || `${key}:`
                );
            } else if (def.type === "boolean") {
                // Propriété booléenne
                value = await utils.confirm(def.prompt || `${key}?`);
            } else {
                // Propriété texte/autre
                value = await utils.inputPrompt(
                    def.prompt || `${key}:`,
                    def.default_value || ""
                );
            }
            
            // Validation requise
            if (def.required && !value) {
                throw new Error(`${key} est obligatoire`);
            }
            
            properties[key] = value || def.default_value || "";
        } else if (def.source === "default") {
            // Appliquer les valeurs par défaut
            properties[key] = def.default_value || null;
        }
    }
    
    return properties;
}

// 5. Select color for the element
async function selectColor() {
    log("Étape 5: Sélection de la couleur");
    
    const options = CONFIG.COLORS.map(c => `${c.emoji} ${c.name}`);
    
    const selectedIndex = await utils.suggester(
        options,
        CONFIG.COLORS.map((_, i) => i),
        false,
        t('selectColor')
    );
    
    if (selectedIndex === undefined) {
        // Couleur par défaut si annulation
        return CONFIG.COLORS[0];
    }
    
    const color = CONFIG.COLORS[selectedIndex];
    log(`Couleur sélectionnée: ${color.name} (${color.value})`);
    
    return color;
}

// 6. Create embed file in current folder
async function createEmbedFile(template, properties, elementType) {
    log("Étape 6: Création du fichier embed");
    
    const activeFile = app.workspace.getActiveFile();
    if (!activeFile) {
        throw new Error(t('noActiveFile'));
    }
    
    // Créer dans le dossier du fichier Excalidraw actif
    const targetFolder = activeFile.parent;
    
    // Nom du fichier basé sur la propriété 'nom' ou 'name'
    const elementName = properties.nom || properties.name || properties.titre || `${elementType}_${properties.uid}`;
    const fileName = `${elementName}.md`;
    const filePath = targetFolder ? `${targetFolder.path}/${fileName}` : fileName;
    
    // Vérifier l'unicité du nom
    let finalPath = filePath;
    let counter = 1;
    while (app.vault.getAbstractFileByPath(finalPath)) {
        const nameWithoutExt = fileName.replace('.md', '');
        finalPath = targetFolder ? 
            `${targetFolder.path}/${nameWithoutExt}_${counter}.md` : 
            `${nameWithoutExt}_${counter}.md`;
        counter++;
    }
    
    // Remplacer les variables dans le template
    let content = template.templateContent;
    for (const [key, value] of Object.entries(properties)) {
        const regex = new RegExp(`\\$\\{${key}\\}`, 'g');
        content = content.replace(regex, value || "");
    }
    
    // Créer le fichier
    const newFile = await app.vault.create(finalPath, content);
    log(`${t('fileCreated')}: ${finalPath}`);
    
    return {
        file: newFile,
        name: elementName,
        path: finalPath
    };
}

// 7. Add visual embed to Excalidraw
async function addVisualEmbed(template, properties, fileInfo, color, elementType) {
    log("Étape 7: Ajout de l'embed visuel");
    
    const activeFile = app.workspace.getActiveFile();
    let excalidrawContent = await app.vault.read(activeFile);
    
    // Parser le JSON du drawing actuel
    const drawingMatch = excalidrawContent.match(/# Drawing\n```json\n([\s\S]*?)\n```/);
    if (!drawingMatch) {
        throw new Error("Structure Excalidraw non reconnue");
    }
    
    const drawingData = JSON.parse(drawingMatch[1]);
    const viewCenter = getViewCenter(excalidrawContent);
    
    // Position de base
    let baseX = viewCenter.x - 200;
    let baseY = viewCenter.y - 150;
    
    // Décalage si d'autres embeds existent
    if (drawingData.elements && drawingData.elements.length > 0) {
        const embedElements = drawingData.elements.filter(el => el.type === "image" && el.fileId);
        if (embedElements.length > 0) {
            const offset = (embedElements.length % 5) * 30;
            baseX += offset;
            baseY += offset;
        }
    }
    
    // Créer les éléments visuels
    const fileId = generateFileId();
    const elements = [];
    
    // 1. Rectangle coloré (fond)
    elements.push({
        id: generateUUID().substring(0, 16),
        type: "rectangle",
        x: baseX,
        y: baseY,
        width: 400,
        height: 100,
        angle: 0,
        strokeColor: "#1e1e1e",
        backgroundColor: color.value,
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
    });
    
    // 2. Texte (type + nom)
    const icon = CONFIG.ELEMENT_TYPES[elementType].label.split(' ')[0];
    const typeLabel = elementType.toUpperCase();
    const elementName = properties.nom || properties.name || properties.titre || "Element";
    
    elements.push({
        id: generateUUID().substring(0, 16),
        type: "text",
        x: baseX + 10,
        y: baseY + 25,
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
        text: `${icon} ${typeLabel}\n${elementName}`,
        rawText: `${icon} ${typeLabel}\n${elementName}`,
        textAlign: "center",
        verticalAlign: "middle",
        containerId: null,
        originalText: `${icon} ${typeLabel}\n${elementName}`,
        lineHeight: 1.25,
        baseline: 45
    });
    
    // 3. Image element (embed)
    elements.push({
        id: generateUUID().substring(0, 16),
        type: "image",
        x: baseX,
        y: baseY + 120,
        width: 400,
        height: 300,
        angle: 0,
        strokeColor: "transparent",
        backgroundColor: "transparent",
        fillStyle: "solid",
        strokeWidth: 1,
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
        status: "saved",
        fileId: fileId,
        scale: [1, 1]
    });
    
    // Ajouter les éléments au drawing
    drawingData.elements = drawingData.elements || [];
    drawingData.elements.push(...elements);
    
    // Ajouter l'entrée dans Embedded files
    const embedEntry = `${fileId}: [[${fileInfo.name}|100%]]`;
    
    const embeddedFilesRegex = /# Embedded files\n([\s\S]*?)(?=\n#|$)/;
    const embeddedFilesMatch = excalidrawContent.match(embeddedFilesRegex);
    
    if (embeddedFilesMatch) {
        let existingContent = embeddedFilesMatch[1].trim();
        const newContent = existingContent ? 
            `${existingContent}\n${embedEntry}` : 
            embedEntry;
        
        excalidrawContent = excalidrawContent.replace(
            embeddedFilesRegex, 
            `# Embedded files\n${newContent}\n`
        );
    } else {
        // Créer la section si elle n'existe pas
        const textElementsIndex = excalidrawContent.indexOf("# Text Elements");
        if (textElementsIndex > -1) {
            excalidrawContent = 
                excalidrawContent.slice(0, textElementsIndex) + 
                `# Embedded files\n${embedEntry}\n\n` +
                excalidrawContent.slice(textElementsIndex);
        }
    }
    
    // Remplacer le JSON du drawing
    const newDrawingJson = JSON.stringify(drawingData, null, "\t");
    excalidrawContent = excalidrawContent.replace(
        /# Drawing\n```json\n[\s\S]*?\n```/,
        `# Drawing\n\`\`\`json\n${newDrawingJson}\n\`\`\``
    );
    
    // Sauvegarder
    await app.vault.modify(activeFile, excalidrawContent);
    await app.vault.adapter.write(activeFile.path, excalidrawContent);
    
    log(`${t('embedAdded')}`);
    
    // Attendre et rafraîchir la vue
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    try {
        const activeLeaf = app.workspace.activeLeaf;
        if (activeLeaf && activeLeaf.view) {
            const view = activeLeaf.view;
            if (view.getViewType && (view.getViewType() === "markdown" || view.getViewType() === "excalidraw")) {
                if (view.setMode) {
                    await view.setMode('source');
                    await new Promise(resolve => setTimeout(resolve, 200));
                    await view.setMode('preview');
                }
            }
        }
    } catch (e) {
        log("Erreur rafraîchissement vue:", e);
    }
}

// === MAIN EXECUTION ===
(async () => {
    try {
        log("=== DÉMARRAGE DU SCRIPT ===");
        console.log(t('scriptStarting'));
        
        // Vérifier le fichier actif
        const activeFile = app.workspace.getActiveFile();
        if (!activeFile) {
            throw new Error(t('noActiveFile'));
        }
        
        // 1. Sélectionner le type d'élément
        const elementType = await selectElementType();
        
        // 2. Charger le template correspondant
        new Notice(t('loadingTemplate'));
        const templateContent = await loadTemplate(elementType);
        
        // 3. Analyser le template
        new Notice(t('analysingTemplate'));
        const template = parseTemplate(templateContent);
        
        // 4. Collecter les propriétés USER
        const properties = await collectUserProperties(template, elementType);
        
        // 5. Sélectionner la couleur
        const color = await selectColor();
        
        // 6. Créer le fichier embed
        new Notice(t('creatingElement'));
        const fileInfo = await createEmbedFile(template, properties, elementType);
        
        // 7. Ajouter l'embed visuel
        await addVisualEmbed(template, properties, fileInfo, color, elementType);
        
        // Message de succès
        new Notice(t('elementCreated') + `: ${fileInfo.name}`);
        log(`=== SUCCÈS === Élément "${fileInfo.name}" créé (type: ${elementType}, couleur: ${color.name})`);
        
    } catch (error) {
        log("=== ERREUR ===", error);
        console.error("ProcessMetaLanguage Error:", error);
        new Notice(`${t('error')}: ${error.message}`);
    }
    
    log("=== FIN DU SCRIPT ===");
})();

/* 
```
*/