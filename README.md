## Projet SpaceZede

## Sommaire
- [Description du projet](#description)
- [Etat d'avancement du projet](#etat)
- [Schéma synoptique du projet](#schema)
- [Outils du projet](#outils)
- [Spécifications techniques du projet](#specifications)
- [Étape par étape](#etape)
- [FAQ](#faq)

<a id="description"></a>
## Description du projet :
#### Présentation de la start-up SpaceZede :  
La société SpaceZede est une start-up prometteuse qui s'est faite remarquée ces dernières années pour ses inventions innovantes comme le lit intelligent connecté qui se plie en 2 tout seul, ou encore la carte de crédit boomerang qui revient quand on la jette.  
Après plusieurs déconvenues comme le lit intelligent connecté qui se pliait en 2 pendant que les gens dormaient dedans ou la carte de crédit boomerang qui ne revenait jamais quand on la jetait, la start-up envisage de faire de l'espèce humaine une espèce intra-marine en établissant des colonies dans les fonds marins à l'aide de la puce pulmonaire "Boule-Chite©" dopé à l'IA.  

#### Composition de la société :  
- Service Direction : Melon Lusk
- Service Finance : Jeff Cassosse
- Service Marketing : Mikey Trump, Kena louest
- Service Production : Dodge Vaillepeur, Clay huessebet
  
#### Objectif du projet :
Vous êtes une jeune équipe de freelances recrutés par la start-up pour mettre en place et gérer le SI en infogérance.  
En effet jusqu'à présent la société n'utilisait pour seul outil de communication que leur puce "neuraluc©" qui n'a pas donné grand résultat jusqu'à présent.  
On a souvent dit à Mikey Trump qu'il n'était pas toujours le pingouin qui glissait le plus loin. Alors quand il a vu qu'il existait un OS avec un pingouin ou un truc du genre, il a souhaité en être équipé. Du coup Kena Louest pareil et Jeff Cassosse pareil.  
Tous les autres utilisateurs seront sous Windows.

<a id="etat"></a>
## Etat d'avancement du projet :
- [ ] Conception des documents : plan d'adressage IPv4 et IPv6 (LLA et ULA), listing machines, schéma synoptique, matrice des flux
- [ ] Configuration du routeur (VLANs et filtrage)
- [ ] Installation et configuration de l'ADDS avec role DNS et DHCP
- [ ] Installation et configuration du serveur de fichiers
- [ ] Installation et configuration du serveur GLPI
- [ ] Installation et configuration du serveur WEB
- [ ] Création des utilisateurs, des dossiers partagés, des GPO et des scripts
- [ ] Déploiement et configuration des machines clientes

<a id="schema"></a>
## Schéma synoptique du projet :

<p align="center">
  <img src="">
</p>

<a id="outils"></a>
## Outils du projet :

[Proxmox (8.2.7)](https://www.proxmox.com/)  
[pfSense (2.7.2)](https://www.pfsense.org/)   
[drawio-desktop (v24.7.17)](https://github.com/jgraph/drawio-desktop)  
[debian (12.8.0)](https://www.debian.org/)  
[Windows Server 2022)](https://www.microsoft.com/fr-fr/evalcenter/download-windows-server-2022)  
[Windows 10](https://www.microsoft.com/en-us/software-download)  
[Ubuntu 24.04](https://www.ubuntu-fr.org/download/)

<a id="specifications"></a>
## Spécifications techniques du projet :

- Serveurs : Windows Server 2022, Server Debian 12.8
- Clients : Ubuntu 24.04.1, Windows 10

<a id="etape"></a>
## Étape par étape : 

**[Etape-1.md](Etape-1.md) :**

- Conception des documents : plan d'adressage IPv4 et IPv6 (LLA et ULA), listing machines, schéma synoptique, matrice des flux

![Plan d'adressage réseau](https://github.com/user-attachments/assets/e92a52e1-01d5-4680-8fb4-14bea278f1be)  

![Liste des équipements](https://github.com/user-attachments/assets/f584062f-7f4d-4c82-9323-79982f59bd52)  

![Matrice des flux réseau](https://github.com/user-attachments/assets/072665e2-d043-43ef-8421-48d9fd4465c7)  

![Schema synoptique](https://github.com/user-attachments/assets/781559d4-ad2d-4794-a27d-1388a2302742)


**[Etape-2.md](Etape-2.md) :**

- Configuration du routeur (VLANs et filtrage)

 **[Etape-3.md](Etape-3.md) :**
 
- Installation et configuration de l'ADDS avec role DNS et DHCP
- Installation et configuration du serveur de fichiers
- Installation et configuration du serveur de téléphonie
- Installation et configuration du serveur GLPI
- Installation et configuration du serveur WEB

**[Etape-4.md](Etape-4.md) :**

- Création des utilisateurs, des dossiers partagés, des GPO et des scripts

**[Etape-5.md](Etape-5.md) :**

- Déploiement et configuration des machines clientes

<a id="faq"></a>
## FAQ :
	- En cours de construction...
