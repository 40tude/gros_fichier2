# Git, problème de synchro avec GitHub quand un fichier trop gros a déjà été commité

C'est un test car j'ai fait 2 fois la même bêtise


Tu sais ce que l'on dit : 
* 1 fois c'est une erreur
* 2 fois c'est une faute

Bref, il faut que je comprenne...

## Typiquement 

1. Je crée un répertoire local
1. Je crée des fichiers, des répertoires
1. Depuis VSCode je crée un dépot sur GitHub
1. Je commit, je synchronise, j'ajoute des fichiers, des sous-répertoires...
1. Tout se passe bien
1. À un moment donné, j'ajoute un gros fichier (gros.csv, >100MB) et là :
  * Le commit a lieu quand je le demande
  * La synchro ne passe pas  
  * github la refuse car un des fichiers est trop gros
  * Du coup je suis coincé car idéalement je souhaite : 
      1. supprimer `gros.csv` du dernier commit et/ou de la dernière synchro
      1. modifier/créer un fichier `.gitignore` où je mentionne `gros.csv` 
      1. faire une synchro dans laquelle il n'y a plus `gros.csv`

Bon allez, je viens de rajouter `gros.csv` dans un répertoire de test ainsi que cette ligne à ce `readme.md`. Ensuite je vais faire un commit et une synchro...

* Je suis de retour
* Ca a coincé
* On me propose de voir les logs mais je n'y comprends rien même si j'essaie de lire le contenu des lignes
* Dans la section source code control de VSCode, je vois le commit et à l'intérieur, le fichier `gros.csv`
* Idéalement j'aimerai faire un click droit, choisir, "remove from sync", créer un `.gitignore` et zou ce serait terminé


### Note
* https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github
* Le warning est à 50 MB et la limte haute à 100 MB
* Idéalement il souhaite que la taille des repos restent inférieure à 5GB




## Si on a fait qu'un commit et pas de sync
* https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github#removing-files-from-a-repositorys-history

* Ouvrir un terminal Git Bash 
  * `TODO` : faire un test dans terminal de VSCode

`cd OneDrive/Documents/Tmp/gros_fichier/`

* `gros.csv` est dans OneDrive/Documents/Tmp/gros_fichier/utils

```
git rm --cached ./utils/gros.csv
git commit --amend -CHEAD
git push
```


## Si on a fait une tentative de sync APRES un commit
* Voir https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository
* On propose d'utiliser
  1. git filter-pro
  1. BFG Repo-Cleaner



1. Laisser VSCode ouvert si besoin
1. Ouvrir un terminal Windows
1. `conda create --name filter_repo python=3.11`
1. `conda activate filter_repo`
1. `conda install git-filter-repo`                      
    * marche pas
1. `python3 -m pip install --user git-filter-repo`      
    * marche pas
1. `pip install --user git-filter-repo`                 
    * fonctionne
    * Il indique : WARNING: The script git-filter-repo.exe is installed in `C:\Users\phili\AppData\Roaming\Python\Python311\Scripts` which is **not** on PATH. Consider adding this directory to PATH
1. cd `OneDrive/Documents/Tmp/gros_fichier/`
1. `git filter-repo --invert-paths --path .\utils\gros.csv`
    * réponse: `filter-repo is not a git command. See git --help`
    * C'est normal filter-repo est pas visible de git
    * On va créer un lien symbolique
1. `git --exec-path` 
    * C 'est pour savoir où git s'attend à trouver les exec à lancer
    * réponse `C:/Program Files/Git/mingw64/libexec/git-core`
1. Ouvrir un terminal **Admin**
1. Aller dans `C:/Program Files/Git/mingw64/libexec/git-core`
1. Saisir `New-Item -ItemType SymbolicLink -Path git-filter-repo.exe -Target C:\Users\phili\AppData\Roaming\Python\Python311\Scripts\git-filter-repo.exe` 
    * Ca créé un lien symbolique nommé git-filter-pro.exe qui pointe sur le vrai git-filter-pro.exe qui est dans `C:\Users\phili\AppData\Roaming\Python\Python311\Scripts`  
1. Revenir dans le terminal user qui est dans le répertoire du projet
1. `git filter-repo --force --invert-paths --path .\utils\gros.csv`
    * Ca a l'air de fontionner
    * Je suis surpris 
    * **TODO** : refaire un test car j'ai tapé get ESPACE filter-repo et pas git TIRET filter-repo
1. De retour dans VSCode (qui n'a jamais été fermé depuis le début)
1. Créer un fichier `.gitignore`  
    * Y mettre une ligne `gros.csv`
    * Sauver
1. Toujours dans VSCode terminer les modifs de ce fichier `readme.md`
1. Sauver
1. Là, ça coince mais on a la possibilité de cliquer sur `Overwrite`
1. Allez, commit, sync
1. Ca râle encore car il indique que le repo `gros_fichier` existe déjà
1. Je fini par créer un repo `gros_fichier2`

Ca correspond pas tout à fait à ce que je voulais.

Faut que je fasse encore 1 ou 2 tests.
1. Faire plus de 1 commit AVANT d'ajouter `gros.csv` et voir si à la fin on a bien gardé l'historique des commits. J'ai peur qu'ils soient perdus auquel cas tout ça n'aura servi à rien car on obtient le même résultat en créant un nouveau dépot sur GitHub (en gardant ou pas le précedent dépôt. Dans ce cas il faut bien sûr des noms différments)
    * J'ai vérifié sur Github directement. Dans le dépôt gros_fichier2 il y a bien tout l'historique du projet. 
1. Faire un test sous PowerShell quand on a ajouté gros.csv mais pas encore fait de commit

### Note 
* Command à utiliser pour repérer les fichiers qui dépassent une certaine taille
* `Get-ChildItem ./ -recurse | where-object {$_.length -gt 100000000} | Sort-Object length | ft fullname, length -auto`