# Fiche de Révision : Administration d'Infrastructures Sécurisées

Cette fiche regroupe l'essentiel des concepts, protocoles et mécanismes de sécurité à connaître par cœur pour les examens et entretiens techniques. Les définitions sont volontairement courtes, précises et structurées par blocs de compétences.

---

## 1. Architecture Réseau & Sécurité

* **VLAN (Virtual LAN) :** Découpage logique d’un commutateur (switch) physique en plusieurs réseaux indépendants pour segmenter le trafic, réduire le domaine de diffusion et améliorer la sécurité.
* **DMZ (Demilitarized Zone) :** Sous-réseau isolé sécurisé par un pare-feu, qui héberge les serveurs accessibles depuis l'extérieur (ex: serveurs Web, Mail, DNS public) afin de protéger le réseau interne de l'entreprise.
* **VPN (Virtual Private Network) :** Tunnel sécurisé et chiffré (généralement via IPsec ou OpenVPN/WireGuard) qui interconnecte deux réseaux distants (Site-à-Site) ou un utilisateur nomade à un réseau d'entreprise (Accès Distant) à travers un réseau non sûr (Internet).
* **Pare-feu (Firewall) :** Équipement matériel ou logiciel qui filtre les flux réseaux entrants et sortants en fonction d'une politique de sécurité prédéfinie (règles de filtrage / ACL).
* **DHCP Relay :** Agent (souvent configuré sur un routeur ou un switch de niveau 3) qui intercepte et transfère les requêtes de diffusion (broadcast) DHCP entre un client et un serveur situés sur des sous-réseaux différents.

---

## 2. Environnement Microsoft & Gestion des Identités

* **Active Directory (AD DS) :** Service d'annuaire de Microsoft qui centralise la gestion des utilisateurs, des ordinateurs, des droits, des groupes et des ressources au sein d'une infrastructure réseau Windows (Domaine).
* **GPO (Group Policy Object) :** Stratégie de groupe permettant de configurer, gérer et sécuriser de manière centralisée et automatisée l'environnement des utilisateurs et des machines au sein d'un domaine Active Directory.
* **AGDLP :** Méthodologie recommandée par Microsoft pour la gestion des permissions : Les **A**ccounts (Comptes) sont placés dans des groupes **G**lobal (Globaux), eux-mêmes imbriqués dans des groupes **D**omain Local (Locaux de domaine), auxquels on applique enfin les **P**ermissions sur la ressource.

---

## 3. Virtualisation & Stockage

* **Hyperviseur de Type 1 (Bare Metal) :** Logiciel de virtualisation installé directement sur le matériel physique (ex: Proxmox VE, VMware ESXi, Hyper-V Server), offrant des performances optimales et une sécurité accrue pour les serveurs de production.
* **Hyperviseur de Type 2 :** Logiciel de virtualisation qui s'exécute par-dessus un système d'exploitation hôte existant (ex: VirtualBox, VMware Workstation), principalement utilisé pour le développement ou les tests.
* **Règle de sauvegarde 3-2-1 :** Stratégie de résilience face aux pertes de données consistant à posséder **3** copies des données, sur **2** supports physiques différents (ex: NAS et disque dur externe), avec **1** copie externalisée (hors site ou cloud).

---

## 4. Protocoles & Services Essentiels

* **DNS (Domain Name System) :** Service fondamental de l'Internet qui traduit les noms de domaine lisibles par l'homme (ex: google.com) en adresses IP utilisables par les machines.
* **NAT (Network Address Translation) :** Mécanisme qui traduit les adresses IP privées d'un réseau local en une (ou plusieurs) adresse IP publique pour permettre l'accès à Internet et économiser l'espace d'adressage IPv4.
* **SIEM (Security Information and Event Management) :** Outil centralisé qui collecte, normalise, analyse et corrèle les journaux d'événements (logs) de toute une infrastructure pour détecter, alerter et réagir face aux menaces de sécurité en temps réel.

---

## 5. Sécurité Avancée & Authentification

* **Kerberos :** Protocole d'authentification réseau par défaut dans Active Directory, utilisant un système de tickets sécurisés gérés par un KDC (Key Distribution Center) pour authentifier les entités sans faire transiter les mots de passe sur le réseau.
* **MFA (Multi-Factor Authentication) :** Méthode de vérification d'identité exigeant au moins deux facteurs de natures différentes parmi : ce que l'on sait (mot de passe), ce que l'on possède (smartphone/clé FIDO), ou ce que l'on est (biométrie).
* **Chiffrement Asymétrique :** Mécanisme de cryptographie utilisant une paire de clés mathématiquement liées : une **clé publique** pour chiffrer la donnée ou vérifier une signature, et une **clé privée** (gardée secrète par son propriétaire) pour déchiffrer ou signer.
* **RADIUS (Remote Authentication Dial-In User Service) :** Protocole centralisant la gestion des accès réseau (concept **AAA** : Authentification, Autorisation, Comptabilité/Accounting) pour sécuriser les connexions Wi-Fi d'entreprise ou les accès VPN.

---

## 6. Surveillance & Détection

* **IDS / IPS (Intrusion Detection / Prevention System) :** Équipements ou logiciels d'analyse du trafic réseau. L'**IDS** détecte et génère des alertes en cas d'activité suspecte ou de signature d'attaque, tandis que l'**IPS** va plus loin en bloquant activement la menace constatée.
* **Proxy (Serveur Mandataire) :** Serveur intermédiaire placé entre un utilisateur et Internet. Il permet de filtrer les accès web (URL, catégories), d'appliquer la politique de sécurité de l'entreprise, d'anonymiser le trafic interne et de mettre en cache les pages pour économiser la bande passante.

---

## 7. Haute Disponibilité & Résilience

* **Cluster de basculement (Failover Cluster) :** Groupe de serveurs physiques ou virtuels indépendants qui collaborent pour garantir la haute disponibilité d'une application ou d'un service. Si un nœud tombe en panne, un autre prend automatiquement le relais (**failover**) de manière transparente pour l'utilisateur.
* **RAID (Redundant Array of Independent Disks) :** Technologie de stockage combinant plusieurs disques physiques en une seule unité logique pour améliorer les performances (ex: RAID 0), la tolérance aux pannes (ex: RAID 1, RAID 5), ou un compromis des deux (ex: RAID 10).
* **PCA / PRA (Plan de Continuité / Reprise d'Activité) :** Ensemble des procédures techniques, humaines et organisationnelles d'une entreprise. Le **PCA** vise à maintenir les services informatiques critiques sans interruption lors d'une crise. Le **PRA** vise à reconstruire et restaurer l'infrastructure le plus rapidement possible après un sinistre majeur (incendie, cyberattaque massive).

---

## 8. Concepts Réseau Cruciaux

* **Routage Statique vs Dynamique :** Le routage statique est défini manuellement par l'administrateur réseau et reste fixe. Le routage dynamique s'appuie sur des protocoles (ex: OSPF, BGP) pour permettre aux routeurs d'échanger leurs tables de routage et de s'adapter automatiquement aux pannes ou aux changements de topologie.
* **ACL (Access Control List) :** Liste ordonnée de règles appliquées sur l'interface d'un routeur ou d'un pare-feu pour autoriser ou interdire le transit des paquets réseau en fonction de critères précis (adresses IP sources/destinations, ports TCP/UDP, protocoles).
