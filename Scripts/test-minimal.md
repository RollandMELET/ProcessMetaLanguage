/*
Test Minimal pour vérifier l'environnement Excalidraw
Script de diagnostic pour valider les objets disponibles

Version: 1.0.0
Date: 2025-01-19  
Author: Rolland MELET & Claude Code

```javascript
*/
console.log("🚀 === TEST MINIMAL DÉMARRÉ ===");
console.log("📋 Test Minimal ASCII - Version 1.0.0");

try {
    console.log("✅ TEST: Basic logging works");
    
    if (typeof ea !== 'undefined') {
        console.log("✅ TEST: ea object available -", typeof ea);
        console.log("🔧 ea methods:", Object.getOwnPropertyNames(ea).slice(0, 10));
    } else {
        console.log("❌ TEST: ea object NOT available");
    }
    
    if (typeof app !== 'undefined') {
        console.log("✅ TEST: app object available -", typeof app);
        console.log("🔧 app.workspace available:", typeof app.workspace);
    } else {
        console.log("❌ TEST: app object NOT available");
    }
    
    if (typeof utils !== 'undefined') {
        console.log("✅ TEST: utils object available -", typeof utils);
        console.log("🔧 utils methods:", Object.getOwnPropertyNames(utils));
    } else {
        console.log("❌ TEST: utils object NOT available");
    }
    
    // Test Notice
    if (typeof Notice !== 'undefined') {
        new Notice("✅ TEST: Script executed successfully");
        console.log("✅ TEST: Notice function working");
    } else {
        console.log("❌ TEST: Notice function NOT available");
    }
    
    console.log("🎉 TEST: Script completed successfully");
    
} catch (error) {
    console.error("❌ TEST: Error occurred", error.message);
    console.error("🔍 Stack trace:", error.stack);
    if (typeof Notice !== 'undefined') {
        new Notice("❌ TEST: Error - " + error.message);
    }
}