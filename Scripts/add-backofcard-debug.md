```javascript
// === Add BackOfTheCard to Drawing - DEBUG VERSION ===
// Version: 1.0.2-debug
// Date: 2025-01-19
// Auteur: Rolland MELET & Claude Code

console.log("🟢 [DEBUT] === DÉMARRAGE DU SCRIPT DEBUG ===");

// Fonction de logging détaillé
function debugLog(step, message, data) {
    const timestamp = new Date().toLocaleTimeString();
    console.log("🔍 [" + timestamp + "] STEP " + step + ": " + message);
    if (data !== undefined) {
        console.log("   📊 Data:", data);
    }
}

debugLog("INIT", "Script chargé, vérification des APIs");

// Vérification des APIs disponibles
debugLog("API-CHECK", "Vérification ea object", typeof ea);
debugLog("API-CHECK", "Vérification app object", typeof app);
debugLog("API-CHECK", "Vérification utils object", typeof utils);
debugLog("API-CHECK", "Vérification Notice class", typeof Notice);

// Vérification préalable
debugLog("VERSION", "Vérification version plugin");
try {
    if (!ea) {
        debugLog("ERROR", "ea object non disponible");
        console.error("❌ ea object non trouvé");
        return;
    }
    
    if (!ea.verifyMinimumPluginVersion) {
        debugLog("ERROR", "ea.verifyMinimumPluginVersion non disponible");
        console.error("❌ ea.verifyMinimumPluginVersion non trouvé");
        return;
    }
    
    const versionCheck = ea.verifyMinimumPluginVersion("2.0.0");
    debugLog("VERSION", "Résultat vérification version", versionCheck);
    
    if (!versionCheck) {
        new Notice("⚠️ Version Excalidraw Plugin insuffisante. Minimum requis: 2.0.0");
        return;
    }
} catch (versionError) {
    debugLog("ERROR", "Erreur lors vérification version", versionError.message);
    console.error("❌ Erreur version:", versionError);
    return;
}

// Configuration et constantes
debugLog("CONFIG", "Définition des constantes");
var CONFIG = {
    version: "1.0.2-debug",
    author: "Rolland MELET & Claude Code",
    debug: true
};
debugLog("CONFIG", "CONFIG définie", CONFIG);

var TYPES_OBJETS = [
    "OBJET", "WORKFLOW", "PROCESSUS", "ÉTAT", "ACTION", 
    "COMPORTEMENT", "VUE", "RÈGLE", "DÉCISION"
];
debugLog("CONFIG", "TYPES_OBJETS définis", TYPES_OBJETS.length + " types");

var COULEURS = {
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
debugLog("CONFIG", "COULEURS définies", Object.keys(COULEURS).length + " couleurs");

// Test des fonctions de base
debugLog("FUNC-TEST", "Test fonctions de base");

function testBasicFunctions() {
    debugLog("FUNC", "Test getActiveFile");
    try {
        if (!app || !app.workspace || !app.workspace.getActiveFile) {
            debugLog("ERROR", "app.workspace.getActiveFile non disponible");
            return false;
        }
        
        var activeFile = app.workspace.getActiveFile();
        debugLog("FUNC", "ActiveFile obtenu", activeFile ? activeFile.name : "null");
        
        debugLog("FUNC", "Test getViewElements");
        if (!ea.getViewElements) {
            debugLog("ERROR", "ea.getViewElements non disponible");
            return false;
        }
        
        var elements = ea.getViewElements();
        debugLog("FUNC", "Elements obtenus", elements ? elements.length : "null");
        
        return true;
    } catch (testError) {
        debugLog("ERROR", "Erreur test fonctions", testError.message);
        return false;
    }
}

// Fonction principale avec logging détaillé
function main() {
    return new Promise(function(resolve, reject) {
        debugLog("MAIN", "=== DÉBUT FONCTION MAIN ===");
        
        try {
            debugLog("MAIN", "Test des fonctions de base");
            if (!testBasicFunctions()) {
                debugLog("ERROR", "Échec test fonctions de base");
                reject(new Error("Test fonctions de base échoué"));
                return;
            }
            
            debugLog("MAIN", "Vérifications préalables");
            
            // Étape 1: Vérifications préalables
            debugLog("STEP-1", "Obtention fichier actif");
            var activeFile = app.workspace.getActiveFile();
            debugLog("STEP-1", "Fichier actif", activeFile);
            
            if (!activeFile) {
                debugLog("ERROR", "Aucun fichier actif");
                new Notice("❌ Veuillez ouvrir un fichier");
                reject(new Error("Aucun fichier actif"));
                return;
            }
            
            debugLog("STEP-1", "Nom fichier", activeFile.name);
            debugLog("STEP-1", "Chemin fichier", activeFile.path);
            
            if (!activeFile.name.endsWith(".excalidraw")) {
                debugLog("ERROR", "Fichier n'est pas .excalidraw");
                new Notice("❌ Veuillez ouvrir un fichier Excalidraw");
                reject(new Error("Fichier non Excalidraw"));
                return;
            }

            debugLog("STEP-1", "Fichier Excalidraw valide");

            // Vérifier qu'il y a exactement un objet dans le dessin
            debugLog("STEP-2", "Obtention éléments dessin");
            var elements = ea.getViewElements();
            debugLog("STEP-2", "Nombre éléments", elements ? elements.length : "null");
            
            if (!elements) {
                debugLog("ERROR", "Impossible d'obtenir les éléments");
                new Notice("❌ Impossible d'accéder aux éléments du dessin");
                reject(new Error("Éléments non accessibles"));
                return;
            }
            
            if (elements.length === 0) {
                debugLog("ERROR", "Aucun élément dans le dessin");
                new Notice("❌ Aucun élément dans le dessin. Créez d'abord un objet.");
                reject(new Error("Dessin vide"));
                return;
            }
            
            if (elements.length > 1) {
                debugLog("WARN", "Trop d'éléments dans le dessin");
                new Notice("⚠️ Le dessin doit contenir exactement UN objet. Sélectionnez ou supprimez les autres éléments.");
                reject(new Error("Trop d'éléments"));
                return;
            }

            debugLog("STEP-2", "Validation éléments OK - 1 élément trouvé");

            // Test de askForTemplateName
            debugLog("STEP-3", "Test askForTemplateName");
            askForTemplateName().then(function(nomTemplate) {
                debugLog("STEP-3", "Nom template reçu", nomTemplate);
                
                if (!nomTemplate) {
                    debugLog("ERROR", "Nom template vide");
                    reject(new Error("Nom template requis"));
                    return;
                }

                // Test de selectObjectType
                debugLog("STEP-4", "Test selectObjectType");
                selectObjectType().then(function(typeObjet) {
                    debugLog("STEP-4", "Type objet reçu", typeObjet);
                    
                    if (!typeObjet) {
                        debugLog("ERROR", "Type objet vide");
                        reject(new Error("Type objet requis"));
                        return;
                    }

                    debugLog("SUCCESS", "Toutes les étapes validées");
                    debugLog("SUCCESS", "Template: " + nomTemplate + ", Type: " + typeObjet);
                    
                    new Notice("✅ Test réussi ! Template: " + nomTemplate + " (" + typeObjet + ")");
                    resolve("Test terminé avec succès");
                    
                }).catch(function(typeError) {
                    debugLog("ERROR", "Erreur sélection type", typeError.message);
                    reject(typeError);
                });
                
            }).catch(function(nameError) {
                debugLog("ERROR", "Erreur nom template", nameError.message);
                reject(nameError);
            });

        } catch (mainError) {
            debugLog("ERROR", "Erreur dans main", mainError.message);
            console.error("❌ Erreur fatale main:", mainError);
            reject(mainError);
        }
    });
}

// Demander le nom du template avec logging
function askForTemplateName() {
    debugLog("INPUT", "Début askForTemplateName");
    
    return new Promise(function(resolve, reject) {
        try {
            debugLog("INPUT", "Vérification utils.inputPrompt", typeof utils.inputPrompt);
            
            if (!utils || !utils.inputPrompt) {
                debugLog("ERROR", "utils.inputPrompt non disponible");
                reject(new Error("utils.inputPrompt non disponible"));
                return;
            }
            
            debugLog("INPUT", "Appel utils.inputPrompt");
            
            var promptResult = utils.inputPrompt(
                "Nom du Template",
                "Entrez le nom du template à créer :",
                "",
                [{
                    caption: "Créer",
                    action: function() { 
                        debugLog("INPUT", "Action callback appelée");
                        return true; 
                    }
                }]
            );
            
            debugLog("INPUT", "Résultat prompt", typeof promptResult);
            
            if (promptResult && typeof promptResult.then === 'function') {
                debugLog("INPUT", "Prompt retourne une Promise");
                
                promptResult.then(function(result) {
                    debugLog("INPUT", "Résultat reçu", result);
                    
                    if (!result || result.trim() === "") {
                        debugLog("WARN", "Nom template vide");
                        new Notice("❌ Nom du template requis");
                        resolve(null);
                        return;
                    }
                    
                    var cleanName = result.trim();
                    debugLog("INPUT", "Nom nettoyé", cleanName);
                    resolve(cleanName);
                    
                }).catch(function(promptError) {
                    debugLog("ERROR", "Erreur prompt", promptError.message);
                    reject(promptError);
                });
            } else {
                debugLog("INPUT", "Prompt retourne directement", promptResult);
                resolve(promptResult);
            }
            
        } catch (inputError) {
            debugLog("ERROR", "Erreur askForTemplateName", inputError.message);
            reject(inputError);
        }
    });
}

// Sélection du type d'objet avec logging
function selectObjectType() {
    debugLog("SELECT", "Début selectObjectType");
    
    return new Promise(function(resolve, reject) {
        try {
            debugLog("SELECT", "Vérification utils.suggester", typeof utils.suggester);
            
            if (!utils || !utils.suggester) {
                debugLog("ERROR", "utils.suggester non disponible");
                reject(new Error("utils.suggester non disponible"));
                return;
            }
            
            debugLog("SELECT", "Appel utils.suggester");
            
            var suggesterResult = utils.suggester(
                function(item) { 
                    debugLog("SELECT", "Display function appelée", item);
                    return item; 
                },
                TYPES_OBJETS,
                false,
                "Sélectionnez le type d'objet ProcessMetaLanguage :"
            );
            
            debugLog("SELECT", "Résultat suggester", typeof suggesterResult);
            
            if (suggesterResult && typeof suggesterResult.then === 'function') {
                debugLog("SELECT", "Suggester retourne une Promise");
                
                suggesterResult.then(function(result) {
                    debugLog("SELECT", "Type sélectionné", result);
                    resolve(result);
                    
                }).catch(function(selectError) {
                    debugLog("ERROR", "Erreur suggester", selectError.message);
                    reject(selectError);
                });
            } else {
                debugLog("SELECT", "Suggester retourne directement", suggesterResult);
                resolve(suggesterResult);
            }
            
        } catch (selectError) {
            debugLog("ERROR", "Erreur selectObjectType", selectError.message);
            reject(selectError);
        }
    });
}

// === EXÉCUTION DEBUG ===
debugLog("EXEC", "Lancement du script principal");

main().then(function(result) {
    debugLog("SUCCESS", "Script terminé avec succès", result);
    console.log("🟢 [FIN] === SCRIPT DEBUG TERMINÉ AVEC SUCCÈS ===");
}).catch(function(error) {
    debugLog("ERROR", "Script terminé avec erreur", error.message);
    console.error("🔴 [ERREUR] === SCRIPT DEBUG ÉCHOUÉ ===", error);
    new Notice("❌ Erreur: " + error.message);
});

console.log("🟡 [INFO] Script debug lancé, vérifiez les logs dans la console");
```