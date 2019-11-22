# PROXMOX-Winter-2019
Projet PROXMOX 

## Sommaire : 
 1. Pré-requis d'architecture.
 2. Inventaire Technologique
 3. But du projet.
 4. Déroulement du projet.
 5. Schéma d'architecture.
 6. Gestion du Maintien en Conditions Opérationnelles. 
## 1.Pré-requis d'architecture :

Serveur Proxmox hébergé chez Kimsufi :

Processeur : i5-750 4 coeurs 2.67GHz.  
RAM : 16Go DDR3 1333MHz.  
Disque dur : 2To.  
<<<<<<< HEAD
OS : Linux Debian 10.
Nom de la VM : Prox6Node1  

## 2. Inventaire Technologique 

Pour notre Lab nous allons utiliser la solution Proxmox 6.0.15 sur une Debian 10.  
Ansible 2.9 afin de lancer des payblooks de création de VMs ou de configuration.  
Et déployer 3 VMs avec un orchestrateur Jenkins 2.205, une VM avec pfSense 2.4.4-p3 et un site web sous WordPress en 5.2.4.
=======
OS : Linux Debian 9 .  
>>>>>>> c94ff03d27345aba5bb94baa7dbdc4ff74dcd07b

## 3. But du projet :

Le projet consiste à faire monter des VM avec Ansible sur le serveur Proxmox. Et comme exemple on va monter un portail captif.
Le but de ce projet est de pouvoir facilement déployer une infrastructure vers un site distant.  
Et également pouvoir changer les configurations des VMs distantes.

## 4. Déroulement du projet :

Nous allons commencer par installer un serveur Proxmox sur un serveur hébergé. Et faire monter des VMs Linux avec des configurations spécifiques tel qu'un portail captif, un serveur apache à l'aide d'Ansible. Afin de pouvoir déployer un environnement / configuration à distance sur des serveurs qui ne sont pas hébergés chez nous. Ils utilisent une technologie différente de celle qu'on utilise habituellement (Proxmox). 

## 5. Schéma d'architecture.
![alt text](https://github.com/alexdoret33/PROXMOX-Winter-2019/blob/master/Images/Diagramme%20Cool.png?raw=true)

## 6. Gestion du Maintien en Conditions Opérationnelles : 

*Moyen de sauvegarde :* Tous les matins une sauvegarde sera réalisé de chacune des VM via un playbook qui lancera des commandes `vzdump`.
**Exemple : ** `ansible-playbook -i ....`

*Création d'une VM :* Lancer le playbook "Nom_du_Playbood" qui créera la VM voulu.
**Exemple :** `ansible-playbook -i ....`

*Suppression d'une VM :* Lancer le playbook "Nom_du_Playbook" afin de supprimer la VM correspondante. **Exemple :** `ansible-playbook -i ....`