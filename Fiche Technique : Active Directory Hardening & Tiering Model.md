# Fiche Technique : Active Directory Hardening & Tiering Model (SecOps)

Cette fiche détaille l'implémentation du modèle de cloisonnement administratif (Tiering) et les mesures de durcissement (hardening) indispensables pour bloquer les mouvements latéraux et la compromission des identités critiques.

---

## 1. Le Modèle de Tiering (Architecture de Confiance)

Le principe fondamental est l'**étanchéité stricte des privilèges**. Les comptes à hauts privilèges ne doivent jamais laisser d'empreintes mémorielles (dans LSASS) sur des machines moins sécurisées ou exposées.

[Image of Microsoft Enterprise Access Model tiering architecture]

### Tier 0 : Contrôle d'Identité Ultime
* **Périmètre :** Contrôleurs de domaine (DC), PKI d'entreprise (ADCS), serveurs d'authentification (MFA, RADIUS), serveurs de synchronisation Cloud (Entra ID Connect), et tous les comptes d'administration associés (Admins du domaine, Admins de l'entreprise).
* **Règle d’or :** Les administrateurs de Tier 0 ne s'authentifient **que** sur des systèmes de Tier 0.

### Tier 1 : Systèmes et Applications d'Entreprise
* **Périmètre :** Serveurs membres, bases de données (SQL), serveurs de fichiers, applications métiers, serveurs web internes, hyperviseurs gérant ces infrastructures.
* **Règle d’or :** Les administrateurs de Tier 1 gèrent les serveurs mais n'ont aucun droit sur le Tier 0, et ne se connectent jamais sur le Tier 2.

### Tier 2 : Postes Clients et Périphériques
* **Périmètre :** Postes de travail des utilisateurs (Windows 10/11), ordinateurs portables, imprimantes, smartphones, et comptes d'utilisateurs standards.
* **Règle d’or :** C'est la zone la plus exposée (phishing, malwares). Aucun secret de Tier 1 ou Tier 0 ne doit y transiter.

---

## 2. Hardening des Connexions par GPO (Implémentation)

Pour forcer le respect du Tiering, on utilise les stratégies de groupe (GPO) appliquées sur les Unités Organisationnelles (OU) de chaque Tier pour restreindre les droits d'attribution des utilisateurs (*User Rights Assignment*).

| Objectif de Sécurité | Configuration GPO à appliquer |
| :--- | :--- |
| **Protéger le Tier 0** | Appliquer une GPO sur les OUs Tier 1 et Tier 2 : <br> 🛑 *Interdire l'ouverture de session locale / TSE / Réseau* aux groupes d'admins Tier 0. |
| **Protéger le Tier 1** | Appliquer une GPO sur les OUs Tier 2 : <br> 🛑 *Interdire l'ouverture de session locale / TSE / Réseau* aux groupes d'admins Tier 1. |
| **Bloquer les comptes locaux** | Appliquer sur tout le domaine (sauf DC) : <br> 🛑 *Interdire l'accès à cette machine depuis le réseau* pour le compte `Administrateur` local (SID-500) pour contrer le Pass-the-Hash de compte à compte. |

---

## 3. Mécanismes de Durcissement (Hardening) Avancés

### A. Le Groupe "Protected Users"
Groupe de sécurité natif d'Active Directory (depuis Windows Server 2012 R2) qui applique un durcissement drastique et automatisé de l'authentification pour les comptes membres (à y placer impérativement pour le Tier 0) :
* **Désactivation totale de NTLM :** Seul Kerberos est autorisé.
* **Pas de mise en cache des secrets :** CredSSP, WDigest, et les clés claires de Kerberos ne sont pas stockées en mémoire RAM.
* **Durée des tickets réduite :** Le TGT Kerberos expire après 4 heures (au lieu de 10 heures par défaut) et ne peut pas être renouvelé indéfiniment.

### B. Windows Defender Credential Guard
Fonctionnalité de sécurité Windows basée sur la virtualisation (**VBS - Virtualization-Based Security**).
* **Mécanisme :** Utilise l'hyperviseur matériel (Hyper-V isolé) pour créer un conteneur sécurisé (LSA Iso) totalement étanche du reste du système d'exploitation.
* **Bénéfice :** Même si un attaquant devient `SYSTEM` sur la machine, il ne peut plus dumper le processus `LSASS` classique car les secrets Kerberos/NTLM n'y sont plus. Cela rend les outils comme *Mimikatz* inopérants pour le vol de credentials en mémoire.

### C. Les gMSA (Group Managed Service Accounts)
Comptes de services managés de manière centralisée par l'Active Directory.
* **Avantage Sec
