/*
Script Excalidraw pour créer un objet ProcessMetaLanguage avec template BackOfTheCard
Version: 1.0.0
Date: 2025-01-18
Auteur: Rolland MELET & Claude Code
Description: Crée un objet avec gestion des propriétés système/user/default/dynamic

Ce script:
1. Permet à l'utilisateur de sélectionner un élément dessiné
2. Collecte les propriétés USER via prompts
3. Génère les propriétés SYSTEM automatiquement
4. Applique les valeurs DEFAULT
5. Crée un fichier embedded avec toutes les métadonnées
6. Respecte les règles ProcessMetaLanguage v2.3

```javascript
*/

// ==========================================
// CONFIGURATION
// ==========================================
const CONFIG = {
    TEMPLATES_FOLDER: "Templates/BackOfCard",
    TEMPLATE_NAME: "template-objet-backofcard",
    OBJECTS_FOLDER: "ProcessMetaLanguage/Objets",
    DEBUG: false
};

// ==========================================
// INTERNATIONALISATION
// ==========================================
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
        selectElement: "Sélectionnez l'élément à transformer en objet",
        noSelection: "Aucun élément sélectionné",
        objectType: "Type d'objet dans le workflow:",
        objectTypeResource: "RESSOURCE - Objet réutilisable (moule, équipement)",
        objectTypeConsumable: "CONSOMMABLE - Élément consommé (matière, composant)",
        objectTypeProduct: "PRODUIT - Objet produit (voussoir, PAC)",
        objectName: "Nom de l'objet:",
        objectDescription: "Description de l'objet (optionnelle):",
        objectCategory: "Catégorie de l'objet:",
        businessProperties: "Propriétés métier (JSON, optionnel):",
        tags: "Tags (séparés par des virgules, optionnel):",
        // Propriétés spécifiques CONSOMMABLE
        stockQuantity: "Quantité en stock:",
        batchNumber: "Numéro de lot:",
        expiryDate: "Date de péremption (AAAA-MM-JJ, optionnel):",
        // Messages
        creatingObject: "Création de l'objet en cours...",
        objectCreated: "✅ Objet créé avec succès",
        error: "❌ Erreur",
        invalidJson: "JSON invalide pour les propriétés métier",
        fileExists: "existe déjà. Nouveau nom:",
        cancelled: "Création annulée",
        assignToProcess: "Assigner à un processus existant?",
        selectProcess: "Sélectionner le processus:",
        noProcess: "Aucun processus (créer plus tard)",
        validationError: "Erreur de validation",
        ruleViolation: "Règle violée: Si processus assigné, état initial obligatoire"
    },
    en: {
        selectElement: "Select the element to transform into an object",
        noSelection: "No element selected",
        objectType: "Object type in workflow:",
        objectTypeResource: "RESOURCE - Reusable object (mold, equipment)",
        objectTypeConsumable: "CONSUMABLE - Consumed element (material, component)",
        objectTypeProduct: "PRODUCT - Produced object (segment, unit)",
        objectName: "Object name:",
        objectDescription: "Object description (optional):",
        objectCategory: "Object category:",
        businessProperties: "Business properties (JSON, optional):",
        tags: "Tags (comma-separated, optional):",
        // Specific properties for CONSUMABLE
        stockQuantity: "Stock quantity:",
        batchNumber: "Batch number:",
        expiryDate: "Expiry date (YYYY-MM-DD, optional):",
        // Messages
        creatingObject: "Creating object...",
        objectCreated: "✅ Object created successfully",
        error: "❌ Error",
        invalidJson: "Invalid JSON for business properties",
        fileExists: "already exists. New name:",
        cancelled: "Creation cancelled",
        assignToProcess: "Assign to existing process?",
        selectProcess: "Select process:",
        noProcess: "No process (create later)",
        validationError: "Validation error",
        ruleViolation: "Rule violated: If process assigned, initial state required"
    }
};

const t = (key) => i18n[lang][key] || i18n['en'][key];

