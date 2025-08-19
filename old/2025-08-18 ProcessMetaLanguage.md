# ToDo
- [ ] Génération des Templates Excalidraw avec "BackOfTheCard"
	- [ ] OBJET
	- [ ] ETAT
	- [ ] COMPORTEMENT
	- [ ] ACTION
	- [ ] VUE / AFFICHAGE
	- [ ] PROCESS
	- [ ] WORKFLOW

# Note
Je remonte la to-do list à l'envers. 

## Décrire les users story

À partir d'un dessin Excalidraw-Obsidian déjà existant, qui décrit un workflow complet avec plusieurs process ou sous-process pour des objets, Je veux pouvoir enrichir le dessin de fichiers embedded qui permettent, de façon standardisée, de décrire le processus des objets, leurs états et leurs actions. 
Pour cela, il faut que je dispose de scripts issus des scripts déjà réalisés, c'est-à-dire de nouvelles versions, de nouveaux scripts, sans modifier les scripts existants, mais en créant bien de nouveaux, qui me permettent de :
- Créer des objets 
- Créer leur processus 
- Creer les états
- Rajouter des suites d'état à l'objet. 
- Rajouter les comprotements et actions à l'objet. 
- Rajouter les vues liées au comportement, donc les affichages liés au comportement, au mode wireframe. 

Dans tous les cas il faut creer les templates corerspondant et peut être 
Il faut peut être redefinir l'ordre de priorité de ces template et script.


Ensuite, en étape finale, pouvoir avoir un script qui transforme ce canvas visuel en fichier markdown descriptif. 

## Faire le point sur les Scripts existants
### embed-object-from-template
Script qui permet de créer un objet depuis un template, de l'embarquer (embed) et d'en quantifier un dessin Excalidraw. 

le script et tous les fichiers de developpement liés sont dans : /Users/rollandmelet/Développement/Projets/EXCALIDRAW_Script_EmbededDrawingFromTemplate

### excalidraw-embedded-extractor
Ce script, inspiré de la Writing Machine de Vizyan, permet de récupérer les fichiers et le descriptif des fichiers embarqués dans un dessin Excalidraw, puis d’en faire un fichier Markdown. Il est à retravailler, car c’est une première version. 

/Users/rollandmelet/Développement/Projets/EXCALIDRAW_Template_ExtractEmbedded

## Faire le point sur les techniques, les plugin à utiliser.

Avant toutes choses il faut avoir das le contexte le fait que nous utilisaons : 
- https://github.com/obsidianmd
- https://github.com/zsviczian/obsidian-excalidraw-plugin
- https://github.com/zsviczian/obsidian-excalidraw-plugin/tree/master/ea-scripts
- https://github.com/blacksmithgu/obsidian-dataview (pour présenter les data associer aux objets)
- https://github.com/SilentVoid13/Templater
