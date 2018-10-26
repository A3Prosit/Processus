
## **I. Définition des termes**

* Kernel : Noyau

* C++

* OS : Operating System - Systeme d'exploitation

* Gestion des processus

* Différences architecturales

* Temps partagé

* Gestion mémoire

* allouée

* primitif système : Fonction de bas niveau (de base) appelée pour construire d'autres fonctions plus évoluées (fork, open...)

* Gestion des utilisateurs

* Dynamiquement

* Procédure sous-jacente

* Architecture

* Gestion matériel

* Java

## **II. Analyse du contexte**

* Quoi ?

Pourquoi un algo algorithme java ne fonctionne pas en C++/Linux.

* Comment ?

En étudiant les processus (des différentes architectures).

* Pourquoi ?

Architectures différentes.

## **III. Définition de la problématique**

* Comment faire fonctionner un programme sur les deux OS ?

* Comment fonctionne les processus sous linux ?

* Comment fonctionne le kernel sous linux ?

## **IV. Définition des contraintes**

## **V. Généralisation**

Compatibilité

## **VI. Hypothèses**

* Incompatibilité est due à la différente gestion du matériel

* Gestion mémoire différente en java et c++ (garbage collector)

* Sous Linux, processus gérés sous ID alors que Windows en nom de services

* Sous linux, chaque thread est géré par des _tasks_struct_

* La gestion des temps de parole est différente

  

## **VII. Plan d’action**

**Études :**

* Différences entre java/C++

***Processus***

* Gestion (différents états etc...)

* Thread (synchronisation)

***Systèmes d’exploitation***

* Scheduler

* Architecture OS

* Architecture Kernel, deux types (les info les plus importantes)

* Rôles / OS

**A Faire :**

***Corbeille*** 1

*Corbeille secondaire

*** le plus important**




------------------

------------
## Différence entre Java et C++

Compilation et portablitié :
- C++ : Préprocessing, création de "fichiers objets", fournit dans un fichier binaire exécutable. Directement utilisable sur la machine. Il faut recompiler la source pour utiliser sur une autre machine.
- Java : Produit un code intermédiaire à partir d'une code source (non binaire), qui a besoin de la JVM (Java Virtual Machine) pour être interprété. Portable sur toute les machines disposant de la JVM.

/!\ Pointeurs :
- C++ : Pointeur représente l'@ de l'objet pointé
- Java : Pas de pointeur


## Processus et thread :

 ### 1 - Processus : 
Processus : Instance de programme entrain de s'exécuter (task sous Linux)