// ==========================================
// FONCTIONS UTILITAIRES
// ==========================================

// Générer un UUID v4
function generateUUID() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        const r = Math.random() * 16 | 0;
        const v = c === 'x' ? r : (r & 0x3 | 0x8);
        return v.toString(16);
    });
}

// Générer un UID court
function generateUID() {
    const date = moment().format('YYYYMMDD');
    const index = Math.floor(Math.random() * 1000).toString().padStart(3, '0');
    return `OBJ${date}${index}`;
}

// Logger pour debug
function log(message, data = null) {
    if (CONFIG.DEBUG) {
        console.log(`[Create Object] ${message}`, data || '');
    }
}

// Charger le template
async function loadTemplate() {
    try {
        const templatePath = `${CONFIG.TEMPLATES_FOLDER}/${CONFIG.TEMPLATE_NAME}.md`;
        const templateFile = app.vault.getAbstractFileByPath(templatePath);
        
        if (!templateFile) {
            throw new Error(`Template non trouvé: ${templatePath}`);
        }
        
        const content = await app.vault.read(templateFile);
        
        // Parser le front matter YAML pour extraire property_definitions
        const frontMatterMatch = content.match(/^---\n([\s\S]*?)\n---/);
        if (!frontMatterMatch) {
            throw new Error("Front matter non trouvé dans le template");
        }
        
        // Parser manuellement les property_definitions (simplification pour le script)
        // Dans un cas réel, utiliser un parser YAML approprié
        return {
            content: content,
            frontMatter: frontMatterMatch[1]
        };
    } catch (error) {
        console.error("Erreur chargement template:", error);
        throw error;
    }
}

// Collecter les propriétés USER
async function collectUserProperties() {
    const properties = {};
    
    // Type d'objet dans le workflow (OBLIGATOIRE)
    const objectTypes = [
        { value: "RESSOURCE", label: t('objectTypeResource') },
        { value: "CONSOMMABLE", label: t('objectTypeConsumable') },
        { value: "PRODUIT", label: t('objectTypeProduct') }
    ];
    
    if (typeof utils.suggester === 'function') {
        const selected = await utils.suggester(
            objectTypes.map(t => t.label),
            objectTypes.map(t => t.value),
            false,
            t('objectType')
        );
        properties.type_objet_workflow = selected;
    } else {
        // Fallback simple
        properties.type_objet_workflow = "PRODUIT";
    }
    
    if (!properties.type_objet_workflow) {
        throw new Error("Le type d'objet est obligatoire");
    }
    
    // Nom (obligatoire)
    properties.nom = await utils.inputPrompt(t('objectName'));
    if (!properties.nom || properties.nom.length < 3) {
        throw new Error("Le nom doit faire au moins 3 caractères");
    }
    
    // Description (optionnelle)
    properties.description = await utils.inputPrompt(t('objectDescription')) || "";
    
    // Catégorie (optionnelle avec suggestions)
    const categories = ["Composant", "Document", "Matériel", "Matière première", "Service", "Autre"];
    if (typeof utils.suggester === 'function') {
        properties.categorie = await utils.suggester(
            categories,
            categories,
            false,
            t('objectCategory')
        ) || "Autre";
    } else {
        properties.categorie = await utils.inputPrompt(t('objectCategory')) || "Autre";
    }
    
    // === PROPRIÉTÉS SPÉCIFIQUES SELON LE TYPE ===
    
    // Pour CONSOMMABLE : demander quantité stock, lot, péremption
    if (properties.type_objet_workflow === "CONSOMMABLE") {
        const stockStr = await utils.inputPrompt(t('stockQuantity'), "0");
        properties.quantite_stock = parseInt(stockStr) || 0;
        
        properties.lot_numero = await utils.inputPrompt(t('batchNumber')) || "";
        
        const expiryStr = await utils.inputPrompt(t('expiryDate')) || "";
        properties.date_peremption = expiryStr || null;
    }
    
    // Propriétés métier (JSON optionnel)
    const jsonInput = await utils.inputPrompt(
        t('businessProperties'),
        '{"exemple": "valeur"}'
    );
    
    if (jsonInput && jsonInput !== '{"exemple": "valeur"}') {
        try {
            properties.proprietes_metier = JSON.parse(jsonInput);
        } catch (e) {
            new Notice(t('invalidJson'));
            properties.proprietes_metier = {};
        }
    } else {
        properties.proprietes_metier = {};
    }
    
    // Tags (optionnel)
    const tagsInput = await utils.inputPrompt(t('tags')) || "";
    properties.tags = tagsInput ? tagsInput.split(',').map(t => t.trim()) : [];
    
    return properties;
}

