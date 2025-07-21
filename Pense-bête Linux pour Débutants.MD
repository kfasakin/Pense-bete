# Pense-bête Linux pour Débutants

## Commandes de base

### `ls` (liste les fichiers/dossiers)
- `-l` : Détails (permissions, taille, propriétaire)  
- `-a` : Affiche les fichiers cachés  
- `-h` : Tailles lisibles (Ko, Mo)  
- `-R` : Liste récursive des sous-dossiers  

### `cd` (change de répertoire)
- `..` : Remonte d'un niveau  
- `-` : Reviens au répertoire précédent  
- `~` : Répertoire personnel  

### `pwd`
Affiche le chemin absolu du répertoire courant  

### `mkdir` (crée un dossier)
- `-p` : Crée les dossiers parents si inexistants  

### `rm` (supprime fichiers/dossiers)
- `-r` : Suppression récursive  
- `-f` : Force sans confirmation  

### `cp` (copie fichiers/dossiers)
- `-r` : Copie récursive  
- `-i` : Demande confirmation avant écrasement  
- `-v` : Affiche les fichiers copiés  

### `mv` (déplace/renomme)
- `-i` : Confirmation avant écrasement  
- `-v` : Affiche les fichiers déplacés  

### `touch` (crée fichier vide)
- `-c` : Ne crée pas si existant  
- `-t` : Définit date/heure spécifique  

## Gestion des permissions

### `chmod` (modifie permissions)
- `+x` : Ajoute permission d'exécution  
- `-r` : Supprime permission de lecture  
- `777` : Toutes permissions pour tous  

### `chown` (change propriétaire)
- `user:group` : Définit propriétaire et groupe  
- `-R` : Applique récursivement  

## Bonnes pratiques
- Toujours vérifier avant de faire `rm -rf` !
- Utiliser `-i` pour les opérations critiques
- `-h` rend les tailles plus lisibles
