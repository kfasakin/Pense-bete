# Pense-bête Commandes Linux

## Répertoires
- Répertoire pour logiciel tiers isolé : `/opt` ✅
- But principal du répertoire `/tmp` : Fichiers temporaires supprimables au redémarrage ✅
- Sous-répertoire de `/var` pour fichiers temporaires persistants : `/var/tmp` ✅

## Processus
- Afficher processus contenant "ssh" avec détails : `ps aux | grep ssh` ✅
- Afficher uniquement les PID des processus "ssh" : `pgrep ssh` ✅
- Observer en temps réel CPU, mémoire, processus : `top` ✅

## Fichiers/Répertoires
- Supprimer répertoire et contenu sans confirmation : `rm -rf tempdir/` ✅
- Créer lien symbolique : `ln -s /chemin/vers/cible monlien` ✅
- Renommer/déplacer fichier : `mv ancien.txt nouveau.txt` ✅
- Copier récursivement avec attributs : `cp -a source/ destination/` ✅
- Lister fichiers (incl. cachés) triés par date : `ls -alt` ✅

## Navigation
- Retour au répertoire personnel : `cd ~` ✅
- Afficher chemin absolu du répertoire courant : `pwd` ✅

## Recherche/Historique
- Recherche inverse dans historique Bash : `Ctrl+R` ✅
- Raccourci début de ligne : `Ctrl+A` ✅
- Supprimer mot par mot vers début : `Ctrl+W` ✅

## Système
- Afficher mémoire (Mo) : `free -m` ✅
- Espace disque en unités lisibles : `df -h` ✅
- Infos système (noyau, OS...) : `uname -a` ✅
- Compter fichiers dans dossier courant : `find . -maxdepth 1 -type f | wc -l` ✅

## Fichiers texte
- Afficher fichier page par page : `more docs.txt` ✅
- 10 premières lignes logs mysql : `journalctl -u mysql -n 10` ✅
- Compter lignes/mots/octets : `wc montexte.txt` ✅
- 15 premières lignes strictes : `head -n 15 fichier.txt` ✅
- Afficher infos fichier détaillées : `ls -l fichier.txt` ✅

## Divers
- Fork bomb : `:(){ :|:& };:` ✅
- Prérequis WSL2 : Activer "Windows Subsystem for Linux" ET "Virtual Machine Platform" ✅
- Rôle principal d'un shell : Interpréter commandes et gérer environnement ✅
