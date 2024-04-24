C'est un test car j'ai fait 2 fois la même bêtise
Tu sais ce que l'on dit : 
* 1 fois c'est une erreur
* 2 fois c'est une faute

Bref faut que je comprenne.


### Typiquement 
1. Je crée un répertoire local
1. Je crée des fichiers, des répertoires
1. Depuis VSCode je crée un dépot sur GitHub
1. Je commit, je synchronise, j'ajoute des fichiers, des sous-répertoires...
1. Tout se passe bien
1. A un moment donné, j'ajoute un gros fichier (gros.csv, >100MB) et là :
  * Le commit a lieu quand je le demande
  * La synchro ne se passe pas bien du tout 
  * Elle n'a pas lieu (github refuse car le fichier est trop gros)
  * Du coup je suis coincé car idéalement je souhaite : 
      1. supprimer le gros fichier du dernier comit et/ou de la dernière synchro
      1. modifier/créer un fichier .gitignore où je mentionne le gros fichier 
      1. faire la synchro dans laquelle du coup il n'y aura plus le gros fichier

Bon allez, je viens de rajouter gros.csv ainsi que cette ligne. Ensuite je vais faire un commit et une synchro...