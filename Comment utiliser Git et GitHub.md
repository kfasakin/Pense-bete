Voici un guide complet, exhaustif et ultra-détaillé au format Markdown (`.md`). Il est conçu comme une véritable documentation professionnelle, allant des concepts fondamentaux jusqu'aux commandes avancées de sécurisation, indispensables pour un administrateur système et réseau.

```markdown
# 📘 Le Guide Ultime : Maîtriser Git & GitHub de A à Z

> **Principe fondamental :** Git est votre machine à voyager dans le temps pour le code et les configurations d'infrastructure (IaC). GitHub en est la forge collaborative et sécurisée dans le Cloud.

---

## 🛠️ Partie 1 : L'Utilité — Pourquoi Git est Vital ?

Dans l'informatique moderne (et particulièrement en SysAdmin / DevSecOps), modifier un fichier directement en production sans historique est une faute grave. 

### À quoi ça sert concrètement ?
* **Le Droit à l'Erreur (Machine à remonter le temps) :** Une modification casse votre playbook Ansible ou votre script PowerShell ? En une commande, vous revenez à l'état exact où tout fonctionnait 10 minutes auparavant.
* **Le Suivi des Responsabilités (Auditable) :** Chaque ligne de code modifiée est signée par un auteur, une date et un message explicatif. C'est l'historique parfait pour la traçabilité de sécurité.
* **Le Travail Collaboratif Synchrone :** Plusieurs administrateurs peuvent travailler sur la même infrastructure en même temps sans jamais écraser les modifications des autres, grâce au système de branches.
* **Le Hub Centralisé (GitHub) :** Permet de centraliser le code, de gérer les droits d'accès (RBAC) et de déclencher des automatisations (CI/CD) comme le déploiement automatique d'une machine virtuelle dès qu'un script est validé.

---

## 📐 Partie 2 : L'Architecture de Git (Comprendre le flux)

Avant de taper une commande, vous devez visualiser la cinématique d'un fichier à travers les **4 zones de Git** :

```text
 [ Espace de Travail ]  ---> ( Modifications locales de vos fichiers )
        |
        |  git add
        v
 [ Zone d'Index (Staging) ] ---> ( Préparation du "carton" de déménagement )
        |
        |  git commit
        v
 [ Dépôt Local (.git) ]     ---> ( Le carton est scellé dans l'historique sur votre PC )
        |
        |  git push
        v
 [ Dépôt Distant (GitHub) ] ---> ( Le carton est envoyé en sécurité dans le Cloud )

```

---

## 🚀 Partie 3 : Configuration Initiale & Sécurisation SSH

### 1. Configuration de l'Identité Locale

Dès l'installation de Git sur votre machine, vous devez déclarer qui vous êtes pour signer vos commits.

```bash
# Configuration globale de votre nom et de votre email
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@axantis.fr"

# Définir "main" comme nom de branche par défaut (standard moderne)
git config --global init.defaultBranch main

# Vérifier votre configuration
git config --list

```

### 2. Sécurisation de la liaison avec GitHub via clé SSH

L'authentification par simple mot de passe est obsolète et interdite par GitHub. On utilise une paire de clés cryptographiques **Ed25519** (plus robuste et rapide que RSA).

```bash
# 1. Générer la paire de clés asymétriques sur votre machine
ssh-keygen -t ed25519 -C "votre.email@axantis.fr"
# Appuyez sur Entrée pour accepter l'emplacement par défaut.

# 2. Démarrer l'agent SSH local pour stocker la clé
eval "$(ssh-agent -s)"

# 3. Ajouter votre clé privée à l'agent
ssh-add ~/.ssh/id_ed25519

# 4. Afficher et copier votre CLÉ PUBLIQUE
cat ~/.ssh/id_ed25519.pub

```

**Action sur GitHub :**
Aller dans *Settings* ➡️ *SSH and GPG keys* ➡️ *New SSH Key* ➡️ Coller le contenu de la clé publique.

```bash
# 5. Tester la connexion sécurisée vers GitHub
ssh -T git@github.com
# Si le message contient "Hi [VotreUser]! You've successfully authenticated...", c'est parfait.

```

---

## 🔄 Partie 4 : Le Workflow Quotidien (Pas à Pas)

### Scénario A : Vous créez un projet de zéro (Local ➡️ Distant)

```bash
# 1. Se positionner dans le dossier de votre projet
cd /chemin/vers/mon/projet_infra

# 2. Initialiser le dépôt Git local (création du dossier caché .git)
git init

# 3. Créer un fichier de documentation
echo "# Mon Projet SecOps" > README.md

# 4. Vérifier l'état de l'Espace de Travail
git status

# 5. Envoyer le fichier dans la zone d'Index (Staging Area)
git add README.md
# Pour tout ajouter d'un coup : git add .

# 6. Fixer l'état dans l'historique local (Commit)
git commit -m "feat: initialisation du projet et ajout du README"

# 7. Relier le dépôt local à votre dépôt vide créé sur GitHub
git remote add origin git@github.com:VotreUtilisateur/NomDeVotreDepot.git

# 8. Envoyer votre historique local vers GitHub
git push -u origin main

```

### Scénario B : Vous récupérez un projet existant (Distant ➡️ Local)

```bash
# 1. Cloner le dépôt GitHub sur votre machine
git clone git@github.com:VotreUtilisateur/NomDeVotreDepot.git

# 2. Aller dans le dossier créé
cd NomDeVotreDepot

# [Vous faites vos modifications sur vos scripts...]

# 3. Voir les lignes modifiées avant de valider
git diff

# 4. Valider et envoyer les modifications
git add .
git commit -m "fix: correction d'une règle de filtrage dans le script"
git push origin main

```

---

## 🌿 Partie 5 : Les Branches et la Gestion des Conflits

La branche `main` représente votre production. Pour développer une nouvelle fonctionnalité (ex: configurer un nouveau VPN pfSense), on crée une **branche d'isolation**.

```bash
# 1. Créer et basculer sur une nouvelle branche
git checkout -b feature/configuration-vpn
# Alternative moderne : git switch -c feature/configuration-vpn

# [Vous travaillez, modifiez, validez vos commits sur cette branche...]
git add .
git commit -m "feat: ajout des règles de tunnel IPsec"

# 2. Envoyer la branche sur GitHub pour relecture
git push origin feature/configuration-vpn

```

### Fusionner la branche (Merge)

Une fois les tests validés, on réintègre le code dans la production (`main`).

```bash
# 1. Revenir sur la branche principale
git switch main

# 2. Récupérer les éventuelles modifications faites par les collègues entre-temps
git pull origin main

# 3. Fusionner la branche de fonctionnalité dans main
git merge feature/configuration-vpn

# 4. Pousser la production à jour sur GitHub
git push origin main

# 5. Supprimer la branche locale devenue inutile
git branch -d feature/configuration-vpn

```

### 💥 Comment résoudre un Conflit de Merge ?

Un conflit survient si deux personnes modifient **la même ligne du même fichier** sur deux branches différentes et tentent de fusionner. Git s'arrête et marque le fichier.

Si vous ouvrez le fichier en conflit, vous verrez ceci :

```text
<<<<<<< HEAD
Le serveur DNS principal est 10.0.0.1 (Modif sur main)
=======
Le serveur DNS principal est 1.1.1.1 (Modif sur votre branche)
>>>>>>> feature/configuration-vpn

```

**Procédure de résolution :**

1. Discutez avec votre collègue pour savoir quelle ligne est la bonne.
2. Supprimez manuellement les balises (`<<<<<<<`, `=======`, `>>>>>>>`) et ne gardez que la ligne finale correcte.
3. Enregistrez le fichier.
4. Finalisez la fusion :

```bash
git add FichierConflit.txt
git commit -m "fix: résolution du conflit sur l'adressage DNS"
git push origin main

```

---

## 🛡️ Partie 6 : Le Hardening Git appliqué aux Admins Infra

### 1. L'Arme Absolue : Le fichier `.gitignore`

En tant qu'administrateur, vos scripts contiennent parfois des données sensibles (clés d'API, mots de passe de comptes de service, sessions de test). **Ils ne doivent JAMAIS monter sur GitHub.**

À la racine de votre projet, créez un fichier nommé impérativement `.gitignore` et listez-y les fichiers ou dossiers à exclure de l'indexation :

```text
# Exclure les fichiers de configuration contenant des secrets
.env
config.private.json
secrets.ps1

# Exclure les dossiers de clés ou certificats SSH
*.pem
*.key
/certs/

# Exclure les fichiers temporaires ou de caches système
.DS_Store
Thumbs.db
/tst/

```

### 2. Rattraper une erreur d'administration

| Commande | Effet opérationnel | Cas d'usage |
| --- | --- | --- |
| `git commit --amend` | Modifie le tout dernier commit local (message ou oubli de fichier). | Vous avez fait une faute de frappe dans votre dernier message de commit. |
| `git revert <ID_COMMIT>` | Crée un **nouveau** commit qui fait exactement l'inverse du commit spécifié. | Une modification a été poussée en production, elle bug, vous voulez l'annuler proprement sans casser l'historique des collègues. |
| `git reset --hard HEAD~1` | **Écrase et supprime** le dernier commit local et détruit les modifications de fichiers. | Vous avez fait n'importe quoi lors du dernier commit local, vous voulez l'effacer définitivement et revenir à l'étape juste avant. *(À manipuler avec précaution)* |

---

## 📊 Tableau Récapitulatif des Commandes de Survie

| Commande | Rôle |
| --- | --- |
| `git init` | Initialise un dépôt Git dans le dossier courant. |
| `git clone [URL]` | Télécharge un projet GitHub existant. |
| `git status` | Montre l'état des fichiers (modifiés, indexés, non suivis). |
| `git add [Fichier]` | Prépare le fichier pour le prochain commit. |
| `git commit -m "[Msg]"` | Enregistre l'état de l'index dans l'historique local. |
| `git push` | Envoie les commits locaux sur le serveur distant (GitHub). |
| `git pull` | Récupère et fusionne les derniers commits distants sur la machine locale. |
| `git log --oneline` | Affiche l'historique des commits de manière ultra-condensée. |

```

```
