# Fiche de Révision Avancée : Ingénierie & Architecture d'Infrastructures Sécurisées

Cette fiche regroupe des concepts complexes et des mécanismes de pointe indispensables pour la conception, le durcissement (hardening) et la défense des infrastructures modernes.

---

## 1. Cryptographie Avancée & Sécurisation des Flux

* **Perfect Forward Secrecy (PFS) :** Propriété cryptographique garantissant que la compromission de la clé privée de chiffrement à long terme (ex: la clé du serveur) ne permet pas de déchiffrer les sessions passées. Elle impose la génération de clés de session éphémères et uniques pour chaque flux (ex: via ECDHE).
* **Échange de clés Diffie-Hellman (DH) :** Protocole permettant à deux parties de générer une clé secrète partagée sur un canal non sécurisé, sans jamais transmettre la clé elle-même, en se basant sur la difficulté mathématique du logarithme discret.
* **HSTS (HTTP Strict Transport Security) :** En-tête de sécurité HTTP forçant le navigateur de l'utilisateur à communiquer avec le serveur uniquement via des connexions chiffrées (HTTPS), empêchant ainsi les attaques par dégradation de protocole (SSL Stripping).

---

## 2. Architecture Réseau Avancée & Segmentation

* **VXLAN (Virtual Extensible LAN) :** Protocole d'encapsulation (Overlay) de niveau 2 dans un réseau de niveau 3. Il permet de s'affranchir de la limite des 4096 VLANs (jusqu'à 16 millions de segments) et d'étendre des réseaux locaux à travers des infrastructures routées (idéal dans les Data Centers).
* **Micro-segmentation :** Pratique de sécurité consistant à segmenter l'infrastructure (souvent virtualisée ou cloud) au niveau le plus granulaire possible (par charge de travail ou conteneur), afin de bloquer les mouvements latéraux d'un attaquant au sein d'un même VLAN/sous-réseau.
* **SD-WAN (Software-Defined WAN) :** Architecture WAN qui utilise un contrôleur logiciel centralisé pour piloter intelligemment le trafic à travers de multiples liaisons physiques (MPLS, Internet, 5G), en fonction de la qualité de service (QoS) et des politiques de sécurité.

---

## 3. Gestion Avancée des Identités & Infrastructures Active Directory

* **Délégation contrainte Kerberos :** Fonctionnalité de sécurité permettant à un service (ex: un serveur Web) d'agir au nom d'un utilisateur auprès d'un autre service spécifique (ex: une base de données), tout en limitant strictement les cibles autorisées pour réduire la surface d'attaque en cas de compromission.
* **Modèle d'administration en couches (Tiering Model) :** Stratégie d'isolation d'Active Directory divisant l'infrastructure en niveaux de confiance (Tier 0 : Contrôleurs de domaine/Identités, Tier 1 : Serveurs/Applications, Tier 2 : Postes de travail). L'objectif est d'empêcher les comptes à hauts privilèges de se connecter sur des machines moins sécurisées pour contrer le vol de jetons (Pass-the-Hash).
* **GPO Loopback Processing :** Mode de traitement des stratégies de groupe qui permet d'appliquer des paramètres utilisateurs spécifiques en fonction de l'ordinateur sur lequel ils se connectent (ex: restreindre un profil utilisateur uniquement lorsqu'il est sur un serveur de rebond/TSE).

---

## 4. Cyberdéfense Active & Supervision

* **EDR / XDR (Endpoint / Extended Detection and Response) :** Évolution de l'antivirus traditionnel. L'**EDR** analyse en continu les comportements sur les endpoints (processus, registre, fichiers) pour détecter les menaces inconnues ou persistantes (APT). L'**XDR** étend cette détection en corrélant les données des endpoints, du réseau, du cloud et des emails.
* **Zero Trust Architecture (ZTA) :** Modèle de sécurité basé sur le principe « Ne jamais faire confiance, toujours vérifier ». Aucun utilisateur ou système n'est considéré comme sûr par défaut, qu'il soit à l'intérieur ou à l'extérieur du réseau périmétrique. Chaque accès fait l'objet d'une authentification, d'une autorisation et d'un chiffrement continus.
* **Pots de miel (Honeypots) :** Leurre informatique (serveur, service ou fichier) délibérément configuré pour être vulnérable afin d'attirer les cyberattaquants. Son but est de détecter précocement les intrusions, d'analyser les techniques de l'attaquant et de générer des alertes hautement qualifiées.

---

## 5. DevSecOps & Durcissement Cloud / Infra

* **Infrastructure as Code (IaC) Hardening :** Pratique consistant à intégrer des analyses de sécurité statiques (SAST) directement dans les scripts de déploiement d'infrastructure (Terraform, Ansible) afin de détecter les erreurs de configuration (ex: ports ouverts par défaut, droits excessifs) avant la mise en production.
* **Immutable Infrastructure :** Concept d'architecture où les serveurs ou conteneurs ne sont jamais modifiés ni mis à jour en production. Si un changement ou un correctif est requis, une nouvelle instance est construite à partir d'une image saine validée, déployée, et l'ancienne est détruite. Cela limite la dérive de configuration et l'ancrage des malwares.
