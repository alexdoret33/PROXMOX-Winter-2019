# PROXMOX-Winter-2019
## Projet PROXMOX 

### Sommaire : 
 1. Pré-requis d'architecture.
 2. Inventaire Technologique
 3. But du projet.
 4. Qu'est-ce que Proxmox ?
 5. Qu'est-ce que pfSense ?
 6. Schéma Réseau
 7. Déroulement du projet.
 8. Schéma d'architecture.
 9. Gestion du Maintien en Conditions Opérationnelles. 
 10. Problème rencontré.

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

### 5. Qu'est-ce que pfSense ?

pfSense est un pare-feu open source, il utilise Packet Filter pour des fonctions de routage et de NAT lui permettant de connecter plusieurs réseaux informatiques. Il comporte l'équivalent libre des outils et services utilisés habituellement sur des routeurs professionnels propriétaires. pfSense convient pour la sécurisation d'un réseau domestique ou de petite entreprise.

Après une brève installation manuelle pour assigner les interfaces réseaux, il s'administre ensuite à distance depuis l'interface web et gère nativement les VLAN.

Comme sur les distributions Linux, pfSense intègre aussi un gestionnaire de paquets pour installer des fonctionnalités supplémentaires comme un proxy, un serveur VoIP1, un Portail Captif...//

## pfSense dans notre projet :

Le pfSense est utilisé pour faire du PAT et du NAT.
En effet, des règles sont crées pour rediriger la connexion vers la VM en fonction du port.

*Exemple :* 

VM1 : 10.10.10.2 port 2222

Règle Proxmox : si une connexion SSH est sur le port 2222, la connexion vers la VM1 est effectuée.

Connexion SSH : <IP_PROXMOX>:2222

Donc pour chaque VM crée, il faut une règle Proxmox associée pour pouvoir s'y connecter en ssh.

### 6. Schéma Réseau :

![alt text](https://github.com/alexdoret33/PROXMOX-Winter-2019/blob/master/Images/Schéma%20Réseau.jpg)

### 7. Déroulement du projet :

Nous allons commencer par installer un serveur Proxmox sur un serveur hébergé et faire monter des VMs Linux avec des configurations spécifiques. Un serveur WordPress et un pfSense ce qui va permettre de faire du NAT et du PAT pour accéder aux VM crée à l'aide d'Ansible. Afin de pouvoir déployer un environnement / configuration à distance sur des serveurs qui ne sont pas hébergés chez nous, nous allons utiliser une technologie différente de celle que l'on utilise habituellement : Proxmox. 

### 8. Schéma d'architecture :
![alt text](https://github.com/alexdoret33/PROXMOX-Winter-2019/blob/master/Images/Diagramme.jpg)

### 9. Gestion du Maintien en Conditions Opérationnelles : 

*Moyen de sauvegarde :* Tous les matins une sauvegarde sera réalisée de chacune des VM via un playbook qui lancera des commandes `qm`.  
**Exemple :** `ansible-playbook playbook-snapshot_VM.yml -i hosts -e "vmid=<VMID> snapname=<NAME>"`

Mais nous n'avons pas pu le valider car pour effectuer un snapshot il nous faut l'offre payante.
Et l'option Backup nécessite un autre disque dur dans notre noeud proxmox. Mais nous n'avons loué qu'un seul disque.

*Création d'une VM :* Lancer le playbook "playbook-create_VM.yml" qui créera la VM voulue.  
**Exemple :** `ansible-playbook playbook-create_VM.yml -i hosts -e "template=<NUMBER> ip=<IP_ADRESS> vmid=<VMID> name=<NAME> memory=<NUMBER_IN_MB> cores=<INTEGER> disk=<NUMBERS_IN_GB>"`

*Suppression d'une VM :* Lancer le playbook "Destroy_VM.yml" afin de supprimer la VM correspondante.   
**Exemple :** `ansible-playbook playbook-destroy_VM.yml -i hosts -e "vmid=<VMID_DELETED_VM>"`

### 10. Problème rencontré :

Lors du lancement du playbook Ansible vers notre Promox nous avons eu une erreur : 

`authorization on proxmox cluster failed with exception: invalid literal for float(): 6.0-4`

Nous avons du modifier la lib promox d'ansible afin de règler le soucis. Le problème a été fixé mais pas intégré à Ansible.
Il suffit de modifier le fichier :

`/home/User/.local/lib/python3.6/site-packages/ansible/modules/cloud/misc/proxmox_kvm.py`

Et modifier la ligne : 

`PVE_MAJOR_VERSION = 3 if float(proxmox.version.get()['version']) < 4.0 else 4`
en : `PVE_MAJOR_VERSION = int(proxmox.version.get()['version'].split('.', 1)[0])`