![](https://www.it-connect.fr/wp-content-itc/uploads/2016/06/graphe-hierarchie-up-p.jpg)
- Premier processus = Swapper (Processus 0 ou scheduler)
- Init = Espace User
- kthreadd = espace noyau

![](https://www.it-connect.fr/wp-content-itc/uploads/2016/06/graphe-hierarchie-user-p.jpg)
Dans l'espace utilisateur (PID 1), on va retrouver :
- Processus gettys : Surveillance des terminaux, gestion des connexions
- Processus daemon: Reste en tâche de fond jusqu'à qu'on face un appel à eux

![](https://www.it-connect.fr/wp-content-itc/uploads/2016/06/graphe-hierarchie-noyau-p.jpg)
Dans l'espace noyau :
- [ktrheadd] est le père
- threads noyaux qui gèrent la partie hardware de la machine. 
- Ils ont une priorité haute et s’exécute dans l'espace d'adressage du noyau.
- Tous les [kthread] sont en crochet (fils)

Commandes utilses : 
- ps : Voir la liste des processus
- top : voir la consommation en temps machine, mémoire et procésseur, en temps réel
- htop : Gestion des processus via une interface
- kill [pid]


**Etats d'un processus** :
- Initialisation : Premier état, en attente d'être mis à prêt. 
- Prêt : Chargé en mémoire, et attend son éxécution sur le processeur
- Elu : Processus en cours d'éxécution par le processeur
- Endormi / Bloqué : Intérrompu, ou attendu un évènement (signal)
- Terminé : Processus terminé, résultat est connu, ou ps shutdown
**Particulier**
- Zombie : Processus terminé et ne peut pas être déchargé de la mémoire (un de ses fils est en cours)
- Swappé : Transféré de la mémoire centrale dans la mémoire virtuelle.
- Préempté : Si un processus consomme trop de ressources, il pourra être préempté par "l'ordonnanceur"

**Fonctionement des processus**** : 

**Système multi-tâches :** Exécuter plusieurs programmes simultanément :
- Temps partagé : Quand un seul processeur, pour plusieurs exécutions, un programme va se faire exécuter dans un autre programme.
- Abstraction du déroulement :Chaque processus à un processeur virtuel, ils s'exécutent en parallèle.
- Variables d'environnement : 
	- Deux processus peuvent utiliser le même code
	- Quand deux processus s’exécutent, un peut être mis en attente/pause. Il est nécessaire de retrouver son état antérieur avec ces variables
- Espace mémoire processus : Chaque processus à son propre espace mémoire "Espace adressage processus"
-  Incidence sur le traitement des durées : Parfois minuteurs pour assurer transition entre processus

**Système multi-utilisateurs :** Capable d'exécuter de façon concurrente (se disputer l'accès à des ressources) et indépendante (réaliser son travail sans se préoccuper des autres) des applis à plusieurs users.
- Mise en place : Laps de temps à chaque user
- Mécanismes associés : 
	- Authentification (Identité)
	- Protection : Programmes qui pourraient bloquer / espionner activités ou processus autres users
	- Comptabilité : Limiter le volume des ressources alloués à chaque user
- Utilisateurs : Espace privé, quota espace disque, visibilité... User = UID (User Identifier)
- Groupe Utilisateurs : GID (Group Identifier)
- Super Utilisateur : Superviseur / Admin : root (accéder à tous les fichiers du système) mais pas du noyau.

 ### 2 - Thread : 

Chaque processus possède :
- Thread contrôleur unique (thread principal, le main)
- Tous les threads d'un même ps partagent le même espace d'adressage (et donc les variables)
- Création d'un thread 100 fois plus rapide qu'un processus
- Chaque variable et fonctions thread commencent par pthread_
- Si un thread lit un variable quand un autre thread la modifie ⇒ Mécanisme MUTEX (verroux)
	- Verroux, qui va protéger les données 
	- Soit disponible, soit vérouillé

**Commandes thread & cie**

- Fonctions relatives aux threads dans l'entête <pthread.h> et bibliothèque libpthread.a
-   pthread_create : _create_ = _créer_ en anglais (donc _créer le thread)_
-   pthread_exit : _exit_ = _sortir_ (donc _sortir du thread_)
-   pthread_join : _join_ = _joindre_ (donc _joindre le thread_)
-   PTHREAD_MUTEX_INITIALIZER : _initializer_ = _initialiser_ (donc _initialiser le mutex_)
-   pthread_mutex_lock : _lock_ = _vérouiller_ (donc _vérouiller le mutex_)
-   pthread_mutex_unlock : _unlock_ = _dévérouiller_ (donc _dévérouiller le mutex_)
-   pthread_cond_wait : _wait_ = _attendre_ (donc _attendre la condition_)


## Système d'exploitation :

⇒ Scheduller = première grande partie du kernel (on le vois après) == Gestionnaire de tâche & processus. Egalement le premier processus.

###  Les trois grandes fonctions d'un OS :

Un OS permet de :
- Charger les programmes les uns après les autres 
- Emule une VM (couche abstraction logicielle pour simplifier aux programmeurs)
- Gestion des ressources (Tampons...)

### Structure externe d'un système d'exploitation [KERNEL / NOYAU] :

- Noyau et utilitaires :
	- Ensemble de routines (sous-programmes) qui constituent le noyau (kernel) 
	- Chargé en mémoire vive à l'initialisation du système 
	- Contient procédures nécessaire au bon fonctionnement du systèmes
	- Les routines les moins importantes : "Utilitaires"
	- Quatres parties :
		- Gestionnaire de tâche / processus (Scheduler / ordonnanceur)
			-  Divise le temps en tranches
			- S'occupe d’interrompre le ps en cours et démarrer l'exécution d'un autre (temps allocation / bloqué...)
		- Gestionnaire de mémoire :
			- S'occupe des parties libres / occupées de la mémoire
			- S'occupe de l'allocation mémoire 
			- S'occupe de Swap entre le disque et la mémoire quand celui ci ne peut pas contenir tous les ps
		- Gestionnaire de fichiers :
			- Masque les spécificités à travers la notion de "fichiers"
		- Gestionnaire de périphériques
			- Envoie des commandes aux périphériques
			- Intercepteur interruptions
			- Traiter erreurs
			- Donne une interface pour le gérer (fichier sous Unix)
	- Deux parties auxiliaires :
	- Chargeur de l'OS : [Auxiliaire]
		- Exécution du BIOS (en RAM à une adresse bien déterminée)
		- Initialise les périphériques
		- Charge un secteur d'un disque et exécute ce qui y est placé
	- Interpréteur de commande : [Auxiliaire : Shell]
		- Exécute une boucle infinie qui affiche une invite, lit le nom du programme saisi par le user et l'exécute

### Structure interne d'un système d'exploitation  :

Il existe 5 pattern de structure "Interne" : 

- Systèmes monolithes :
	- Collection de procédures chacune pouvant appeler n’importe quelle autre procédure
	- Organisation chaotique la plus répandue
	- Chaque procédure est visible par les autres
	- Ex : MS-DOS
- Systèmes à modes noyau et utilisateur :
	- Système démarre en mode noyau :
		- Initialisation des périphériques
		- Routines de service pour appels systèmes
		- Passe en mode utilisateur
	- Mode utilisateur :
		- Pas accès aux périphériques
		- Doit utiliser appels systèmes vers le noyau
		- échanges entre noyau et user
- Systèmes à couches :
	- Chacune couche s'appuie sur celle qui lui est immédiatement inférieure
	![](http://www.noelshack.com/2018-43-5-1540539959-capture.png)
	
- Système à micro-noyau :
	- Primitives de synchronisation
	- Gestionnaire de tâche simple
	- Mécanisme de communication entre processus
	- Ce sont des processus systèmes qui s’exécutent au dehors pour implémenter les autres fonctionnalités d'un OS.	
	- Plus lent qu'un système monolithiques malgré beaucoup d'avantages (gestion mémoire vive, composantes matérielles dans le micro noyau...)
- Systèmes à modules :
	- Fichier objet lié au noyau appellé "Module"
	- Objet constitué de fichiers / pilote périphérique {...}
	- Exécuté en mode noyau  au nom du processus courant
	- On peut injecter le module dans le noyau
		- Approche modulaire : Chaque module peut-être lié ou dédié en cours d'exécution du système
		- Indépendance vis à vis de la plateforme : Cross-Platform
		- Utilisation économique de la mémoire : Ajout/suppression d'un module transparent 
		- Aucune perte de performances : Aucun passage de message (tout est à portée)

NB : 
- Appels systèmes : Instructions étendues pour la communication entre OS et User
- Signaux : Fournir des informations aux processus depuis l'OS.

