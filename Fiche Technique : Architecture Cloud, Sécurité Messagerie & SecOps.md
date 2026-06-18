# 📑 Fiche Technique : Architecture Cloud, Sécurité Messagerie & SecOps

> [cite_start]**Contexte de production :** Sécurisation et transition vers la "Modern Auth" (OAuth 2.0) pour les services d'arrière-plan et périphériques (IoT) au sein d'Axantis[cite: 1, 2]. [cite_start]Documentation originale établie par Cabral Isaque le 05/02/2026[cite: 4].

---

## 🛡️ 1. Gestion des Identités (Microsoft Entra ID)

[cite_start]Pour permettre à un équipement ou un service tiers d'interagir avec le cloud, il est nécessaire de l'enregistrer dans l'annuaire via une identité numérique dédiée[cite: 46].

* [cite_start]**Inscription d'application (App Registration) :** Enregistrement de l'entité sous un nom d'affichage explicite, tel que `Scanner SMTP AUTH2`[cite: 46, 72].
* [cite_start]**Configuration d'accès (Tenant) :** Choix du mode **Locataire unique** (*Single Tenant*) pour restreindre l'usage de l'application exclusivement aux comptes de l'organisation[cite: 79].
* [cite_start]**Point d'ancrage local :** Utilisation d'un type de client public ou natif, configuré avec un URI de redirection défini sur `https://localhost`[cite: 84, 87, 88].

---

## 📊 2. Autorisations Déléguées vs d'Application

[cite_start]Le choix du type d'autorisation détermine la surface d'attaque et le comportement de l'application sur les API (Microsoft Graph / Exchange Online)[cite: 126, 133, 147].

* [cite_start]**Autorisations déléguées (Delegated) :** Requises lorsque l'application doit accéder à l'API en présence d'un utilisateur physiquement connecté[cite: 148, 149].
* [cite_start]**Autorisations d'application (Application) :** Indispensables lorsque l'application s'exécute de manière autonome en arrière-plan (Démon ou Service) sans aucun utilisateur connecté[cite: 150, 151].
* [cite_start]**Consentement de l'administrateur :** Approbation obligatoire et centralisée par un administrateur général du tenant pour valider les autorisations d'application critiques avant leur mise en production[cite: 117, 160].

> [cite_start]⚠️ **Principe du Moindre Privilège :** Pour un service automatisé de messagerie, on attribue exclusivement l'autorisation d'application spécifique `SMTP.SendAsApp` liée à Office 365 Exchange Online[cite: 141, 150, 157].

---

## 🔄 3. Focus : OAuth 2.0 Device Code Flow (DCF)

[cite_start]Ce flux d'autorisation est spécifiquement conçu pour les périphériques connectés (IoT, terminaux industriels ou copieurs de type Xerox VersaLink et AltaLink) qui ne possèdent pas de navigateur web complet pour la saisie des identifiants[cite: 2, 10, 186, 306].

### Les points de terminaison (Endpoints) Entra ID à maîtriser :
* [cite_start]**Nom d'hôte d'authentification :** `login.microsoftonline.com`[cite: 253, 254].
* [cite_start]**Chemin du code de l'appareil :** `oauth2/v2.0/devicecode`[cite: 255, 256].
* [cite_start]**Chemin d'obtention du jeton (Token) :** `oauth2/v2.0/token`[cite: 257, 258].

---

## 💻 4. Console SecOps : Durcissement via PowerShell

Par défaut, Microsoft 365 bloque l'authentification SMTP héritée (Basic Auth). [cite_start]Le durcissement consiste à maintenir ce blocage global tout en appliquant une exception granulaire par boîte mail de service[cite: 169, 177].

```powershell
# 1. Installation et chargement du module d'administration Exchange Online
Install-Module ExchangeOnlineManagement [cite: 171, 172]

# 2. Connexion sécurisée au tenant cloud avec un compte administrateur
Connect-ExchangeOnline [cite: 173, 174]

# 3. Audit de la posture de sécurité de la boîte de service spécifiée
# Si la valeur retournée est "True", le protocole SMTP hérité est correctement désactivé.
Get-CASMailbox -Identity "scan@tondomaine.com" | fl SmtpClientAuthenticationDisabled [cite: 176, 177]

# 4. Application de l'exception de sécurité (Activation du SMTP authentifié)
# On bascule la restriction à $false pour autoriser explicitement cette boîte mail.
Set-CASMailbox -Identity "scan@tondomaine.com" -SmtpClientAuthenticationDisabled $false [cite: 177]
