# PROXMOX-Winter-2019
## Projet PROXMOX 

### Sommaire : 
 1. Pré-requis d'architecture.
 2. Inventaire Technologique
 3. But du projet.
 4. Déroulement du projet.
 5. Schéma d'architecture.
 6. Gestion du Maintien en Conditions Opérationnelles. 
### 1.Pré-requis d'architecture :

Serveur Proxmox hébergé chez Kimsufi :

Processeur : i5-750 4 coeurs 2.67GHz.  
RAM : 16Go DDR3 1333MHz.  
Disque dur : 2To.  
OS : Linux Debian 10.            
Nom de la VM : Prox6Node1  

### 2. Inventaire Technologique :

Pour notre Lab nous allons utiliser la solution Proxmox 6.0.15 sur une Debian 10.  
Ansible 2.9 afin de lancer des payblooks de création de VMs ou de configuration.  
Et déployer 3 VMs avec un orchestrateur Jenkins 2.205, une VM avec pfSense 2.4.4-p3 et un site web sous WordPress en 5.2.4.  

### 3. But du projet :

Le projet consiste à faire monter des VM avec Ansible sur le serveur Proxmox. 
Le but de ce projet est de pouvoir facilement déployer une infrastructure (Monitorée et sauvegardée).  
Et également pouvoir changer les configurations des VMs distantes.

### 4. Qu'est-ce que Proxmox ?
Proxmox est une solution de virtualisation basé sur Linux KVM (Debian 64bits) permettant de créer des machines virtuelles de type OpenVZ et KVM. Il s’agit d’une solution de type bare metal dans le sens de directement opérationnel sur la machine, c’est-à-dire sans OS. Ce nom caractérise les hyperviseurs de type 1 (on dit aussi natif) dans lequel l’hyperviseur minimaliste, allégé et optimisé, se conduit comme un moniteur démarrant le matériel, connectant le réseau et lançant les machines virtuelles. ESX Server de VMware, LPAR de IBM ou encore HYPER-V de Microsoft sont des hyperviseurs type 1. Proxmox s’administre via une interface web (https://serveur_proxmox:8006/ ) et fournit une vue globale de l’ensemble des VM installées . En plus de cette interface web, il est tout à fait possible de créer des scripts pour automatiser certaines tâches, via les commandes natives de OpenVZ (vzctl).

### 5. Schéma Réseau :

![alt text](https://github.com/alexdoret33/PROXMOX-Winter-2019/blob/master/Images/Schéma%20Réseau.jpg)

### 6. Déroulement du projet :

Nous allons commencer par installer un serveur Proxmox sur un serveur hébergé et faire monter des VMs Linux avec des configurations spécifiques. Un serveur WordPress et un pfSense ce qui va permettre de faire du NAT et du PAT pour accéder aux VM crée à l'aide d'Ansible. Afin de pouvoir déployer un environnement / configuration à distance sur des serveurs qui ne sont pas hébergés chez nous, nous allons utiliser une technologie différente de celle que l'on utilise habituellement : Proxmox. 

### 7. Schéma d'architecture :
![alt text](https://github.com/alexdoret33/PROXMOX-Winter-2019/blob/master/Images/Diagramme.jpg)

### 8. Gestion du Maintien en Conditions Opérationnelles : 

*Moyen de sauvegarde :* Tous les matins une sauvegarde sera réalisée de chacune des VM via un playbook qui lancera des commandes `vzdump`.
**Exemple :** `ansible-playbook -i ....`

*Création d'une VM :* Lancer le playbook "Nom_du_Playbook" qui créera la VM voulu.
**Exemple :** `ansible-playbook -i ....`

*Suppression d'une VM :* Lancer le playbook "Nom_du_Playbook" afin de supprimer la VM correspondante. **Exemple :** `ansible-playbook -i ....`
