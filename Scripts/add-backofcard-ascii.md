```javascript
// Add BackOfTheCard to Drawing - ASCII Version
// Version: 1.0.3-ascii
// Date: 2025-01-19
// Author: Rolland MELET & Claude Code

console.log("[DEBUG] Script initialization started");

// Configuration
var CONFIG = {
    version: "1.0.3-ascii",
    author: "Rolland MELET & Claude Code",
    debug: true
};

var OBJECT_TYPES = [
    "OBJET", "WORKFLOW", "PROCESSUS", "ETAT", "ACTION", 
    "COMPORTEMENT", "VUE", "REGLE", "DECISION"
];

var COLORS = {
    "OBJET": "#4CAF50",
    "WORKFLOW": "#FF9800", 
    "PROCESSUS": "#2196F3",
    "ETAT": "#9C27B0",
    "ACTION": "#F44336",
    "COMPORTEMENT": "#00BCD4",
    "VUE": "#E91E63",
    "REGLE": "#795548",
    "DECISION": "#FFC107"
};

console.log("[DEBUG] Configuration loaded", CONFIG);

// Detailed logging function
function debugLog(step, message, data) {
    var timestamp = new Date().toLocaleTimeString();
    console.log("[DEBUG] " + timestamp + " STEP " + step + ": " + message);
    if (data !== undefined) {
        console.log("[DATA] " + JSON.stringify(data));
    }
}

// API Check function
function checkAPIs() {
    debugLog("API-CHECK", "Starting API verification");
    
    var results = {
        ea: typeof ea !== 'undefined',
        app: typeof app !== 'undefined', 
        utils: typeof utils !== 'undefined',
        Notice: typeof Notice !== 'undefined'
    };
    
    debugLog("API-CHECK", "API check results", results);
    return results;
}

// Main function
function main() {
    return new Promise(function(resolve, reject) {
        debugLog("MAIN", "Starting main execution");
        
        try {
            // Check APIs
            var apiCheck = checkAPIs();
            if (!apiCheck.ea || !apiCheck.app || !apiCheck.utils || !apiCheck.Notice) {
                debugLog("ERROR", "Missing required APIs", apiCheck);
                reject(new Error("Required APIs not available"));
                return;
            }
            
            debugLog("MAIN", "All APIs available");
            
            // Check plugin version
            debugLog("VERSION", "Checking plugin version");
            if (!ea.verifyMinimumPluginVersion("2.0.0")) {
                debugLog("ERROR", "Plugin version insufficient");
                new Notice("Plugin version insufficient. Minimum: 2.0.0");
                reject(new Error("Plugin version too old"));
                return;
            }
            
            debugLog("VERSION", "Plugin version OK");
            
            // Get active file
            debugLog("FILE", "Getting active file");
            var activeFile = app.workspace.getActiveFile();
            
            if (!activeFile) {
                debugLog("ERROR", "No active file");
                new Notice("Please open a file");
                reject(new Error("No active file"));
                return;
            }
            
            debugLog("FILE", "Active file found", {
                name: activeFile.name,
                path: activeFile.path
            });
            
            if (!activeFile.name.endsWith(".excalidraw")) {
                debugLog("ERROR", "File is not Excalidraw");
                new Notice("Please open an Excalidraw file");
                reject(new Error("Not an Excalidraw file"));
                return;
            }
            
            debugLog("FILE", "Excalidraw file validated");
            
            // Check elements
            debugLog("ELEMENTS", "Getting drawing elements");
            var elements = ea.getViewElements();
            
            if (!elements) {
                debugLog("ERROR", "Cannot get elements");
                new Notice("Cannot access drawing elements");
                reject(new Error("Elements not accessible"));
                return;
            }
            
            debugLog("ELEMENTS", "Elements retrieved", {
                count: elements.length
            });
            
            if (elements.length === 0) {
                debugLog("ERROR", "No elements in drawing");
                new Notice("No elements in drawing. Create an object first.");
                reject(new Error("Drawing is empty"));
                return;
            }
            
            if (elements.length > 1) {
                debugLog("WARN", "Too many elements");
                new Notice("Drawing must contain exactly ONE object");
                reject(new Error("Too many elements"));
                return;
            }
            
            debugLog("ELEMENTS", "Element count validated");
            
            // Get template name
            debugLog("INPUT", "Getting template name");
            askTemplateName().then(function(templateName) {
                debugLog("INPUT", "Template name received", templateName);
                
                if (!templateName) {
                    debugLog("ERROR", "No template name");
                    reject(new Error("Template name required"));
                    return;
                }
                
                // Get object type
                debugLog("SELECT", "Getting object type");
                selectObjectType().then(function(objectType) {
                    debugLog("SELECT", "Object type received", objectType);
                    
                    if (!objectType) {
                        debugLog("ERROR", "No object type");
                        reject(new Error("Object type required"));
                        return;
                    }
                    
                    // Generate template
                    debugLog("GENERATE", "Generating template");
                    var templateContent = generateSimpleTemplate(templateName, objectType);
                    debugLog("GENERATE", "Template generated", {
                        length: templateContent.length
                    });
                    
                    // Create file
                    debugLog("FILE-CREATE", "Creating template file");
                    createTemplateFile(templateName, templateContent).then(function(file) {
                        debugLog("FILE-CREATE", "Template file created", file.path);
                        
                        // Add visual elements
                        debugLog("VISUAL", "Adding visual elements");
                        addTypeToDrawing(objectType, templateName);
                        
                        // Create embed
                        debugLog("EMBED", "Creating embed");
                        createEmbed(file).then(function() {
                            debugLog("SUCCESS", "All operations completed");
                            new Notice("Template created successfully: " + templateName);
                            resolve("Success");
                            
                        }).catch(function(embedError) {
                            debugLog("ERROR", "Embed creation failed", embedError.message);
                            reject(embedError);
                        });
                        
                    }).catch(function(fileError) {
                        debugLog("ERROR", "File creation failed", fileError.message);
                        reject(fileError);
                    });
                    
                }).catch(function(typeError) {
                    debugLog("ERROR", "Type selection failed", typeError.message);
                    reject(typeError);
                });
                
            }).catch(function(nameError) {
                debugLog("ERROR", "Name input failed", nameError.message);
                reject(nameError);
            });
            
        } catch (mainError) {
            debugLog("ERROR", "Main function error", mainError.message);
            reject(mainError);
        }
    });
}

// Ask for template name
function askTemplateName() {
    debugLog("PROMPT", "Starting template name prompt");
    
    return new Promise(function(resolve, reject) {
        try {
            if (!utils || !utils.inputPrompt) {
                debugLog("ERROR", "utils.inputPrompt not available");
                reject(new Error("inputPrompt not available"));
                return;
            }
            
            var result = utils.inputPrompt(
                "Template Name",
                "Enter the template name:",
                "",
                [{
                    caption: "Create",
                    action: function() { return true; }
                }]
            );
            
            if (result && typeof result.then === 'function') {
                result.then(function(name) {
                    debugLog("PROMPT", "Name received from promise", name);
                    resolve(name && name.trim() ? name.trim() : null);
                }).catch(reject);
            } else {
                debugLog("PROMPT", "Name received directly", result);
                resolve(result && result.trim() ? result.trim() : null);
            }
            
        } catch (error) {
            debugLog("ERROR", "Template name prompt error", error.message);
            reject(error);
        }
    });
}

// Select object type
function selectObjectType() {
    debugLog("SUGGESTER", "Starting object type selection");
    
    return new Promise(function(resolve, reject) {
        try {
            if (!utils || !utils.suggester) {
                debugLog("ERROR", "utils.suggester not available");
                reject(new Error("suggester not available"));
                return;
            }
            
            var result = utils.suggester(
                function(item) { return item; },
                OBJECT_TYPES,
                false,
                "Select object type:"
            );
            
            if (result && typeof result.then === 'function') {
                result.then(function(type) {
                    debugLog("SUGGESTER", "Type received from promise", type);
                    resolve(type);
                }).catch(reject);
            } else {
                debugLog("SUGGESTER", "Type received directly", result);
                resolve(result);
            }
            
        } catch (error) {
            debugLog("ERROR", "Object type selection error", error.message);
            reject(error);
        }
    });
}

// Generate simple template
function generateSimpleTemplate(name, type) {
    debugLog("TEMPLATE", "Generating template content");
    
    var uid = generateUID(type);
    var date = new Date().toISOString().split('T')[0];
    
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
    
    content += "# " + type + ": " + name + "\n\n";
    
    content += "## Metadata\n\n";
    content += "- **UID**: " + uid + "\n";
    content += "- **Type**: " + type + "\n";
    content += "- **Created**: " + date + "\n";
    content += "- **Status**: Draft\n";
    content += "- **Version**: 1.0.0\n\n";
    
    content += "## Description\n\n";
    content += "[Describe the purpose and role of this element]\n\n";
    content += "**Example**: " + getDescriptionExample(type) + "\n\n";
    
    content += "## Rules\n\n";
    content += "### Business Rules\n";
    content += "- [ ] Respect defined business logic\n";
    content += "- [ ] Validate integrity constraints\n";
    content += "- [ ] Verify entry conditions\n\n";
    
    content += "### Technical Rules\n";
    content += "- [ ] Follow technical standards\n";
    content += "- [ ] Maintain acceptable performance\n\n";
    
    content += "### Transition Rules\n";
    content += "- [ ] Allow passage according to conditions\n\n";
    
    content += "## Properties\n\n";
    content += "### Basic Properties\n";
    content += "- **name**: Element name\n";
    content += "- **description**: Detailed description\n";
    content += "- **status**: Current state (active, inactive, suspended)\n";
    content += "- **owner**: Element owner\n";
    content += "- **creation_date**: Creation date\n";
    content += "- **modification_date**: Last modification\n\n";
    
    content += "### Specific Properties\n";
    content += getSpecificProperties(type) + "\n\n";
    
    content += "## Relations\n\n";
    content += "### Incoming Relations\n";
    content += "- **predecessor_1**: [Type] - Relation description\n";
    content += "- **parent**: [Type] - Hierarchical relation\n\n";
    
    content += "### Outgoing Relations\n";
    content += "- **successor_1**: [Type] - Relation description\n";
    content += "- **children**: [Type] - Hierarchical relations\n\n";
    
    content += "## States and Transitions\n\n";
    content += getStatesExample(type) + "\n\n";
    
    content += "## Metrics and KPIs\n\n";
    content += "- **Average_time**: Average processing duration\n";
    content += "- **Success_rate**: Success percentage\n";
    content += "- **Active_instances**: Active instances\n\n";
    
    content += "## Control Points\n\n";
    content += "- **Validation_1**: Verify data integrity\n";
    content += "- **Validation_2**: Control permissions\n";
    content += "- **Validation_3**: Validate business rules\n\n";
    
    content += "## Documentation\n\n";
    content += "### References\n";
    content += "- [Link to business documentation]\n";
    content += "- [Link to technical specifications]\n\n";
    
    content += "### Usage Examples\n";
    content += "1. **Trigger**: [Trigger event]\n";
    content += "2. **Processing**: [Processing steps]\n";
    content += "3. **Result**: [Expected result]\n\n";
    
    content += "## Notes\n\n";
    content += "### To Complete\n";
    content += "- [ ] Finalize business rules\n";
    content += "- [ ] Validate properties\n";
    content += "- [ ] Test transitions\n";
    content += "- [ ] Document use cases\n\n";
    
    content += "### History\n";
    content += "- " + date + ": Template creation\n\n";
    
    content += "---\n";
    content += "**Template generated by ProcessMetaLanguage v" + CONFIG.version + "**\n";
    
    return content;
}

// Generate UID
function generateUID(type) {
    var prefix = type.substring(0, 3).toUpperCase();
    var date = new Date().toISOString().slice(0, 10).replace(/-/g, '');
    var random = Math.floor(Math.random() * 1000).toString();
    while (random.length < 3) random = '0' + random;
    return prefix + date + random;
}

// Get description example
function getDescriptionExample(type) {
    var examples = {
        "OBJET": "This object represents a business entity (ex: Order, Product, Client) following a defined process.",
        "WORKFLOW": "This workflow orchestrates multiple processes to achieve a business objective (ex: Complete Order Processing).",
        "PROCESSUS": "This process describes the complete lifecycle of an object from initial to final state.",
        "ETAT": "This state represents a specific situation an object can be in at a given moment.",
        "ACTION": "This action triggers a state transition and can modify object properties."
    };
    return examples[type] || "Describe the purpose of this element";
}

// Get specific properties
function getSpecificProperties(type) {
    var specific = {
        "OBJET": "- **process_id**: Associated process identifier\n- **current_state**: Current object state\n- **business_properties**: Domain-specific data",
        "WORKFLOW": "- **orchestrated_processes**: List of coordinated processes\n- **triggers**: Events that initiate the workflow\n- **sync_points**: Coordination moments",
        "PROCESSUS": "- **target_object**: Object type managed by this process\n- **possible_states**: List of process states\n- **initial_state**: Starting point\n- **final_states**: End points"
    };
    return specific[type] || "- **specific_property**: Type-specific value";
}

// Get states example
function getStatesExample(type) {
    var states = {
        "OBJET": "- **Created**: Initial state after creation\n- **Processing**: Object being processed\n- **Validated**: Object approved\n- **Completed**: Final state",
        "PROCESSUS": "- **Inactive**: Process not started\n- **Active**: Process running\n- **Suspended**: Process temporarily stopped\n- **Completed**: Process finished"
    };
    return states[type] || "- **State_1**: Description\n- **State_2**: Description";
}

// Create template file
function createTemplateFile(name, content) {
    debugLog("FILE-CREATE", "Starting file creation");
    
    return new Promise(function(resolve, reject) {
        try {
            var activeFile = app.workspace.getActiveFile();
            var targetFolder = activeFile.parent;
            var fileName = "template-" + name.replace(/\s+/g, '-').toLowerCase() + ".md";
            var filePath = targetFolder.path + "/" + fileName;
            
            debugLog("FILE-CREATE", "File path", filePath);
            
            // Check if file exists
            var existingFile = app.vault.getAbstractFileByPath(filePath);
            if (existingFile) {
                debugLog("FILE-CREATE", "File already exists, will overwrite");
            }
            
            app.vault.create(filePath, content).then(function(file) {
                debugLog("FILE-CREATE", "File created successfully");
                resolve(file);
            }).catch(function(createError) {
                debugLog("ERROR", "File creation failed", createError.message);
                reject(createError);
            });
            
        } catch (error) {
            debugLog("ERROR", "File creation error", error.message);
            reject(error);
        }
    });
}

// Add type to drawing
function addTypeToDrawing(type, name) {
    debugLog("VISUAL", "Adding type text to drawing");
    
    try {
        var elements = ea.getViewElements();
        var firstElement = elements[0];
        
        var textX = firstElement.x;
        var textY = firstElement.y - 60;
        
        var typeText = ea.addText(textX, textY, type + ": {{" + name + "}}", {
            fontSize: 16,
            fontFamily: 1,
            textAlign: "left",
            verticalAlign: "middle",
            strokeColor: COLORS[type] || "#333333"
        });
        
        debugLog("VISUAL", "Type text added successfully");
        
    } catch (error) {
        debugLog("WARN", "Visual text addition failed", error.message);
        // Don't block the process for this error
    }
}

// Create embed
function createEmbed(templateFile) {
    debugLog("EMBED", "Starting embed creation");
    
    return new Promise(function(resolve, reject) {
        try {
            var elements = ea.getViewElements();
            var firstElement = elements[0];
            
            var embedX = firstElement.x + firstElement.width + 50;
            var embedY = firstElement.y;
            
            var embed = ea.addEmbeddable(embedX, embedY, 400, 300, templateFile.path);
            
            ea.addElementsToView().then(function() {
                debugLog("EMBED", "Embed created and added successfully");
                resolve();
            }).catch(function(addError) {
                debugLog("ERROR", "Failed to add elements to view", addError.message);
                reject(addError);
            });
            
        } catch (error) {
            debugLog("ERROR", "Embed creation error", error.message);
            reject(error);
        }
    });
}

// Execute main function
console.log("[DEBUG] Starting main execution");
main().then(function(result) {
    console.log("[SUCCESS] Script completed successfully: " + result);
}).catch(function(error) {
    console.error("[ERROR] Script failed: " + error.message);
    new Notice("Error: " + error.message);
});
```