// Générer les propriétés SYSTEM
function generateSystemProperties(userName = "User") {
    return {
        id: generateUUID(),
        uid: generateUID(),
        url_propre: `/objects/${generateUID()}`,
        created_at: new Date().toISOString(),
        created_by: userName,
        modified_at: new Date().toISOString(),
        modified_by: userName,
        version: 1,
        historique: [{
            timestamp: new Date().toISOString(),
            action: "creation",
            user: userName,
            details: "Objet créé via script ProcessMetaLanguage"
        }]
    };
}

// Appliquer les valeurs DEFAULT selon le type
function applyDefaultProperties(objectType) {
    const baseDefaults = {
        processus_objet_id: null,
        etat_actuel_id: null,
        etat_actuel_nom: null,
        workflow_parent_id: null,
        priorite: "normale",
        statut: "actif",
        liens: [],
        fichiers_attaches: []
    };
    
    // Ajouter les propriétés spécifiques selon le type
    switch(objectType) {
        case "RESSOURCE":
            return {
                ...baseDefaults,
                cycles_utilisation: 0,
                disponibilite: "disponible"
            };
            
        case "CONSOMMABLE":
            return {
                ...baseDefaults,
                // quantite_stock, lot_numero et date_peremption sont déjà collectés dans USER
            };
            
        case "PRODUIT":
            return {
                ...baseDefaults,
                date_production: null,
                conformite: null,
                traçabilite_composants: []
            };
            
        default:
            return baseDefaults;
    }
}

// Vérifier l'existence de processus
async function checkExistingProcesses() {
    // Rechercher les fichiers de type processus
    const allFiles = app.vault.getFiles();
    const processes = [];
    
    for (const file of allFiles) {
        if (file.path.includes("ProcessMetaLanguage") && file.path.includes("processus")) {
            const content = await app.vault.read(file);
            if (content.includes('template_type: "processus"')) {
                processes.push({
                    name: file.basename,
                    path: file.path
                });
            }
        }
    }
    
    return processes;
}

// Valider les règles ProcessMetaLanguage
function validateRules(properties) {
    // Règle fondamentale: Si processus_objet_id existe, etat_actuel_id obligatoire
    if (properties.processus_objet_id && !properties.etat_actuel_id) {
        throw new Error(t('ruleViolation'));
    }
    
    return true;
}

// Créer le contenu du fichier
function createFileContent(template, properties) {
    let content = template.content;
    
    // Remplacer toutes les variables ${xxx}
    for (const [key, value] of Object.entries(properties)) {
        const regex = new RegExp(`\\$\\{${key}\\}`, 'g');
        
        if (value === null || value === undefined) {
            content = content.replace(regex, "Non défini");
        } else if (typeof value === 'object') {
            content = content.replace(regex, JSON.stringify(value, null, 2));
        } else if (Array.isArray(value)) {
            content = content.replace(regex, value.join(', '));
        } else {
            content = content.replace(regex, value.toString());
        }
    }
    
    // Remplacer les variables non trouvées par "Non défini"
    content = content.replace(/\$\{[^}]+\}/g, "Non défini");
    
    return content;
}

