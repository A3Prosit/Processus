
## **I. Définition des termes**

* Kernel

* C++

* OS

* Gestion des processus

* Différences architecturales

* Temps partagé

* Gestion mémoire

* allouée

* primitif système

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
## Structure d'un système d'exploitation :
Un OS permet de :
- Charger les programmes les uns après les autres 
- Emule une VM (couche abstraction logicielle pour simplifier aux programmeurs)
- Gestion des ressources (Tampons...)

**Système multi-tâches :** Exécuter plusieurs programmes simultanément :
- Processus : Instance de programme entrain de s'exécuter (task sous Linux)
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


## Structure externe d'un système d'exploitation :

- Noyau et utilitaires :
	- 
