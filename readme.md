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

* Ca a coincé
* On me propose de voir les logs mais je n'y comprends rien même si j'essaie de lire le contenu des lignes
* Dans la section source code control de VSC, je vois le commit et à l'intérieur, le fichier gros.csv
* Idéalement j'aimerai faire un click droit, choisir, "remove from sync", créer un .gitignore et zou ce serait terminé
* 

### Note
* https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github
* Le warning est à 50 MB

* https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github#removing-files-from-a-repositorys-history
* git rm --cached gros.csv


### Si on a fait un commit et pas de sync
ouvrir un terminal Git Bash 
  * TODO : faire un test dans terminal de VSCode
cd OneDrive/Documents/Tmp/gros_fichier/
gros.csv est dans OneDrive/Documents/Tmp/gros_fichier/utils

git rm --cached ./utils/gros.csv
git commit --amend -CHEAD
git push


### Si on a fait une tentative de sync
* gitfilter pro
* BFG Repo-Cleaner
* Voir https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository
* 


1. ouvrir terminal conda
1. conda create --name filter_repo python=3.11
1. conda activate filter_repo
1. conda install git-filter-repo                      marche pas
1. python3 -m pip install --user git-filter-repo      marche pas
1. pip install --user git-filter-repo                 fonctionne
  * Il indique : WARNING: The script git-filter-repo.exe is installed in 'C:\Users\phili\AppData\Roaming\Python\Python311\Scripts' which is not on PATH.
  Consider adding this directory to PATH
1. cd OneDrive/Documents/Tmp/gros_fichier/
1. git filter-repo --invert-paths --path .\utils\gros.csv
  * réponse git: 'filter-repo' is not a git command. See 'git --help'.
1. git --exec-path 
  * réponse C:/Program Files/Git/mingw64/libexec/git-core
1. Ouvrir terminal Admin
1. Aller dans C:/Program Files/Git/mingw64/libexec/git-core
1. Saisir New-Item -ItemType SymbolicLink -Path git-filter-repo.exe -Target C:\Users\phili\AppData\Roaming\Python\Python311\Scripts\git-filter-repo.exe  
1. git filter-repo --force --invert-paths --path .\utils\gros.csv
  * Ca a l'air de fontionner
1. De retour dans VSCode (qui n'a jamais été fermé depuis le début)
1. Créer un git .ignore  
  * mettre une ligne gros.csv
1. Revenir dans l'arborescence
1. terminer les modifs de ce fichier readme.md
1. sauver
1. Là ça coince mais on a la possibilité de Owerite
1. Allez, commit, sync
