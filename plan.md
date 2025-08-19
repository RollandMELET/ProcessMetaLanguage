  # Plan d'Action ProcessMetaLanguage - Stratégie Simplifiée v2

  **Date** : 2025-01-19
  **Version** : 2.0
  **Auteur** : Rolland MELET & Claude Code

  ## 📋 Objectif Principal
  Créer progressivement un système pour enrichir les dessins Excalidraw avec des "BackOfTheCard" structurés, en
  commençant très simplement.

  ## 📂 Architecture des Environnements

  ### Environnement Principal (Production)
  - **Chemin**: `/Users/rollandmelet/Library/CloudStorage/GoogleDrive-rm@360sc.io/Mon Drive/OBSIDIAN/CHATTERS/002 -
  Projets/PERSO/ProcessMetaLanguage`
  - **Usage**: Développement et documentation principale
  - **État**: Contient les templates v2.0.0 créés

  ### Environnement de Test
  - **Chemin**: `/Users/rollandmelet/Développement/Projets/ProcessMetaLanguage/VaultTestObsidian/PML-Beta-Test-v2`
  - **Usage**: Tests avant déploiement
  - **État**: Vault de test isolé

  ## 🔄 Plan en 5 Phases

  ### PHASE 1 : Configuration GitHub (15 min)
  1. Créer `.gitignore` pour exclure `old/` et fichiers d'environnement
  2. Initialiser le repo GitHub
  3. Premier commit structuré

  ### PHASE 2 : Script "Add BackOfTheCard to Drawing" (45 min)
  Créer `add-backofcard-to-drawing.md` qui :
  - S'applique à un fichier Excalidraw existant contenant un seul objet
  - Demande le nom du template (qui sera aussi le nom du fichier créé)
  - Génère automatiquement un template avec structure BackOfTheCard
  - Ajoute côté dessin : le type (OBJET, WORKFLOW, PROCESS, ÉTAT, ACTION, etc.) + placeholder du nom
  - Crée des sections pré-remplies avec exemples pour guider l'utilisateur :
    * Métadonnées YAML
    * Description
    * **Rules** (règles de gestion)
    * Propriétés (avec exemples)
    * Relations
    * États/Transitions
    * Notes

  ### PHASE 3 : Validation et Ajustement du Template Généré (15 min)
  - Tester le script sur un dessin simple
  - Vérifier que le template généré est éditable
  - Ajuster la structure si nécessaire
  - S'assurer que les exemples sont pertinents et inspirants

  ### PHASE 4 : Test dans Environnement de Test (20 min)
  1. Copier le nouveau script vers le vault de test
  2. Créer un dessin Excalidraw simple
  3. Tester le script
  4. Documenter les résultats

  ### PHASE 5 : Documentation et Itération (15 min)
  6. Créer guide d'utilisation simple
  7. Préparer la prochaine itération
  8. Liste des améliorations futures

  ## ⚡ Principes Directeurs
  - **KISS** : Commencer très simple
  - **Itératif** : Complexifier progressivement
  - **Testable** : Chaque phase produit quelque chose de testable
  - **Documenté** : Chaque étape est documentée
  - **Séparation** : Test isolé avant déploiement

  ## 📊 Résultats Attendus
  - ✅ GitHub configuré
  - ✅ Script simple fonctionnel
  - ✅ Template workflow basique
  - ✅ Tests effectués
  - ✅ Documentation claire

  ## 🚀 Futures Itérations
  Après validation de cette base :
  1. Enrichir progressivement les templates
  2. Ajouter de l'intelligence au script
  3. Créer les scripts de liaison
  4. Développer le transformateur canvas→markdown