// Obtenir un nom de fichier unique
async function getUniqueFileName(baseName, folder) {
    let fileName = `${baseName}.md`;
    let filePath = folder ? `${folder}/${fileName}` : fileName;
    let counter = 1;
    
    while (app.vault.getAbstractFileByPath(filePath)) {
        fileName = `${baseName}_${counter}.md`;
        filePath = folder ? `${folder}/${fileName}` : fileName;
        counter++;
    }
    
    return { fileName, filePath };
}

// ==========================================
// SCRIPT PRINCIPAL
// ==========================================
(async () => {
    try {
        log("Démarrage du script create-object-backofcard");
        
        // 1. Vérifier la sélection
        const selectedElements = ea.getViewSelectedElements();
        if (selectedElements.length === 0) {
            new Notice(t('noSelection'));
            return;
        }
        
        const selectedElement = selectedElements[0];
        log("Élément sélectionné:", selectedElement);
        
        // 2. Charger le template
        new Notice(t('creatingObject'));
        const template = await loadTemplate();
        log("Template chargé");
        
        // 3. Collecter les propriétés USER
        const userProperties = await collectUserProperties();
        log("Propriétés utilisateur collectées:", userProperties);
        
        // 4. Générer les propriétés SYSTEM
        const systemProperties = generateSystemProperties();
        log("Propriétés système générées:", systemProperties);
        
        // 5. Appliquer les propriétés DEFAULT selon le type
        const defaultProperties = applyDefaultProperties(userProperties.type_objet_workflow);
        log("Propriétés par défaut appliquées:", defaultProperties);
        
        // 6. Optionnel: Assigner à un processus existant
        const processes = await checkExistingProcesses();
        if (processes.length > 0) {
            const assignProcess = await utils.confirm(t('assignToProcess'));
            
            if (assignProcess) {
                const processOptions = [...processes.map(p => p.name), t('noProcess')];
                const selected = await utils.suggester(
                    processOptions,
                    processOptions,
                    false,
                    t('selectProcess')
                );
                
                if (selected && selected !== t('noProcess')) {
                    const selectedProcess = processes.find(p => p.name === selected);
                    defaultProperties.processus_objet_id = selectedProcess.name;
                    
                    // Si processus assigné, créer un état initial par défaut
                    defaultProperties.etat_actuel_id = "etat_initial_" + systemProperties.uid;
                    defaultProperties.etat_actuel_nom = "État Initial";
                }
            }
        }
        
        // 7. Fusionner toutes les propriétés
        const allProperties = {
            ...defaultProperties,
            ...systemProperties,
            ...userProperties
        };
        
        log("Propriétés fusionnées:", allProperties);
        
        // 8. Valider les règles
        validateRules(allProperties);
        
        // 9. Créer le contenu du fichier
        const fileContent = createFileContent(template, allProperties);
        
        // 10. Créer le dossier si nécessaire
        const objectsFolder = CONFIG.OBJECTS_FOLDER;
        if (!app.vault.getAbstractFileByPath(objectsFolder)) {
            await app.vault.createFolder(objectsFolder);
        }
        
        // 11. Obtenir un nom de fichier unique
        const { fileName, filePath } = await getUniqueFileName(
            allProperties.nom.replace(/[^a-zA-Z0-9-_]/g, '_'),
            objectsFolder
        );
        
        // 12. Créer le fichier
        const newFile = await app.vault.create(filePath, fileContent);
        log("Fichier créé:", filePath);
        
        // 13. Créer l'embed dans Excalidraw
        const fileId = generateUUID().substring(0, 40);
        
        // Obtenir le fichier Excalidraw actuel
        const activeFile = app.workspace.getActiveFile();
        if (!activeFile) {
            throw new Error("Aucun fichier actif");
        }
        
        let excalidrawContent = await app.vault.read(activeFile);
        
        // Ajouter l'image element pour l'embed
        const imageElement = {
            id: generateUUID().substring(0, 16),
            type: "image",
            x: selectedElement.x,
            y: selectedElement.y + selectedElement.height + 20,
            width: 400,
            height: 250,
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
        };
        
        // Parser et mettre à jour le JSON du drawing
        const drawingMatch = excalidrawContent.match(/# Drawing\n```json\n([\s\S]*?)\n```/);
        if (drawingMatch) {
            const drawingData = JSON.parse(drawingMatch[1]);
            drawingData.elements = drawingData.elements || [];
            drawingData.elements.push(imageElement);
            
            const newDrawingJson = JSON.stringify(drawingData, null, "\t");
            excalidrawContent = excalidrawContent.replace(
                /# Drawing\n```json\n[\s\S]*?\n```/,
                `# Drawing\n\`\`\`json\n${newDrawingJson}\n\`\`\``
            );
        }
        
        // Ajouter l'entrée dans Embedded files
        const embedEntry = `${fileId}: [[${fileName.replace('.md', '')}|100%]]`;
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
            // Ajouter la section si elle n'existe pas
            const textElementsIndex = excalidrawContent.indexOf("# Text Elements");
            if (textElementsIndex > -1) {
                excalidrawContent = 
                    excalidrawContent.slice(0, textElementsIndex) + 
                    `# Embedded files\n${embedEntry}\n\n` +
                    excalidrawContent.slice(textElementsIndex);
            }
        }
        
        // 14. Sauvegarder les modifications
        await app.vault.modify(activeFile, excalidrawContent);
        
        // 15. Ajouter une flèche de connexion si nécessaire
        if (selectedElement) {
            // Créer une flèche entre l'élément sélectionné et le nouvel embed
            const arrow = {
                id: generateUUID().substring(0, 16),
                type: "arrow",
                x: selectedElement.x + selectedElement.width / 2,
                y: selectedElement.y + selectedElement.height,
                width: 0,
                height: 20,
                angle: 0,
                strokeColor: "#1e1e1e",
                backgroundColor: "transparent",
                fillStyle: "solid",
                strokeWidth: 2,
                strokeStyle: "solid",
                roughness: 0,
                opacity: 100,
                groupIds: [],
                frameId: null,
                roundness: { type: 2 },
                seed: Math.floor(Math.random() * 2000000000),
                versionNonce: Math.floor(Math.random() * 2000000000),
                isDeleted: false,
                boundElements: null,
                updated: Date.now(),
                link: null,
                locked: false,
                startBinding: {
                    elementId: selectedElement.id,
                    focus: 0,
                    gap: 1
                },
                endBinding: {
                    elementId: imageElement.id,
                    focus: 0,
                    gap: 1
                },
                lastCommittedPoint: null,
                startArrowhead: null,
                endArrowhead: "arrow",
                points: [[0, 0], [0, 20]]
            };
            
            // Ajouter la flèche au drawing
            const drawingMatch2 = excalidrawContent.match(/# Drawing\n```json\n([\s\S]*?)\n```/);
            if (drawingMatch2) {
                const drawingData = JSON.parse(drawingMatch2[1]);
                drawingData.elements.push(arrow);
                
                const newDrawingJson = JSON.stringify(drawingData, null, "\t");
                excalidrawContent = excalidrawContent.replace(
                    /# Drawing\n```json\n[\s\S]*?\n```/,
                    `# Drawing\n\`\`\`json\n${newDrawingJson}\n\`\`\``
                );
                
                await app.vault.modify(activeFile, excalidrawContent);
            }
        }
        
        // 16. Message de succès
        new Notice(`${t('objectCreated')}: ${allProperties.nom}`);
        log("Objet créé avec succès:", allProperties);
        
        // 17. Optionnel: Ouvrir le fichier créé dans un nouvel onglet
        const leaf = app.workspace.getLeaf('tab');
        await leaf.openFile(newFile);
        
    } catch (error) {
        console.error("Erreur dans create-object-backofcard:", error);
        new Notice(`${t('error')}: ${error.message}`);
    }
})();

/* 
```
*/